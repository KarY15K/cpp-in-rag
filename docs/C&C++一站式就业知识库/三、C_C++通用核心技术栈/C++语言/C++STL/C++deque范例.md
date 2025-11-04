# C++ deque范例

`std::deque`（双端队列）是 C++ STL 中的一种动态数组容器，支持在头部和尾部高效地插入和删除元素，同时允许随机访问。以下是 `std::deque` 的常用范例及其执行结果：

---

### 1. **创建和初始化 deque**




```plain
<span class="token macro property"><span class="token directive-hash">#</span><span class="token directive keyword">include</span> <span class="token string"><iostream></span></span>

<span class="token macro property"><span class="token directive-hash">#</span><span class="token directive keyword">include</span> <span class="token string"><deque></span></span>

<span class="token keyword">int</span> <span class="token function">main</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>

    <span class="token comment">// 创建一个空的 deque</span>

    std<span class="token double-colon punctuation">::</span>deque<span class="token operator"><</span><span class="token keyword">int</span><span class="token operator">></span> deque1<span class="token punctuation">;</span>

    <span class="token comment">// 创建并初始化一个包含 5 个元素的 deque，初始值为 0</span>

    std<span class="token double-colon punctuation">::</span>deque<span class="token operator"><</span><span class="token keyword">int</span><span class="token operator">></span> <span class="token function">deque2</span><span class="token punctuation">(</span><span class="token number">5</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

    <span class="token comment">// 创建并初始化一个包含 5 个元素的 deque，初始值为 10</span>

    std<span class="token double-colon punctuation">::</span>deque<span class="token operator"><</span><span class="token keyword">int</span><span class="token operator">></span> <span class="token function">deque3</span><span class="token punctuation">(</span><span class="token number">5</span><span class="token punctuation">,</span> <span class="token number">10</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

    <span class="token comment">// 使用初始化列表创建 deque</span>

    std<span class="token double-colon punctuation">::</span>deque<span class="token operator"><</span><span class="token keyword">int</span><span class="token operator">></span> deque4 <span class="token operator">=</span> <span class="token punctuation">{</span><span class="token number">1</span><span class="token punctuation">,</span> <span class="token number">2</span><span class="token punctuation">,</span> <span class="token number">3</span><span class="token punctuation">,</span> <span class="token number">4</span><span class="token punctuation">,</span> <span class="token number">5</span><span class="token punctuation">}</span><span class="token punctuation">;</span>

    <span class="token comment">// 输出 deque4 的内容</span>

    std<span class="token double-colon punctuation">::</span>cout <span class="token operator"><<</span> <span class="token string">"deque4: "</span><span class="token punctuation">;</span>

    <span class="token keyword">for</span> <span class="token punctuation">(</span><span class="token keyword">int</span> i <span class="token operator">:</span> deque4<span class="token punctuation">)</span> <span class="token punctuation">{</span>

        std<span class="token double-colon punctuation">::</span>cout <span class="token operator"><<</span> i <span class="token operator"><<</span> <span class="token string">" "</span><span class="token punctuation">;</span>

    <span class="token punctuation">}</span>

    <span class="token keyword">return</span> <span class="token number">0</span><span class="token punctuation">;</span>

<span class="token punctuation">}</span>

```



**执行结果：**

```plain
deque4: 1 2 3 4 5<br></br><br></br>

```



---

### 2. **添加元素到 deque**




```plain
<span class="token macro property"><span class="token directive-hash">#</span><span class="token directive keyword">include</span> <span class="token string"><iostream></span></span>

<span class="token macro property"><span class="token directive-hash">#</span><span class="token directive keyword">include</span> <span class="token string"><deque></span></span>

<span class="token keyword">int</span> <span class="token function">main</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>

    std<span class="token double-colon punctuation">::</span>deque<span class="token operator"><</span><span class="token keyword">int</span><span class="token operator">></span> deque<span class="token punctuation">;</span>

    <span class="token comment">// 在尾部添加元素</span>

    deque<span class="token punctuation">.</span><span class="token function">push_back</span><span class="token punctuation">(</span><span class="token number">10</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

    deque<span class="token punctuation">.</span><span class="token function">push_back</span><span class="token punctuation">(</span><span class="token number">20</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

    deque<span class="token punctuation">.</span><span class="token function">push_back</span><span class="token punctuation">(</span><span class="token number">30</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

    <span class="token comment">// 在头部添加元素</span>

    deque<span class="token punctuation">.</span><span class="token function">push_front</span><span class="token punctuation">(</span><span class="token number">5</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

    deque<span class="token punctuation">.</span><span class="token function">push_front</span><span class="token punctuation">(</span><span class="token number">1</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

    <span class="token comment">// 使用 emplace_back 和 emplace_front 直接构造元素</span>

    deque<span class="token punctuation">.</span><span class="token function">emplace_back</span><span class="token punctuation">(</span><span class="token number">40</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

    deque<span class="token punctuation">.</span><span class="token function">emplace_front</span><span class="token punctuation">(</span><span class="token number">0</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

    <span class="token comment">// 输出 deque 内容</span>

    std<span class="token double-colon punctuation">::</span>cout <span class="token operator"><<</span> <span class="token string">"deque: "</span><span class="token punctuation">;</span>

    <span class="token keyword">for</span> <span class="token punctuation">(</span><span class="token keyword">int</span> i <span class="token operator">:</span> deque<span class="token punctuation">)</span> <span class="token punctuation">{</span>

        std<span class="token double-colon punctuation">::</span>cout <span class="token operator"><<</span> i <span class="token operator"><<</span> <span class="token string">" "</span><span class="token punctuation">;</span>

    <span class="token punctuation">}</span>

    <span class="token keyword">return</span> <span class="token number">0</span><span class="token punctuation">;</span>

<span class="token punctuation">}</span>

```



