# C++ multimap常用范例

`C++`中`multimap`常用的范例，展示了其基本的使用方法以及对应的执行结果说明。

### 范例一：简单的插入与遍历
 

```cpp
#include <iostream>
#include <map>
using namespace std;

int main() {
    multimap<int, string> myMultimap;

    // 插入元素
    myMultimap.insert(make_pair(1, "Apple"));
    myMultimap.insert(make_pair(2, "Banana"));
    myMultimap.insert(make_pair(1, "Cherry"));

    // 遍历 multimap
    for (auto it = myMultimap.begin(); it!= myMultimap.end(); ++it) {
        cout << "Key: " << it->first << ", Value: " << it->second << endl;
    }

    return 0;
}
```

 



**执行结果**：

 

```plain
Key: 1, Value: Apple
Key: 1, Value: Cherry
Key: 2, Value: Banana
```

在这个范例中，`multimap`允许有重复的键（这里键为`1`出现了两次），遍历的时候按照键从小到大的顺序输出，对应的值也依次输出。

 

### 范例二：基于键查找元素（返回范围）
 

```cpp
#include <iostream>
#include <map>
using namespace std;

int main() {
    multimap<int, string> myMultimap;
    myMultimap.insert(make_pair(1, "Apple"));
    myMultimap.insert(make_pair(2, "Banana"));
    myMultimap.insert(make_pair(1, "Cherry"));

    auto range = myMultimap.equal_range(1);
    for (auto it = range.first; it!= range.second; ++it) {
        cout << "Key: " << it->first << ", Value: " << it->second << endl;
    }

    return 0;
}
```



**执行结果**：

 

```plain
Key: 1, Value: Apple
Key: 1, Value: Cherry
```



这里通过`equal_range`函数，根据给定的键（这里是`1`），它返回一个包含所有匹配该键的元素的范围（是一对迭代器），然后遍历这个范围就可以输出对应键的所有值。

 

### 范例三：删除指定键的元素
 

```cpp
#include <iostream>
#include <map>
using namespace std;

int main() {
    multimap<int, string> myMultimap;
    myMultimap.insert(make_pair(1, "Apple"));
    myMultimap.insert(make_pair(2, "Banana"));
    myMultimap.insert(make_pair(1, "Cherry"));

    int keyToDelete = 1;
    auto range = myMultimap.equal_range(keyToDelete);
    myMultimap.erase(keyToDelete);

    for (auto it = myMultimap.begin(); it!= myMultimap.end(); ++it) {
        cout << "Key: " << it->first << ", Value: " << it->second << endl;
    }

    return 0;
}
```

 



**执行结果**：

 

```plain
Key: 2, Value: Banana
```



在这个例子中，先通过`equal_range`获取要删除键对应的元素范围，然后使用`erase`函数（传入键或者范围都可以删除元素），这里传入键`1`就把所有键为`1`的元素都删除了，最后遍历剩下的元素输出结果，只剩下键为`2`的元素了。

### 范例四：统计每个键出现的次数
 

```cpp
#include <iostream>
#include <map>
using namespace std;

int main() {
    multimap<int, string> myMultimap;
    myMultimap.insert(make_pair(1, "Apple"));
    myMultimap.insert(make_pair(2, "Banana"));
    myMultimap.insert(make_pair(1, "Cherry"));
    myMultimap.insert(make_pair(3, "Date"));
    myMultimap.insert(make_pair(3, "Elder"));

    map<int, int> keyCount;
    for (auto it = myMultimap.begin(); it!= myMultimap.end(); ++it) {
        keyCount[it->first]++;
    }

    for (auto it = keyCount.begin(); it!= keyCount.end(); ++it) {
        cout << "Key: " << it->first << " appears " << it->second << " times." << endl;
    }

    return 0;
}
```



**执行结果**：

 

 

```plain
Key: 1 appears 2 times.
Key: 2 appears 1 times.
Key: 3 appears 2 times.
```



这个范例中，先定义了一个普通的`map`用于统计每个键出现的次数。然后遍历`multimap`，每遇到一个键就在对应的统计`map`中把该键的计数加`1`，最后遍历统计`map`输出每个键出现的次数情况。



希望这些范例能帮助你更好地理解和掌握`C++`中`multimap`的使用方法。



> 更新: 2025-04-17 22:32:17  
> 原文: <https://www.yuque.com/linuxer/gscfv1/qgyxmyosr063t613>