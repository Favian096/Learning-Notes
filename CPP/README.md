# CPP Notes
## Basic
### 数据类型
#### - 基本数据类型
```c++
  布尔型	 bool     1字节
  字符型	 char     1字节
  整型   	  int     4字节
  浮点型	 float    4字节
  双浮点型	double    8字节
  无类型	 void     缺失
  宽字符型	wchar_t  2或4字节(可以存储 Unicode 字符)
```

#### - 类型修饰符
```C++
  signed
  unsigned
  short
  long
  //例如
  unsigned int //可以将int正数范围扩大一倍
  short int    //可以简写为 short
  long int     //可以简写为 long
  long long    //比long更大
```

 #### - typedef 声明

- **typedef {type} {newTypeName}**

   - 示例
   ```c++
   typedef short int wchar_t  //其实wchar_t就是short int
   ```
#### - 枚举类型
```c++
   enum {enumName} {
       name[=num],
       name[=num],
       name[=num],
       ...
   }
```

#### - 其他
- 指针类型
- 数组类型
- 结构体类型
- 类 类型
- 共用体类型

- 示例
```c++
enum color { red, green=5, blue };
...
color r = red;
color b = blur;
```
- *其中 r = 0, b = 6*




#### - 类型转换
- 静态转换
  即**强转类型转换**, 不进行运行时类型检查
  
  ```c++
  int i = 10;
  float f = static_cast<float>(i);
  ```
  
- 动态转换
  用于**类继承中的基类和派生类之间指针或应用的转换**
  ```c++
  class Base{};
  class Derived : public Base{};
  Base* p_base = new Derived;
  Derived* p_derived = dynamic_cast<Derived>(p_base);
  ```

- 常量转换
  用于**将const类型对象转为非const类型**
  
  ```c++
  const int i = 10;
  int& r = const_cast<int&>(i);
  ```
  
- 重新解释转换
  用于**将一个数据类型的值重新解释为另一个数据类型的值**, 不进行任何类型检
  ```c++
  int i = 10;
  float f = reinterpret_cast<float&>(i);
  ```



### 变量作用域

- 局部变量
- 全局变量
- 类作用域
- 块作用域
  - 即代码中 使用{...} 包裹的作用域



### 常量const

- 又叫 *字面量*
- 有两种定义方式
  - 使用 #define 进行定义(即宏替换)
  - 使用 const 关键字
  ```c++
  const int I = 100; // 表示I为常量100
  ```
- **const本质上就是固定被修饰的变量**
    
    ```c++
    int a = 8;
    int * const p = &a;  // 主要用来防止指针地址被修改
    *p = 10;
    cout << *p; //输出的结果为 10 因为const固定的是a的地址, 而不是a的值
    /*
    同时固定值和地址
    const int * const p = &a;
    */
    ```
- 修饰
  - 修饰普通变量
  - 修饰指针变量
  - 修饰函数形参和函数返回值
  ```c++
  int fun(const int a); //没什么用
  const func(int b);    //没什么用
  ```
  - 修饰成员函数
  - **!注: const 不能与 static 连用**

- 其他类型限定符
  - volatile    表变量可能因为程序外的因素改变
  - restrict    表背修饰的指针是唯一访问它所指对象的方式
  - mutable     表类中的成员变量可以在const成员函数中被修改
  - static      定义静态变量
  - register    定义寄存器变量

### 存储类
- 用于定义变量|函数的范围可见性和声明周期
- auto      用于声明变量时推断类型, 函数声明时作返回值占位符
- register  用来定义存储在寄存器中的变量
- static    用来修饰变量的生命周期(同java)
- extern    用来提供一个全局变量的引用, 对所有程序文件均可见(多文件共享一个变量或函数)
- mutable   用于对象成员替代常量, mutable成员可以通过const成员函数修改
- thread_local  声明的变量仅可在所属的线程上访问, 随线程创建和销毁

### 函数使用
- 基本同C, 同java
- !在源文件中定义函数给另一个文件使用时, **必须使用函数声明在函数顶部**
- 函数参数
  - 传值调用 fun(int a, int b = 3)
  - 指针调用 fun(int \*a, int \*b)
  - 应用调用 fun(int &a, int &b)
  ```c++
  //示例
  void fun(int &a, int \*b){
      // &a表示参数a的地址, a表示地址处存储的值
      // b表示参数b的地址, *b表示地址处存储的值
      cout << (&a == b);
      cout << (a == *b);
      //以上结果均为true
  }
  ```
- lambda函数
  - \[capture\](parameters)->return-type{body}
  - \[captrue\](parameters)->{body}

### 基本补充
- c++的rand()函数默认会产生0~数据范围的整数, 不是0~1的小数
- c++中支持多维数组:
  ```c++
  type name[num][num][num] = ... //三维数组
  ```

- 字符串: 
  c++提供了两种风格的字符串
  - C 风格字符串(以'\0'作为结束标志)
  - string类 类型(不会以'\0'结束字符串, 面向对象的用)
    支持 str = str1 + str2;

- C++指针
  - 基本同C语言
  - C++中增加了空指针的定义, 即:
  ```C++
  int *ptr = NULL;
  ```

- C++的引用
  - 简介: **引用变量就是已存在变量的另一个名字**
  - !注: 
    - 不存在空引用(存在空指针), 引用必须连接到合法内存
    - 一旦引用被初始化为一个对象, 就不能指向另一个对象
    - 引用必须创建时初始化
  - 在函数中使用引用作为参数比一般的参数更安全
  - 在函数中返回引用

- 时间与日期
  - C++标准库没有提供日期类型需要引入\<ctime\>

- 输入和输出
  - \<iostream\> 下属对象: 
    - cin 标准输入流
    - cout 标准输出流
    - cerr 非缓冲标准错误流
    - clog 缓冲标准错误流/标准日志流
  - \<iomanip\> 如: setw和setprecision
  - \<fstream\>

- C++的struct基本同C(注意结构体指针要用->访问成员)





