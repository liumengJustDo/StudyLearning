cC++学习笔记

# 一：入门语法

就目前来说基础语法大部分都和c语言类似 ， 然后接下来就是介绍c++与c语言不相同的地方

1. C++中的字符串(string)

   ```cpp
   string 变量;
   string str1;
   //赋值
   string str2 = "ShangHai";
   string str3 = str2;
   str3[3] = '2';//对某个字符赋值
   ```

它里面的字符串是可以直接定义的，然后我们可以将字符串变量直接赋值；这一点与我们学习的 java 语言类似！！



接下来就是c++语言的核心内容



# 1 面向对象

> 类也是一种数据类型。

类的声明：

class 类名
{
	public:
		公有数据成员;
		公有成员函数;
	private:
		私有数据成员;
		私有成员函数;
	protected:
		保护数据成员;
		保护成员函数;
};

```tex
成员函数的定义：类内，类外，类外内联函数

//类外
返回类型 类名:成员函数名(参数列表)
{
	函数体;
}
//内联函数：类外
inline 返回类型 类名:成员函数名(参数列表)
{
	函数体;
}
```



内联函数的代码会直接嵌入到主调函数中，可以节省调用时间，如果成员函数在类内定义，自动为内联函数。

### 1.2 类成员的访问权限以及类的封装

和Java、C#不同的是，C++中public、private、protected只能修饰类的成员，不能修饰类，C++中的类没有共有私有之分
类内部没有访问权限的限制，都可以互相访问。
在C++中用class定义的类中，其成员的默认存取权限是private。

类外	派生类	类内

```c
public	Y	Y	Y
protected	N	Y	Y
private	N	N	Y
```

### 1.3 对象

### 

```c
//1.声明类同时定义对象
class 类名
{
	类体;
}对象名列表;
//2.先声明类，再定义对象
类名 对象名(参数列表);//参数列表为空时，()可以不写
//3. 不出现类名，直接定义对象
class 
{
	类体;
}对象名列表;
//4.在堆上创建对象
Person p(123, "yar");//在栈上创建对象
Person *pp = new Person(234,"yar");//在堆上创建对象
```

注：不可以在定义类的同时对其数据成员进行初始化，因为类不是一个实体，不合法但是能编译运行
对象成员的引用：对象名.数据成员名 或者 对象名.成员函数名(参数列表)

```c++
#include <iostream>
#include <string.h>
using namespace  std;

class Animal   //class 首字母 小写 
{
	public:
		string Animalname;
		int Animalage;
		string Animalfood;
	void set(string name,int age,string food);
	string get(void);
};  //这里记得有个分号 

void Animal::set(string name , int age , string food)
{
	Animalname=name;
	Animalage=age;
	Animalfood=food;
}

string Animal::get(void)
{
	return Animalname+"爱吃"+Animalfood;  //c++支持拼接字符串 
}

int main()
{
	Animal an;
	an.set("xixi",3,"fish");
	 string message = an.get(); 
	 cout<<message;
	 return 0; 
}
```



就python和C++的学习过程中我发现 ， 其中的语法相似度很高

### 1.4 构造函数

是一种特殊的成员函数，主要功能是为对象分配存储空间，以及为类成员变量赋初值构造函数名必须与类名相同
没有任何返回值和返回类型
创建对象自动调用，不需要用户来调用，且只掉用一次
类没有定义任何构造函数，编译系统会自动为这个类生成一个默认的无参构造函数
构造函数定义

```c
//1.类中定义 2.类中声明，类外定义
[类名::]构造函数名(参数列表)
{
	函数体
}
```



创建对象

类名 对象名(参数列表);//参数列表为空时，()可以不写

```c
带默认参数的构造函数

class Person
{
public:
	Person(int = 0,string = "张三");
	void show();
private:
	int age;
	string name;
};
Person::Person(int a, string s)
{
	cout<<a<<" "<<s<<endl;
	age = a;
	name = s;
}
void Person::show()
{
	cout << "age="<<age << endl;
	cout << "name=" <<name << endl;
}
int main()
{
     Person p; //0 张三
	Person p2(12);//12 张三
	Person p3(123, "yar");//123 yar
	return 0;
}
```



```c
带参数初始化表的构造函数

类名::构造函数名(参数列表):参数初始化表
{
	函数体;
}
参数初始化列表的一般形式：
参数名1(初值1),参数名2(初值2),...,参数名n(初值n)
class Person
{
public:
	Person(int = 0,string = "张三");
	void show();
private:
	int age;
	string name;
};
Person::Person(int a, string s):age(a),name(s) //成员初始化 感觉还不是很好 了解即可
{
	cout << a << " " << s << endl;
}

```



