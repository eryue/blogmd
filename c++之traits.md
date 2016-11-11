---
title: traits
date: 2016-09-12
categories: technology
tags: c/c++
---


## 萃取技术
侯捷的stl源码剖析中指出，traits编程技法，利用“内嵌型别”的编程技法和编译器的template参数推导功能，萃取出型别。

参照SGI STL中iterator_traits就可以很好的理解：

```cpp
template <class Iterator>
struct iterator_traits {
  typedef typename Iterator::iterator_category iterator_category;
  typedef typename Iterator::value_type        value_type;
  typedef typename Iterator::difference_type   difference_type;
  typedef typename Iterator::pointer           pointer;
  typedef typename Iterator::reference         reference;
};

template <class T>
struct iterator_traits<T*> {
  typedef random_access_iterator_tag iterator_category;
  typedef T                          value_type;
  typedef ptrdiff_t                  difference_type;
  typedef T*                         pointer;
  typedef T&                         reference;
};

template <class T>
struct iterator_traits<const T*> {
  typedef random_access_iterator_tag iterator_category;
  typedef T                          value_type;
  typedef ptrdiff_t                  difference_type;
  typedef const T*                   pointer;
  typedef const T&                   reference;
};

```

后面两个分别是第一个的原生指针和原生const指针设计的偏特化(partial specialization)版本，因为原生指针不是class type，无法定义内嵌型别，
所以无法使用第一个。添加两个偏特化版本，当编译器编译时，如果是原生指针，则会使用相应的偏特化版本。

iterator_traits是STL制定的规范，用于萃取迭代器的特性。SGI新增有__type_traits，用于萃取型别(type)的特性，这里的型别特性是指：

* has_trivial_default_constructor;
* has_trivial_copy_constructor
* has_trivial_assignment_operator
* has_trivial_destructor
* is_POD_type

```cpp

template <class type>
struct __type_traits { 
   typedef __false_type    has_trivial_default_constructor;
   typedef __false_type    has_trivial_copy_constructor;
   typedef __false_type    has_trivial_assignment_operator;
   typedef __false_type    has_trivial_destructor;
   typedef __false_type    is_POD_type;
};

```

其他c++基本类型都有相应的特化版本，并且所有成员的值都是__true_type，表示这些型别都可以用最快速的方式(例如memcpy)来进行拷贝(copy)或
赋值(assign)操作。

```cpp
__STL_TEMPLATE_NULL struct __type_traits<char> {
   typedef __true_type    has_trivial_default_constructor;
   typedef __true_type    has_trivial_copy_constructor;
   typedef __true_type    has_trivial_assignment_operator;
   typedef __true_type    has_trivial_destructor;
   typedef __true_type    is_POD_type;
};
```


