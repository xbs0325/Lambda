# Lambda
C++ 中的 Lambda 表达式
在 C++ 11 和更高版本中，Lambda 表达式（通常称为 Lambda）是一种在被调用的位置或作为参数传递给函数的位置定义匿名函数对象（闭包）的简便方法。 Lambda 通常用于封装传递给算法或异步函数的少量代码行。

# Lambda 表达式的各个部分
## 下面是作为第三个参数 std::sort() 传递给函数的简单 lambda：

```
#include <algorithm>
#include <cmath>

void abssort(float* x, unsigned n) {
    std::sort(x, x + n,
        // Lambda expression begins
        [](float a, float b) {
            return (std::abs(a) < std::abs(b));
        } // end of lambda expression
    );
}
```

下图显示了 lambda 语法的各个部分：

![lambdaexpsyntax](https://github.com/xbs0325/Lambda/assets/115852434/cb036153-589f-42be-b039-b180829ede62)




 1.capture 子句（在 C++ 规范中也称为 Lambda 引导）
 
 2.参数列表（可选） （也称为 Lambda 声明符）

 3.mutable 规范（可选）

 4.exceptionspecification （可选）

 5.trailing-return-type（可选）。

 6.Lambda 体。


## 1.capture 子句（在 C++ 规范中也称为 Lambda 引导）

Lambda 可在其主体中引入新的变量（用 C++14）

空 capture 子句 [ ] 指示 lambda 表达式的主体不访问封闭范围中的变量。

可以使用默认捕获模式来指示如何捕获 Lambda 体中引用的任何外部变量：[&] 表示通过引用捕获引用的所有变量，而 [=] 表示通过值捕获它们。 可以使用默认捕获模式，然后为特定变量显式指定相反的模式。

！！注意！！子句中的变量不可重复
例如
```
struct S { void f(int i); };

void S::f(int i) {
    [&, i]{};      // OK
    [&, &i]{};     // ERROR: i preceded by & when & is the default
    [=, this]{};   // ERROR: this when = is the default
    [=, *this]{ }; // OK: captures this by value. See below.
    [i, i]{};      // ERROR: i repeated
}
```

还有 类内用lamada需要加this或*this参数

## 2.参数列表（可选） （也称为 Lambda 声明符）

Lambda 既可以捕获变量，也可以接受输入参数。 参数列表（在标准语法中称为 Lambda 声明符）是可选的，它在大多数方面类似于函数的参数列表。

参数列表是可选的，因此在不将自变量传递到 Lambda 表达式，并且其 Lambda 声明符不包含 exception-specification、trailing-return-type 或 mutable 的情况下，可以省略空括号。

## 3.mutable 规范（可选）

在 lambda 表达式中，默认情况下，被捕获的变量是只读的（const），即 lambda 表达式的函数调用运算符 operator() 是 const-qualified。这意味着 lambda 表达式默认情况下不能修改通过值捕获的变量。

然而，通过使用 mutable 关键字，可以取消 lambda 表达式的 const 限定，使其能够修改通过值捕获的变量。

## 4.exception-specification（可选）。异常声明

在 C++ 中，exception-specification（异常规范）用于在函数声明中指定函数可能抛出的异常类型。它允许程序员声明函数可能引发的异常，并且可以为调用者提供有关函数可能抛出的异常的信息。

在早期的 C++ 标准中，异常规范使用特殊的语法来指定函数可能引发的异常类型。例如：
`
void myFunction() throw (ExceptionType1, ExceptionType2, ...);
`
noexcept 用于指定函数不会抛出任何异常。例如：
`
void myFunction() noexcept;
`
throw() 用于指定函数不会抛出任何异常。例如：
`
void myFunction() throw();
`
上述声明与 noexcept 的效果相同，表示 myFunction() 不会抛出任何异常。

需要注意的是，C++11 标准之后，编译器已经不再强制要求使用异常规范。实际上，现代的 C++ 更加倾向于使用异常安全的编程实践和异常处理机制，而不是依赖于异常规范来管理异常。因此，在新的代码中，通常不需要使用异常规范来指定函数的异常行为。

## 5.返回类型
```
auto lambda = []() -> double { // 显式指定返回类型为 double
    if (condition) {
        return 3.14;
    } else {
        return 2.71;
    }
};
```
Lambda 表达式的返回类型可以由编译器自动推断，也可以显式指定。在 C++11 中，可以使用自动推断或尾置返回类型语法来指定返回类型。在 C++14 及更高版本中，通常可以依赖编译器的自动推断，而无需显式指定返回类型

## 6.Lambda 体

Lambda 表达式的 Lambda 体是一个复合语句。 它可以包含普通函数或成员函数体中允许的任何内容。 普通函数和 lambda 表达式的主体均可访问以下变量类型：

从封闭范围捕获变量，如前所述。

参数。

本地声明变量。

类数据成员（在类内部声明并且捕获 this 时）。

具有静态存储持续时间的任何变量（例如，全局变量）。

# 例子
```
#include <iostream>
using namespace std;

int main()
{
   int m = 0;
   int n = 0;
   [&, n] (int a) mutable { m = ++n + a; }(4);
   cout << m << endl << n << endl;
}
```

输出:

```
5
0
```
  
