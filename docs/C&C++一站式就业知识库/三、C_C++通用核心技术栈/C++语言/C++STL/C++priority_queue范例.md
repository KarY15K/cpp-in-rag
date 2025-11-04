# C++ priority_queue范例

`std::priority_queue` 是 C++ STL 中的一种容器适配器，基于堆（通常是最大堆）实现，用于管理按优先级排序的元素。以下是 `std::priority_queue` 的常用范例及其执行结果：

---

### 1. **创建和初始化 priority_queue**
 

```plain
#include <iostream>
#include <queue>

int main() {
    // 创建一个空的 priority_queue（默认是最大堆）
    std::priority_queue<int> pq1;

    // 使用 vector 初始化 priority_queue
    std::vector<int> vec = {3, 1, 4, 1, 5, 9};
    std::priority_queue<int> pq2(vec.begin(), vec.end());

    // 输出 pq2 的内容
    std::cout << "pq2: ";
    while (!pq2.empty()) {
        std::cout << pq2.top() << " "; // 访问堆顶元素
        pq2.pop(); // 弹出堆顶元素
    }
    return 0;
}
```

**执行结果：**

 

 

pq2: 9 5 4 3 1 1

---

### 2. **添加元素到 priority_queue**
 

```plain
#include <iostream>
#include <queue>

int main() {
    std::priority_queue<int> pq;

    // 使用 push 添加元素
    pq.push(10);
    pq.push(30);
    pq.push(20);

    // 输出 priority_queue 的内容
    std::cout << "pq: ";
    while (!pq.empty()) {
        std::cout << pq.top() << " ";
        pq.pop();
    }
    return 0;
}
```

**执行结果：**

 

 

pq: 30 20 10

---

### 3. **访问 priority_queue 的堆顶元素**
 

```plain
#include <iostream>
#include <queue>

int main() {
    std::priority_queue<int> pq;

    // 添加元素
    pq.push(10);
    pq.push(30);
    pq.push(20);

    // 访问堆顶元素
    std::cout << "Top element: " << pq.top() << std::endl;

    return 0;
}
```

**执行结果：**

 

 

Top element: 30

---

### 4. **删除 priority_queue 的堆顶元素**
 

```plain
#include <iostream>
#include <queue>

int main() {
    std::priority_queue<int> pq;

    // 添加元素
    pq.push(10);
    pq.push(30);
    pq.push(20);

    // 删除堆顶元素
    pq.pop();

    // 输出剩余元素
    std::cout << "pq after pop: ";
    while (!pq.empty()) {
        std::cout << pq.top() << " ";
        pq.pop();
    }
    return 0;
}
```

**执行结果：**

 

 

pq after pop: 20 10

---

### 5. **获取 priority_queue 的大小**
 

```plain
#include <iostream>
#include <queue>

int main() {
    std::priority_queue<int> pq;

    // 添加元素
    pq.push(10);
    pq.push(30);
    pq.push(20);

    // 获取 priority_queue 的大小
    std::cout << "pq size: " << pq.size() << std::endl;

    return 0;
}
```

**执行结果：**

 

 

pq size: 3

---

### 6. **检查 priority_queue 是否为空**
 

```plain
#include <iostream>
#include <queue>

int main() {
    std::priority_queue<int> pq;

    // 检查 priority_queue 是否为空
    if (pq.empty()) {
        std::cout << "pq is empty!" << std::endl;
    }

    // 添加元素
    pq.push(10);

    // 再次检查
    if (!pq.empty()) {
        std::cout << "pq is not empty!" << std::endl;
    }

    return 0;
}
```

**执行结果：**

 

 

```plain
pq is empty!
pq is not empty!
```

---

### 7. **自定义比较函数（最小堆）**
默认情况下，`std::priority_queue` 是最大堆。可以通过自定义比较函数实现最小堆。

#### 示例：
 

