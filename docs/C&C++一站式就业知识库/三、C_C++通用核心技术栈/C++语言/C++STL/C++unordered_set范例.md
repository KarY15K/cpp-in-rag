# C++ unordered_set范例

`unordered_set`是 C++ 标准库中的关联容器，用于存储唯一的元素，并且元素的存储是无序的。以下是一些`unordered_set`常用的范例及执行结果：

### 范例一：基本插入和遍历


 

 

```cpp
#include <iostream>
#include <unordered_set>

int main() {
    // 创建unordered_set
    unordered_set<int> mySet;

    // 插入元素
    mySet.insert(5);
    mySet.insert(3);
    mySet.insert(7);
    mySet.insert(5);  // 尝试插入重复元素

    // 遍历unordered_set
    for (const auto& element : mySet) {
        cout << element << " ";
    }
    cout << endl;

    return 0;
}
```

 



**执行结果**：  
执行结果可能是`3 7 5`，也可能是其他无序的排列。因为`unordered_set`中的元素是无序存储的，每次运行输出的顺序可能不同，但可以确定的是重复元素`5`只会出现一次。

 

### 范例二：查找元素
 

```cpp
#include <iostream>
#include <unordered_set>

int main() {
    unordered_set<int> mySet = {1, 2, 3, 4, 5};

    // 查找元素
    int target = 3;
    auto it = mySet.find(target);
    if (it!= mySet.end()) {
        cout << "元素 " << target << " 存在于unordered_set中" << endl;
    } else {
        cout << "元素 " << target << " 不存在于unordered_set中" << endl;
    }

    return 0;
}
```

 



**执行结果**：  
`元素3存在于unordered_set中`



在这个范例中，使用`find`函数查找元素`3`，如果找到，`find`函数返回指向该元素的迭代器；如果未找到，返回`unordered_set`的`end`迭代器。

 

### 范例三：删除元素
 

 

```cpp
#include <iostream>
#include <unordered_set>

int main() {
    unordered_set<int> mySet = {1, 2, 3, 4, 5};

    // 删除元素
    int elementToDelete = 4;
    mySet.remove(elementToDelete);

    // 输出删除元素后的unordered_set
    for (const auto& element : mySet) {
        cout << element << " ";
    }
    cout << endl;

    return 0;
}
```

 



**执行结果**：  
执行结果可能是`1 2 3 5`，也可能是其他无序的排列。可以确定的是元素`4`已被删除。

### 
### 范例四：获取元素个数和判断是否为空
 

```cpp
#include <iostream>
#include <unordered_set>

int main() {
    unordered_set<int> mySet;

    // 插入元素
    mySet.insert(1);
    mySet.insert(2);
    mySet.insert(3);

    // 获取元素个数
    cout << "unordered_set中元素的个数: " << mySet.size() << endl;

    // 判断是否为空
    if (mySet.empty()) {
        cout << "unordered_set为空" << endl;
    } else {
        cout << "unordered_set不为空" << endl;
    }

    return 0;
}
```

 



**执行结果**：

 

```plain
unordered_set中元素的个数: 3
unordered_set不为空
```



在这个范例中，使用`size`函数获取`unordered_set`中元素的个数，使用`empty`函数判断`unordered_set`是否为空。



> 更新: 2025-04-17 22:32:35  
> 原文: <https://www.yuque.com/linuxer/gscfv1/bzufi9aahyhz6vm8>