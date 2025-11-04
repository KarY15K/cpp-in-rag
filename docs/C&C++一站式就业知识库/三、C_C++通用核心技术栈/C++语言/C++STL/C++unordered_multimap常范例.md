# C++ unordered_multimap常范例

`C++`中`unordered_multimap`的常用范例以及对应的执行结果：

### 范例一：基本插入与遍历
 

```cpp
#include <iostream>
#include <unordered_map>
#include <string>

int main() {
    std::unordered_multimap<std::string, int> myMultimap;

    // 插入元素
    myMultimap.insert(std::make_pair("apple", 5));
    myMultimap.insert(std::make_pair("banana", 3));
    myMultimap.insert(std::make_pair("apple", 7));

    // 遍历unordered_multimap
    for (const auto& element : myMultimap) {
        std::cout << "键: " << element.first << ", 值: " << element.second << std::endl;
    }

    return 0;
}
```

 



**执行结果**：  
输出的顺序是无序的，并且由于允许重复键，类似如下形式（每次运行顺序可能不同）：

 

```plain
键: apple, 值: 5
键: apple, 值: 7
键: banana, 值: 3
```



因为`unordered_multimap`基于哈希表实现，其元素存储顺序不定，且它能存储多个相同键的不同值的元素，这里就展示了键为`apple`有两个对应不同值的情况。

 

### 范例二：基于键查找元素（使用 equal_range）
```cpp
#include <iostream>
#include <unordered_map>
#include <string>

int main() {
    std::unordered_multimap<std::string, int> myMultimap;
    myMultimap.insert(std::make_pair("apple", 5));
    myMultimap.insert(std::make_pair("banana", 3));
    myMultimap.insert(std::make_pair("apple", 7));

    // 查找键为"apple"的元素范围
    auto range = myMultimap.equal_range("apple");
    std::cout << "键为apple的元素有: ";
    for (auto it = range.first; it!= range.second; ++it) {
        std::cout << it->second << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

 



**执行结果**：

 

```plain
键为apple的元素有: 5 7
```



`equal_range`函数会返回一个包含所有匹配给定键的元素的范围（由一对迭代器表示），这里查找键为`apple`的元素范围，然后遍历该范围输出对应的值，展示了如何获取重复键对应的多个值。

 

### 范例三：删除指定键的所有元素
```cpp
#include <iostream>
#include <unordered_map>
#include <string>

int main() {
    std::unordered_multimap<std::string, int> myMultimap;
    myMultimap.insert(std::make_pair("apple", 5));
    myMultimap.insert(std::make_pair("banana", 3));
    myMultimap.insert(std::make_pair("apple", 7));

    // 删除键为"apple"的所有元素
    myMultimap.erase("apple");

    // 遍历删除元素后的unordered_multimap
    for (const auto& element : myMultimap) {
        std::cout << "键: " << element.first << ", 值: " << element.second << std::endl;
    }

    return 0;
}
```

 



**执行结果**：

```plain
键: banana, 值: 3
```



使用`erase`函数传入键，可以删除所有该键对应的元素，这里删除了键为`apple`的所有元素，然后遍历输出剩余元素，展示了删除操作的用法。

 

### 范例四：统计每个键对应的元素个数


```cpp
#include <iostream>
#include <unordered_map>
#include <string>

int main() {
    std::unordered_multimap<std::string, int> myMultimap;
    myMultimap.insert(std::make_pair("apple", 5));
    myMultimap.insert(std::make_pair("banana", 3));
    myMultimap.insert(std::make_pair("apple", 7));
    myMultimap.insert(std::make_pair("cherry", 9));
    myMultimap.insert(std::make_pair("cherry", 11));

    // 统计每个键对应的元素个数
    std::unordered_map<std::string, int> keyCount;
    for (const auto& element : myMultimap) {
        keyCount[element.first]++;
    }

    for (const auto& pair : keyCount) {
        std::cout << "键: " << pair.first << " 对应的元素个数: " << pair.second << std::endl;
    }

    return 0;
}
```

 

**执行结果**：

 

```plain
键: apple对应的元素个数: 2
键: banana对应的元素个数: 1
键: cherry对应的元素个数: 2
```



首先定义了一个普通的`unordered_map`用于统计每个键对应的元素个数，然后遍历`unordered_multimap`，每遇到一个键就在统计的`unordered_map`中把该键的计数加`1`，最后遍历统计用的`unordered_map`输出每个键对应的元素个数情况，展示了对键出现频次的统计方法。



> 更新: 2025-04-17 22:36:51  
> 原文: <https://www.yuque.com/linuxer/gscfv1/gd4fzm2aeuev8mnd>