```c
构造函数重载：构造函数名字相同，参数个数和参数类型不一样。

class Person
{
public:
	Person();
	Person(int = 0,string = "张三");
	Person(double,string);
	void show();
private:
	int age;
	double height;
	string name;
};
...
```



```c
拷贝构造函数

类名::类名(类名&对象名)
{
	函数体;
}
class Person
{
public:
	Person(Person &p);//声明拷贝构造函数
	Person(int = 0,string = "张三");
	void show();
private:
	int age;
	string name;
};
Person::Person(Person &p)//定义拷贝构造函数
{
	cout << "拷贝构造函数" << endl;
	age = 0;
	name = "ABC";
}
Person::Person(int a, string s):age(a),name(s)
{
	cout << a << " " << s << endl;
}
int main()
{
	Person p(123, "yar");
	Person p2(p);
	p2.show();
	return 0;
}
//输出
123 yar
拷贝构造函数
age=0
name=ABC
```



### 1.5 析构函数

是一种特殊的成员函数，当对象的生命周期结束时，用来释放分配给对象的内存空间爱你，并做一些清理的工作。

析构函数名与类名必须相同。
析构函数名前面必须加一个波浪号~。
没有参数，没有返回值，不能重载。
一个类中只能有一个析构函数。
没有定义析构函数，编译系统会自动为和这个类生成一个默认的析构函数。
析构函数的定义：

    //1.类中定义 2.类中声明，类外定义
    [类名::]~析构函数名()
    {
    	函数体;
    }



### 1.6 对象指针

对象指针的声明和使用

```c
类名 *对象指针名;
对象指针 = &对象名;
//访问对象成员
对象指针->数据成员名
对象指针->成员函数名(参数列表)
Person p(123, "yar");
Person* pp = &p;
Person* pp2 = new Person(234,"yar")
pp->show();
```

指向对象成员的指针



```c
数据成员类型 *指针变量名 = &对象名.数据成员名;
函数类型 (类名::*指针变量名)(参数列表);
指针变量名=&类名::成员函数名;
(对象名.*指针变量名)(参数列表);
Person p(123, "yar");
void(Person::*pfun)();
pfun = &Person::show;
(p.*pfun)();

```

this指针
每个成员函数都有一个特殊的指针this，它始终指向当前被调用的成员函数操作的对象



```c
class Person
{
public:
	Person(int = 0,string = "张三");
	void show();
private:
	int age;
	string name;
};
Person::Person(int a, string s):age(a),name(s)
{
	cout << a << " " << s << endl;
}
void Person::show()
{
	cout << "age="<<this->age << endl;
	cout << "name=" <<this->name << endl;
}
```

### 1.7 静态成员



以关键字static开头的成员为静态成员，多个类共享。

    static 成员变量属于类，不属于某个具体的对象
    静态成员函数只能访问类中静态数据成员
    静态数据成员



```c
//类内声明，类外定义
class xxx
{
	static 数据类型 静态数据成员名;
}
数据类型 类名::静态数据成员名=初值
//访问
类名::静态数据成员名;
对象名.静态数据成员名;
对象指针名->静态数据成员名;
```



```c++
静态成员函数

//类内声明，类外定义
class xxx
{
	static 返回值类型 静态成员函数名(参数列表);
}
返回值类型 类名::静态成员函数名(参数列表)
{
	函数体;
}
//访问
类名::静态成员函数名(参数列表);
对象名.静态成员函数名(参数列表);
对象指针名->静态成员函数名(参数列表);
```

### 1.8 友元

借助友元(friend)，可以使得其他类中得成员函数以及全局范围内得函数访问当前类得private成员。
友元函数

    友元函数不是类的成员函数，所以没有this指针，必须通过参数传递对象。
    友元函数中不能直接引用对象成员的名字，只能通过形参传递进来的对象或对象指针来引用该对象的成员。