**执行结果：**

```plain
deque: 0 1 5 10 20 30 40<br></br><br></br>

```



---

### 3. **访问 deque 元素**




```plain
<span class="token macro property"><span class="token directive-hash">#</span><span class="token directive keyword">include</span> <span class="token string"><iostream></span></span>

<span class="token macro property"><span class="token directive-hash">#</span><span class="token directive keyword">include</span> <span class="token string"><deque></span></span>

<span class="token keyword">int</span> <span class="token function">main</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>

    std<span class="token double-colon punctuation">::</span>deque<span class="token operator"><</span><span class="token keyword">int</span><span class="token operator">></span> deque <span class="token operator">=</span> <span class="token punctuation">{</span><span class="token number">1</span><span class="token punctuation">,</span> <span class="token number">2</span><span class="token punctuation">,</span> <span class="token number">3</span><span class="token punctuation">,</span> <span class="token number">4</span><span class="token punctuation">,</span> <span class="token number">5</span><span class="token punctuation">}</span><span class="token punctuation">;</span>

    <span class="token comment">// 使用下标访问元素</span>

    std<span class="token double-colon punctuation">::</span>cout <span class="token operator"><<</span> <span class="token string">"Element at index 2: "</span> <span class="token operator"><<</span> deque<span class="token punctuation">[</span><span class="token number">2</span><span class="token punctuation">]</span> <span class="token operator"><<</span> std<span class="token double-colon punctuation">::</span>endl<span class="token punctuation">;</span>

    <span class="token comment">// 使用 at() 访问元素（会检查边界）</span>

    std<span class="token double-colon punctuation">::</span>cout <span class="token operator"><<</span> <span class="token string">"Element at index 3: "</span> <span class="token operator"><<</span> deque<span class="token punctuation">.</span><span class="token function">at</span><span class="token punctuation">(</span><span class="token number">3</span><span class="token punctuation">)</span> <span class="token operator"><<</span> std<span class="token double-colon punctuation">::</span>endl<span class="token punctuation">;</span>

    <span class="token comment">// 访问第一个和最后一个元素</span>

    std<span class="token double-colon punctuation">::</span>cout <span class="token operator"><<</span> <span class="token string">"First element: "</span> <span class="token operator"><<</span> deque<span class="token punctuation">.</span><span class="token function">front</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator"><<</span> std<span class="token double-colon punctuation">::</span>endl<span class="token punctuation">;</span>

    std<span class="token double-colon punctuation">::</span>cout <span class="token operator"><<</span> <span class="token string">"Last element: "</span> <span class="token operator"><<</span> deque<span class="token punctuation">.</span><span class="token function">back</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator"><<</span> std<span class="token double-colon punctuation">::</span>endl<span class="token punctuation">;</span>

    <span class="token keyword">return</span> <span class="token number">0</span><span class="token punctuation">;</span>

<span class="token punctuation">}</span>

```



**执行结果：**

```plain
Element at index 2: 3
Element at index 3: 4
First element: 1
Last element: 5<br></br><br></br>

```



---

### 4. **遍历 deque**




