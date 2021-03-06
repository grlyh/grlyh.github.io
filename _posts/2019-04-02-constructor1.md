---
layout:     post                    # 使用的布局（不需要改）
title:      构造函数(一) 构造函数及重载               # 标题 
subtitle:   #副标题
date:       2019-04-02              # 时间
author:     lyk                      # 作者
header-img: img/constructor1.png    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - c++
    - OOP
---
## 什么是构造函数

构造函数是初始化类对象的类成员函数。在c++中，当对象被创建时，自动调用构造函数，构造函数是该类的一个特殊成员的函数

**构造函数和一般的成员函数有什么不同**
- 构造函数的名称和类的名称完全相同
- 构造函数没有返回类型(包括void)
- 创建对象的时候自动调用构造函数
- 如果我们不写构造函数，编译器会自动创建一个没有参数的构造函数，函数主体内什么也没有（默认构造函数）
- 函数体中不能有return语句

## 构造函数
### 默认构造函数

A类中编译器自动生成的构造函数如下
```cpp
A(){}
```

没有参数的构造函数，不论是编译器自动生成的还是自己写的，都称为默认构造函数。

如果写了构造函数，编译器就不会自动生成默认构造函数。

```cpp
#include <bits/stdc++.h>

using namespace std;
class A {
	public :
		int a, b;
		A() {
			a = 10, b = 20;
		}
};
int main() {
	A tmp;
	printf("%d %d\n", tmp.a, tmp.b);
	A *p = new A;
	printf("%d %d\n", p->a, p->b); 
	return 0;
}
```
输出
```cpp
10 20
10 20
```

### 参数化构造函数
可以将参数传递给构造函数，只需要向任意函数添加参数一样向构造函数里添加参数，用参数进行初始化。

```cpp
#include <bits/stdc++.h>

using namespace std;
class A {
	public :
		int a, b;
		A(int x, int y) {
			a = x, b = y;
		}
};
int main() {
	A tmp(10, 20);
	printf("%d %d\n", tmp.a, tmp.b);
	A *p = new A(30, 40);
	printf("%d %d\n", p->a, p->b); 
	return 0;
}
```

输出：
```cpp
10 20
30 40
```

如果你的构造函数里有参数但你这么创建对象
```cpp
A tmp;          //报错

A *p = new A;   //报错

A tmp(10);      //可以，相当于tmp(10, 0);
```
报错是因为上面两条语句均没有涉及到构造函数的参数，因此编译器会认为这两个对象应该用默认构造函数初始化，但 A 类已经有了一个构造函数，编译器不会自动生成默认构造函数，于是 A 类不存在默认构造函数，所以上面两条语句就无法完成对象的初始化，导致编译报错。

<font color = "red">在对象生成的时候，一定会自动调用一个构造函数对其初始化，对象一旦生成，就再也不会再这个对象上调用构造函数。</font>

<font color = "red">构造函数并不会为对象分配空间，再对象创建时内存空间已经被分配好，构造函数只负责初始化这段内存空间。</font>

构造函数不但能隐性的调用，也可以显性的调用。
```cpp
#include <bits/stdc++.h>

using namespace std;
class A {
	public :
		int a, b;
		A(int x, int y) {a = x, b = y;}
};
int main() {
	A tmp = A(10, 20);
	printf("%d %d\n", tmp.a, tmp.b);
	return 0;
}
```
~~看起来这没有什么用~~

## 构造函数的重载
构造函数是可以重载的，即写多个构造函数，它们具有不同的参数表和相同的名称，如果没有参数信息，编译器就认为调用默认构造函数。

**特点**
- 重载构造函数具有不同的参数表和相同的名称
- 根据传参个数决定调用哪个构造函数
- 创建对象时要传参数让编译器知道调用哪个构造函数



```cpp
#include <bits/stdc++.h>

using namespace std;
class A {
	public :
		int a, b;
		A(int x, int y) {
			a = x, b = y;
		}
		A(int x) {
			a = x, b = 1;
		}
		A() {
			a = b = 0;
		}
		void mul() {
			printf("%d\n", a * b);
		}
};

int main() {
	A a(10, 20);
	A b(20);
	A c = 10;      //c=10可以视作c(10)
	A d;		
	a.mul(), b.mul(), c.mul(), d.mul();
	return 0;
}
```


输出
```cpp
200
20
10
0
```
