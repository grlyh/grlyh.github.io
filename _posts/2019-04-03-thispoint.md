---
layout:     post                    # 使用的布局（不需要改）
title:      this指针               # 标题 
subtitle:   #副标题
date:       2019-04-03              # 时间
author:     lyk                      # 作者
header-img: img/post-thispoint.jpg  #这篇文章标题背景图片
catalog: true                # 是否归档
tags:                     #标签
    - c++
    - OOP
---
## 什么是this
this是一个const指针，存的是**当前对象**的地址，指向**当前对象**，通过this指针可以访问类中的所有成员。

当前对象是指正在使用的对象，比如`a.print()`，`a`就是当前对象。

**关于this**
1. 每个对象都有this指针，通过this来访问自己的地址。
2. 每个成员函数都有一个指针形参（构造函数没有这个形参），名字固定，称为this指针，this是隐式的。
3. 编译器在编译时会自动对成员函数进行处理，将对象地址作实参传递给成员函数的第一个形参this指针。
4. this指针是编译器自己处理的形参，不能在成员函数的形参中添加this指针的参数定义。
5. this只能在成员函数中使用，全局函数，静态函数不能使用this。因为静态函数没有固定对象。
## this的使用

```cpp
#include <bits/stdc++.h>

using namespace std;
class A {
	private :
		int a;
	public :
		A(int x = 0) : a(x) {}
		void set(int x) {
			a = x;
		}
		void print() {printf("%d\n", a);} 
};

int main() {
	A a, b;
	int x;
	a.set(111);
	b.set(222);
	a.print();
	b.print();
	return 0;
}
```
输出：
```cpp
111
222
```
可以看出赋值的时候是分别给当前对象的成员赋的值。

就像上文中提到的3一样，拿`set()`函数来说，其实编译器在编译的时候是这样的
```cpp
void set(A *this, int x) {
	this->a = x;
}
```
### 何时调用
那什么时候要调用this指针呢？

**1. 在类的非静态成员函数中返回对象的本身时候，直接用`return *this`。**

**2. 传入函数的形参与成员变量名相同时**

例如
```cpp
#include <bits/stdc++.h>

using namespace std;
class A {
	private :
		int x;
	public :
		A() {x = 0;}
		void set(int x) {
			x = x;
		}
		void print() {
			printf("%d\n", x);
		}
};

int main() {
	A a, b;
	int x;
	a.set(111);
	b.set(222);
	a.print();
	b.print();
	return 0;
}
```
输出是
```cpp
0
0
```
这时因为我们的set()函数中，编译器会认为我们把成员`$x$`的值赋给了参数`$x$`；

如果我们改成这样，就没有问题了
```cpp
#include <bits/stdc++.h>

using namespace std;
class A {
	private :
		int x;
	public :
		A() {x = 0;}
		void set(int x) {
			this->x = x;
		}
		void print() {
			printf("%d\n", x);
		}
};

int main() {
	A a, b;
	int x;
	a.set(111);
	b.set(222);
	a.print();
	b.print();
	return 0;
}
```
这样输出的就是
```cpp
111
222
``

而且这段代码一目了然，左值是类成员x，右值是形参x。