```plain
<span class="token macro property"><span class="token directive-hash">#</span><span class="token directive keyword">include</span> <span class="token string"><iostream></span></span>

<span class="token macro property"><span class="token directive-hash">#</span><span class="token directive keyword">include</span> <span class="token string"><deque></span></span>

<span class="token keyword">int</span> <span class="token function">main</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>

    std<span class="token double-colon punctuation">::</span>deque<span class="token operator"><</span><span class="token keyword">int</span><span class="token operator">></span> deque <span class="token operator">=</span> <span class="token punctuation">{</span><span class="token number">1</span><span class="token punctuation">,</span> <span class="token number">2</span><span class="token punctuation">,</span> <span class="token number">3</span><span class="token punctuation">,</span> <span class="token number">4</span><span class="token punctuation">,</span> <span class="token number">5</span><span class="token punctuation">}</span><span class="token punctuation">;</span>

    <span class="token comment">// 使用下标遍历</span>

    std<span class="token double-colon punctuation">::</span>cout <span class="token operator"><<</span> <span class="token string">"Using index: "</span><span class="token punctuation">;</span>

    <span class="token keyword">for</span> <span class="token punctuation">(</span>size_t i <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">;</span> i <span class="token operator"><</span> deque<span class="token punctuation">.</span><span class="token function">size</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span> <span class="token operator">++</span>i<span class="token punctuation">)</span> <span class="token punctuation">{</span>

        std<span class="token double-colon punctuation">::</span>cout <span class="token operator"><<</span> deque<span class="token punctuation">[</span>i<span class="token punctuation">]</span> <span class="token operator"><<</span> <span class="token string">" "</span><span class="token punctuation">;</span>

    <span class="token punctuation">}</span>

    std<span class="token double-colon punctuation">::</span>cout <span class="token operator"><<</span> std<span class="token double-colon punctuation">::</span>endl<span class="token punctuation">;</span>

    <span class="token comment">// 使用范围 for 循环遍历</span>

    std<span class="token double-colon punctuation">::</span>cout <span class="token operator"><<</span> <span class="token string">"Using range-based for loop: "</span><span class="token punctuation">;</span>

    <span class="token keyword">for</span> <span class="token punctuation">(</span><span class="token keyword">int</span> i <span class="token operator">:</span> deque<span class="token punctuation">)</span> <span class="token punctuation">{</span>

        std<span class="token double-colon punctuation">::</span>cout <span class="token operator"><<</span> i <span class="token operator"><<</span> <span class="token string">" "</span><span class="token punctuation">;</span>

    <span class="token punctuation">}</span>

    std<span class="token double-colon punctuation">::</span>cout <span class="token operator"><<</span> std<span class="token double-colon punctuation">::</span>endl<span class="token punctuation">;</span>

    <span class="token comment">// 使用迭代器遍历</span>

    std<span class="token double-colon punctuation">::</span>cout <span class="token operator"><<</span> <span class="token string">"Using iterator: "</span><span class="token punctuation">;</span>

    <span class="token keyword">for</span> <span class="token punctuation">(</span><span class="token keyword">auto</span> it <span class="token operator">=</span> deque<span class="token punctuation">.</span><span class="token function">begin</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span> it <span class="token operator">!=</span> deque<span class="token punctuation">.</span><span class="token function">end</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span> <span class="token operator">++</span>it<span class="token punctuation">)</span> <span class="token punctuation">{</span>

        std<span class="token double-colon punctuation">::</span>cout <span class="token operator"><<</span> <span class="token operator">*</span>it <span class="token operator"><<</span> <span class="token string">" "</span><span class="token punctuation">;</span>

    <span class="token punctuation">}</span>

    std<span class="token double-colon punctuation">::</span>cout <span class="token operator"><<</span> std<span class="token double-colon punctuation">::</span>endl<span class="token punctuation">;</span>

    <span class="token keyword">return</span> <span class="token number">0</span><span class="token punctuation">;</span>

<span class="token punctuation">}</span>

```



**执行结果：**

```plain
Using index: 1 2 3 4 5
Using range-based for loop: 1 2 3 4 5
Using iterator: 1 2 3 4 5<br></br><br></br>

```



---

### 5. **删除 deque 元素**




```plain
<span class="token macro property"><span class="token directive-hash">#</span><span class="token directive keyword">include</span> <span class="token string"><iostream></span></span>

<span class="token macro property"><span class="token directive-hash">#</span><span class="token directive keyword">include</span> <span class="token string"><deque></span></span>

<span class="token keyword">int</span> <span class="token function">main</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>

    std<span class="token double-colon punctuation">::</span>deque<span class="token operator"><</span><span class="token keyword">int</span><span class="token operator">></span> deque <span class="token operator">=</span> <span class="token punctuation">{</span><span class="token number">1</span><span class="token punctuation">,</span> <span class="token number">2</span><span class="token punctuation">,</span> <span class="token number">3</span><span class="token punctuation">,</span> <span class="token number">4</span><span class="token punctuation">,</span> <span class="token number">5</span><span class="token punctuation">}</span><span class="token punctuation">;</span>

    <span class="token comment">// 删除头部元素</span>

    deque<span class="token punctuation">.</span><span class="token function">pop_front</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

    <span class="token comment">// 删除尾部元素</span>

    deque<span class="token punctuation">.</span><span class="token function">pop_back</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

    <span class="token comment">// 删除指定位置的元素（删除第 2 个元素）</span>

    <span class="token keyword">auto</span> it <span class="token operator">=</span> deque<span class="token punctuation">.</span><span class="token function">begin</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

    std<span class="token double-colon punctuation">::</span><span class="token function">advance</span><span class="token punctuation">(</span>it<span class="token punctuation">,</span> <span class="token number">1</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

    deque<span class="token punctuation">.</span><span class="token function">erase</span><span class="token punctuation">(</span>it<span class="token punctuation">)</span><span class="token punctuation">;</span>

    <span class="token comment">// 输出剩余元素</span>

    std<span class="token double-colon punctuation">::</span>cout <span class="token operator"><<</span> <span class="token string">"deque after deletions: "</span><span class="token punctuation">;</span>

    <span class="token keyword">for</span> <span class="token punctuation">(</span><span class="token keyword">int</span> i <span class="token operator">:</span> deque<span class="token punctuation">)</span> <span class="token punctuation">{</span>

        std<span class="token double-colon punctuation">::</span>cout <span class="token operator"><<</span> i <span class="token operator"><<</span> <span class="token string">" "</span><span class="token punctuation">;</span>

    <span class="token punctuation">}</span>

    <span class="token keyword">return</span> <span class="token number">0</span><span class="token punctuation">;</span>

<span class="token punctuation">}</span>

```



