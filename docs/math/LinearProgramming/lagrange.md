# 拉格朗日对偶

> 构造对偶的目的是什么？
>
> Lagrange 对偶通过对偶函数，构造了一个下界（弱对偶），然后对于下界取 max，得到一个上界，当上界和原函数相等时（强对偶），我们就得到了我们想要的东西。
>
> 从这种意义上来说，拉格朗日对偶构造了线性规划的对偶。

## 共轭函数

共轭函数 (Conjugate Function) 是凸分析中的一个重要概念，它为给定的函数 $f$ 定义了一个新的函数 $f^*$，提供了关于 $f$ 的另一种几何和代数视角。

### 定义

!!! Definition "共轭函数"

    给定函数 $f: \mathbb{R}^n \to \mathbb{R}$，其共轭函数 $f^*: \mathbb{R}^n \to \mathbb{R}$ 定义为：

    \begin{align*}
    f^*(y) = \sup_{x \in \text{dom} f} \left( y^T x - f(x) \right)
    \end{align*}

    其中，$\text{dom} f$ 是 $f$ 的定义域，$y$ 是一个向量。

### 几何意义

共轭函数 $f^*(y)$ 可以理解为：$f^*(y)$ 是 $f$ 的所有斜率为 $y$ 的支撑超平面的截距的最小值的负数。

### 性质

1. **凸性**：无论 $f$ 是否是凸函数，$f^*$ 始终是凸函数（因为它是仿射函数的逐点上确界）。

2. **Fenchel 不等式**：对于任意 $x$ 和 $y$，有：

\begin{align*}
f(x) + f^*(y) \geq y^T x
\end{align*}

3. **双共轭**：如果 $f$ 是闭凸函数（即下半连续且凸），则 $f^{**} = f$。这意味着共轭操作是对合的。

4. **可微性**：如果 $f$ 是严格凸且可微的，则 $f^*$ 的梯度与 $f$ 的梯度互为逆映射，即：

\begin{align*}
\nabla f^*(y) = \arg \max_x \left( y^T x - f(x) \right)
\end{align*}

5. **线性变换下的共轭**：如果 $g(x) = f(Ax + b)$，则 $g^*(y) = f^*(A^{-T} y) - b^T A^{-T} y$。

## Lagrange 对偶

我们熟悉 Lagrange 乘子法，它是求解带有等式约束的最值问题的方法，而 Lagrange 对偶函数可以理解为是它的一种拓展，它能解决带有等式和不等式约束的问题，也就是标准形式的优化问题。标准形式的优化问题形如

\begin{align*}
&\min f_0(x) \\
&s.t. \begin{cases}
f_i(x) \leq 0, i = 1, \ldots, m \\
h_j(x) = 0, j = 1, \ldots, p
\end{cases}
\end{align*}

设问题的定义域 $D = \cap_{i=0}^m \text{dom } f_i \cap \cap_{j=1}^p \text{dom } h_j$ 非空，最优值为 $p^*$。

定义 Lagrange 对偶函数 $g: \mathbb{R}^m \times \mathbb{R}^p \to \mathbb{R}$，对 $\lambda \in \mathbb{R}^m, v \in \mathbb{R}^p$ 有

\begin{align*}
g(\lambda, v) = \inf_{x \in D} L(x, \lambda, v) = \inf_{x \in D} \left( f_0(x) + \sum_{i=1}^m \lambda_i f_i(x) + \sum_{j=1}^p v_j h_j(x) \right)
\end{align*}

如果 Lagrange 函数关于 x 无下界，那么对偶函数取值为 $-\infty$。无论原问题的凸性如何，Lagrange 对偶函数都是凹函数。因为它是一族关于 $\lambda, v$ 的线性函数的逐点下确界。

对偶函数值构成了原问题最优值的下界，即对于任意的 $\lambda \geq 0$ 和 $v$，有

\begin{align*}
g(\lambda, v) \leq p^*
\end{align*}

证明是显然的。

## Lagrange 对偶问题

前面提到对偶函数值给出原问题最优值的下界，那么我们希望得到最优的下界，也就是

\begin{align*}
&\max g(\lambda, v) \\
&s.t. \lambda \geq 0
\end{align*}

这是一个凸优化问题，它的凸性与原问题的凸性无关。

所以本质上，我们就是要求 $\max_{\lambda,v} \min_x L(x, \lambda, v)$，设计一个惩罚函数，使得 $L$ 取最小值最困难。

## 强对偶性与 Slater 条件

如果 Lagrange 对偶问题的最优解是 $d^*$，我们知道一定有 $d^* \leq p^*$。在一些情况下，有 $d^* = p^*$，那么此时强对偶性成立。

如果原问题是凸的，那么在一些情况下（并不是所有情况）强对偶性会成立。一个简单的约束准则是 Slater 条件，当原问题是凸的且 Slater 条件成立，那么强对偶性成立。对于如下形式的凸问题

\begin{align*}
&\min f_0(x) \\
&s.t. \begin{cases}
f_i(x) \leq 0, i = 1, \ldots, m \\
Ax = b
\end{cases}
\end{align*}

