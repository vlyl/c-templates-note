# Class Template 类模板

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
        throw std::out_of_range("Stacj<>::pop(): empty stack");
    }
    elems.pop_back();
}
```