**执行结果：**

```plain
deque after deletions: 2 4<br></br><br></br>

```



---

### 6. **获取 deque 的大小**




```plain
<span class="token macro property"><span class="token directive-hash">#</span><span class="token directive keyword">include</span> <span class="token string"><iostream></span></span>

<span class="token macro property"><span class="token directive-hash">#</span><span class="token directive keyword">include</span> <span class="token string"><deque></span></span>

<span class="token keyword">int</span> <span class="token function">main</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>

    std<span class="token double-colon punctuation">::</span>deque<span class="token operator"><</span><span class="token keyword">int</span><span class="token operator">></span> deque <span class="token operator">=</span> <span class="token punctuation">{</span><span class="token number">1</span><span class="token punctuation">,</span> <span class="token number">2</span><span class="token punctuation">,</span> <span class="token number">3</span><span class="token punctuation">,</span> <span class="token number">4</span><span class="token punctuation">,</span> <span class="token number">5</span><span class="token punctuation">}</span><span class="token punctuation">;</span>

    <span class="token comment">// 获取元素个数</span>

    std<span class="token double-colon punctuation">::</span>cout <span class="token operator"><<</span> <span class="token string">"Size: "</span> <span class="token operator"><<</span> deque<span class="token punctuation">.</span><span class="token function">size</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator"><<</span> std<span class="token double-colon punctuation">::</span>endl<span class="token punctuation">;</span>

    <span class="token keyword">return</span> <span class="token number">0</span><span class="token punctuation">;</span>

<span class="token punctuation">}</span>

```



**执行结果：**

```plain
Size: 5<br></br><br></br>

```



---

### 7. **deque 的排序**




```plain
<span class="token macro property"><span class="token directive-hash">#</span><span class="token directive keyword">include</span> <span class="token string"><iostream></span></span>

<span class="token macro property"><span class="token directive-hash">#</span><span class="token directive keyword">include</span> <span class="token string"><deque></span></span>

<span class="token macro property"><span class="token directive-hash">#</span><span class="token directive keyword">include</span> <span class="token string"><algorithm></span> <span class="token comment">// 包含 sort 函数</span></span>

<span class="token keyword">int</span> <span class="token function">main</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>

    std<span class="token double-colon punctuation">::</span>deque<span class="token operator"><</span><span class="token keyword">int</span><span class="token operator">></span> deque <span class="token operator">=</span> <span class="token punctuation">{</span><span class="token number">5</span><span class="token punctuation">,</span> <span class="token number">3</span><span class="token punctuation">,</span> <span class="token number">1</span><span class="token punctuation">,</span> <span class="token number">4</span><span class="token punctuation">,</span> <span class="token number">2</span><span class="token punctuation">}</span><span class="token punctuation">;</span>

    <span class="token comment">// 对 deque 进行升序排序</span>

    std<span class="token double-colon punctuation">::</span><span class="token function">sort</span><span class="token punctuation">(</span>deque<span class="token punctuation">.</span><span class="token function">begin</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span> deque<span class="token punctuation">.</span><span class="token function">end</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

    <span class="token comment">// 输出排序后的 deque</span>

    std<span class="token double-colon punctuation">::</span>cout <span class="token operator"><<</span> <span class="token string">"Sorted deque: "</span><span class="token punctuation">;</span>

    <span class="token keyword">for</span> <span class="token punctuation">(</span><span class="token keyword">int</span> i <span class="token operator">:</span> deque<span class="token punctuation">)</span> <span class="token punctuation">{</span>

        std<span class="token double-colon punctuation">::</span>cout <span class="token operator"><<</span> i <span class="token operator"><<</span> <span class="token string">" "</span><span class="token punctuation">;</span>

    <span class="token punctuation">}</span>

    <span class="token keyword">return</span> <span class="token number">0</span><span class="token punctuation">;</span>

<span class="token punctuation">}</span>

```