```plain
#include <iostream>
#include <queue>
#include <vector>

int main() {
    // 使用 greater<int> 实现最小堆
    std::priority_queue<int, std::vector<int>, std::greater<int>> pq;

    // 添加元素
    pq.push(10);
    pq.push(30);
    pq.push(20);

    // 输出 priority_queue 的内容
    std::cout << "pq (min-heap): ";
    while (!pq.empty()) {
        std::cout << pq.top() << " ";
        pq.pop();
    }
    return 0;
}
```

**执行结果：**

 

 

pq (min-heap): 10 20 30

---

### 8. **使用自定义数据结构**
`std::priority_queue` 可以存储自定义数据结构，但需要定义比较函数。

#### 示例：
 

```plain
#include <iostream>
#include <queue>
#include <vector>

struct Task {
    int priority;
    std::string name;

    // 重载 < 运算符，用于比较优先级
    bool operator<(const Task& other) const {
        return priority < other.priority; // 最大堆
    }
};

int main() {
    std::priority_queue<Task> pq;

    // 添加任务
    pq.push({3, "Task 1"});
    pq.push({1, "Task 2"});
    pq.push({2, "Task 3"});

    // 输出任务
    std::cout << "Tasks in priority order:\n";
    while (!pq.empty()) {
        Task task = pq.top();
        std::cout << task.name << " (Priority: " << task.priority << ")\n";
        pq.pop();
    }
    return 0;
}
```

**执行结果：**

 

 

```plain
Tasks in priority order:
Task 1 (Priority: 3)
Task 3 (Priority: 2)
Task 2 (Priority: 1)
```

---

### 9. **实现 Dijkstra 算法**
`std::priority_queue` 常用于实现 Dijkstra 算法，用于求解单源最短路径。

#### 示例：
 

```plain
#include <iostream>
#include <queue>
#include <vector>
#include <utility> // for std::pair

void Dijkstra(const std::vector<std::vector<std::pair<int, int>>>& graph, int start) {
    int n = graph.size();
    std::vector<int> dist(n, INT_MAX);
    std::priority_queue<std::pair<int, int>, std::vector<std::pair<int, int>>, std::greater<std::pair<int, int>>> pq;

    dist[start] = 0;
    pq.push({0, start});

    while (!pq.empty()) {
        int u = pq.top().second;
        pq.pop();

        for (const auto& edge : graph[u]) {
            int v = edge.first;
            int weight = edge.second;

            if (dist[u] + weight < dist[v]) {
                dist[v] = dist[u] + weight;
                pq.push({dist[v], v});
            }
        }
    }

    // 输出最短路径
    std::cout << "Shortest distances from node " << start << ":\n";
    for (int i = 0; i < n; ++i) {
        std::cout << "Node " << i << ": " << dist[i] << "\n";
    }
}

int main() {
    // 图的邻接表表示
    std::vector<std::vector<std::pair<int, int>>> graph = {
        {{1, 4}, {2, 1}}, // 节点 0 的邻居
        {{3, 1}},         // 节点 1 的邻居
        {{1, 2}, {3, 5}}, // 节点 2 的邻居
        {}                // 节点 3 的邻居
    };

    // 从节点 0 开始 Dijkstra 算法
    Dijkstra(graph, 0);
    return 0;
}
```

**执行结果：**

 

 

```plain
Shortest distances from node 0:
Node 0: 0
Node 1: 3
Node 2: 1
Node 3: 4
```

---

### 总结
`std::priority_queue` 的常用操作包括：

+ `push()`：添加元素。
+ `pop()`：删除堆顶元素。
+ `top()`：访问堆顶元素。
+ `empty()`：检查是否为空。
+ `size()`：获取元素个数。

通过自定义比较函数，可以实现最大堆或最小堆。`std::priority_queue` 常用于任务调度、Dijkstra 算法、哈夫曼编码等需要按优先级处理元素的场景。



> 更新: 2025-04-17 22:29:33  
> 原文: <https://www.yuque.com/linuxer/gscfv1/sa4ny6bsx4noi67i>