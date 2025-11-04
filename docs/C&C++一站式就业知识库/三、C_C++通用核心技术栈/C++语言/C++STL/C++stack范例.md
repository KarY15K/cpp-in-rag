# C++ stack范例

`std::stack` 是 C++ STL 中的一种容器适配器，基于 `deque`、`list` 或 `vector` 实现，遵循后进先出（LIFO）的原则。以下是 `std::stack` 的常用范例及其执行结果：

---

### 1. **创建和初始化 stack**
 

```plain
#include <iostream>
#include <stack>

int main() {
    // 创建一个空的 stack
    std::stack<int> stack1;

    // 使用 deque 初始化 stack
    std::deque<int> deque = {1, 2, 3, 4, 5};
    std::stack<int> stack2(deque);

    // 输出 stack2 的内容
    std::cout << "Stack2: ";
    while (!stack2.empty()) {
        std::cout << stack2.top() << " "; // 访问栈顶元素
        stack2.pop(); // 弹出栈顶元素
    }
    return 0;
}
```

**执行结果：**

Stack2: 5 4 3 2 1

---

### 2. **添加元素到 stack**
 

```plain
#include <iostream>
#include <stack>

int main() {
    std::stack<int> stack;

    // 使用 push 添加元素
    stack.push(10);
    stack.push(20);
    stack.push(30);

    // 输出 stack 的内容
    std::cout << "Stack: ";
    while (!stack.empty()) {
        std::cout << stack.top() << " ";
        stack.pop();
    }
    return 0;
}
```

**执行结果：**

Stack: 30 20 10

---

### 3. **访问 stack 的栈顶元素**
 

```plain
#include <iostream>
#include <stack>

int main() {
    std::stack<int> stack;

    // 添加元素
    stack.push(10);
    stack.push(20);
    stack.push(30);

    // 访问栈顶元素
    std::cout << "Top element: " << stack.top() << std::endl;

    return 0;
}
```

**执行结果：**

Top element: 30

---

### 4. **删除 stack 的栈顶元素**
 

```plain
#include <iostream>
#include <stack>

int main() {
    std::stack<int> stack;

    // 添加元素
    stack.push(10);
    stack.push(20);
    stack.push(30);

    // 删除栈顶元素
    stack.pop();

    // 输出剩余元素
    std::cout << "Stack after pop: ";
    while (!stack.empty()) {
        std::cout << stack.top() << " ";
        stack.pop();
    }
    return 0;
}
```

**执行结果：**

Stack after pop: 20 10

---

### 5. **获取 stack 的大小**
 

```plain
#include <iostream>
#include <stack>

int main() {
    std::stack<int> stack;

    // 添加元素
    stack.push(10);
    stack.push(20);
    stack.push(30);

    // 获取 stack 的大小
    std::cout << "Stack size: " << stack.size() << std::endl;

    return 0;
}
```

**执行结果：**

Stack size: 3

---

### 6. **检查 stack 是否为空**
 

```plain
#include <iostream>
#include <stack>

int main() {
    std::stack<int> stack;

    // 检查 stack 是否为空
    if (stack.empty()) {
        std::cout << "Stack is empty!" << std::endl;
    }

    // 添加元素
    stack.push(10);

    // 再次检查
    if (!stack.empty()) {
        std::cout << "Stack is not empty!" << std::endl;
    }

    return 0;
}
```

**执行结果：**

```plain
Stack is empty!
Stack is not empty!
```

---

### 7. **交换两个 stack 的内容**
 

```plain
#include <iostream>
#include <stack>

int main() {
    std::stack<int> stack1;
    std::stack<int> stack2;

    // 添加元素
    stack1.push(10);
    stack1.push(20);
    stack2.push(30);
    stack2.push(40);

    // 交换两个 stack 的内容
    stack1.swap(stack2);

    // 输出 stack1 的内容
    std::cout << "Stack1: ";
    while (!stack1.empty()) {
        std::cout << stack1.top() << " ";
        stack1.pop();
    }

    // 输出 stack2 的内容
    std::cout << "\nStack2: ";
    while (!stack2.empty()) {
        std::cout << stack2.top() << " ";
        stack2.pop();
    }

    return 0;
}
```

**执行结果：**

```plain
Stack1: 40 30
Stack2: 20 10
```

---

### 8. **使用自定义底层容器**
`std::stack` 默认使用 `std::deque` 作为底层容器，但也可以指定其他容器（如 `std::vector` 或 `std::list`）。

 

```plain
#include <iostream>
#include <stack>
#include <vector>

int main() {
    // 使用 vector 作为底层容器
    std::stack<int, std::vector<int>> stack;

    // 添加元素
    stack.push(10);
    stack.push(20);
    stack.push(30);

    // 输出 stack 的内容
    std::cout << "Stack: ";
    while (!stack.empty()) {
        std::cout << stack.top() << " ";
        stack.pop();
    }
    return 0;
}
```

**执行结果：**

Stack: 30 20 10

---

### 总结
`std::stack` 的常用操作包括：

+ `push()`：添加元素到栈顶。
+ `pop()`：删除栈顶元素。
+ `top()`：访问栈顶元素。
+ `empty()`：检查栈是否为空。
+ `size()`：获取栈的大小。
+ `swap()`：交换两个栈的内容。

通过这些操作，可以轻松实现后进先出（LIFO）的逻辑。



> 更新: 2025-04-17 22:28:34  
> 原文: <https://www.yuque.com/linuxer/gscfv1/dry4y7fzwi5dygry>