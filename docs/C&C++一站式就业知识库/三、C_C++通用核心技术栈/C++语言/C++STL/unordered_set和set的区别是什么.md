# unordered_set和set的区别是什么

`std::unordered_set` 和 `std::set` 是 C++ 标准库中的两种关联容器，它们都用于存储唯一的元素，但在底层实现、性能特性和使用场景上有显著区别。以下是它们的详细对比：

---

### **1. 底层实现**
+ `**std::set**`：
    - 基于红黑树（一种平衡二叉搜索树）实现。
    - 元素按升序排序（默认按 `<` 运算符排序，或自定义比较函数）。
    - 插入、删除和查找操作的时间复杂度为 O(log n)。
+ `**std::unordered_set**`：
    - 基于哈希表实现。
    - 元素不按任何顺序存储，顺序取决于哈希函数和桶的分布。
    - 插入、删除和查找操作的平均时间复杂度为 O(1)，最坏情况下为 O(n)（哈希冲突严重时）。

---

### **2. 元素顺序**
+ `**std::set**`：
    - 元素按升序排序（默认按 `<` 运算符排序）。
    - 遍历时元素是有序的。
+ `**std::unordered_set**`：
    - 元素无序存储。
    - 遍历时元素的顺序是未定义的，可能每次运行都不同。

---

### **3. 性能**
+ `**std::set**`：
    - 适合需要有序存储的场景。
    - 插入、删除和查找操作的时间复杂度为O(logn)。
    - 由于基于红黑树，内存开销较大。
+ `**std::unordered_set**`：
    - 适合需要快速查找的场景。
    - 插入、删除和查找操作的平均时间复杂度为 O(1)。
    - 由于基于哈希表，内存开销较小，但哈希冲突可能影响性能。

---

### **4. 自定义排序或哈希函数**
+ `**std::set**`：
    - 可以通过自定义比较函数实现自定义排序。

例如：

    -  std::set<int, std::greater<int>> mySet; // 按降序排序
+ `**std::unordered_set**`：
    - 可以通过自定义哈希函数和相等比较函数实现自定义哈希行为。

例如：

    -  

```plain
struct MyHash {
    size_t operator()(const std::string& key) const {
        return key.length(); // 自定义哈希函数
    }
};
std::unordered_set<std::string, MyHash> mySet;
```

---

### **5. 使用场景**
+ `**std::set**`：
    - 需要元素有序存储的场景。
    - 需要频繁进行范围查找（如查找大于某个值的元素）。
    - 例如：存储学生成绩并按成绩排序。
+ `**std::unordered_set**`：
    - 需要快速查找、插入和删除的场景。
    - 不需要元素有序存储。
    - 例如：去重、缓存、快速查找是否存在某个元素。

---

### **6. 示例对比**
#### `**std::set**`** 示例**
```plain
#include <iostream>
#include <set>

int main() {
    std::set<int> mySet = {5, 2, 8, 2, 1}; // 插入元素

    std::cout << "Set elements (sorted): ";
    for (const auto& elem : mySet) {
        std::cout << elem << " "; // 输出有序元素
    }
    std::cout << std::endl;

    return 0;
}
```

**输出**：

```plain
Set elements (sorted): 1 2 5 8
```

#### `**std::unordered_set**`** 示例**
```plain
#include <iostream>
#include <unordered_set>

int main() {
    std::unordered_set<int> mySet = {5, 2, 8, 2, 1}; // 插入元素

    std::cout << "Unordered_set elements: ";
    for (const auto& elem : mySet) {
        std::cout << elem << " "; // 输出无序元素
    }
    std::cout << std::endl;

    return 0;
}
```

**输出**（可能每次运行不同）：

```plain
Unordered_set elements: 1 8 2 5
```

---

### **7. 总结对比**
| 特性 | `std::set` | `std::unordered_set` |
| --- | --- | --- |
| **底层实现** | 红黑树 | 哈希表 |
| **元素顺序** | 有序（默认升序） | 无序 |
| **插入/删除/查找复杂度** | O(logn) | 平均O(1)，最坏O(n) |
| **内存开销** | 较大 | 较小 |
| **自定义排序/哈希** | 支持自定义比较函数 | 支持自定义哈希函数和相等比较函数 |
| **适用场景** | 需要**有****<font style="color:rgb(0, 0, 0);">序存储</font>**或范围查找 | 需要快速查找、插入和删除 |


---

### **选择建议**
+ 如果需要元素**<font style="color:rgb(0, 0, 0);background-color:rgb(251, 238, 184);">有序存储或进行范围查找</font>**，选择 `std::set`。
+ 如果需要快速查找、插入和删除，且不关心元素顺序，选择 `std::unordered_set`。

根据具体需求选择合适的容器，可以显著提高程序的性能和效率。



> 更新: 2025-04-17 22:32:56  
> 原文: <https://www.yuque.com/linuxer/gscfv1/rvksgqvqgv8oixkp>