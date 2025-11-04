# C++ unordered_map范例

以下是一些 C++ 中`unordered_map`常用的范例以及对应的执行结果：

### 范例一：基本的插入与遍历
 

 

```cpp
#include <iostream>
#include <unordered_map>
#include <string>

int main() {
    // 创建unordered_map，键为字符串，值为整数
    std::unordered_map<std::string, int> myMap;

    // 插入元素
    myMap["apple"] = 5;
    myMap["banana"] = 3;
    myMap["cherry"] = 7;

    // 遍历unordered_map
    for (const auto& pair : myMap) {
        std::cout << "键: " << pair.first << ", 值: " << pair.second << std::endl;
    }

    return 0;
}
```

 



**执行结果**：  
输出结果的顺序是无序的，可能类似如下形式（每次运行顺序可能不同）：

 

 

```plain
键: cherry, 值: 7
键: apple, 值: 5
键: banana, 值: 3
```



因为`unordered_map`基于哈希表实现，元素存储顺序取决于哈希函数的计算结果，所以遍历输出时顺序不固定。

 

### 范例二：查找元素
 

 

```cpp
#include <iostream>
#include <unordered_map>
#include <string>

int main() {
    std::unordered_map<std::string, int> myMap;
    myMap["apple"] = 5;
    myMap["banana"] = 3;
    myMap["cherry"] = 7;

    // 查找元素
    std::string targetKey = "banana";
    auto it = myMap.find(targetKey);
    if (it!= myMap.end()) {
        std::cout << "找到键为 " << targetKey << " 的元素，值为: " << it->second << std::endl;
    } else {
        std::cout << "未找到键为 " << targetKey << " 的元素" << std::endl;
    }

    return 0;
}
```

 



**执行结果**：

 

 

```plain
找到键为banana的元素，值为: 3
```



通过`find`函数查找指定键的元素，如果找到则返回指向该元素的迭代器，可通过迭代器访问对应的值；若没找到则返回`unordered_map`的`end`迭代器。

 

### 范例三：修改元素的值
 

 

```cpp
#include <iostream>
#include <unordered_map>
#include <string>

int main() {
    std::unordered_map<std::string, int> myMap;
    myMap["apple"] = 5;
    myMap["banana"] = 3;
    myMap["cherry"] = 7;

    // 修改元素的值
    std::string keyToModify = "apple";
    myMap[keyToModify] = 10;

    // 输出修改后的值
    std::cout << "键为 " << keyToModify << " 的元素，修改后的值为: " << myMap[keyToModify] << std::endl;

    return 0;
}
```

 



**执行结果**：

 

 

```plain
键为apple的元素，修改后的值为: 10
```



直接通过键来访问元素，就像访问数组元素一样，然后可以对其值进行修改操作，这里将键为`apple`的元素值从`5`修改为了`10`。

 

### 范例四：删除元素
 

 

```cpp
#include <iostream>
#include <unordered_map>
#include <string>

int main() {
    std::unordered_map<std::string, int> myMap;
    myMap["apple"] = 5;
    myMap["banana"] = 3;
    myMap["cherry"] = 7;

    // 删除元素
    std::string keyToDelete = "banana";
    myMap.erase(keyToDelete);

    // 遍历删除元素后的unordered_map
    for (const auto& pair : myMap) {
        std::cout << "键: " << pair.first << ", 值: " << pair.second << std::endl;
    }

    return 0;
}
```



**执行结果**：  
输出结果顺序是无序的，类似如下形式（每次运行顺序可能不同）：

 

 

```plain
键: cherry, 值: 7
键: apple, 值: 5
```



使用`erase`函数根据键来删除对应的元素，这里删除了键为`banana`的元素，然后遍历输出剩余元素。

 

### 范例五：统计元素个数和判断是否为空
 

 

```cpp
#include <iostream>
#include <unordered_map>
#include <string>

int main() {
    std::unordered_map<std::string, int> myMap;
    myMap["apple"] = 5;
    myMap["banana"] = 3;
    myMap["cherry"] = 7;

    // 统计元素个数
    std::cout << "unordered_map中元素的个数: " << myMap.size() << std::endl;

    // 判断是否为空
    if (myMap.empty()) {
        std::cout << "unordered_map为空" << std::endl;
    } else {
        std::cout << "unordered_map不为空" << std::endl;
    }

    return 0;
}
```

 



**执行结果**：

 

 

```plain
unordered_map中元素的个数: 3
unordered_map不为空
```



通过`size`函数获取`unordered_map`中元素的个数，使用`empty`函数判断其是否为空。



> 更新: 2025-04-17 22:33:33  
> 原文: <https://www.yuque.com/linuxer/gscfv1/uf7puoe5cpkk04um>