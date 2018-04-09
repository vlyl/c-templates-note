# 函数模板

## 简单的函数模板，返回两个类型中的最大者

```cpp
// basic/max.hpp
template<typename T>
inline T const& max(T const& a, T const& b)
{
    return a < b ? b : a;
}

// 使用模板
std::string s1 = "abc";
std::string s2 = "bcde";
std::cout << ::max(s1, s2) << std::endl;
```

1. 模板中的类型T需要有operator< 方法
2. max函数调用前的::是为了区别标准库中的max方法，

## 模板实参推导(deduction)

**模板实参不允许进行自动类型转换**，所有的T类型必须一致

普通函数：
```cpp

// declaration
int func(int a);

// call
func(1.1);    // OK, double类型因式转换为int类型 
```

上例中

```cpp
max(1, 2);  //OK
max(1, 2.3);//Error, 第一个T是int, 第二个T是double
```

需要手动进行类型转换

```cpp
max(static_cast<double>(1), 2.3);
```
或者显示指定T类型

```cpp
max<double>(1, 2.3);    // 两个参数都是double类型
```

# 模板参数

函数模板有两种类型的参数
1. 模板参数 `template<typename T>` T是模板参数
2. 函数调用参数 `max(T const& a, T const& b)` a, b 是函数调用参数（形参）

注：C++ 11中，模板参数和函数参数一样，都可以指定默认值（缺省值），如: `template<typename T1, typename T2 = int>` 

模板函数无法推导出返回类型

```cpp
template<typename T1, typename T2, typename RT>
inline RT const& max(T1 const& a, T2 const& b)
{
    return a < b ? b : a;
}

// 调用
max(1, 2);    // error C2783: “const RT &max(const T1 &,const T2 &)”: 未能为“RT”推导 模板 参数
max<int, double, double>(1, 2.3); // OK 显示指定模板实参
```

显示指定模板实参，还可以只指定第一个模板实参，让后面的实参进行类型推导

```cpp
template<typename RT， typename T1, typename T2>    // 注意模板实参的位置
inline RT const& max(T1 const& a, T2 const& b)
{
    return a < b ? b : a;
}

// 调用
max<double>(1, 2.3); // OK 指定第一个实参，返回类型是double， 其余的实参类型通过调用推导出来
```

## 重载函数模板

当一个非模板函数（普通函数）和一个同名模板函数同时存在，而且该函数模板可以实例化为这个非模板函数，如果其他条件都相同的话，重载解析过程通常会调用非模板函数，而不会从该模板产生一个实例。除非显示制定调用模板函数：`max<>(3, 4)`