```c
//1.将非成员函数声明为友元函数
class Person
{
public:
	Person(int = 0,string = "张三");
	friend void show(Person *pper);//将show声明为友元函数
private:
	int age;
	string name;
};
Person::Person(int a, string s):age(a),name(s)
{
	cout << a << " " << s << endl;
}
void show(Person *pper)
{
	cout << "age="<< pper->age << endl;
	cout << "name=" << pper->name << endl;
}
int main()
{;
	Person *pp = new Person(234,"yar");
	show(pp);
	system("pause");
	return 0;
}
//2.将其他类的成员函数声明为友元函数
//person中的成员函数可以访问MobilePhone中的私有成员变量
class MobilePhone;//提前声明
//声明Person类
class Person
{
public:
	Person(int = 0,string = "张三");
	void show(MobilePhone *mp);
private:
	int age;
	string name;
};
//声明MobilePhone类
class MobilePhone
{
public:
	MobilePhone();
	friend void Person::show(MobilePhone *mp);
private:
	int year;
	int memory;
	string name;
};
MobilePhone::MobilePhone()
{
	year = 1;
	memory = 4;
	name = "iphone 6s";
}
Person::Person(int a, string s):age(a),name(s)
{
	cout << a << " " << s << endl;
}
void Person::show(MobilePhone *mp)
{
	cout << mp->year << "年  " << mp->memory << "G " << mp->name << endl;
}
int main()
{
	Person *pp = new Person(234,"yar");
	MobilePhone *mp = new MobilePhone;
	pp->show(mp);
	system("pause");
	return 0;
}
```

友元类
当一个类为另一个类的友元时，称这个类为友元类。 友元类的所有成员函数都是另一个类中的友元成员。
语法形式：friend [class] 友元类名



类之间的友元关系不能传递
类之间的友元关系是单向的
友元关系不能被继承

```c++
class HardDisk
{
public:
	HardDisk();
	friend class Computer;
private:
	int capacity;
	int speed;
	string brand;
};
HardDisk::HardDisk():capacity(128),speed(0),brand("三星"){
}
class Computer
{
public:
	Computer(HardDisk hd);
	void start();
private:
	string userName;
	string name;
	int ram;
	string cpu;
	int osType;
	HardDisk hardDisk;
	
};
Computer::Computer(HardDisk hd):userName("yar"),name("YAR-PC"),ram(16),cpu("i7-4710"),osType(64)
{
	cout << "正在创建computer..." << endl;
	this->hardDisk = hd;
	this->hardDisk.speed = 5400;
	cout << "硬盘转动...speed = " << this->hardDisk.speed << "转/分钟" << endl;
	
}
void Computer::start()
{cout << hardDisk.brand << " " << hardDisk.capacity << "G" << hardDisk.speed << "转/分钟" << endl;
cout << "笔记本开始运行..." << endl;
}
int main()
{
	HardDisk hd;
	Computer cp(hd);
	cp.start();
	system("pause");
	return 0;
}
```


​	

### 1.9 类(class)与结构体(struct)的区别

引入C语言的结构体，是为了保证和c程序的兼容性。
c语言中的结构体不允许定义函数成员，且没有访问控制权限的属性。
c++为结构体引入了成员函数，访问控制权限，继承，多态等面向对象特性。
c语言中，空结构体的大小为0，而C++中空结构体大小为1。
class中成员默认是private,struct中的成员默认是public。
class继承默认是private继承，而struct继承默认是public继承。
class可以使用模版，而struct不能。
举个例子：

//结构体默认权限为public

```c
struct person
{
	void show();
	string name;
	int age;
};
int main()
{
	person p;
	p.name = "heiren";
	p.age = 666;
	p.show();
	cout <<"name="<< p.name <<" age="<< p.age << endl;
	system("pause");
	return 0;
}
```

### 1.10 练习题目

```c
#include <iostream>
using namespace std;

class Student
{
	public:
		void display()
		{
		cout<<"age:"<<age<<endl;
		cout<<"name"<<name<<endl;
		}
	public:
	 string name;
	 int age;
	 int sex;
				   
 };
 
class Student1:public Student
{
	public:
		void display()
		{
			cout<<"address"<<address<<endl;
		}
	     void get(string sname, int sage,string saddress);
	
	private:
	 string address;
	

};
void Student1::get(string sname, int sage,string saddress)
{
	name=sname;
	age=sage;
	address=saddress;
}

int main()
{
	Student1 s;
	s.get("liumeng",3,"wuhan");
	cout<<s.name<<endl;
    s.display();  //这个display 和我们之前的引用感觉还是很相似的  
    return 0;
}
```

 注意事项

1. class类结束后我们需要将后面要加个； ，  这个尤其要注意到

  2.对于类的方法 我们首先要在public中要定义 然后要以类的名义来写 void Student::display() 这里后面可以不用加分号

  3.然后相比较与友元函数 主要是引用对象 的私有属性

# 2 继承和派生

### 2.1 继承和派生概述

继承就是再一个已有类的基础上建立一个新类，已有的类称基类或父类，新建立的类称为派生类和子类；派生和继承是一个概念，角度不同而已，继承是儿子继承父亲的产业，派生是父亲把产业传承给儿子。

- 一个基类可以派生出多个派生类，一个派生类可以继承多个基类
   派生类的声明：

