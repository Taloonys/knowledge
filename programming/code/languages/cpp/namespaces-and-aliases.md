# namespace

> Пространство имён 

```cpp
namespace showcase {
  constexpr void Func() noexcept {}
};

showcase::Func();

// also

{
  namespace sh = showcase;
  sh::Func();
}

{
  using namespace showcase; // <-- `using` example
  Func();
}

```
### Tips
- Как правило весь свой код включают в свои namespace'ы, не считая фреймворковых случаев
- Если вы пишете код прямо в `.h` и не хотите, чтобы какая-то его часть была видна из-вне то как практика - это оформляют так:
```cpp
namespace my_stuff {
namespace details {
  static constexpr auto private_param = 5;    // при инклюде данного файла, внутренний неймспейс (`details`) не будет виден "снаружи"
}

//...
}
```

# ADL
https://en.cppreference.com/w/cpp/language/adl

# using
```cpp
using clients_t = std::unordered_map<ClientId, ClientInfo>;
```
[Using-declaration - cppreference.com](https://en.cppreference.com/w/cpp/language/using_declaration)
### Tips
- использовать как можно чаще, чтобы сделать код читабельным... но знать меру!
