# const

## 链接属性

### C语言默认是外部链接属性

```c
// test.c
const int m_A = 0; // C语言下默认是外部链接属性

// main.c
#include <stdio.h>

int main()
{
    extern const int m_A; // 告诉编译器外部有一个m_A
    // 外部链接属性即别的文件不需要包含头文件就能访问

    printf("m_A = %d",m_A);
    return 0;
}
```

```shell
m_A = 0
```

### C++默认是内部链接属性

```cpp
// test.cpp
const int m_A = 0; // C++下默认是内部链接属性

// main.cpp
#include <cstdio>

int main()
{
    extern const int m_A; // 报错

    printf("m_A = %d",m_A);
    return 0;
}
```

解决方法：提高作用域

```cpp
// test.cpp
extern const int m_A = 0;
```

## const分配内存情况

```cpp
#include <iostream>
#include <cstdlib>
#include <string> // 输出string时需要的头文件

using namespace std;

// 1. 对const修饰的局部变量取地址，会分配临时内存
void test01()
{
    const int a = 0;
    int *p = (int*)&a; // 会为a分配临时内存空间
}

// 2. 使用变量初始化const修饰的局部变量
void test02()
{
    int a = 0;
    const int m_A = a; // 会分配内存

    int *p = (int*)&m_A;
    *p = 100;
    cout << m_A << endl;
}

// 对于自定义的数据类型也会分配内存
struct Person
{
    string name;
    int age;
};

void test03()
{
    Person p1;
    // p1.name = "张三";
    // p1.age = 18;

    Person *p = (Person *)&p1;
    p->name = "李四";
    p->age = 19;

    cout << p1.name << "年龄：" << p1.age << endl;
}

int main()
{
    test02();
    test03();
    return EXIT_SUCCESS;
}
```

## 尽量用const代替#define

**如果因为常量的使用报错，#define会很难找到是哪里出了问题**

* 区别
    * #define
        * **没有作用域**
        * 没有数据类型
    * const
        * 有类型，可以进行编译器安全检查
        * 有作用域

```cpp
#include <iostream>
#include <cstdlib>

using namespace std;

#define MAX 1024

void func(int a)
{
    cout << "func(int a)" << endl;
}

void func(short a)
{
    cout << "func(short a)" << endl;
}

int main()
{
    func(MAX); // 调用哪个函数由编译器决定
    return EXIT_SUCCESS;
}
```

```shell
func(int a)
```