**执行结果：**

```plain
Sorted deque: 1 2 3 4 5<br></br><br></br>

```



---

### 8. **deque 的交换**




```plain
<span class="token macro property"><span class="token directive-hash">#</span><span class="token directive keyword">include</span> <span class="token string"><iostream></span></span>

<span class="token macro property"><span class="token directive-hash">#</span><span class="token directive keyword">include</span> <span class="token string"><deque></span></span>

<span class="token keyword">int</span> <span class="token function">main</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>

    std<span class="token double-colon punctuation">::</span>deque<span class="token operator"><</span><span class="token keyword">int</span><span class="token operator">></span> deque1 <span class="token operator">=</span> <span class="token punctuation">{</span><span class="token number">1</span><span class="token punctuation">,</span> <span class="token number">2</span><span class="token punctuation">,</span> <span class="token number">3</span><span class="token punctuation">}</span><span class="token punctuation">;</span>

    std<span class="token double-colon punctuation">::</span>deque<span class="token operator"><</span><span class="token keyword">int</span><span class="token operator">></span> deque2 <span class="token operator">=</span> <span class="token punctuation">{</span><span class="token number">4</span><span class="token punctuation">,</span> <span class="token number">5</span><span class="token punctuation">,</span> <span class="token number">6</span><span class="token punctuation">}</span><span class="token punctuation">;</span>

    <span class="token comment">// 交换两个 deque 的内容</span>

    deque1<span class="token punctuation">.</span><span class="token function">swap</span><span class="token punctuation">(</span>deque2<span class="token punctuation">)</span><span class="token punctuation">;</span>

    <span class="token comment">// 输出交换后的结果</span>

    std<span class="token double-colon punctuation">::</span>cout <span class="token operator"><<</span> <span class="token string">"deque1: "</span><span class="token punctuation">;</span>

    <span class="token keyword">for</span> <span class="token punctuation">(</span><span class="token keyword">int</span> i <span class="token operator">:</span> deque1<span class="token punctuation">)</span> <span class="token punctuation">{</span>

        std<span class="token double-colon punctuation">::</span>cout <span class="token operator"><<</span> i <span class="token operator"><<</span> <span class="token string">" "</span><span class="token punctuation">;</span>

    <span class="token punctuation">}</span>

    std<span class="token double-colon punctuation">::</span>cout <span class="token operator"><<</span> <span class="token string">"\ndeque2: "</span><span class="token punctuation">;</span>

    <span class="token keyword">for</span> <span class="token punctuation">(</span><span class="token keyword">int</span> i <span class="token operator">:</span> deque2<span class="token punctuation">)</span> <span class="token punctuation">{</span>

        std<span class="token double-colon punctuation">::</span>cout <span class="token operator"><<</span> i <span class="token operator"><<</span> <span class="token string">" "</span><span class="token punctuation">;</span>

    <span class="token punctuation">}</span>

    <span class="token keyword">return</span> <span class="token number">0</span><span class="token punctuation">;</span>

<span class="token punctuation">}</span>

```



**执行结果：**

```plain
deque1: 4 5 6
deque2: 1 2 3<br></br><br></br><br></br>

```



---

### 9. **deque 的反转**




```plain
<span class="token macro property"><span class="token directive-hash">#</span><span class="token directive keyword">include</span> <span class="token string"><iostream></span></span>

<span class="token macro property"><span class="token directive-hash">#</span><span class="token directive keyword">include</span> <span class="token string"><deque></span></span>

<span class="token macro property"><span class="token directive-hash">#</span><span class="token directive keyword">include</span> <span class="token string"><algorithm></span> <span class="token comment">// 包含 reverse 函数</span></span>

<span class="token keyword">int</span> <span class="token function">main</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>

    std<span class="token double-colon punctuation">::</span>deque<span class="token operator"><</span><span class="token keyword">int</span><span class="token operator">></span> deque <span class="token operator">=</span> <span class="token punctuation">{</span><span class="token number">1</span><span class="token punctuation">,</span> <span class="token number">2</span><span class="token punctuation">,</span> <span class="token number">3</span><span class="token punctuation">,</span> <span class="token number">4</span><span class="token punctuation">,</span> <span class="token number">5</span><span class="token punctuation">}</span><span class="token punctuation">;</span>

    <span class="token comment">// 反转 deque</span>

    std<span class="token double-colon punctuation">::</span><span class="token function">reverse</span><span class="token punctuation">(</span>deque<span class="token punctuation">.</span><span class="token function">begin</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span> deque<span class="token punctuation">.</span><span class="token function">end</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

    <span class="token comment">// 输出反转后的 deque</span>

    std<span class="token double-colon punctuation">::</span>cout <span class="token operator"><<</span> <span class="token string">"Reversed deque: "</span><span class="token punctuation">;</span>

    <span class="token keyword">for</span> <span class="token punctuation">(</span><span class="token keyword">int</span> i <span class="token operator">:</span> deque<span class="token punctuation">)</span> <span class="token punctuation">{</span>

        std<span class="token double-colon punctuation">::</span>cout <span class="token operator"><<</span> i <span class="token operator"><<</span> <span class="token string">" "</span><span class="token punctuation">;</span>

    <span class="token punctuation">}</span>

    <span class="token keyword">return</span> <span class="token number">0</span><span class="token punctuation">;</span>

<span class="token punctuation">}</span>

```



