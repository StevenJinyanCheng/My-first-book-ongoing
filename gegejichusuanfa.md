# 各个基础算法

算法是编程最重要的部分，如果不知道那么只能做基础语法题了。
这篇文章由两个部分组成：

- [算法](#算法)
- [算法数据结构](#算法数据结构)

## 关于时间复杂度的一些事

时间复杂度是一种描述**算法在输入增长时速度的影响**。
假如算法执行$f(n)$个操作（n是算法输入大小），那么如果算法是$O(g(n))$的，那么：$$f(n) \le c \times g(n)$$（当$n$足够大时）。  
但是，一般算法时间复杂度真正重要的是$\Theta$，因为如果算法是$\Theta(g(n))$的，那么：$$c_1 \times g(n) \le f(n) \le c_2 \times g(n)$$（当$n$足够大时）。  
同时，**常数**也很重要。常数是$O$里的$c$，$\Theta$里的$c_2$。  
一般C++语言一秒一亿个操作。操作总数是：  
（假设算法执行$t$次，算法输入大小限制$n$，时间复杂度$f(n)$，常数$c$）
$$操作总数=t \times f(n) \times c$$
如果操作总数小于一亿×描述，那么就应该不会TLE。

## 算法

### 排序算法

排序是一种最基础的算法。有很多种不同的排序算法，但是我们只需要知道两种就行了。

#### 标准库 `std::sort`

这个是一个再`<algorithm>`库里面的一个函数。  
用法：

```cpp
template< class RandomIt >
void sort( RandomIt first, RandomIt last );
template< class RandomIt, class Compare >
void sort( RandomIt first, RandomIt last, Compare comp );
```

时间复杂度：$ O(n \times \log n)$，所以$n<1000000$的时候不会TLE。  
如果用数组那么这样使用：

```cpp
int a[1000005]; // 定义数组
//                 输入数组
//                 假设要排序m到n
sort(a+m, a+n+1);
//                 假设要用cmp函数
sort(a+m, a+n+1, cmp);
```

否则看[这里](https://www.apiref.com/cpp-zh/cpp/algorithm/sort.html)。

#### 计数排序/箱排序

上代码

```cpp
int a[1000005], box[1005];    // 定义数组，假设数组最大值1000
for(int i = m;i <= n;i++){    // 同上
  box[++a[i]]++;
}
for(int i = 0;i <= 1000;i++){ // 输出
  for(int j = 1;j <= box[i];j++){
    cout << i << ' ';
  }
}
```

注意：只能排序最大值不太大的一些整数，也只能从小到大排序。

### DFS

DFS（深度优先搜索）是一种计算所有排列的算法。
注意：`Python3`有`itertools`，可以计算所有排列，但是DFS其实更好。
首先，DFS是一种使用函数递归的算法，所以有可能MLE。  
DFS单独一个算法只有一种应用：暴力枚举。
DFS主要有三步：  

1. 如果递归完了，那么就检验，输出，`return;`
2. 循环
3. 递归

比如多重循环改DFS：

```cpp
// P1008 三连击 部分解

int main(){
    for(int a = 1;a <= 9;a++){
        for(int b = 1;b <= 9;b++){
            for(int c = 1;c <= 9;c++){
                // ...
            }
        }
    }
    return 0;
}
```

用DFS：

```cpp
// P1008 三连击 部分解
int plan[3+5];
void dfs(int step){
    if (step == 3 + 1){ // 递归结束
        // 判断plan数组，输出...
        return 0; // 一定加！！！！！！
    }
    for(int i = 1;i <= 9;i++){
        plan[step] = i; // 改变计划
        dfs(step+1); // 递归
    }
}
int main(){
    dfs(1);
    return 0;
}
```

只能用DFS：全排列（P1706 模板）

```cpp
// P1706 全排列问题 解法
#include <cstdio>
#include <cstring>
#include <iostream>
using namespace std;

int n, plan[13], flag[13];

void dfs(int step) {
    if (step == n + 1) {
        for (int i = 1; i <= n; i++) { // 输出
            cout.width(5);
            cout << plan[i];
        }
        cout << "\n";
        return;
    }
    for (int i = 1; i <= n; i++) {
        if (!flag[i]) { // 没用过
            flag[i] = 1;// 标记
            plan[s] = i;// 改变计划
            dfs(s + 1); // 递归
            flag[i] = 0;// 解除标记
        }
    }
}

int main() {
    cin >> n;
    dfs(1);
    return 0;
}
```

再来个实际运用：弱化版背包（题目出自小猴编程）  
【题目描述】  
有 $k$ 类物品和一个强度 $m$ 的背包。第 $i$ 类物品个数 $n_i$个，每个重量$w_i$，价值$v_i$。
选择若干个物品放入背包, 但是总重量不能超过背包强度$m$。
求出可以放入背包的物品总价值的最大值。  
【输入格式】  
第$1$行, 包含两个正整数 $k$，$m$用空格分隔；
第$2∼k+1$ 行， 每行包含$3$个正整数$n_i$，$w_i$，$v_i$, 用空格分隔。  
【输出格式】  
一个整数, 输出价值总和的最大值。
题目出自小猴编程，原题名叫`简单背包2`，侵权必删。

```cpp
#include <iostream>
#include <algorithm>
using namespace std;

int k, m, n[10], w[10], v[10], maxn = 0;

void dfs(int step, int zhong, int qian) {
    if (step == k + 1) {
        maxn = max(maxn, qian);
        return;
    }

    for (int i = 0; i <= n[step]; i++) { // 枚举
        if (zhong + i * w[step] > m) break; // 装不下，不用再递归
        dfs(step + 1, zhong + i * w[step], qian + i * v[step]); // 递归
    }
}

int main() {
    cin >> k >> m;
    for (int i = 1; i <= k; i++) {
        cin >> n[i] >> w[i] >> v[i];
    }
    dfs(1, 0, 0);
    cout << maxn;
    return 0;
}

```

### 二分和三分

#### 二分

二分是一种查找东西的算法，本质上就是一种折半搜索范围直到找到目标。常用于二分查找，二分多项式根查找。  
假如有一个1到100的数，你每猜一次都能知道是大，是小，还是成功了，那么你可以先猜50，然后如果小了，范围能缩小到$0 \sim 50 - 1$，如果大了，范围能缩小到$50 + 1 \sim 100$。  
这样一直折半，最终范围会只有一个数。
所以我们有一个办法尽量找左边$\le$的数的位置（整数）：

1. 把`l`设为数字下限，`r`设成数字上限。
2. 重复执行直到`l >= r`
2.1. `mid = (l + r) / 2`
2.2. 如果`mid >= 想要的数` 那么 `r = mid`
2.3. 否则 `l = mid + 1`

如果要找右边的数也差不多。

浮点数的方法，因为有精度问题，所以并不需要一直查直到`l >= r`，只要`r - l <= 允许精度`就可以了。

练习：洛谷[P1024](https://www.luogu.com.cn/problem/P1024) [P3382](https://www.luogu.com.cn/problem/P3382)

## 算法数据结构

### 栈与队列

在算法数据结构部分，**栈**和**队列**是两种最基础且常用的数据结构。它们在许多算法中扮演着关键角色，如深度优先搜索（DFS）和广度优先搜索（BFS）等。下面将详细介绍这两种数据结构，包括它们的定义、操作、实现方法以及应用实例。

---

#### 栈（Stack）

![image](https://cdn.luogu.com.cn/upload/image_hosting/5qxy9fz2.png)
![image](https://cdn.luogu.com.cn/upload/image_hosting/8uwv2pa2.png)
栈是一种**先进后出**（FILO，First In Last Out）的数据结构。它类似于现实生活中的叠盘子，最后放上去的盘子最先被取下。

**基本操作：**

1. **push（进栈）**：将一个元素添加到栈顶。
2. **pop（出栈）**：移除并返回栈顶的元素。
3. **top（栈顶）**：返回栈顶的元素但不移除它。
4. **empty（是否为空）**：检查栈是否为空。
5. **size（大小）**：返回栈中元素的数量。

**使用 `std::stack` 实现栈：**

```cpp
#include <stack>
#include <iostream>
using namespace std;

int main(){
    stack<int> s;
    
    // 进栈
    s.push(10);
    s.push(20);
    s.push(30);
    
    // 查看栈顶元素
    cout << "栈顶元素: " << s.top() << endl; // 输出 30
    
    // 出栈
    s.pop();
    cout << "出栈后新的栈顶元素: " << s.top() << endl; // 输出 20
    
    // 检查栈是否为空
    if(!s.empty()){
        cout << "栈不为空，当前大小: " << s.size() << endl;
    }
    
    return 0;
}
```

**注意事项：**

- 使用 `std::stack` 时，如果栈为空再执行 `pop()` 或 `top()` 操作，会导致未定义行为（通常是运行时错误）。
- 为了避免这种情况，可以在执行这些操作前先使用 `empty()` 检查栈是否为空。

**使用 `std::vector` 模拟栈：**

有时，为了获得更多的灵活性，可以使用 `std::vector` 来模拟栈的行为：

```cpp
#include <vector>
#include <iostream>
using namespace std;

int main(){
    vector<int> stack_sim;
    
    // 进栈
    stack_sim.push_back(10);
    stack_sim.push_back(20);
    stack_sim.push_back(30);
    
    // 查看栈顶元素
    if(!stack_sim.empty()){
        cout << "栈顶元素: " << stack_sim.back() << endl; // 输出 30
    }
    
    // 出栈
    if(!stack_sim.empty()){
        stack_sim.pop_back();
        cout << "出栈后新的栈顶元素: " << stack_sim.back() << endl; // 输出 20
    }
    
    return 0;
}
```

##### **应用实例：表达式求值**

栈常用于计算表达式的值，例如中缀表达式转换为后缀表达式，或者直接计算后缀表达式。

```cpp
#include <iostream>
#include <stack>
#include <string>
using namespace std;

// 简单的后缀表达式计算器
int evaluatePostfix(const string& expr){
    stack<int> s;
    for(char ch : expr){
        if(isdigit(ch)){
            s.push(ch - '0');
        }
        else{
            int val2 = s.top(); s.pop();
            int val1 = s.top(); s.pop();
            switch(ch){
                case '+': s.push(val1 + val2); break;
                case '-': s.push(val1 - val2); break;
                case '*': s.push(val1 * val2); break;
                case '/': s.push(val1 / val2); break;
            }
        }
    }
    return s.top();
}

int main(){
    string expr = "231*+9-"; // (2 + 3 * 1) - 9 = -4
    cout << "表达式 " << expr << " 的计算结果是: " << evaluatePostfix(expr) << endl;
    return 0;
}
```

---

#### 队列（Queue）

![image](https://cdn.luogu.com.cn/upload/image_hosting/gf3fb4ug.png)

队列是一种**先进先出**（FIFO，First In First Out）的数据结构。它类似于现实生活中的排队系统，最先进入队列的人最先被服务。

**基本操作：**

1. **enqueue（入队）**：将一个元素添加到队列的末尾。
2. **dequeue（出队）**：移除并返回队列的第一个元素。
3. **front（队首）**：返回队列的第一个元素但不移除它。
4. **back（队尾）**：返回队列的最后一个元素但不移除它。
5. **empty（是否为空）**：检查队列是否为空。
6. **size（大小）**：返回队列中元素的数量。

**使用 `std::queue` 实现队列：**

```cpp
#include <queue>
#include <iostream>
using namespace std;

int main(){
    queue<int> q;
    
    // 入队
    q.push(1);
    q.push(2);
    q.push(3);
    
    // 查看队首元素
    cout << "队首元素: " << q.front() << endl; // 输出 1
    
    // 出队
    q.pop();
    cout << "出队后新的队首元素: " << q.front() << endl; // 输出 2
    
    // 检查队列是否为空
    if(!q.empty()){
        cout << "队列不为空，当前大小: " << q.size() << endl;
    }
    
    return 0;
}
```

**注意事项：**

- 使用 `std::queue` 时，如果队列为空再执行 `pop()` 或 `front()` 操作，会导致未定义行为（通常是运行时错误）。
- 为了避免这种情况，可以在执行这些操作前先使用 `empty()` 检查队列是否为空。

**使用 `std::deque` 模拟队列：**

`std::queue` 默认基于 `std::deque` 实现，但也可以直接使用 `std::deque` 来实现队列，以获得更多的功能。

```cpp
#include <deque>
#include <iostream>
using namespace std;

int main(){
    deque<int> q;
    
    // 入队
    q.push_back(10);
    q.push_back(20);
    q.push_back(30);
    
    // 查看队首元素
    if(!q.empty()){
        cout << "队首元素: " << q.front() << endl; // 输出 10
    }
    
    // 出队
    if(!q.empty()){
        q.pop_front();
        cout << "出队后新的队首元素: " << q.front() << endl; // 输出 20
    }
    
    return 0;
}
```

**使用 `std::vector` 模拟队列（效率较低，不推荐）：**

```cpp
#include <vector>
#include <iostream>
using namespace std;

class MyQueue {
    vector<int> v;
public:
    void enqueue(int x){
        v.push_back(x);
    }
    void dequeue(){
        if(!v.empty()){
            v.erase(v.begin());
        }
    }
    int front(){
        if(!v.empty()){
            return v.front();
        }
        return -1; // 或其他错误标志
    }
    bool empty(){
        return v.empty();
    }
    int size(){
        return v.size();
    }
};

int main(){
    MyQueue q;
    q.enqueue(1);
    q.enqueue(2);
    q.enqueue(3);
    
    cout << "队首元素: " << q.front() << endl; // 输出 1
    
    q.dequeue();
    cout << "出队后新的队首元素: " << q.front() << endl; // 输出 2
    
    if(!q.empty()){
        cout << "队列不为空，当前大小: " << q.size() << endl;
    }
    
    return 0;
}
```

**注意**：使用 `std::vector` 实现队列的 `dequeue` 操作会导致 $O(n)$ 的时间复杂度，因为每次移除第一个元素后，所有后续元素都需要移动。这在队列元素较多时会显著影响性能，因此推荐使用 `std::queue` 或 `std::deque`。

##### **应用实例：广度优先搜索（BFS）**

队列在图的广度优先搜索（BFS）算法中扮演着核心角色，用于按层次遍历图中的节点。
模板：  

```cpp
#include <iostream>
#include <queue>
using namespace std;
int bfs_search_num(int n){
    queue<int> q;
    int steps = 0; // 计数，查找n
    q.push(初始值);
    while(!q.empty()){
        q.push(队列顶上的东西的下部节点);
        q.pop();
        steps++;
    }
}
```

在这个例子中，队列用于存储待访问的节点，确保按照广度优先的顺序遍历图中的所有节点。
  
---

### 栈与队列的比较

| 特性           | 栈 (Stack)              | 队列 (Queue)              |
|----------------|-------------------------|---------------------------|
| 访问顺序       | 后进先出 (FILO)         | 先进先出 (FIFO)           |
| 主要操作       | `push`, `pop`, `top`    | `enqueue`, `dequeue`, `front`, `back` |
| 常见实现       | `std::stack`, `std::vector`, `std::deque` | `std::queue`, `std::deque`, `std::list` |
| 典型应用场景   | 函数调用管理、表达式求值、深度优先搜索（DFS） | 任务调度、广度优先搜索（BFS）、缓冲区管理 |
| 时间复杂度     | 所有基本操作均为 $O(1)$ | 所有基本操作均为 $O(1)$ |

**选择使用栈还是队列取决于具体的应用场景**。如果需要按照后进先出的顺序处理元素，选择栈；如果需要按照先进先出的顺序处理元素，选择队列。
参考[这里(stack)](https://www.apiref.com/cpp-zh/cpp/header/stack.html)和[这里(queue)](https://www.apiref.com/cpp-zh/cpp/header/queue.html)。

### 堆

堆是一种树状数据结构。  
堆可以用来堆排序，时间复杂度$O(n \times \log(n))$。  
**基础操作：**  

1. 维护堆 把节点`p`放到正确位置。
2. 加入节点 把节点`p`加入堆顶部，再从下到上维护。
3. 堆顶部节点 不用多说
4. 删除顶部节点 删除顶部节点，再从上到下维护堆。
具体实现见[OIer_DFLJ](https://www.luogu.com/user/1228191)的[这篇文章](https://www.luogu.com/article/873rkxy1)。

---

## 结束语

本文为“智慧结晶”，由[$\texttt{\color{Blue}{INeedToLearnCoding}}$](https://www.luogu.com/user/1351963)编写。
