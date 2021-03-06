---
title: C++ 类和对象的定义
date: 2019-07-14 11:40:56
tags: [C++]
categories: C++基础
---

类是面向对象程序设计的核心，它实际上是一种新的数据类型，也是实现抽象类型的工具，因为类是通过抽象数据类型的方法来实现一种数据类型。类是对某一类对象的抽象，而对象是某一种类的一个实例。


# 1 类的定义
类的定义一般分为声明和实现部分。声明部分用来声明该类中的成员，包含数据成员的声明和成员函数的声明。成员函数是用来对数据成员进行操作的，又称“方法”。实现部分用来对成员函数进行定义。
概括的说，声明部分是用来告诉使用者“干什么”，而实现部分是告诉使用者“怎么干”。
<!-- more -->
## 1.1 定义
C++中定义类的一般格式：
```
class <类名>
{
    public：
        [<公有型数据和函数>]
    private:
        [<私有型数据和函数>]
    protected：
        [<保护型数据和函数>]
}；
<各个成员函数的实现>
```
其中，class是定义类的关键字，class后面是用户定义的类名。
注意，类体中最后一个花括号后面的分号"；"不能省略。

## 1.2 访问权限
类中的关键字public, private, protected声明了类中的成员与程序其他部分的（或类外)之间的关系，称为访问权限。
对 public 成员来说： 它们是公有的，能被外面的程序访问；
对 private 成员来说： 它们是私有的，不能被外面的程序访问，数据成员只能由类中的函数所使用，成员函数只允许在类中调用；
对 protected 成员来说： 它们是受保护的，具有半公开性，可以在类中或是子类中访问。

## 1.3 各成员函数的实现
如果一个函数成员在类中定义，则其实现部分将不需要。当类的成员函数在类外定义时，必须由作用域运算符"::"来通知编译系统该函数所属的类。如:
```
class CMeter
{
    public:
        double m_nPercent;      //声明一个公有数据成员
        void StepIt();          //声明一个公有成员函数
        void SetPos(int nPos);  //声明一个公有成员函数
        int GetPos()
        {
            return m_nPos;
        }   //定义一个公有成员函数并定义
    private:
        int m_nPos;             //声明一个私有数据成员
}；
void CMeter::StepIt()
{
    m_nPos++;
}
void CMeter::SetPos(int nPos)
{
    m_nPos = nPos;
}
```

## 1.4 注意
1）类中的数据成员的类型可以是任意的，包含整形、浮点型、字符型、数组、指针和引用，也可以是另一个类的对象，但不允许对所定义的数据成员进行初始化，也不能指定除了static外的任何存储类型（register,auto,extern)。
2）在public, private或protected后面定义的所有成员都是公有的、私有的或保护的，直到下一个public, private或protected出现为止。若没有指定，则默认是private的。
3）关键字public, private, protected可以在类中出现多次，且前后顺序没有关系。但建议一般把public放前面。
4）进行类设计时，通常将数据声明为私有，而将大多数成员函数声明为公有的。这样类以外的代码就无法直接访问类的私有数据，从而实现了数据的封装。而公有成员函数可以为内部的私有数据成员提供外部接口。
5）尽量类单独放一个文件中，或将类的声明放在.h头文件中，其实现放在同名的.cpp文件中。



# 2 对象的定义
作为一种复杂的数据构造类型，类声明后就可以定义该类的对象。
## 2.1 定义方式
与结构类型一样，它有三种定义方式
1）声明之后定义；
2）声明之时定义；
3）一次性定义；
但是为了提高程序的可读性，在程序中应尽量在对象的声明之后定义方式，一般格式：
```
<类名><对象名表>；
```
其中，类名是已经定义过的类的标识符，对象名可以有一个或是多个，多个是要用逗号分格。被类定义的对象可以是一个普通对象，也可以是一个数组或指针对象。例如：
    CMeter myMeter, \*pMeter, Meters[2];
这时，myMeter是一个普通对象，\*pMeter是一个指针对象，Meters[2]是一个数组对象。

## 2.2 对象的成员
一个对象的成员就是该对象的类所定义的数据成员和成员函数。访问对象的成员和函数成员：
```
<对象名>.<成员变量>
<对象名>.<成员函数>()
```
例如： myMeter.m_nPercent, myMeter.SetIt(), Meters[1].SetPos(2).
需要说明的是，一个类的对象只能访问该类的公有型成员，而对于私有型成员则不能访问。
若对象是一个指针，则对象的成员访问形式如下：
```
<对象指针名>-><成员变量>
<对象指针名>-><成员函数>(参数表)
```
其中，下面的两种表示是等价的：
```
<对象指针名>-><成员变量>
(*<对象指针名>).<成员变量>
```
另外，对于引用类型对象，其成员访问形式与一般对象的成员访问形式相同。如：
```
CMeter &other = one;
cout << other.GetPos() << endl;
```


# 3 类的作用域和成员访问权限
类的作用域是指在类的定义中由一对花括号所括起来的部分。从类的定义中可知，类中可以定义变量，定义函数。
类的作用域与文件作用域是不一样的，在类作用域中定义的变量不能使用auto、register和extern等修饰符，只能是static修饰符，而定义的函数也不能用extern修饰符。另外，类中的的静态成员变量和静态成员函数还具有类外的连接属性。

## 3.1 类名的作用域
1）如果在类声明时指定了类名，则类的作用域是从类名指定的位置开始一直到文件结尾。
2）若类的声明是放在头文件中，则类名在文件中的作用范围是从包含预处理指令为止处开始一直到文件结尾。
3）如果在类的声明前就已经使用了该类名定义对象，则必须在使用前进行声明： class <类名>; 如：
```
class COne;     //将COne类提前声明
class COne;     //可以声明多次

class CTwo
{
    private:
        COne a;     //数据成员a是已定义的COne类对象
};
class COne
{
    //....
};
```

## 3.2 类中成员的可见性
1）在类中使用成员时，成员声明的前后不会影响该成员在类中的使用，这是类作用域的特殊性。例如：
```
class A
{
    void f1()
    {
        f2();   //调用类中的成员函数f2()
        cout << a << endl;      //使用类中的成员变量a
    }
    void f2();
    int a;
};
```
2）由于类的成员函数可以在类外定义，因而此时由"类名::"指定开始一直到函数体最后一个花括号为止的范围也是该类的作用域的范围。
```
class A
{
    void f1();
    //...
};
void A::f1()
{
    //...
}
```
从A::f1()开始一直到f1()函数体最后一个花括号为止的范围都是属于类A的作用域。
3）在同一个类的作用域中，不管成员具有怎么样的访问权限，都可在类作用域中使用，而在类作用域外不可使用。例如：
```
class A
{
    public:
        int a;
        //...
};
a = 10; //错误，不能在类的作用域外直接使用类中的成员。
```
## 3.3 类外对象成员的可见性
对于访问权限public、private和protected来说，只有在子类中或者用对象来方位成员时，它们才会起作用。在用类外对象来访问成员时，只能访问public成员，而对private和protected均不能访问。
