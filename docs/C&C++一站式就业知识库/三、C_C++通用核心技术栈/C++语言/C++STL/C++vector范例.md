# C++ vector范例

`std::vector` 是 C++ STL 中最常用的动态数组容器，支持高效的随机访问和动态扩容。以下是 `std::vector` 的常用范例及其执行结果：

---

### 1. **创建和初始化 vector**
 

```cpp
#include <iostream>
#include <vector>

int main() {
    // 创建一个空的 vector
    std::vector<int> vec1;

    // 创建并初始化一个包含 5 个元素的 vector，初始值为 0
    std::vector<int> vec2(5);

    // 创建并初始化一个包含 5 个元素的 vector，初始值为 10
    std::vector<int> vec3(5, 10);

    // 使用初始化列表创建 vector
    std::vector<int> vec4 = {1, 2, 3, 4, 5};

    // 输出 vec4 的内容
    std::cout << "vec4: ";
    for (int i : vec4) {
        std::cout << i << " ";
    }
    return 0;
}
```

**执行结果：**

vec4: 1 2 3 4 5

---

### 2. **添加元素到 vector**
 

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> vec;

    // 在尾部添加元素
    vec.push_back(10);
    vec.push_back(20);
    vec.push_back(30);

    // 使用 emplace_back 直接构造元素
    vec.emplace_back(40);

    // 输出 vector 内容
    std::cout << "vec: ";
    for (int i : vec) {
        std::cout << i << " ";
    }
    return 0;
}
```

**执行结果：**

 

 

vec: 10 20 30 40

---

### 3. **访问 vector 元素**
 

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> vec = {1, 2, 3, 4, 5};

    // 使用下标访问元素
    std::cout << "Element at index 2: " << vec[2] << std::endl;

    // 使用 at() 访问元素（会检查边界）
    std::cout << "Element at index 3: " << vec.at(3) << std::endl;

    // 访问第一个和最后一个元素
    std::cout << "First element: " << vec.front() << std::endl;
    std::cout << "Last element: " << vec.back() << std::endl;

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

### 4. **遍历 vector**
 

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> vec = {1, 2, 3, 4, 5};

    // 使用下标遍历
    std::cout << "Using index: ";
    for (size_t i = 0; i < vec.size(); ++i) {
        std::cout << vec[i] << " ";
    }
    std::cout << std::endl;

    // 使用范围 for 循环遍历
    std::cout << "Using range-based for loop: ";
    for (int i : vec) {
        std::cout << i << " ";
    }
    std::cout << std::endl;

    // 使用迭代器遍历
    std::cout << "Using iterator: ";
    for (auto it = vec.begin(); it != vec.end(); ++it) {
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

### 5. **修改 vector 元素**
 

```plain
#include <iostream>
#include <vector>

int main() {
    std::vector<int> vec = {1, 2, 3, 4, 5};

    // 修改指定位置的元素
    vec[2] = 100;

    // 输出修改后的 vector
    std::cout << "Modified vec: ";
    for (int i : vec) {
        std::cout << i << " ";
    }
    return 0;
}
```

**执行结果：**

 

 

Modified vec: 1 2 100 4 5

---

### 6. **删除 vector 元素**
 

```plain
#include <iostream>
#include <vector>

int main() {
    std::vector<int> vec = {1, 2, 3, 4, 5};

    // 删除最后一个元素
    vec.pop_back();

    // 删除指定位置的元素（删除第 2 个元素）
    vec.erase(vec.begin() + 1);

    // 输出剩余元素
    std::cout << "vec after deletions: ";
    for (int i : vec) {
        std::cout << i << " ";
    }
    return 0;
}
```

**执行结果：**

 

 

vec after deletions: 1 3 4

---

### 7. **获取 vector 的大小和容量**
 

```plain
#include <iostream>
#include <vector>

int main() {
    std::vector<int> vec = {1, 2, 3, 4, 5};

    // 获取元素个数
    std::cout << "Size: " << vec.size() << std::endl;

    // 获取当前容量
    std::cout << "Capacity: " << vec.capacity() << std::endl;

    // 调整大小（如果新大小大于当前大小，会用默认值填充）
    vec.resize(10);

    // 预留空间（避免频繁扩容）
    vec.reserve(20);

    return 0;
}
```

**执行结果：**

 

 

```plain
Size: 5
Capacity: 5
```

---

### 8. **vector 的排序**
 

```plain
#include <iostream>
#include <vector>
#include <algorithm> // 包含 sort 函数

int main() {
    std::vector<int> vec = {5, 3, 1, 4, 2};

    // 对 vector 进行升序排序
    std::sort(vec.begin(), vec.end());

    // 输出排序后的 vector
    std::cout << "Sorted vec: ";
    for (int i : vec) {
        std::cout << i << " ";
    }
    return 0;
}
```

**执行结果：**

 

 

Sorted vec: 1 2 3 4 5

---

### 9. **二维 vector**
```plain
#include <iostream>
#include <vector>

int main() {
    // 创建一个 3x3 的二维 vector
    std::vector<std::vector<int>> vec = {
        {1, 2, 3},
        {4, 5, 6},
        {7, 8, 9}
    };

    // 访问二维 vector 的元素
    std::cout << "Element at (1, 2): " << vec[1][2] << std::endl;

    // 遍历二维 vector
    for (const auto& row : vec) {
        for (int val : row) {
            std::cout << val << " ";
        }
        std::cout << std::endl;
    }

    return 0;
}
```

**执行结果：**

 

 

```plain
Element at (1, 2): 6
1 2 3
4 5 6
7 8 9
```

---

### 10. **vector 的交换**
 

```plain
#include <iostream>
#include <vector>

int main() {
    std::vector<int> vec1 = {1, 2, 3};
    std::vector<int> vec2 = {4, 5, 6};

    // 交换两个 vector 的内容
    vec1.swap(vec2);

    // 输出交换后的结果
    std::cout << "vec1: ";
    for (int i : vec1) {
        std::cout << i << " ";
    }
    std::cout << "\nvec2: ";
    for (int i : vec2) {
        std::cout << i << " ";
    }
    return 0;
}
```

**执行结果：**

 

 

```plain
vec1: 4 5 6
vec2: 1 2 3
```

---

### 总结
`std::vector` 的常用操作包括：

+ 创建和初始化。
+ 添加、删除和修改元素。
+ 访问和遍历元素。
+ 获取大小和容量。
+ 排序和交换。

这些操作使 `std::vector` 成为 C++ 中最灵活和高效的容器之一。

 



> 更新: 2025-04-17 22:26:51  
> 原文: <https://www.yuque.com/linuxer/gscfv1/lk6v5g4x53ghy8l8>