**执行结果：**

```plain
Reversed deque: 5 4 3 2 1<br></br><br></br>

```



---

### 10. **deque 的插入**




```plain
<span class="token macro property"><span class="token directive-hash">#</span><span class="token directive keyword">include</span> <span class="token string"><iostream></span></span>

<span class="token macro property"><span class="token directive-hash">#</span><span class="token directive keyword">include</span> <span class="token string"><deque></span></span>

<span class="token keyword">int</span> <span class="token function">main</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>

    std<span class="token double-colon punctuation">::</span>deque<span class="token operator"><</span><span class="token keyword">int</span><span class="token operator">></span> deque <span class="token operator">=</span> <span class="token punctuation">{</span><span class="token number">1</span><span class="token punctuation">,</span> <span class="token number">2</span><span class="token punctuation">,</span> <span class="token number">3</span><span class="token punctuation">,</span> <span class="token number">4</span><span class="token punctuation">,</span> <span class="token number">5</span><span class="token punctuation">}</span><span class="token punctuation">;</span>

    <span class="token comment">// 在指定位置插入元素</span>

    <span class="token keyword">auto</span> it <span class="token operator">=</span> deque<span class="token punctuation">.</span><span class="token function">begin</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

    std<span class="token double-colon punctuation">::</span><span class="token function">advance</span><span class="token punctuation">(</span>it<span class="token punctuation">,</span> <span class="token number">2</span><span class="token punctuation">)</span><span class="token punctuation">;</span> <span class="token comment">// 将迭代器移动到第 3 个位置</span>

    deque<span class="token punctuation">.</span><span class="token function">insert</span><span class="token punctuation">(</span>it<span class="token punctuation">,</span> <span class="token number">100</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

    <span class="token comment">// 输出插入后的 deque</span>

    std<span class="token double-colon punctuation">::</span>cout <span class="token operator"><<</span> <span class="token string">"deque after insertion: "</span><span class="token punctuation">;</span>

    <span class="token keyword">for</span> <span class="token punctuation">(</span><span class="token keyword">int</span> i <span class="token operator">:</span> deque<span class="token punctuation">)</span> <span class="token punctuation">{</span>

        std<span class="token double-colon punctuation">::</span>cout <span class="token operator"><<</span> i <span class="token operator"><<</span> <span class="token string">" "</span><span class="token punctuation">;</span>

    <span class="token punctuation">}</span>

    <span class="token keyword">return</span> <span class="token number">0</span><span class="token punctuation">;</span>

<span class="token punctuation">}</span>

```



**执行结果：**

```plain
deque after insertion: 1 2 100 3 4 5<br></br><br></br>

```



---

### 总结
`std::deque` 的常用操作包括：

+ `push_back()` 和 `push_front()`：在尾部或头部添加元素。
+ `pop_back()` 和 `pop_front()`：删除尾部或头部元素。
+ `emplace_back()` 和 `emplace_front()`：直接在尾部或头部构造元素。
+ `operator[]` 和 `at()`：访问元素。
+ `size()`：获取元素个数。
+ `empty()`：检查是否为空。
+ `insert()` 和 `erase()`：在指定位置插入或删除元素。
+ `sort()` 和 `reverse()`：排序和反转。

通过这些操作，可以高效地管理双端队列中的数据。

### 11. deque里的emplace_back有什么优势
`std::deque` 中的 `emplace_back` 是 C++11 引入的一个成员函数，用于在 `deque` 的尾部直接构造元素。与 `push_back` 相比，`emplace_back` 具有以下优势：

---

### 1. **避免不必要的拷贝或移动操作**
+ `push_back`：需要先构造一个临时对象，然后将该对象拷贝或移动到容器中。
+ `emplace_back`：直接在容器中构造对象，避免了临时对象的创建和拷贝/移动操作。

