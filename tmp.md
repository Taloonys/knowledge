# По ссылке
```cpp
#include <stdio.h>
#include <vector>
#include <iostream>


class F1 {
public:
    int* a = nullptr;
};


class F {
public:
    template <typename T>
    void foo1(std::vector<T>& v) { } 

private:    
    int a_ = 55;
};


template <>
void F::foo1<F1>(std::vector<F1>& v) { 
    v[0].a = &(this->a_);
}


int main()
{
    F f;
    F1 f1;

    std::vector<F1> v = {f1};
    f.foo1(v);
    std::cout << *(v[0].a);

    return 0;
}
```
# По значению
```cpp
#include <stdio.h>
#include <vector>
#include <iostream>


class F1 {
public:
    int* a = nullptr;
};


class F {
public:
    template <typename T>
    void foo(std::vector<T> v) { } 

private:    
    int a_ = 55;
};


class std::vector<F1> {
    int& val_;
    
public:
    vector(int& int) {
        val_ = int;   
    }
};


template <>
void F::foo<F1>(std::vector<F1> v) { 
    std::cout << "here";
}


int main()
{
    F f;
    F1 f1;

    std::vector<F1> v = {f1};
    f.foo(v);
    std::cout << *(v[0].a);

    return 0;
}
```