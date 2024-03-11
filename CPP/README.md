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
- 示例
```c++
enum color { red, green=5, blue };
...
color r = red;
color b = blur;
```
- *其中 r = 0, b = 6*


#### - 其他
- 指针类型 *
- 数组类型 []
- 结构体类型 struct
- 类 类型 class
- 共用体类型 union



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
  - 引用调用 fun(int &a, int &b)
  ```c++
  //示例
  void fun(int &a, int *b){
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
  int *ptr = NULL;    // 其实NULL 就是 0
  int *ptr = nullptr; //推荐使用这种
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
    - cerr 非缓冲标准错误流 (控制台显示为红色)
    - clog 缓冲标准错误流/标准日志流(控制台显示红色)
  - \<iomanip\> 如: setw和setprecision
  - \<fstream\>

- C++的struct基本同C(注意结构体指针要用->访问成员)

- C++的**范围解析运算符** ::



## OOP

### 基本概念

- C++增加了面向对象的概念, 类是C++的核心特性: 
  - 类中的数据: **成员变量**;
  - 类中的函数: **成员函数**;
  - 类似一种模版, 可以创建多个具有相同属性和行为的对象

- C++的类和对象的基本定义:

  ``` c++
  class {className}
  {
      {public|private|protect}:   // 访问修饰符
      	type variableName;
      	type functionName(){};
      	...
  }
  
  className class1;
  className class2;
  ```

#### 成员函数

- C++ 的成员函数可以在内部直接定义, 也可以在内部声明后, 在外部使用**范围解析运算符定义**

  ```c++
  class Box {
  public:
      double length;
      double width;
      double height;
  
      double getWidth() {
          return this->width;
      }
  
      double getHeight(void);
  };
  
  //重新定义|实现, 相当于override
  double Box::getHeight() {
      return this->height;
  }
  ```

  

#### 访问修饰符

- C++的类访问修饰符在类结束或或下一个访问修饰符之前有效

- 基本使用: **成员和类的默认访问修饰符是 private**。

  ```c++
  class Base{
      double width; //默认为private类型
      public:
      ...
      protected:  // 与private相似, 但protect修饰的成员在派生类|子类中是可以访问的
      ...
      private:  //只有类和友缘函数可以访问私有成员
      ...
  }
  ```

  

#### 构造&析构函数

##### 构造函数

- 函数名称与类名完全相同

- 用于初始化成员变量, 没有返回值(也不会返回void)

- 每次创建新对象的时候运行

  ```c++
  class Line{
      double length;
      double width;
      public:
      	Line();   // 构造函数
      	Line(double len);  // 重载构造函数
      	Line(double len, double width); // 重载构造函数
  }
  
  //没有返回值
  Line::Line(){
      cout << "Object is being created !" << endl;
  }
  
  /* 使用初始化列表初始化length, (更简洁)
     相当于在方法内部写  this.length = len;*/
  Line::Line(double len) : length(len){ 
      cout << "Object is being created ! length init ->" << len << endl; 
  }
  
  //同上
  Line::Line(double length, double width): length(len), width(width){
      cout << "Object is being created ! length width init ->" << endl; 
  }
  ```



##### 析构函数

- 析构函数名与类名相同, 只是在前面添加了**~**作为前缀

- 析构函数不能带有任何参数, 没有返回值, 用于**程序前关闭资源, 释放内存**

  ```c++
  class Line{
      public:
      	Line();   // 构造函数
      	~Line(){   //析构函数
              cout << "Object is delete !" << endl;
          }
  }
  
  // 当然也可以这样: 
  //Line::~Line(){
  //	cout << "Object is delete !" << endl;
  //}
  ```

  

#### 拷贝构造函数

- 简言之, 就是*用已创建的对象来初始化新的对象(同类型)*

- **如果在类中没有定义拷贝构造函数，编译器会自行定义一个默认的**

- **默认的拷贝构造函数会逐个成员地复制源对象的所有成员变量**

- **如果类带有指针变量，并有动态内存分配，则它必须有一个拷贝构造函数。**

  ```c++
  class Line{
      public:
      	Line(const Line &line); // 自定义拷贝构造函数
  }
  ```

- 通常用于:

  - 初始化新的对象

    ```c++
    Line line2(line1);  // 使用拷贝构造函数, 构建一个与line1相同的对象
    Line line2 = line1; // 与上行代码相同的意思, 会自动调用拷贝构造函数
    ```

  - 将对象作为参数传递

    ```c++
    void display(Line obj)// 即obj会调用拷贝构造函数, 自身初始化为传参的line
    {...}  
    ```

  - 函数返回对象, 接收这个对象

    ```c++
    Line line2 = func(); // func()函数返回一个Line对象 初始化line2
    ```



#### 友元函数和友元类

- 类的友元函数定义在外部, 但有权访问类的所有private和protected成员

- **友元函数不是成员函数, 没有this指针, 外部定义不加 className :: **

- 友元函数基本用法:  (**friend 关键字**)

  ```c++
  class Line {
  private:
      double width;
  
  public:
      void setWidth(double width) {
          this->width = width;
      };
  
  	// 友元函数(参数就是这个对象, 由此可以访问对象的所有成员)
      friend void printWidth(Line line) {
          cout << line.width << endl;
      }
  
  };
  
  // 也可以这样, 不需要加 Line:: 因为友元函数不是成员函数
  //void printWidth(Line line) {
  //    cout << line.width << endl;
  //}
  
  int main() {
      Line line;
      line.setWidth(3.5);
      // 直接就可以调用, 因为友元函数声明就相当于已经在外部定义了
      printWidth(line);  // 输出 3.5
      return 0;
  }
  ```

- 友元类: **友元类的所有成员函数都是源类的友元函数**

  ```c++
  class Line {
  private:
      int width;
      double height;
  
  public:
      // 声明类A为友元类
      friend class A;
  
      void printWidth() {
          cout << width << endl;
      }
  };
  
  class A {
  public:
      // A中的所有函数都是Line的友元函数(需要传递对象做参数)
      void setWidth(Line &line, int width) {
          line.width = width;
      }
  };
  
  int main() {
      Line line;
      A a;
      a.setWidth(line, 5);
      line.printWidth();
      return 0;
  }
  ```

- **!注: 友元函数访问非static成员需要在对象作为参数, 访问static成员或全局变量则不需要**



#### 内联函数

- 内联函数是为了解决函数调用的效率问题, 编译后会用函数体替换调用的位置

- **!注: 内联函数一般是只有几行的函数, 通常只用于精短的函数**

  - 一般不允许出现循环和判断语句
  - 在类中定义的(有函数体)的函数都是内敛函数(写inline为显示内联函数, 不写算隐式内联函数)

  ```c++
  //函数声明前加上 inline 限定符
  inline void println(msg){
      cout << msg << endl;
  }
  ```

  

#### this指针和类指针

- this是一个特殊的指针, 指向当前对象的实例

- 只有类中的成员才有this指针

- 简言之, **this就是实例化对象的地址**(下方代码两处输出结果相同)

  ```c++
  class Line{
      public: 
      	void printThis(){
              cout << this << endl;   //结果相同
          }
  }
  
  int main(){
      Line line;
      line.printThis();
      cout << &line << endl;    //结果相同
  }
  ```

- 类的指针(同结构体):  **使用 -> 进行访问**

  ```c++
  class Line{
      public: 
      	void printThis(){
              cout << this << endl;   //结果相同
          }
  }
  
  int main(){
      Line *line = new Line;    //动态内存分配(可以new Line(@param))
      line->printThis();
      cout << line << endl;    //结果相同
  }
  
  //可以这样
  {
      Line *obj = new Line(123);
      Line &line = *obj;
      line.printThis();		 //结果相同
      cout << &line << endl;	 //结果相同
      cout << *obj << endl;	 //结果相同
  }
  ```

  

#### 静态成员

- **类的静态成员在所有对象中是共享的(需要创建对象)**

- 如果不存在初始化语句, 在创建第一个对象时, 类中所有的静态数据都会初始化为0

- 使用 :: 对静态成员变量进行初始化, 

  **不能在类中初始化静态变量(除非用const), 必须在类外部初始化**

- **外部初始化静态变量要加上类型**

  **(为了单独分配内存以便多对象共用, 故创建对象时不会为static成员变量分配内存)**

- **类的静态函数不需要创建对象**

- 可以使用 :: 直接使用类的静态函数

- !注 : 类的静态函数没有this指针, 只能访问静态成员

  ```c++
  class Line {
  public:
  
      Line() {
          ObjCount++;
      }
  	
      //需要在类的外部分配内存, 以便多对象共用
      static int ObjCount;
      
      //除非这样, 才会在类的对象创建时分配内存
  	//static const int ObjCount;
      
      static int getObjCount() {
          return Line::ObjCount;
      }
  };
  
  // 注意: 初始化需要加上类型(int)
  int Line::ObjCount = 0;
  
  int main() {
      Line line1;  // 所有对象共用静态变量
      Line line2;  // 所有对象共用静态变量
      cout << Line::ObjCount << endl;  //输出为2
      cout << Line::getObjCount() << endl;  //输出为2
      return 0;
  }
  ```

  
