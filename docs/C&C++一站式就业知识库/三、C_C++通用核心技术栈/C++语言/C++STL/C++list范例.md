# C++ list范例

`std::list` 是 C++ STL 中的双向链表容器，支持高效的插入和删除操作，但不支持随机访问。以下是 `std::list` 的常用范例及其执行结果：

---

### 1. **创建和初始化 list**
 

```plain
#include <iostream>
#include <list>

int main() {
    // 创建一个空的 list
    std::list<int> list1;

    // 创建并初始化一个包含 5 个元素的 list，初始值为 0
    std::list<int> list2(5);

    // 创建并初始化一个包含 5 个元素的 list，初始值为 10
    std::list<int> list3(5, 10);

    // 使用初始化列表创建 list
    std::list<int> list4 = {1, 2, 3, 4, 5};

    // 输出 list4 的内容
    std::cout << "list4: ";
    for (int i : list4) {
        std::cout << i << " ";
    }
    return 0;
}
```

**执行结果：**

```plain
list4: 1 2 3 4 5
```

---

### 2. **添加元素到 list**
 

```plain
#include <iostream>
#include <list>

int main() {
    std::list<int> list;

    // 在尾部添加元素
    list.push_back(10);
    list.push_back(20);
    list.push_back(30);

    // 在头部添加元素
    list.push_front(5);
    list.push_front(1);

    // 使用 emplace_back 和 emplace_front 直接构造元素
    list.emplace_back(40);
    list.emplace_front(0);

    // 输出 list 内容
    std::cout << "list: ";
    for (int i : list) {
        std::cout << i << " ";
    }
    return 0;
}
```

**执行结果：**

```plain
list: 0 1 5 10 20 30 40
```

---

### 3. **访问 list 元素**
 

```plain
#include <iostream>
#include <list>

int main() {
    std::list<int> list = {1, 2, 3, 4, 5};

    // 访问第一个和最后一个元素
    std::cout << "First element: " << list.front() << std::endl;
    std::cout << "Last element: " << list.back() << std::endl;

    return 0;
}
```

**执行结果：**

```plain
First element: 1
Last element: 5
```

---

### 4. **遍历 list**
 

```plain
#include <iostream>
#include <list>

int main() {
    std::list<int> list = {1, 2, 3, 4, 5};

    // 使用范围 for 循环遍历
    std::cout << "Using range-based for loop: ";
    for (int i : list) {
        std::cout << i << " ";
    }
    std::cout << std::endl;

    // 使用迭代器遍历
    std::cout << "Using iterator: ";
    for (auto it = list.begin(); it != list.end(); ++it) {
        std::cout << *it << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

**执行结果：**

```plain
Using range-based for loop: 1 2 3 4 5
Using iterator: 1 2 3 4 5
```

---

### 5. **删除 list 元素**
 

```plain
#include <iostream>
#include <list>

int main() {
    std::list<int> list = {1, 2, 3, 4, 5};

    // 删除头部元素
    list.pop_front();

    // 删除尾部元素
    list.pop_back();

    // 删除指定位置的元素（删除第 2 个元素）
    auto it = list.begin();
    std::advance(it, 1);
    list.erase(it);

    // 输出剩余元素
    std::cout << "list after deletions: ";
    for (int i : list) {
        std::cout << i << " ";
    }
    return 0;
}
```

**执行结果：**

```plain
list after deletions: 2 4
```

---

### 6. **获取 list 的大小**
 

```plain
#include <iostream>
#include <list>

int main() {
    std::list<int> list = {1, 2, 3, 4, 5};

    // 获取元素个数
    std::cout << "Size: " << list.size() << std::endl;

    return 0;
}
```

**执行结果：**

```plain
Size: 5
```

---

### 7. **list 的排序**
 

```plain
#include <iostream>
#include <list>

int main() {
    std::list<int> list = {5, 3, 1, 4, 2};

    // 对 list 进行升序排序
    list.sort();

    // 输出排序后的 list
    std::cout << "Sorted list: ";
    for (int i : list) {
        std::cout << i << " ";
    }
    return 0;
}
```

**执行结果：**

```plain
Sorted list: 1 2 3 4 5
```

---

### 8. **list 的去重**
 

```plain
#include <iostream>
#include <list>

int main() {
    std::list<int> list = {1, 2, 2, 3, 3, 3, 4, 5};

    // 去重（需要先排序）
    list.sort();
    list.unique();

    // 输出去重后的 list
    std::cout << "Unique list: ";
    for (int i : list) {
        std::cout << i << " ";
    }
    return 0;
}
```

**执行结果：**

```plain
Unique list: 1 2 3 4 5
```

---

### 9. **list 的合并**
 

```plain
#include <iostream>
#include <list>

int main() {
    std::list<int> list1 = {1, 3, 5};
    std::list<int> list2 = {2, 4, 6};

    // 合并两个 list（需要先排序）
    list1.sort();
    list2.sort();
    list1.merge(list2);

    // 输出合并后的 list1
    std::cout << "Merged list: ";
    for (int i : list1) {
        std::cout << i << " ";
    }
    return 0;
}
```

**执行结果：**

```plain
Merged list: 1 2 3 4 5 6
```

---

### 10. **list 的交换**
 

```plain
#include <iostream>
#include <list>