Slater 条件说的是存在一点 $x \in \text{relint } D$，也就是 D 的相对内点，使下式成立

\begin{align*}
f_i(x) < 0, i = 1, \ldots, m, \quad Ax = b
\end{align*}

而在不等式约束 $f_i$ 中，如果其中一些是仿射函数（线性函数）时，Slater 条件可以进一步的改进。如果最前面的 k 个约束 $f_1, \ldots, f_k$ 是仿射的，则 Slater 条件可以改进为存在一点 $x \in \text{relint } D$ 使得

\begin{align*}
f_i(x) \leq 0, i = 1, \ldots, k, \quad f_i(k) < 0, i = k+1, \ldots, m, \quad Ax = b
\end{align*}

也就是说其中仿射的不等式约束是不需要严格成立的。

## 线性规划对偶问题

根据 Slater 条件，我们不难发现，对于任意的线性规划问题，只要是原问题是可行的，就满足（改进）Slater 条件，也就是强对偶性成立。因此研究线性规划问题的对偶问题是很有价值的。另一方面研究线性规划的对偶问题对我们后续的处理、解释等都是有好处的。

### 标准形式线性规划

对于标准形式的线性规划，即

\begin{align*}
&\min c^T x \\
&s.t. \begin{cases}
Ax = b \\
x \geq 0
\end{cases}
\end{align*}

它的 Lagrange 函数为

\begin{align*}
L(x, \lambda, v) = c^T x - \lambda^T x + v^T (Ax - b) = -b^T v + (c + A^T v - \lambda)^T x
\end{align*}

所以它的 Lagrange 对偶函数为

\begin{align*}
g(\lambda, v) = \inf_x L(x, \lambda, v) = -b^T v + \inf_x (c + A^T v - \lambda)^T x
\end{align*}

我们知道非常函数的仿射函数是无界的，因此

\begin{align*}
g(\lambda, v) = \begin{cases}
-b^T v, A^T v - \lambda + c = 0 \\
-\infty, A^T v - \lambda + c \neq 0
\end{cases}
\end{align*}

所以它的对偶问题就为

\begin{align*}
&\max -b^T v \\
&s.t. \begin{cases}
A^T v - \lambda + c = 0 \\
\lambda \geq 0
\end{cases}
\end{align*}

它等价于

\begin{align*}
&\max -b^T v \\
&s.t. A^T v + c \geq 0
\end{align*}

### 不等式形式线性规划

不等式形式线性规划即

\begin{align*}
&\min c^T x \\
&s.t. Ax \leq b
\end{align*}

它的 Lagrange 函数为

\begin{align*}
L(x, \lambda) = c^T x + \lambda^T (Ax - b) = -b^T \lambda + (A^T \lambda + c)^T x
\end{align*}

所以它的 Lagrange 对偶函数为

\begin{align*}
g(\lambda) = \inf_x L(x, \lambda) = -b^T \lambda + \inf_x (A^T \lambda + c)^T x
\end{align*}

我们知道，对于一个仿射函数，如果不是常函数，那么它是无界的，因此

\begin{align*}
g(\lambda) = \begin{cases}
-b^T \lambda, A^T \lambda + c = 0 \\
-\infty, A^T \lambda + c \neq 0
\end{cases}
\end{align*}

所以它的对偶问题就为

\begin{align*}
&\max -b^T \lambda \\
&s.t. \begin{cases}
A^T \lambda + c = 0 \\
\lambda \geq 0
\end{cases}
\end{align*}

可以看到，标准形式线性规划的对偶问题是只含有不等式约束的线性规划，只含有不等式约束的线性规划的对偶问题是标准形式的线性规划。

### 一般形式的线性规划

对于一般形式的线性规划，也就是

\begin{align*}
&\min c^T x + d \\
&s.t. \begin{cases}
Gx \leq h \\
Ax = b
\end{cases}
\end{align*}

它的 Lagrange 函数为

\begin{align*}
L(x, \lambda, v) = c^T x + d + \lambda^T (Gx - h) + v^T (Ax - b) = -b^T v - h^T \lambda + d + \left( c + A^T v + G^T \lambda \right)^T x
\end{align*}

所以它的 Lagrange 对偶函数为

\begin{align*}
g(\lambda, v) = \inf_x L(x, \lambda, v) = -b^T v - h^T \lambda + d + \inf_x \left( c + A^T v + G^T \lambda \right)^T x
\end{align*}

利用仿射函数的性质，得

\begin{align*}
g(\lambda, v) = \begin{cases}
-b^T v - h^T \lambda + d, c + A^T v + G^T \lambda = 0 \\
-\infty, c + A^T v + G^T \lambda \neq 0
\end{cases}
\end{align*}

所以它的对偶问题为

\begin{align*}
&\max -b^T v - h^T \lambda + d \\
&s.t. \begin{cases}
c + A^T v + G^T \lambda = 0 \\
\lambda \geq 0
\end{cases}
\end{align*}


