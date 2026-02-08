
> Данный паттерн известен также как "**Заместитель**", суть паттерна заключается в создании объекта-суррогата, который может выступать в роли другого объекта и замещать его.

## Субъекты

Мини-пример (Python):
```python
class Image:
    def __init__(self, path): self.path = path
    def show(self): print("render", self.path)

class LazyImage:
    def __init__(self, path): self.path = path; self._real=None
    def show(self):
        if not self._real: self._real = Image(self.path)  # отложенная загрузка
        self._real.show()

img = LazyImage("heavy.png")
img.show()  # создастся только при первом вызове
```

# Когда полезно
- Ленивая инициализация тяжёлых объектов.
- Логирование/кеширование/контроль доступа перед настоящим объектом.
### Subject

- Представляет собой общий интерфейс для Proxy и RealSubject

```cpp
class Image {
public:
	virtual void Display()  = 0;
};

```

### Real Object

- Реальный объект, для которого создаётся Proxy

```cpp
class RealImage final : public Image {
	std::string filename_;
public:
	explicit RealImage(const std::string filename) : filename_(filename) {
		std::cout << "Loading real image: " << filename_ << std::endl;
	}

public:
	virtual void Display override {
		print("Displaying image");
	}
}

```

### Proxy

- Заместитель реального объекта. Хранит ссылку на RealObject (т.е. контролирует доступ к нему), может управлять его созданием и удалением. Так же возможна переадресация запросов на RealObject

```cpp
class ImageProxy : public Image {
	std::unique_ptr<RealImage> real_image_;
	std::string filename_;
public:
	ImageProxy(const std::string filename) : filename_(filename), real_image_(nullptr) {}

public:
	void Display() override {
		if (!real_image_ )
			real_image_ = std::make_unique<RealImage>(filename);
		real_image_->Display();
	}
}

```

### Client

```cpp
int main()
{
	auto image = std::make_unique<ImageProxy>("example.jpg");

	// При первом вызове создаст объект и обратиться к нему
	image->Display();

	// Т.к. объект уже был создан, то повторно он не будет создан, и обращению к нему всё так же произойдёт
	image->Display();

	return 0;
}

```

## Нюансы

- Не очень удобно синхронизировать данные между реальным объектом и прокси

## Use-cases

- Когда надо осуществлять взаимодействие по сети, а объект-прокси должен имитировать поведения объекта в другом адресном пространстве. Использование прокси позволяет снизить накладные издержки при передачи данных через сеть. Подобная ситуация еще называется **удалённый заместитель (remote proxies)**
- Когда нужно управлять доступом к ресурсу, создание которого требует больших затрат. Реальный объект создается только тогда, когда он действительно может понадобится, а до этого все запросы к нему обрабатывает прокси-объект. Подобная ситуация еще называется **виртуальный заместитель (virtual proxies)**
- Когда необходимо разграничить доступ к вызываемому объекту в зависимости от прав вызывающего объекта. Подобная ситуация еще называется **защищающий заместитель (protection proxies)**
- Когда нужно вести подсчет ссылок на объект или обеспечить потокобезопасную работу с реальным объектом. Подобная ситуация называется **"умные ссылки" (smart reference)**
