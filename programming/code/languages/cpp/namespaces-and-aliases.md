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

# ADL

https://en.cppreference.com/w/cpp/language/adl
# using

```cpp
using clients_t = std::unordered_map<ClientId, ClientInfo>;
```
[Using-declaration - cppreference.com](https://en.cppreference.com/w/cpp/language/using_declaration)
