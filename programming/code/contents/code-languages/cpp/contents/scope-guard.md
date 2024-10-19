
> Это идеология со специальной обёрткой на что угодно, что будет уничтожать некоторый подсовываемый в него объект…

# Наиболее простая и достаточная реализация

```cpp
template <typename F>
class ScopeExit
{
public:
    ScopeExit(F DeleteFunction) noexcept 
        : DeleteFunction_(DeleteFunction)
    { }

    ~ScopeExit() 
    { 
        DeleteFunction_(); 
    }

private:
    F DeleteFunction_;
};

// 
// Has to use this func for template until C++17 (no CTAD)
//

// template<typename F>
// ScopeExit<F> MakeScopeExit(F&& f);
// {
// 	 return ScopeExit<F>(std::forward<F>(f));
// } 

//
// usage (here for C++17 with CTAD)
//

void FileDisturb() // -> побеспокоить файл =)
{ 
  std::ofstream my_file("filename.txt");
  auto file_raii = ScopeExit([&my_file]{
	  my_file.close(); // говорим, как надо уничтожать наш объект
  }
  
  my_file << "Files can be tricky, but it is fun enough!";
} // уничтожается file_raii -> вызывает my_file.close()
```

# Какие-то примеры, если найдётся вдохновение…

```cpp
class ScopeGuard
{
public:
  ScopeGuard () 
   : engaged_ (true) 
  { /* Захват ресурса */ }
  
  ~ScopeGuard ()  
  { 
    if (engaged_) 
     { /* Освобождение ресурса */} 
  }
  void release () 
  { 
     engaged_ = false; 
     /* Ресурс не будет освобожден в деструкторе */ 
  }
  
private:
  bool engaged_;
};

void some_init_function ()
{
  ScopeGuard guard;
  // В случае возникновения исключения ресурс будет освободжен
  // в случае каких-то других ошибок мы можем просто выйти 
	// из функции, ресурс также будет освобожден
  guard.release (); 
  // При нормальном завершении ресурс освобождать не нужно
}
```

```cpp
namespace my {
    using std::function;

    class Non_copyable
    {
    public:
        Non_copyable()                               = default;

    public:
        Non_copyable& operator=( Non_copyable&& )    = default;
        Non_copyable( Non_copyable&& )               = default;
        
    private:
        Non_copyable& operator=(Non_copyable const&) = delete;
        Non_copyable (Non_copyable const&)           = delete;
    };

    class Scope_guard
        : public Non_copyable
    {

    public:
        friend void dismiss(Scope_guard& g) 
        { 
          g.cleanup_ = []{}; 
        }

        ~Scope_guard() 
        { 
          cleanup_(); 
        }

        template<typename Func>
        Scope_guard(Func const& cleanup)
            : cleanup_(cleanup)
        { }

        Scope_guard(Scope_guard&& other)
            : cleanup_(std::move(other.cleanup_ ))
        { 
          dismiss(other); 
        }
        
     private:
        function<void()>    cleanup_;
    };

}  // namespace my

#include <iostream>

void foo() {}

int main()
{    
    my::Scope_guard const final_action = []{ 
      std::wclog << "Finished! (Exit from main.)\n"; 
    };

    std::wcout << "The answer is probably " << 6*7 << ".\n";
}
```