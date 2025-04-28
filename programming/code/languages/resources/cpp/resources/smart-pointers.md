# smart pointers are …

> Данный механизм является частным случаем идиомы **RAII**. При работе с указателями стоит постоянно обращать внимание на время жизни объектов под ними, а также освобождением памяти под динамически выделенный объект. Не всегда понятно извне, кто именно будет отвечать за удаление приходящего в аргументы функции объекта и т.д. и т.п. Для борьбы с такими нюансами указателей существует обёртка смарт-указателей.
> 

Общая логика работы смарт указателей заключается в том, что объект, находящийся под таким указателем становится уникальным, т.е. смарт указатель может обладать только “уникальными объектами”. Если объект под таким указателем уничтожится, то и такой указатель перестанет на что либо указывать (обычный указатель же продолжит), если такой указатель удалится, то объект под ним тоже исчезнет из памяти. При выходе из области видимости для объекта под таким указателем вызывается деструктор.

Существует несколько видов смарт-указателей:

# std::unique_ptr

> Умный указатель, владеющий динамически выделенным ресурсом. Он воплощает прям идею, что один указатель владеет одним уникальным объектом. По перфомансу полностью сопоставим с обычным указателем (Т*)
> 

```cpp
std::unique_ptr<int>    p1 = std::make_unique<int>();
std::unique_ptr<int[]>  p2 = std::make_unique<int[]>(50);
std::unique_ptr<Object> p3 = std::make_unique<Object>("Lamp");
```

```cpp
struct MyDeleter { // Создание "кастомного" удаления
    void operator()(int* ptr) {
        std::cout << "Custom Deleter: Deleting pointer" << std::endl;
        delete ptr;
    }
};

int main() {
    std::unique_ptr<int, MyDeleter> p1(new int(5), MyDeleter());
    return 0; // Custom Deleter will be called when p1 goes out of scope
}
```

# std::shared_ptr

> Умный указатель, владеющий разделяемым динамически выделенным ресурсом. Несколько std::shared_ptr могут владеть одним и тем же ресурсом, и внутренний счетчик ведет их учет. Когда кол-во обладателей объектом становится равным 0, тогда объект, соответственно, уничтожается. По перфомансу ощутимо медленнее обычного указателя + довольно толстый из-за наличия управляющего блока (подсчёта ссылок).
> 

```cpp
std::shared_ptr<int>    p1 = std::make_shared<int>();
std::shared_ptr<Object> p2 = std::make_shared<Object>("Lamp");
```

```cpp
void compute()
{
  std::shared_ptr<int> ptr = std::make_shared<int>(100);
  // ptr.use_count() == 1
  std::shared_ptr<int> ptr_copy = ptr;   // Сделать копию: с shared_ptr возможно!
  // ptr.use_count() == 2
  // ptr_copy.use_count() == 2, в конце концов, это одни и те же базовые данные.
} // Здесь `ptr` и `ptr_copy` выходят из области действия. Больше никаких ссылок  
  // исходные данные (т.е. use_count() == 0), поэтому они автоматически убираются.
int main()
{
  compute();
}
```

## Control block

> Для подсчёта ссылок на ресурс явно должен быть какой-то внутренний механизм… Владеющие ресурсом объекты shared_ptr совместно используют один блок управления. Блок управления содержит следующее:
> 
- количество объектов shared_ptr, владеющих ресурсом;
- количество объектов shared_ptr, указывающих на ресурс;
- метод удаления для этого ресурса, если он есть;
- пользовательский распределитель для блока управления, если он есть.

## std::shared_ptr<>() vs std::make_shared<>()

В давние времена, когда не было CTAD’а, стандартная библиотека наплодила множество make-методов для создания объектов, с С++17 и его появлением многое в плане шаблонов стало лучше… 

```cpp
void DoSth(std::shared_ptr<CoolType>(new CoolType(true)
           int a,
           std::function<void()> handler) {...}
```

В С++ компилятор спокойно имеет право при вызове данной функции инициализировать данные в любом порядке, разве что new CoolType будет вызван перед инициализацией shared_ptr.
Допустим, что будет такой порядок вызовов:

- new CoolType
- int a
- handler
- std::shared_ptr…

