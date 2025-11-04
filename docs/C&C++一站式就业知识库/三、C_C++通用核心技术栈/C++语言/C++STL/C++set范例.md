# C++ set范例

`std::set` 是 C++ STL 中的一种关联容器，基于红黑树实现，存储唯一且有序的元素。以下是 `std::set` 的常用范例及其执行结果：

---

### 1. **创建和初始化 set**
 

```plain
#include <iostream>
#include <set>

int main() {
    // 创建一个空的 set
    std::set<int> set1;

    // 使用初始化列表创建 set
    std::set<int> set2 = {3, 1, 4, 1, 5, 9};

    // 输出 set2 的内容
    std::cout << "set2: ";
    for (int i : set2) {
        std::cout << i << " ";
    }
    return 0;
}
```

**执行结果：**

 

 

set2: 1 3 4 5 9

---

### 2. **添加元素到 set**
 

```plain
#include <iostream>
#include <set>

int main() {
    std::set<int> set;

    // 使用 insert 添加元素
    set.insert(10);
    set.insert(30);
    set.insert(20);

    // 输出 set 的内容
    std::cout << "set: ";
    for (int i : set) {
        std::cout << i << " ";
    }
    return 0;
}
```

**执行结果：**

 

 

set: 10 20 30

---

### 3. **访问 set 的元素**
`std::set` 不支持直接通过下标访问元素，但可以通过迭代器访问。

#### 示例：
 

```plain
#include <iostream>
#include <set>

int main() {
    std::set<int> set = {10, 20, 30};

    // 使用迭代器访问元素
    auto it = set.begin();
    std::advance(it, 1); // 移动到第 2 个元素
    std::cout << "Second element: " << *it << std::endl;

    return 0;
}
```

**执行结果：**

 

 

Second element: 20

---

### 4. **删除 set 的元素**
 

```plain
#include <iostream>
#include <set>

int main() {
    std::set<int> set = {10, 20, 30};

    // 删除指定元素
    set.erase(20);

    // 输出剩余元素
    std::cout << "set after erase: ";
    for (int i : set) {
        std::cout << i << " ";
    }
    return 0;
}
```

**执行结果：**

 

 

set after erase: 10 30

---

### 5. **查找 set 中的元素**
 

```plain
#include <iostream>
#include <set>

int main() {
    std::set<int> set = {10, 20, 30};

    // 查找元素
    auto it = set.find(20);
    if (it != set.end()) {
        std::cout << "Element 20 found!" << std::endl;
    } else {
        std::cout << "Element 20 not found!" << std::endl;
    }

    return 0;
}
```

**执行结果：**

 

 

Element 20 found!

---

### 6. **获取 set 的大小**
 

```plain
#include <iostream>
#include <set>

int main() {
    std::set<int> set = {10, 20, 30};

    // 获取 set 的大小
    std::cout << "set size: " << set.size() << std::endl;

    return 0;
}
```

**执行结果：**

 

 

set size: 3

---

### 7. **检查 set 是否为空**
 

```plain
#include <iostream>
#include <set>

int main() {
    std::set<int> set;

    // 检查 set 是否为空
    if (set.empty()) {
        std::cout << "set is empty!" << std::endl;
    }

    // 添加元素
    set.insert(10);

    // 再次检查
    if (!set.empty()) {
        std::cout << "set is not empty!" << std::endl;
    }

    return 0;
}
```

**执行结果：**

 

 

```plain
set is empty!
set is not empty!
```

---

### 8. **自定义比较函数**
默认情况下，`std::set` 按升序排序。可以通过自定义比较函数实现降序排序。

#### 示例：
 

```plain
#include <iostream>
#include <set>

int main() {
    // 使用 greater<int> 实现降序排序
    std::set<int, std::greater<int>> set = {10, 20, 30};

    // 输出 set 的内容
    std::cout << "set (descending): ";
    for (int i : set) {
        std::cout << i << " ";
    }
    return 0;
}
```

**执行结果：**

 

 

set (descending): 30 20 10

---

### 9. **使用自定义数据结构**
`std::set` 可以存储自定义数据结构，但需要定义比较函数。

#### 示例：
 

```plain
#include <iostream>
#include <set>
#include <string>

struct Person {
    std::string name;
    int age;

    // 重载 < 运算符，用于比较
    bool operator<(const Person& other) const {
        return age < other.age; // 按年龄升序排序
    }
};

int main() {
    std::set<Person> set;

    // 添加元素
    set.insert({"Alice", 30});
    set.insert({"Bob", 20});
    set.insert({"Charlie", 25});

    // 输出 set 的内容
    std::cout << "People sorted by age:\n";
    for (const auto& person : set) {
        std::cout << person.name << " (" << person.age << ")\n";
    }
    return 0;
}
```

**执行结果：**

 

 

```plain
People sorted by age:
Bob (20)
Charlie (25)
Alice (30)
```

---

### 10. **合并两个 set**
 

```plain
#include <iostream>
#include <set>

int main() {
    std::set<int> set1 = {1, 2, 3};
    std::set<int> set2 = {3, 4, 5};

    // 合并两个 set
    set1.insert(set2.begin(), set2.end());

    // 输出合并后的 set1
    std::cout << "Merged set: ";
    for (int i : set1) {
        std::cout << i << " ";
    }
    return 0;
}
```

**执行结果：**

 

 

Merged set: 1 2 3 4 5

---

### 11. **查找 set 的上下界**
`std::set` 提供了 `lower_bound()` 和 `upper_bound()` 方法，用于查找元素的上下界。

#### 示例：
 

```plain
#include <iostream>
#include <set>

int main() {
    std::set<int> set = {10, 20, 30, 40, 50};

    // 查找下界（第一个 >= 25 的元素）
    auto lb = set.lower_bound(25);
    std::cout << "Lower bound of 25: " << *lb << std::endl;

    // 查找上界（第一个 > 30 的元素）
    auto ub = set.upper_bound(30);
    std::cout << "Upper bound of 30: " << *ub << std::endl;

    return 0;
}
```

**执行结果：**

 

 

```plain
Lower bound of 25: 30
Upper bound of 30: 40
```

---

### 总结
`std::set` 的常用操作包括：

+ 创建和初始化。
+ 添加、删除和查找元素。
+ 遍历元素。
+ 自定义排序规则。
+ 合并集合。
+ 查找上下界。

`std::set` 适用于需要存储唯一且有序元素的场景，如去重、排序、范围查询等。



> 更新: 2025-04-17 22:30:27  
> 原文: <https://www.yuque.com/linuxer/gscfv1/ld8dxgi0nwkhemid>