```c
//继承方式为可选项，默认为private，还有public,protected
class 派生类名：[继承方式]基类名
{
	派生类新增加的成员声明;
};
```

继承方式：

- public-基类的public成员和protected成员的访问属性保持不变，私有成员不可见。
- private-基类的public成员和protected成员成为private成员,只能被派生类的成员函数直接访问，私有成员不可见。
- protected-基类的public成员和protected成员成为protected成员,只能被派生类的成员函数直接访问，私有成员不可见。
- 利用using关键字可以改变基类成员再派生类中的访问权限；using只能修改基类中public和protected成员的访问权限。

```c++
class Base
{
public:
	void show();
protected:
	int aa;
	double dd;
};
void Base::show(){
}
class Person:public Base
{
public:
	using Base::aa;//将基类的protected成员变成public
	using Base::dd;//将基类的protected成员变成public
private:
	using Base::show;//将基类的public成员变成private
	string name;
};
int main()
{
	Person *p = new Person();
	p->aa = 12;
	p->dd = 12.3;
	p->show();//出错
	delete p;
	return 0;
}

```
### 2.2 多继承

一个派生类同时继承多个基类的行为。

```c++
class 派生类名:继承方式1 基类1,继承方式2 基类2
{
	派生类主体;
};
```



### 2.3 虚基类

c++引入虚基类使得派生类再继承间接共同基类时只保留一份同名成员。

- 虚继承的目的是让某个类做出声明，承诺愿意共享它的基类。其中，这个被共享的基类就称为虚基类（Virtual Base Class）。
- 派生类的 同名成员 比虚基类的 优先级更高
  虚基类的声明：class 派生类名:virtual 继承方式 基类名

```c++
class  A//虚基类
{
protected:
	int a;
};
class B: virtual public A
{
protected:
	int b;
};
class C:virtual public A
{
protected:
	int c;
};
class D:public B,public C
{
protected:
	int d;
	void show()
	{
		b = 123;
		c = 23;
		a = 1;
	}
};

```


- 如果 B 或 C 其中的一个类定义了a，也不会有二义性，派生类的a 比虚基类的a 优先级更高。
- 如果 B 和 C 中都定义了 a，那么D直接访问a 将产生二义性问题。

# 3.多态和虚函数

## 15.1 向上转型

数据类型的转换，编译器会将小数部分直接丢掉（不是四舍五入）



```c++
int a = 66.9;
printf("%d\n", a);//66
float b = 66;
printf("%f\n", b);//66.000000


```

- 只能将将派生类赋值给基类（C++中称为向上转型）： 派生类对象赋值给基类对象、将派生类指针赋值给基类指针、将派生类引用赋值给基类引用
- 派生类对象赋值给基类对象，舍弃派生类新增的成员；派生类指针赋值给基类指针，没有拷贝对象的成员，也没有修改对象本身的数据，仅仅是改变了指针的指向；派生类引用赋值给基类引用，和指针的一样。

​    向上转型后通过基类的对象、指针、引用只能访问从基类继承过去的成员（包括成员变量和成员函数），不能访问派生类新增的成员

## 15.2 多态

不同的对象可以使用同一个函数名调用不同内容的函数。



- 静态多态性-在程序编译时系统就决定调用哪个函数，比如函数重载和静态多态性
- 动态多态性-在程序运行过程中动态确定调用那个函数，通过虚函数实现的。



## 15.3 虚函数

实现程序多态性的一个重要手段，使用基类对象指针访问派生类对象的同名函数。

将基类中的函数声明为虚函数，派生类中的同名函数自动为虚函数。
声明形式：virtual 函数类型 函数名 (参数列表);
构造函数不能声明为虚函数，析构函数可以声明为虚函数。

```c
class  A
{
public:
	virtual void show()
	{
		cout << "A show" << endl;
	}
};
class B:  public A
{
public:
	void show()
	{
		cout << "B show" << endl;
	}
};
int main()
{
B b;
b.show();//B show
A *pA = &b;
pA->show();//B show 如果show方法前没用virtual声明为虚函数，这里会输出A show

system("pause");
return 0;
}
```


​	

## 15.4 纯虚函数

在基类中不执行具体的操作，只为派生类提供统一结构的虚函数，将其声明为虚函数。



```c
class  A
{
public:
	virtual void show() = 0;
};
class B:  public A
{
public:
	void show()
	{
		cout << "B show" << endl;
	}
};
```

抽象类：包含纯虚函数的类称为抽象类。由于纯虚函数不能被调用，所以不能利用抽象类创建对象，又称抽象基类。









