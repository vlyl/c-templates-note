# Class Template 类模板

## 定义

```cpp
#include <vector>
#include <stdexcept>

template<typename T>
class Stack {
public:
    void push(T const&);        // 压入元素
    void pop();                 // 弹出元素
    T top() const;              // 返回栈顶元素
    bool empty() const {        
        return elems.empty();   // 返回栈是否为空
    }

private:
    std::vector<T> elems;       // 存储元素容器
}；
```

```cpp
template<typename T>
void Stack<T>::push (T const& elem) {
    elems.push_back(elem);
}

template<typename T>
void Stack<T>::pop() {
    if (elems.empty()) {
        throw std::out_of_range("Stack<>::pop(): empty stack");
    }
    elems.pop_back();
}

template<typename T>
T Stack<T>::top() const {
    if (elems.empty()) {
        throw std::out_of_range("Stack<>::top(): empty stack");
    }
    return elems.back();
}
```


该类定义出的类型是`Stack<T>`，当需要使用该类的类型时，必须使用`Stack<T>`。例如当声明拷贝构造函数和赋值运算符，应该这么写：

```cpp
template<typename T>
class Stack {
    Stack(Stack<T> const &);
    Stack<T>& operator=(Stack<T> const &);
};
```

而当需要使用类名而不是类型时，需要使用`Stack`， 比如声明类的构造函数和析构函数：

```cpp
template<typename T>
class Stack {
    Stack();
    ~Stack();
};

```

## 使用
只有那些被调用的成员函数，才会产生这些函数的实例化代码。即，对于类模板，成员函数只有在被使用的时候才会被实例化，而不是在实例化类型的时候也实例化所有的成员函数。


## 特化(Specialization)

可以指定模板实参，来优化基于该类型实参的特定类型实现，这个过程称为特化。

进行类模板特化时，每个成员函数都必须重新定义为普通函数（不能是含有模板参数的函数模板），原来模板函数中的T也相应的被特化类型取代。

```cpp
template<>
class Stack<std::string> {
    ...
};

void Stack<std::string>::push(std::string const& elem) {
    elems.push_back(elem);
}
```

局部特化

我理解的局部特化就是对模板参数进行初步的规定，而不是直接给定模板实参

类模板
```cpp
template<typename T1, typename T2>
class MyClass {

};

```

局部特化例子

```cpp

// 两个模板参数具有相同类型
template<typename T>
class MyClass<T, T> {};

// 第二个模板参数是int类型
template<typename T>
class MyClass<T, int> {};

// 两个模板参数都是指针类型
template<typename T1, typename T2>
class MyClass<T1*, T2*> {};

```

## 缺省（默认 default）模板实参

可以为函数的参数指定默认值（缺省值），同样的，也可以为模板参数指定缺省值，即缺省模板实参。
使用时也像带有默认参数的函数一样调用。

```cpp
template<typename T1, typename T2 = int>
class Stack {};
```