int main() {
    std::list<int> list1 = {1, 2, 3};
    std::list<int> list2 = {4, 5, 6};

    // 交换两个 list 的内容
    list1.swap(list2);

    // 输出交换后的结果
    std::cout << "list1: ";
    for (int i : list1) {
        std::cout << i << " ";
    }
    std::cout << "\nlist2: ";
    for (int i : list2) {
        std::cout << i << " ";
    }
    return 0;
}
```

**执行结果：**

```plain
list1: 4 5 6
list2: 1 2 3
```

---

### 11. **list 的反转**
 

```plain
#include <iostream>
#include <list>

int main() {
    std::list<int> list = {1, 2, 3, 4, 5};

    // 反转 list
    list.reverse();

    // 输出反转后的 list
    std::cout << "Reversed list: ";
    for (int i : list) {
        std::cout << i << " ";
    }
    return 0;
}
```

**执行结果：**

```plain
Reversed list: 5 4 3 2 1
```

---

### 12. **list 的插入**
 

```plain
#include <iostream>
#include <list>

int main() {
    std::list<int> list = {1, 2, 3, 4, 5};

    // 在指定位置插入元素
    auto it = list.begin();
    std::advance(it, 2); // 将迭代器移动到第 3 个位置
    list.insert(it, 100);

    // 输出插入后的 list
    std::cout << "list after insertion: ";
    for (int i : list) {
        std::cout << i << " ";
    }
    return 0;
}
```

**执行结果：**

```plain
list after insertion: 1 2 100 3 4 5
```

---

### 总结
`std::list` 的常用操作包括：

+ `push_back()` 和 `push_front()`：在尾部或头部添加元素。
+ `pop_back()` 和 `pop_front()`：删除尾部或头部元素。
+ `emplace_back()` 和 `emplace_front()`：直接在尾部或头部构造元素。
+ `front()` 和 `back()`：访问第一个和最后一个元素。
+ `size()`：获取元素个数。
+ `empty()`：检查是否为空。
+ `insert()` 和 `erase()`：在指定位置插入或删除元素。
+ `sort()`、`unique()`、`merge()` 和 `reverse()`：排序、去重、合并和反转。

通过这些操作，可以高效地管理双向链表中的数据。





### 有哪些应用场景吗
### **13 去重和排序**
`std::list` 提供了 `sort()` 和 `unique()` 成员函数，可以方便地对元素进行排序和去重。

#### 应用场景：
+ **数据清洗**：在数据处理中，可能需要去除重复数据。
+ **排行榜**：在游戏或竞赛中，需要对玩家或队伍进行排序。

#### 示例：
 

```plain
#include <iostream>
#include <list>

int main() {
    std::list<int> scores = {85, 90, 78, 90, 85, 95};

    // 排序
    scores.sort();

    // 去重
    scores.unique();

    // 输出结果
    for (int score : scores) {
        std::cout << score << " ";
    }
    return 0;
}
```

**执行结果：**

```plain
78 85 90 95
```

---

### 14  **实现 LRU 缓存**
`std::list` 可以用于实现 LRU（最近最少使用）缓存算法，因为它在头部和尾部的插入和删除操作非常高效。

#### 应用场景：
+ **缓存系统**：在缓存系统中，需要淘汰最近最少使用的数据。

#### 示例：
 

```plain
#include <iostream>
#include <list>
#include <unordered_map>

class LRUCache {
private:
    int capacity;
    std::list<int> cacheList; // 存储键
    std::unordered_map<int, std::list<int>::iterator> cacheMap; // 键到迭代器的映射

public:
    LRUCache(int capacity) : capacity(capacity) {}

    int get(int key) {
        if (cacheMap.find(key) == cacheMap.end()) {
            return -1; // 未找到
        }
        // 将访问的键移动到链表头部
        cacheList.splice(cacheList.begin(), cacheList, cacheMap[key]);
        return *cacheMap[key];
    }

    void put(int key, int value) {
        if (cacheMap.find(key) != cacheMap.end()) {
            // 如果键已存在，更新值并移动到链表头部
            cacheList.splice(cacheList.begin(), cacheList, cacheMap[key]);
            *cacheMap[key] = value;
        } else {
            if (cacheList.size() == capacity) {
                // 删除链表尾部的键
                int lastKey = cacheList.back();
                cacheList.pop_back();
                cacheMap.erase(lastKey);
            }
            // 插入新键到链表头部
            cacheList.push_front(key);
            cacheMap[key] = cacheList.begin();
        }
    }
};

int main() {
    LRUCache cache(2);
    cache.put(1, 1);
    cache.put(2, 2);
    std::cout << cache.get(1) << " "; // 返回 1
    cache.put(3, 3); // 淘汰键 2
    std::cout << cache.get(2) << " "; // 返回 -1
    cache.put(4, 4); // 淘汰键 1
    std::cout << cache.get(1) << " "; // 返回 -1
    std::cout << cache.get(3) << " "; // 返回 3
    std::cout << cache.get(4) << " "; // 返回 4
    return 0;
}
```

**执行结果：**

1 -1 -1 3 4



> 更新: 2025-04-17 22:27:52  
> 原文: <https://www.yuque.com/linuxer/gscfv1/eotoo7spa193nl6z>