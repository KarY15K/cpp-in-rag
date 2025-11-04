# C++ unordered_multiset范例

以下是几个关于`C++`中`unordered_multiset`的常用范例以及对应的执行结果说明：

### 范例一：基本的插入与遍历
 

```cpp
#include <iostream>
#include <unordered_set>

int main() {
    std::unordered_multiset<int> mySet;
    // 插入元素
    mySet.insert(5);
    mySet.insert(3);
    mySet.insert(7);
    mySet.insert(5);  // 插入重复元素

    // 遍历unordered_multiset
    for (const auto& element : mySet) {
        std::cout << element << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

 



**执行结果**：  
输出结果可能是无序的，类似 `5 5 3 7` 或者其他包含这几个数字且无序的排列形式。因为 `unordered_multiset` 是无序存储元素的，并且允许存在重复元素，所以这里插入的两个 `5` 都会被存储并输出。

### 范例二：统计元素出现的次数
 

```cpp
#include <iostream>
#include <unordered_set>

int main() {
    std::unordered_multiset<int> mySet;
    mySet.insert(1);
    mySet.insert(2);
    mySet.insert(1);
    mySet.insert(3);
    mySet.insert(2);

    // 统计元素出现的次数
    for (int num : {1, 2, 3}) {
        std::cout << num << " 出现的次数为: " << mySet.count(num) << std::endl;
    }

    return 0;
}
```

 



**执行结果**：

 

 

```plain
1 出现的次数为: 2
2 出现的次数为: 2
3 出现的次数为: 1
```



在这个范例中，通过 `count` 函数可以统计 `unordered_multiset` 中指定元素出现的次数，这里分别统计了 `1`、`2`、`3` 出现的频次并输出。

### 范例三：基于范围的查找（equal_range）
 

 

```cpp
#include <iostream>
#include <unordered_set>

int main() {
    std::unordered_multiset<int> mySet;
    mySet.insert(1);
    mySet.insert(2);
    mySet.insert(1);
    mySet.insert(3);

    auto range = mySet.equal_range(1);
    std::cout << "键为1的元素有: ";
    for (auto it = range.first; it!= range.second; ++it) {
        std::cout << *it << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

 



**执行结果**：

 

 

```plain
键为1的元素有: 1 1
```



`equal_range` 函数会返回一个包含所有匹配给定元素的范围（由一对迭代器表示），这里查找键为 `1` 的元素范围，然后遍历该范围输出对应的元素，展示了 `unordered_multiset` 中重复元素的查找方式。

 

### 范例四：删除指定元素
 

 

```cpp
#include <iostream>
#include <unordered_set>

int main() {
    std::unordered_multiset<int> mySet;
    mySet.insert(1);
    mySet.insert(2);
    mySet.insert(1);
    mySet.insert(3);

    mySet.erase(1);
    for (const auto& element : mySet) {
        std::cout << element << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

 



**执行结果**：  
输出结果可能是无序的，类似 `2 3` 或者其他排列形式。这里使用 `erase` 函数删除所有值为 `1` 的元素，之后遍历剩余元素进行输出，展示了如何移除 `unordered_multiset` 中的元素。



`unordered_multiset` 结合了无序存储以及允许重复元素这两个特点，在很多需要处理有重复元素且对顺序要求不高的场景中比较实用。



> 更新: 2025-04-17 22:33:15  
> 原文: <https://www.yuque.com/linuxer/gscfv1/wu7td1aammsebs7k>