#### 示例对比：
```plain
<span class="token macro property"><span class="token directive-hash">#</span><span class="token directive keyword">include</span> <span class="token string"><iostream></span></span>

<span class="token macro property"><span class="token directive-hash">#</span><span class="token directive keyword">include</span> <span class="token string"><deque></span></span>

<span class="token keyword">class</span> <span class="token class-name">MyClass</span> <span class="token punctuation">{</span>

<span class="token keyword">public</span><span class="token operator">:</span>

    <span class="token function">MyClass</span><span class="token punctuation">(</span><span class="token keyword">int</span> a<span class="token punctuation">,</span> <span class="token keyword">int</span> b<span class="token punctuation">)</span> <span class="token punctuation">{</span>

        std<span class="token double-colon punctuation">::</span>cout <span class="token operator"><<</span> <span class="token string">"Constructor called! a = "</span> <span class="token operator"><<</span> a <span class="token operator"><<</span> <span class="token string">", b = "</span> <span class="token operator"><<</span> b <span class="token operator"><<</span> std<span class="token double-colon punctuation">::</span>endl<span class="token punctuation">;</span>

    <span class="token punctuation">}</span>

    <span class="token function">MyClass</span><span class="token punctuation">(</span><span class="token keyword">const</span> MyClass<span class="token operator">&</span> other<span class="token punctuation">)</span> <span class="token punctuation">{</span>

        std<span class="token double-colon punctuation">::</span>cout <span class="token operator"><<</span> <span class="token string">"Copy constructor called!"</span> <span class="token operator"><<</span> std<span class="token double-colon punctuation">::</span>endl<span class="token punctuation">;</span>

    <span class="token punctuation">}</span>

    <span class="token function">MyClass</span><span class="token punctuation">(</span>MyClass<span class="token operator">&&</span> other<span class="token punctuation">)</span> <span class="token keyword">noexcept</span> <span class="token punctuation">{</span>

        std<span class="token double-colon punctuation">::</span>cout <span class="token operator"><<</span> <span class="token string">"Move constructor called!"</span> <span class="token operator"><<</span> std<span class="token double-colon punctuation">::</span>endl<span class="token punctuation">;</span>

    <span class="token punctuation">}</span>

<span class="token punctuation">}</span><span class="token punctuation">;</span>

<span class="token keyword">int</span> <span class="token function">main</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>

    std<span class="token double-colon punctuation">::</span>deque<span class="token operator"><</span>MyClass<span class="token operator">></span> deque<span class="token punctuation">;</span>

    <span class="token comment">// 使用 push_back</span>

    std<span class="token double-colon punctuation">::</span>cout <span class="token operator"><<</span> <span class="token string">"Using push_back:"</span> <span class="token operator"><<</span> std<span class="token double-colon punctuation">::</span>endl<span class="token punctuation">;</span>

    deque<span class="token punctuation">.</span><span class="token function">push_back</span><span class="token punctuation">(</span><span class="token function">MyClass</span><span class="token punctuation">(</span><span class="token number">1</span><span class="token punctuation">,</span> <span class="token number">2</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span> <span class="token comment">// 先构造临时对象，再移动</span>

    <span class="token comment">// 使用 emplace_back</span>

    std<span class="token double-colon punctuation">::</span>cout <span class="token operator"><<</span> <span class="token string">"\nUsing emplace_back:"</span> <span class="token operator"><<</span> std<span class="token double-colon punctuation">::</span>endl<span class="token punctuation">;</span>

    deque<span class="token punctuation">.</span><span class="token function">emplace_back</span><span class="token punctuation">(</span><span class="token number">3</span><span class="token punctuation">,</span> <span class="token number">4</span><span class="token punctuation">)</span><span class="token punctuation">;</span> <span class="token comment">// 直接在容器中构造对象</span>

    <span class="token keyword">return</span> <span class="token number">0</span><span class="token punctuation">;</span>

<span class="token punctuation">}</span>

```



**输出：**

```plain
Using push_back:
Constructor called! a = 1, b = 2
Move constructor called!

Using emplace_back:
Constructor called! a = 3, b = 4
```



从输出可以看出：

+ `push_back` 调用了构造函数和移动构造函数。
+ `emplace_back` 只调用了构造函数，避免了额外的开销。

---

### 2. **支持多参数构造**
+ `push_back`：只能传递一个已经构造好的对象。
+ `emplace_back`：可以传递构造对象所需的参数，支持多参数构造函数。

