# this指针

[TOC]

## this指针的指向

>**this指针指向被调用的成员函数所属的对象**

```cpp
#include <iostream>
#include <cstdlib>

using namespace std;

class Person
{
public:
    int a;

    void func(int var)
    {
        this->a = var; // 相当于a = var
        cout << this << ": " << this->a << endl;
    }
};

int main()
{
    Person p1;
    Person p2;
    p1.func(1);
    p2.func(2);

    return 0;
}
```

```shell
0x61fe8c: 1
0x61fe88: 2
```

## this指针与链式编程思想

```cpp
#include <iostream>
#include <cstdlib>

using namespace std;

class Person
{
public:
    int age;

    // 每一个 非静态 成员函数内部都隐藏加了一个this指针
    void func(int age)
    {
        this->age = age; // this指针可以解决名称冲突
    }

    Person& personAddAge(Person &p)
    {
        this->age += p.age;
        return *this; // *this指向本体，this指向本体的指针
    }
};

int main()
{
    Person p1;
    Person p2;
    p1.func(10);
    p2.func(20);

    cout << "p1的年龄:" << p1.age << endl;

    p2.personAddAge(p1).personAddAge(p1).personAddAge(p1); // 链式编程思想

    cout << "p2的年龄:" << p2.age << endl;

    return 0;
}
```

```shell
p1的年龄:10
p2的年龄:50
```

## 空指针访问成员函数

```cpp
#include <iostream>
#include <cstdlib>

using namespace std;

class Person
{
public:
    int age;

    Person(int age)
    {
        this->age = age;
    }

    void showPerson()
    {
        cout << "showPerson函数调用" << endl;
    }

    void showAge()
    {
        if (this == nullptr)
        {
            return;
        }
        // this = nullptr
        cout << "showAge函数调用 age=" << age << endl;
    }
};

int main()
{
    Person *p = nullptr;
    p->showPerson();
    p->showAge();

    return 0;
}
```

```shell
showPerson函数调用
```

## 常函数和常对象

```cpp
#include <iostream>
#include <cstdlib>

using namespace std;

class Person
{
public:
    int age;

    Person(int age)
    {
        this->age = age;
    }

    void showPerson()
    {
        if (this == nullptr)
        {
            return;
        }
        cout << "showPerson函数调用" << endl;
    }

    // this指针的本质 指针常量
    // this指针的类型 Person *const this
    void showAge() const // const修饰的是this指针，当成员函数后面写了const，这个函数称为常函数
    {
        if (this == nullptr)
        {
            return;
        }
        // this->age = 10; // error: assignment of member 'Person::age' in read-only object
        this->Height = 10;
        cout << "age=" << age << endl;
    }

    mutable int Height; // 如果加了mutable关键字，即使在常函数中也能修改
};

int main()
{
    const Person p1(10);

    // p1.age = 20; // 常对象不可以直接修改成员变量
    p1.showAge(); // 常对象可以调用常函数
    // p1.showPerson(); // 常对象不可以调用普通函数
    p1.Height = 20; // mutable修饰的成员变量在常对象也可以修改

    return 0;
}
```