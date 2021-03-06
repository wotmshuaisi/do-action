---
title: C/C++ - 入门01
p: cpp/cpp-intro1
date: 2018-08-16 16:01:41
tags:
  - c/c++
---

### 1. 基础用法

#### 1.1 比较两个字符串或者string对象是否相等

在我们遇到需要比较两个字符串是否相等的时候，如果比较的是 `char *`  字符串，使用

```cpp
int strcmp(const char* s1,const char* s2)

当s1<s2时，返回为负数
当s1==s2时，返回值= 0
当s1>s2时，返回正数
```
> 需要注意的是在比较两个字符串的时候，不能使用 `==`，`==` 比较的是两个字符串的地址是否相等。如果比较的是两个字符，`==` 是可以的。

```cpp
std::string str = "123456";
// 相等为1，不等为0
printf("%d", str == "123456");
// 相等为0，不等非0
printf("%d", str.compare("123456"));
这样是可以的，string 对象的比较可以使用 `==`, 相等为1，不等为0
str.compare("");  相等为0，不等非0
```

#### 1.2 C++中string、char *、char[]的转换

```cpp
////// string to char *
string str = "hello";
// - Way 1
const char *p = str.data();
char *pp = const_cast<char *>(str.data());

// - Way 2
const char *w2 = str.c_str();

// - Way 3
char w3[50];
str.copy(w3, 5, 0);
*(w3 + 5) = '\0';

////// char * to string
const char *char2str = "000000";
std::string ss = char2str;

////// string to char[]
std::string a2s = "dagah";
char aa[8];
int i;
for( i=0; i<a2s.length(); i++)
aa[i] = a2s[i];
aa[i] = '\0';
```

#### 1.3 char * 和 char[] 的区别

- `char *a = "abcd"` 此时"abcd"存放在常量区。通过指针只可以访问字符串常量，而不可以改变它; 而 `char a[20] = "abcd"` 此时 "abcd"存放在栈。可以通过指针去访问和修改数组内容。

### 2. 常见错误

#### 2.1 #error This file requires compiler and library support for the ISO C++ 2011 错误解决办法

同样的代码，我在 Mac 上运行和在 Linux 上运行，就有不同，在 Linux 上运行就会报这样的错误。解决办法是：在 CMakeLists.txt 中添加如下配置

```cpp
include(CheckCXXCompilerFlag)
CHECK_CXX_COMPILER_FLAG("-std=c++11" COMPILER_SUPPORTS_CXX11)
CHECK_CXX_COMPILER_FLAG("-std=c++0x" COMPILER_SUPPORTS_CXX0X)
if(COMPILER_SUPPORTS_CXX11)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11") 
elseif(COMPILER_SUPPORTS_CXX0X)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
else()
    message(STATUS "The compiler ${CMAKE_CXX_COMPILER} has no C++11 support. Please use a different C++ compiler.")
endif()
```
原因分析：跟 C++ 的编译器支持有关系，在 C++ 中的一些特性，只有在特定的版本才会支持，比如说 C++ 11 标准等。C++ 常用的编译器有GCC等，不同版本的 GCC 对 C++ 的标准支持是不一样的。 [C++ 11 标准的特性可以看这里](https://en.wikipedia.org/wiki/C%2B%2B11) ，这里 [compiler_support](https://en.cppreference.com/w/cpp/compiler_support) 可以找到编译器版本对C++的支持。