#### 示例：
```plain
<span class="token macro property"><span class="token directive-hash">#</span><span class="token directive keyword">include</span> <span class="token string"><iostream></span></span>

<span class="token macro property"><span class="token directive-hash">#</span><span class="token directive keyword">include</span> <span class="token string"><deque></span></span>

<span class="token keyword">class</span> <span class="token class-name">MyClass</span> <span class="token punctuation">{</span>

<span class="token keyword">public</span><span class="token operator">:</span>

    <span class="token function">MyClass</span><span class="token punctuation">(</span><span class="token keyword">int</span> a<span class="token punctuation">,</span> <span class="token keyword">int</span> b<span class="token punctuation">,</span> <span class="token keyword">int</span> c<span class="token punctuation">)</span> <span class="token punctuation">{</span>

        std<span class="token double-colon punctuation">::</span>cout <span class="token operator"><<</span> <span class="token string">"Constructor called! a = "</span> <span class="token operator"><<</span> a <span class="token operator"><<</span> <span class="token string">", b = "</span> <span class="token operator"><<</span> b <span class="token operator"><<</span> <span class="token string">", c = "</span> <span class="token operator"><<</span> c <span class="token operator"><<</span> std<span class="token double-colon punctuation">::</span>endl<span class="token punctuation">;</span>

    <span class="token punctuation">}</span>

<span class="token punctuation">}</span><span class="token punctuation">;</span>

<span class="token keyword">int</span> <span class="token function">main</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>

    std<span class="token double-colon punctuation">::</span>deque<span class="token operator"><</span>MyClass<span class="token operator">></span> deque<span class="token punctuation">;</span>

    <span class="token comment">// 使用 emplace_back 传递多个参数</span>

    deque<span class="token punctuation">.</span><span class="token function">emplace_back</span><span class="token punctuation">(</span><span class="token number">1</span><span class="token punctuation">,</span> <span class="token number">2</span><span class="token punctuation">,</span> <span class="token number">3</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

    <span class="token keyword">return</span> <span class="token number">0</span><span class="token punctuation">;</span>

<span class="token punctuation">}</span>

```



**输出：**

```plain
Constructor called! a = 1, b = 2, c = 3
```



---

### 3. **性能优势**
由于 `emplace_back` 避免了临时对象的创建和拷贝/移动操作，因此在性能上通常优于 `push_back`，尤其是在以下场景中：

+ 对象的构造开销较大（例如，对象包含动态内存分配或复杂的初始化逻辑）。
+ 容器中存储的对象不支持拷贝或移动操作（例如，`std::unique_ptr`）。

---

### 4. **代码简洁性**
`emplace_back` 可以直接传递构造参数，使代码更加简洁直观。

#### 示例：
```plain
<span class="token macro property"><span class="token directive-hash">#</span><span class="token directive keyword">include</span> <span class="token string"><iostream></span></span>

<span class="token macro property"><span class="token directive-hash">#</span><span class="token directive keyword">include</span> <span class="token string"><deque></span></span>

<span class="token macro property"><span class="token directive-hash">#</span><span class="token directive keyword">include</span> <span class="token string"><string></span></span>

<span class="token keyword">int</span> <span class="token function">main</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>

    std<span class="token double-colon punctuation">::</span>deque<span class="token operator"><</span>std<span class="token double-colon punctuation">::</span>string<span class="token operator">></span> deque<span class="token punctuation">;</span>

    <span class="token comment">// 使用 push_back</span>

    deque<span class="token punctuation">.</span><span class="token function">push_back</span><span class="token punctuation">(</span>std<span class="token double-colon punctuation">::</span><span class="token function">string</span><span class="token punctuation">(</span><span class="token string">"Hello"</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

    <span class="token comment">// 使用 emplace_back</span>

    deque<span class="token punctuation">.</span><span class="token function">emplace_back</span><span class="token punctuation">(</span><span class="token string">"World"</span><span class="token punctuation">)</span><span class="token punctuation">;</span> <span class="token comment">// 更简洁</span>

    <span class="token comment">// 输出内容</span>

    <span class="token keyword">for</span> <span class="token punctuation">(</span><span class="token keyword">const</span> <span class="token keyword">auto</span><span class="token operator">&</span> str <span class="token operator">:</span> deque<span class="token punctuation">)</span> <span class="token punctuation">{</span>

        std<span class="token double-colon punctuation">::</span>cout <span class="token operator"><<</span> str <span class="token operator"><<</span> <span class="token string">" "</span><span class="token punctuation">;</span>

    <span class="token punctuation">}</span>

    <span class="token keyword">return</span> <span class="token number">0</span><span class="token punctuation">;</span>

<span class="token punctuation">}</span>

```



**输出：**

```plain
Hello World
```



---

### 总结
| 特性 | `push_back` | `emplace_back` |
| --- | --- | --- |
| **临时对象** | 需要创建临时对象 | 不需要创建临时对象 |
| **拷贝/移动操作** | 可能涉及拷贝或移动操作 | 无拷贝或移动操作 |
| **多参数构造** | 不支持 | 支持 |
| **性能** | 较低（涉及额外开销） | 较高（直接构造） |
| **代码简洁性** | 需要显式构造对象 | 直接传递构造参数 |


因此，在大多数情况下，推荐使用 `emplace_back`，尤其是在需要高效构造对象或传递多个构造参数的场景中。



> 更新: 2025-04-17 22:22:32  
> 原文: <https://www.yuque.com/linuxer/gscfv1/ngrn03o8p59lvmyl>