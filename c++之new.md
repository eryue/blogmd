---
title: new
date: 2016-08-31
categories: technology
tags: c/c++
---




## new(delete)、operator new(delete)和 placement new
new是一个运算符(new operator)，so，不能重载。new做两件事：      

* 调用operator new分配内存
* 调用相关对象的构造函数

operator new是一个函数，所以，可以重载，只分配内存。

重载全局operator new

```cpp
void* operator new(size_t size)
{
    return malloc(size);
}
```
重载局部operator new

```cpp
void* Test::operator new(size_t size)
{
    return ::operator new(size);    //调用全局operator new
}
```

placement new只是operator new重载的一个版本。不能够被自定义的版本代替。它并不分配内存，只是返回指向已经分配好的某段内存的一个指针。
placement new允许在一个已经分配好的内存中（栈或者堆中）构造一个新的对象

```cpp
char buf[sizeof(Test)];     //分配内存
Test *t = new (buf) Test;   //调用placement new构造对象
t->~Test();                 //用完后需要调用析构函数
delete buf;                 //释放内存
```

在SGI中construct函数就是用的placement new(调用T1的copy construct)

```cpp
template <class T1, class T2>
inline void construct(T1* p, const T2& value) {
  new (p) T1(value);
}
```
