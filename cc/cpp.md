# C++软件工程

## C与C++的区别
Data + Function -> variables
Data Members + Members Function -> objects

C++把数据和函数包在一起，数据可以有很多份，函数只有一份。

+ C++ 中分为两大类:
  - 类中不带指针(Complime)
  - 类中带指针

1. 引用头文件，防卫式声明

2. inline函数声明
   - 只能把类的函数声明为inline函数，全局函数不能声明为inline
   - inline只是给编译器个优化建议，但是只是个建议，简单的函数会被inline
   
3. 访问级别(access level)
   public区域:可被外界调用的函数
   private区域:数据和私有函数

4. 构造函数
   + 不会手动调用构造函数，创建对象会自动调用构造函数
   + 创建对象的三种方式:
     - complex c1(2,1):拷贝构造
     - complex:无参构造
     - complex\* p = new complex(4):赋值拷贝
   + 构造函数可以有很多-overloading(重载)
5. 构造函数的专有赋初始值方式\-\-初始化列(initialization list)
   
   ```
   class Complex
   {
    public:
      Complex(double r=0, double i=0)
        :re(r), im(i)
      {}
    private:
      double re, im;
   };

+ 初始化的两个阶段:
  - 先是初值列
  - 其次是构造函数体内赋值
