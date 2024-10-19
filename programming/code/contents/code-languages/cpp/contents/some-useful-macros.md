# Sources

- [https://github.com/lheric/libgitlevtbus/blob/master/src/gitldef.h](https://github.com/lheric/libgitlevtbus/blob/master/src/gitldef.h)

---

# Getter-setter generators

## Q_Property (it’s a qt feature)

```cpp
#define ADD_QPROP_PR(type, name) \
    public: \
        Q_PROPERTY(type name MEMBER m_##name) \
    private: \
        type m_##name;
```

```cpp
#define ADD_QPROP_PR_INIT(type, name, init) \
    public: \
        Q_PROPERTY(type name MEMBER m_##name) \
    private: \
        type m_##name = init;
```

```cpp
#define ADD_QPROP_RO(type, name, getter) \
    public: \
        Q_PROPERTY(type name MEMBER m_##name READ getter) \
        type& getter() { return m_##name; } \
        type const & getter() const{ return m_##name; } \
    private: \
        type m_##name;
```

```cpp
#define ADD_QPROP_RO_INIT(type, name, getter, init) \
    public: \
        Q_PROPERTY(type name MEMBER m_##name READ getter) \
        type& getter() { return m_##name; } \
        type const & getter() const{ return m_##name; } \
    private: \
        type m_##name = init;
```

```cpp
#define ADD_QPROP_RW(type, name, getter, setter) \
    public: \
        Q_PROPERTY(type name MEMBER m_##name READ getter WRITE setter NOTIFY name##Changed) \
        type& getter() { return m_##name; } \
        type const & getter() const{ return m_##name; } \
        void setter(type name) { m_##name = name; emit name##Changed(name);} \
        Q_SIGNAL void name##Changed(type& name); \
    private: \
        type m_##name;
```

```cpp
#define ADD_QPROP_RW_INIT(type, name, getter, setter, init) \
    public: \
        Q_PROPERTY(type name MEMBER m_##name READ getter WRITE setter NOTIFY name##Changed) \
        type& getter() { return m_##name; } \
        type const & getter() const{ return m_##name; } \
        void setter(type name) { m_##name = name; emit name##Changed(name);} \
        Q_SIGNAL void name##Changed(type& name); \
    private: \
        type m_##name = init;
```

## Simple class member

```cpp
#define ADD_FIELD_INIT(type, name, getter, setter, init) \
    public: \
        type& getter() { return m_##name; } \
        type const & getter() const{ return m_##name; } \
        void setter(type name) { m_##name = name; } \
    private: \
        type m_##name = init;
```

```cpp
#define ADD_FIELD(type, name, getter, setter) ADD_CLASS_FIELD(type, name, getter, setter)
#define ADD_CLASS_FIELD(type, name, getter, setter) \
    public: \
        type& getter() { return m_##name; } \
        type const & getter() const{ return m_##name; } \
        void setter(type name) { m_##name = name; } \
    private: \
        type m_##name;
```

```cpp
#define ADD_FIELD_NOSETTER_INIT(type, name, getter, init) \
    public: \
        type& getter() { return m_##name; } \
        type const & getter() const{ return m_##name; } \
    private: \
        type m_##name = init;
```

```cpp
#define ADD_FIELD_NOSETTER(type, name, getter) ADD_CLASS_FIELD_NOSETTER(type, name, getter)
#define ADD_CLASS_FIELD_NOSETTER(type, name, getter) \
    public: \
        type& getter() { return m_##name; } \
        type const & getter() const{ return m_##name; } \
    private: \
        type m_##name;
```

```cpp
#define ADD_FIELD_PRIVATE_INIT(type, name, init ) \
    private: \
        type m_##name = init;
```

```cpp
#define ADD_FIELD_PRIVATE(type, name ) ADD_CLASS_FIELD_PRIVATE(type, name )
#define ADD_CLASS_FIELD_PRIVATE(type, name ) \
    private: \
        type m_##name;
```

---

# Singleton pattern

> **Thread safe**
> 

```cpp

#define SINGLETON_PATTERN_DECLARE(classname)\
    public: \
        static classname* getInstance() { QMutexLocker cLocker(&m_cGetInstanceMutex); if(m_instance==NULL) m_instance=new classname(); return m_instance; } \
    private: \
        static classname* m_instance; \
        static QMutex m_cGetInstanceMutex;

#define SINGLETON_PATTERN_IMPLIMENT(classname)\
    classname* classname::m_instance = NULL; \
    QMutex classname::m_cGetInstanceMutex;

/*! PROTOTYPE PATTERN */
#define CLONABLE(classname)\
    public:\
        virtual classname* clone() const { return new classname(*this); }
```

---

# Make a callback functor

```cpp
#define MAKE_CALLBACK(memberFunc) \
    std::bind( &memberFunc, this, std::placeholders::_1 )
```

```cpp
#define MAKE_CALLBACK_OBJ(object, memberFunc) \
    std::bind( &memberFunc, &(object), std::placeholders::_1 )
```

---

# Scope Guard (C++11 Must have)

```cpp
template <typename F>
struct ScopeExit {
    ScopeExit(F f) : f(f) {}
    ~ScopeExit() { f(); }
    F f;
};
template <typename F>
ScopeExit<F> MakeScopeExit(F f) {
    return ScopeExit<F>(f);
}
#define SCOPE_EXIT(code) \
    auto scope_exit_##__LINE__ = MakeScopeExit([=](){code;})
```

---