Теперь такая ситуация, что handler вызывает исключение.. new CoolType был создан на куче ранее, а shared_ptr его не получил.. утечка… **Т.е. в shared_ptr конструкции происходит аллокация отдельно объекта и отдельно управляющего блока → т.е. их 2.**
Для такого ранее как раз был бы полезен метод по типу **std::make_shared<>(), который инициализирует сразу и объект и управляющей блок для него → происходит лишь 1 аллокация объекта** 

<aside>
💡 В 17 стандарте такое пофиксили и теперь это не проблема. Возможно, что make_shared теперь даже медленнее shared_ptr. Также, из-за того, что управляющий блок живёт, пока есть weak_ptr, основной объект созданный таким методом будет жить неявно, пока есть weak_ptr, ведь он был создан вместе с управляющим блоком через make_shared.

</aside>

## shared_from_this (since C++11)

> Фича из STL (изначально была в boost), позволяющая классу делать shared_ptr на самого себя
> 

[https://en.cppreference.com/w/cpp/memory/enable_shared_from_this](https://en.cppreference.com/w/cpp/memory/enable_shared_from_this)

<aside>
💡 Если класс использует внутри себя share_from_this(), то объект такого класса должен быть создан исключительно с помощью shared_ptr, иначе вылетит bad_weak_ptr. Если объект является полем другого объекта, создаваемого с помощью shared_ptr, то это не считается, обязательно надо быть самому явно под shared_ptr

</aside>

**tricks:**

```cpp
// Fabric method
class Ttt 
{
	private: 
		Ttt() {};
		
	public:
		static std::shared_ptr<Ttt> Create()
		{
			return shared_from_this();
		}
}

// Expanding object lifetime
auto func_that_will_invoke_after_operation_complete =
	[lifetime = shared_from_this()] {};
	
async_read(any_socket_to_read_from, 
           any_flags, 
           func_that_will_invoke_after_operation_complete);
					 // gurantee that we will be alive until operation is done
```

# std::weak_ptr

> Подобен std::shared_ptr, но не увеличивает счетчик. Weak_ptr также не обладает объектом. Он существует для решения проблемы, что 2 shared_ptr могут указывать друг на друга, т.е. их уничтожение будет невозможно. В таком случае существует weak_ptr, если заменить один из shared_ptr на weak, то перекрёстное уничтожение станет возможным.
> 

<aside>
💡 В shared_ptr была речь про управляющий блок… так вот, использование weak_ptr удерживает существование этого блока, т.к. ему надо как-то понимать, жив объект или нет

</aside>

```cpp
std::shared_ptr<int> p_shared = std::make_shared<int>(100);
std::weak_ptr<int>   p_weak1(p_shared);
std::weak_ptr<int>   p_weak2(p_weak1);
```

```cpp
//Имеется класс компонента
struct Component: 
    public std::enable_shared_from_this<Component>
{   //Который содержит умные указатели на
    std::vector<std::shared_ptr<Component>> children;//дочерние компоненты
    std::shared_ptr<Component> parent;//и на родителя

    void add(std::shared_ptr<Component> v) 
    {
        children.push_back(std::move(v));
        children.back()->parent = shared_from_this();
    }
    Component()
    { 
        std::cout << "Component::Component()" << std::endl; 
    }
    ~Component() 
    { 
        std::cout << "Component::~Component()" << std::endl; 
    }
};

int main()
{
    std::shared_ptr<Component> component1 = std::make_shared<Component>();
    std::shared_ptr<Component> component2 = std::make_shared<Component>();
    component1->add(std::move(component2));
}
// удаление невозможно, однако если поменять
// std::shared_ptr<Component> parent; 
// на std::weak_ptr<Component> parent; 
// то всё будет удаляться
```

# std::auto_ptr (NEVER USE IT)

> Умный указатель древних времён, не стоит его более использовать… у него был неприятный для контейнеров механизм, называемый *разрушающим копированием,* при попытке обладать одним объектом двумя указателями происходило высвобождение одного из указателей, что косвенно провоцировало двойное удаление объекта. Для оператора delete такое поведение не страшно, однако при работе с контейнерами - очень даже. unique_ptr полностью заменил такое.
>