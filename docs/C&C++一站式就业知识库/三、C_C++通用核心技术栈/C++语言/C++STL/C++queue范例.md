# C++ queue范例

`std::deque`（双端队列）是 C++ STL 中的一种动态数组容器，支持在头部和尾部高效地插入和删除元素，同时允许随机访问。以下是 `std::deque` 的常用范例及其执行结果：

---

### 1. **创建和初始化 deque**
 

```plain
#include <iostream>
#include <deque>

int main() {
    // 创建一个空的 deque
    std::deque<int> deque1;

    // 创建并初始化一个包含 5 个元素的 deque，初始值为 0
    std::deque<int> deque2(5);

    // 创建并初始化一个包含 5 个元素的 deque，初始值为 10
    std::deque<int> deque3(5, 10);

    // 使用初始化列表创建 deque
    std::deque<int> deque4 = {1, 2, 3, 4, 5};

    // 输出 deque4 的内容
    std::cout << "deque4: ";
    for (int i : deque4) {
        std::cout << i << " ";
    }
    return 0;
}
```

**执行结果：**

```plain
deque4: 1 2 3 4 5
```

---

### 2. **添加元素到 deque**
 

```plain
#include <iostream>
#include <deque>

int main() {
    std::deque<int> deque;

    // 在尾部添加元素
    deque.push_back(10);
    deque.push_back(20);
    deque.push_back(30);

    // 在头部添加元素
    deque.push_front(5);
    deque.push_front(1);

    // 使用 emplace_back 和 emplace_front 直接构造元素
    deque.emplace_back(40);
    deque.emplace_front(0);

    // 输出 deque 内容
    std::cout << "deque: ";
    for (int i : deque) {
        std::cout << i << " ";
    }
    return 0;
}
```

**执行结果：**

```plain
deque: 0 1 5 10 20 30 40
```

---

### 3. **访问 deque 元素**
 

```plain
#include <iostream>
#include <deque>

int main() {
    std::deque<int> deque = {1, 2, 3, 4, 5};

    // 使用下标访问元素
    std::cout << "Element at index 2: " << deque[2] << std::endl;

    // 使用 at() 访问元素（会检查边界）
    std::cout << "Element at index 3: " << deque.at(3) << std::endl;

    // 访问第一个和最后一个元素
    std::cout << "First element: " << deque.front() << std::endl;
    std::cout << "Last element: " << deque.back() << std::endl;

    return 0;
}
```

**执行结果：**

```plain
Element at index 2: 3
Element at index 3: 4
First element: 1
Last element: 5
```

---

### 4. **遍历 deque**
 

```plain
#include <iostream>
#include <deque>

int main() {
    std::deque<int> deque = {1, 2, 3, 4, 5};

    // 使用下标遍历
    std::cout << "Using index: ";
    for (size_t i = 0; i < deque.size(); ++i) {
        std::cout << deque[i] << " ";
    }
    std::cout << std::endl;

    // 使用范围 for 循环遍历
    std::cout << "Using range-based for loop: ";
    for (int i : deque) {
        std::cout << i << " ";
    }
    std::cout << std::endl;

    // 使用迭代器遍历
    std::cout << "Using iterator: ";
    for (auto it = deque.begin(); it != deque.end(); ++it) {
        std::cout << *it << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

**执行结果：**

```plain
Using index: 1 2 3 4 5
Using range-based for loop: 1 2 3 4 5
Using iterator: 1 2 3 4 5
```

---

### 5. **删除 deque 元素**
 

```plain
#include <iostream>
#include <deque>

int main() {
    std::deque<int> deque = {1, 2, 3, 4, 5};

    // 删除头部元素
    deque.pop_front();

    // 删除尾部元素
    deque.pop_back();

    // 删除指定位置的元素（删除第 2 个元素）
    auto it = deque.begin();
    std::advance(it, 1);
    deque.erase(it);

    // 输出剩余元素
    std::cout << "deque after deletions: ";
    for (int i : deque) {
        std::cout << i << " ";
    }
    return 0;
}
```

**执行结果：**

```plain
deque after deletions: 2 4
```

---

### 6. **获取 deque 的大小**
 

```plain
#include <iostream>
#include <deque>

int main() {
    std::deque<int> deque = {1, 2, 3, 4, 5};

    // 获取元素个数
    std::cout << "Size: " << deque.size() << std::endl;

    return 0;
}
```

**执行结果：**

```plain
Size: 5
```

---

### 7. **deque 的排序**
 

```plain
#include <iostream>
#include <deque>
#include <algorithm> // 包含 sort 函数

int main() {
    std::deque<int> deque = {5, 3, 1, 4, 2};

    // 对 deque 进行升序排序
    std::sort(deque.begin(), deque.end());

    // 输出排序后的 deque
    std::cout << "Sorted deque: ";
    for (int i : deque) {
        std::cout << i << " ";
    }
    return 0;
}
```

**执行结果：**

```plain
Sorted deque: 1 2 3 4 5
```

---

### 8. **deque 的交换**
 

```plain
#include <iostream>
#include <deque>

int main() {
    std::deque<int> deque1 = {1, 2, 3};
    std::deque<int> deque2 = {4, 5, 6};

    // 交换两个 deque 的内容
    deque1.swap(deque2);

    // 输出交换后的结果
    std::cout << "deque1: ";
    for (int i : deque1) {
        std::cout << i << " ";
    }
    std::cout << "\ndeque2: ";
    for (int i : deque2) {
        std::cout << i << " ";
    }
    return 0;
}
```

**执行结果：**

```plain
deque1: 4 5 6
deque2: 1 2 3
```

---

### 9. **deque 的反转**
 

```plain
#include <iostream>
#include <deque>
#include <algorithm> // 包含 reverse 函数

