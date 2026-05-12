---
tags: [common]
aliases: [Usual anatomy]
---

# Usual anatomy

![Untitled](../../../images/common__Untitled%2015.png)

💡 Написание своего сервера или сервиса не входит в терминологию ниже, это отдельные пользовательские абстракции

- Io object
    
    > Представляет собой некоторый api, позволяющий обращаться к Io service, а-ля использовать send, recieve и т.п., чаще всего представителями такого инстанции являются сокеты, acceptor’ы, resolver’ы и т.п.
    > 
- Io service
    
    > Сервис отвечает за определённого рода упаковку запросов с Io object’а в, грубо говоря, пары ID ↔ Handler, которые потом передаст в Io context
    > 
- Io context
    
    > Отвечает за создание очереди из пар ID ↔ Handler и их исполнение с помощью, например, операционной системы
    > 
    
    [Chapter 32. Boost.Asio - I/O Services and I/O Objects](https://theboostcpplibraries.com/boost.asio-io-services-and-io-objects)
    

# Future and Promise

> Это ключевые понятия, реализующие меж-поточное или асинхронное взаимодействие. Это некоторые абстрактные переменные, которые работают всегда в паре, работает это по принципу - “пока не придёт значение, ничего делать не буду” правильнее всего будет понять через пример:

```cpp
#define BOOST_THREAD_PROVIDES_FUTURE
#include <boost/thread.hpp>
#include <boost/thread/future.hpp>
#include <functional>
#include <iostream>

//
// Вычисляем некоторую сумму и сохраняем в обещание
//

void Accumulate(boost::promise<int> &p)
{
	int sum = 0;
	for (int i = 0; i < 5; ++i)
	{
		sum += i;
	}

	//
	// 4. Сохраняем integer в обещание
	//

	p.set_value(sum); 
}

int main()
{
	//
	// 1. "обещаем", что будет integer
	//

	boost::promise<int> p; 

	//
	// 2. связываем "обещание" и "будущую переменную"
	//

	boost::future<int> f = p.get_future(); 

	//
	// 3. Запускаем функцию Accumulate в другой поток, и передаём ему ссылку на "обещание"
	//

	boost::thread t{Accumulate, std::ref(p)}; 

	//
	// 5. Пытаемся получить из "будущей переменной" значение
	// Почему пытаемся и при чём тут "будущее" ?
	// вызов get() блокирующий, пока в него не поместим значение,
	// текущий поток будет заблокирован
	//

	std::cout << f.get() << '\n';
}
```

---

# Async wait

> Разница между wait и await → wait обычно блокирует вызывающий поток, пока не придёт результат/ответ, а await, условно, не блокирующий (там есть некоторая ос-ная реализаци

---

# Prepare/Commit for buffers

Возьму в пример, boost::asio буфферы, где есть buffer, mutable_buffer, dynamic_buffer и т.п. в разных реализациях сокетов. Они все могут принимать в себя контейнеры типа вектора, дабы каждый раз не реалокейтить память, но работает это всё вообще не автоматически…

```cpp
  // d_duff - просто обёртка буффер, он не является MutableSequence, 
  // т.е. нельзя обратиться к его началу и чё т начать перезаписывать
  auto d_buff = boost::asio::dynamic_buffer(buffer_);
  
  // 
  auto completion_token = 
	  [&d_buff,
     lifetime = shared_from_this(), 
		 handler  = std::move(external_handler)]
			 (const bsys::error_code& ec, std::size_t bytes_transferred) mutable { 
			 // async_receive подсунет нужные значения в такую сигнатуру c ec и bytes_transferred
	  //...
	  d_buff.commit(bytes_transferred); 
	  // зафиксирует то кол-во байт, которое по итогу было прочитано и "отпускает" лишнее
  }
  
  // prepare заранее резервирует последовательную область памяти в указанном кол-ве байт
  // и возращает MutableSequence, который требуется методу чтения данных с сокета
  spp_soc_t::async_receive(d_buff.prepare(4096),
                           flags, 
                           std::move(completion_token));
```

Для обычного буфера для каждого считывания нового может быть необходим вызов resize, dynamic_buffer именно относительно resize() делает всё автоматически