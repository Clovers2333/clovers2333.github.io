# 搜索与求解

## 搜索算法基础

搜索算法的形式化描述如下：

- 状态：$S$，表示问题的状态空间；
- 动作：$A$，表示问题的动作空间；
- 状态转移：$f(s, a)$，表示状态 $s$ 经过动作 $a$ 转移到的下一个状态；
- 路径/代价：$g(s, a)$，表示从初始状态到状态 $s$ 经过动作 $a$ 的代价；
- 目标测试：$goal(s)$，表示判断状态 $s$ 是否为目标状态。

### 评价指标
- 完备性：能否找到解（不一定最优）
- 最优性：能否保证找到的第一个解是最优解
- 时间复杂度（通过扩展的结点数量衡量）
- 空间复杂度（通过同时记录的结点数量衡量）

| 符号 | 含义                                       |
| ---- | ------------------------------------------ |
| b    | 分支因子，即搜索树中每个节点最大的分支数目 |
| d    | 根节点到最浅的目标结点的路径长度           |
| m    | 搜索树中路径的最大可能长度                 |
| n    | 状态空间中状态的数量                       |

### 算法框架
```assembly
F <- {根节点}
while F != ∅ do
    n <- pick_from(F)
    F <- F - {n}
    if goal_test(n) then
        return n.path
    end
    F <- F ∪ successor_nodes(n)
end
```

- pick_from 决定扩展结点的顺序，successor_nodes 决定哪些节点可被放入边缘集合（fringe set，也叫开表，open list）以在后面扩展（expand）
- 每次从边缘集合中取出最上层（最浅）的结点时是广度优先搜索（breadth first search，BFS）
- 每次从边缘集合中取出最下层（最深）的结点时是深度优先搜索（depth first search，DFS）
- 放弃扩展部分结点的做法称为剪枝（pruning）

## 启发式搜索

- 利用启发函数（heuristic function）和评价函数（evaluation function）来指导搜索的过程，以期望更快地找到解。
- 启发函数 $h(n)$，用来评估从结点 $n$ 到目标结点的代价，启发函数通常非负。
- 评价函数 $f(n)$，从当前节点出发，根据评价函数来选择后续节点。

### 贪婪最佳优先搜索

- 评价函数 $f(n) = h(n)$​，即选择启发函数值最小的结点进行扩展。
- 在避免环以后，就可以在有限时间内搜到解（具有完备性）
- 可能会搜不到最优解，因为局部最优。

### A*搜索

- 评价函数 $f(n) = g(n) + h(n)$，即选择启发函数值最小的结点进行扩展。
- 其中 $g(n)$ 为从初始结点到结点 $n$ 的代价。
- 保证找到的第一个解是最优解。

| 符号      | 含义                                       |
| --------- | ------------------------------------------ |
| h(n)      | 节点 n 的启发函数取值                      |
| g(n)      | 从起始节点到节点 n 所对应路径的代价        |
| f(n)      | 节点 n 的评价函数取值                      |
| c(n,a,n') | 从节点 n 执行动作 a 到达节点 n' 的单步代价 |
| h*(n)     | 从节点 n 出发到达终止节点的最小代价        |

#### 性能分析

一个良好的启发函数需要满足如下两种性质：

1. **可容性（admissible）**：
   
   - 对于任意结点 $n$，有 $h(n) \leq h^*(n)$。
   - 如果 $n$ 是目标结点，则有 $h(n) = 0$。
   - 可以这样理解满足可容性的启发函数：启发函数不会过高估计（over-estimate）从结点 $n$ 到终止结点所应该付出的代价（即估计代价小于等于实际代价）。

2. **一致性（consistency）**：
   
   - 启发函数的一致性指满足条件：
     $$
     h(n) \leq c(n, a, n') + h(n')
     $$
     这里 $c(n, a, n')$ 表示结点 $n$ 通过动作 $a$ 到达其相应的后继结点 $n'$ 的代价（三角不等式原则）。

**满足一致性条件的启发函数一定满足可容性条件！**

**如果启发函数是可容的，那么A*算法满足最优性**

## 对抗搜索

两个智能体，一个要最大化利益，一个要最小化利益。

### 最小最大搜索

略.

### Alpha-Beta 剪枝

略.

### 蒙特卡洛树搜索

在探索和经验之间选取一个平衡，进行搜索。

#### UCB 函数

$$
\text{UCB} = v_1 + C \cdot \sqrt{\frac{\ln N}{n_1}}
$$

其中 $v_1$ 是节点估计的值（比如胜率），$n_1$ 是节点被访问的次数，而 $N$ 则是其父节点已经被访问的总次数，$C$ 是可调整参数。

#### 基本步骤

- 选择（selection）：算法从搜索树的根节点开始，向下递归选择子节点直到到达叶子结点或者到达还具有未被扩展的子节点的节点 L，向下递归选择的过程可由 UCB1 算法来实现，在递归选择过程中记录下每个节点被选择的次数和每个节点得到的奖励均值
- 扩展（expansion）：如果节点 L 还不是一个终止节点，则随机扩展它的一个未被扩展过的后继边缘节点 M
- 模拟（simulation）：从节点 M 出发，模拟扩展搜索树，直到找到一个终止节点
- 反向传播（back propagation）：用模拟所得结果回溯更新模拟路径中 M 及以上节点的奖励均值和被访问次数