int main() {
    std::deque<int> deque = {1, 2, 3, 4, 5};

    // 反转 deque
    std::reverse(deque.begin(), deque.end());

    // 输出反转后的 deque
    std::cout << "Reversed deque: ";
    for (int i : deque) {
        std::cout << i << " ";
    }
    return 0;
}
```

**执行结果：**

```plain
Reversed deque: 5 4 3 2 1
```

---

### 10. **deque 的插入**
 

```plain
#include <iostream>
#include <deque>

int main() {
    std::deque<int> deque = {1, 2, 3, 4, 5};

    // 在指定位置插入元素
    auto it = deque.begin();
    std::advance(it, 2); // 将迭代器移动到第 3 个位置
    deque.insert(it, 100);

    // 输出插入后的 deque
    std::cout << "deque after insertion: ";
    for (int i : deque) {
        std::cout << i << " ";
    }
    return 0;
}
```

**执行结果：**

```plain
deque after insertion: 1 2 100 3 4 5
```

---

### 总结
`std::deque` 的常用操作包括：

+ `push_back()` 和 `push_front()`：在尾部或头部添加元素。
+ `pop_back()` 和 `pop_front()`：删除尾部或头部元素。
+ `emplace_back()` 和 `emplace_front()`：直接在尾部或头部构造元素。
+ `operator[]` 和 `at()`：访问元素。
+ `size()`：获取元素个数。
+ `empty()`：检查是否为空。
+ `insert()` 和 `erase()`：在指定位置插入或删除元素。
+ `sort()` 和 `reverse()`：排序和反转。

通过这些操作，可以高效地管理双端队列中的数据。



### 11. deque里的emplace_back有什么优势
`std::deque` 中的 `emplace_back` 是 C++11 引入的一个成员函数，用于在 `deque` 的尾部直接构造元素。与 `push_back` 相比，`emplace_back` 具有以下优势：

---

### 1. **避免不必要的拷贝或移动操作**
+ `**push_back**`：需要先构造一个临时对象，然后将该对象拷贝或移动到容器中。
+ `**emplace_back**`：直接在容器中构造对象，避免了临时对象的创建和拷贝/移动操作。

#### 示例对比：
```plain
#include <iostream>
#include <deque>

class MyClass {
public:
    MyClass(int a, int b) {
        std::cout << "Constructor called! a = " << a << ", b = " << b << std::endl;
    }

    MyClass(const MyClass& other) {
        std::cout << "Copy constructor called!" << std::endl;
    }

    MyClass(MyClass&& other) noexcept {
        std::cout << "Move constructor called!" << std::endl;
    }
};

int main() {
    std::deque<MyClass> deque;

    // 使用 push_back
    std::cout << "Using push_back:" << std::endl;
    deque.push_back(MyClass(1, 2)); // 先构造临时对象，再移动

    // 使用 emplace_back
    std::cout << "\nUsing emplace_back:" << std::endl;
    deque.emplace_back(3, 4); // 直接在容器中构造对象

    return 0;
}
```

**输出：**

```plain
Using push_back:
Constructor called! a = 1, b = 2
Move constructor called!

Using emplace_back:
Constructor called! a = 3, b = 4
```

从输出可以看出：

+ `push_back` 调用了构造函数和移动构造函数。
+ `emplace_back` 只调用了构造函数，避免了额外的开销。

---

### 2. **支持多参数构造**
+ `**push_back**`：只能传递一个已经构造好的对象。
+ `**emplace_back**`：可以传递构造对象所需的参数，支持多参数构造函数。

#### 示例：
```plain
#include <iostream>
#include <deque>

class MyClass {
public:
    MyClass(int a, int b, int c) {
        std::cout << "Constructor called! a = " << a << ", b = " << b << ", c = " << c << std::endl;
    }
};

int main() {
    std::deque<MyClass> deque;

    // 使用 emplace_back 传递多个参数
    deque.emplace_back(1, 2, 3);

    return 0;
}
```

**输出：**

Constructor called! a = 1, b = 2, c = 3

---

### 3. **性能优势**
由于 `emplace_back` 避免了临时对象的创建和拷贝/移动操作，因此在性能上通常优于 `push_back`，尤其是在以下场景中：

+ 对象的构造开销较大（例如，对象包含动态内存分配或复杂的初始化逻辑）。
+ 容器中存储的对象不支持拷贝或移动操作（例如，`std::unique_ptr`）。

---

### 4. **代码简洁性**
`emplace_back` 可以直接传递构造参数，使代码更加简洁直观。

#### 示例：
```plain
#include <iostream>
#include <deque>
#include <string>

int main() {
    std::deque<std::string> deque;

    // 使用 push_back
    deque.push_back(std::string("Hello"));

    // 使用 emplace_back
    deque.emplace_back("World"); // 更简洁

    // 输出内容
    for (const auto& str : deque) {
        std::cout << str << " ";
    }
    return 0;
}
```

**输出：**

Hello World

---

### 总结
| 特性 | `push_back` | `emplace_back` |
| --- | --- | --- |
| **临时对象** | 需要创建临时对象 | 不需要创建临时对象 |
| **拷贝/移动操作** | 可能涉及拷贝或移动操作 | 无拷贝或移动操作 |
| **多参数构造** | 不支持 | 支持 |
| **性能** | 较低（涉及额外开销） | 较高（直接构造） |
| **代码简洁性** | 需要显式构造对象 | 直接传递构造参数 |


因此，在大多数情况下，推荐使用 `emplace_back`，尤其是在需要高效构造对象或传递多个构造参数的场景中。

 



> 更新: 2025-04-17 22:28:11  
> 原文: <https://www.yuque.com/linuxer/gscfv1/gukv1gpwlqsfhgb7>