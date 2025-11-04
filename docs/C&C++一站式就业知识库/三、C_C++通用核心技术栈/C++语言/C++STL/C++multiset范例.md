# C++  multiset范例

`std::map` 的最常用范例，包含插入、遍历、查找、修改、删除等操作，并附上执行结果。

---

### **范例代码**
 

```plain
#include <iostream>
#include <map> // map 头文件

int main() {
    // 创建一个 map
    std::map<std::string, int> myMap;

    // 插入键值对
    myMap["Alice"]   = 25;
    myMap["Bob"]     = 30;
    myMap["Charlie"] = 35;
    myMap.insert({"David", 40}); // 另一种插入方式

    // 遍历 map
    std::cout << "Map elements: " << std::endl;
    for (const auto& pair : myMap) {
        std::cout << "  " << pair.first << ": " << pair.second << std::endl;
    }

    // 查找元素
    auto it = myMap.find("Bob");
    if (it != myMap.end()) {
        std::cout << "\nFound Bob, age: " << it->second << std::endl;
    } else {
        std::cout << "\nBob not found!" << std::endl;
    }

    // 修改元素
    myMap["Alice"] = 26; // 修改 Alice 的年龄
    std::cout << "\nAlice's new age: " << myMap["Alice"] << std::endl;

    // 删除元素
    myMap.erase("Charlie"); // 删除键为 "Charlie" 的键值对

    // 再次遍历 map
    std::cout << "\nMap elements after erasing Charlie: " << std::endl;
    for (const auto& pair : myMap) {
        std::cout << "  " << pair.first << ": " << pair.second << std::endl;
    }

    // 检查键是否存在
    if (myMap.count("David") > 0) {
        std::cout << "\nDavid exists in the map!" << std::endl;
    } else {
        std::cout << "\nDavid does not exist in the map!" << std::endl;
    }

    return 0;
}
```

---

### **执行结果**
运行上述代码后，输出如下：

 

 

```plain
Map elements:
  Alice: 25
  Bob: 30
  Charlie: 35
  David: 40

Found Bob, age: 30

Alice's new age: 26

Map elements after erasing Charlie:
  Alice: 26
  Bob: 30
  David: 40

David exists in the map!
```

---

### **代码解析**
1. **插入键值对**：
    - 使用 `[]` 运算符或 `insert()` 方法插入键值对。
    - 例如：`myMap["Alice"] = 25` 或 `myMap.insert({"David", 40})`。
2. **遍历 map**：
    - 使用范围 for 循环遍历 `map`，输出键值对。
    - 键值对按升序排列（默认按键的字典序）。
3. **查找元素**：
    - 使用 `find()` 方法查找键，返回指向该键值对的迭代器。
    - 如果未找到，返回 `end()`。
4. **修改元素**：
    - 使用 `[]` 运算符直接修改键对应的值。
    - 例如：`myMap["Alice"] = 26`。
5. **删除元素**：
    - 使用 `erase()` 方法删除指定键的键值对。
    - 例如：`myMap.erase("Charlie")`。
6. **检查键是否存在**：
    - 使用 `count()` 方法检查键是否存在。如果存在，返回 `1`；否则返回 `0`。

---

### **常用场景**
`std::map` 的常见使用场景包括：

1. **存储键值对并按键排序**：
    - 例如，存储学生姓名和年龄，按姓名排序。
2. **快速查找键对应的值**：
    - 由于 `map` 基于红黑树实现，查找操作的时间复杂度为 �(log⁡�)O(logn)。
3. **统计键的出现次数**：
    - 例如，统计单词在文本中出现的次数。
4. **去重并排序**：
    - 例如，将一组数据去重后按升序排列。
5. **实现字典或映射关系**：
    - 例如，将国家名称映射到首都。

---

### **总结**
`std::map` 是一个功能强大的容器，适用于需要存储键值对并按键排序的场景。通过这个范例，你可以掌握以下操作：

+ 插入键值对。
+ 遍历键值对。
+ 查找键对应的值。
+ 修改键对应的值。
+ 删除键值对。
+ 检查键是否存在。

这些操作是 `std::map` 最常用的功能，能够满足大多数实际需求。



### 怎么用map实现统计键的出现次数
使用 `std::map` 可以很方便地统计键的出现次数。`map` 的键是唯一的，而值可以存储该键的出现次数。每次遇到一个键时，将其对应的值加 1 即可。

---

### **实现代码**
以下是一个统计字符串中每个单词出现次数的示例：

 

```plain
#include <iostream>
#include <map>
#include <vector>
#include <string>

int main() {
    // 输入数据：一组单词
    std::vector<std::string> words = {"apple", "banana", "apple", "orange", "banana", "apple"};

    // 创建一个 map 来统计单词出现次数
    std::map<std::string, int> wordCount;

    // 遍历单词列表，统计每个单词的出现次数
    for (const auto& word : words) {
        wordCount[word]++; // 如果 word 不存在，会自动插入并初始化为 0，然后加 1
    }

    // 输出统计结果
    std::cout << "Word count statistics: " << std::endl;
    for (const auto& pair : wordCount) {
        std::cout << "  " << pair.first << ": " << pair.second << std::endl;
    }

    return 0;
}
```

---

### **执行结果**
运行上述代码后，输出如下：

```plain
Word count statistics:
  apple: 3
  banana: 2
  orange: 1
```

---

### **代码解析**
1. **输入数据**：
    - 使用 `std::vector<std::string>` 存储一组单词。
2. **创建 map**：
    - 使用 `std::map<std::string, int>` 来存储每个单词及其出现次数。
3. **统计出现次数**：
    - 遍历单词列表，对每个单词执行 `wordCount[word]++`。
    - 如果 `word` 不存在于 `map` 中，`wordCount[word]` 会自动插入并初始化为 `0`，然后加 `1`。
    - 如果 `word` 已存在，则直接将其对应的值加 `1`。
4. **输出结果**：
    - 遍历 `map`，输出每个单词及其出现次数。

---

### 
 

 

 



> 更新: 2025-04-17 22:31:06  
> 原文: <https://www.yuque.com/linuxer/gscfv1/ebn2g5dnot7a1czl>