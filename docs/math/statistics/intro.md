# Introduction

## 概率论基本定义

### 概率空间
一个标准的**概率空间**由三元组 $(\Omega, \mathcal{F}, P)$ 定义，它为概率论提供了严格的公理化基础。

- **样本空间 $\Omega$**： 所有可能结果的集合。
- **事件域 $\mathcal{F}$ ($\sigma$-代数)**： $\Omega$ 的一些子集构成的集合，它满足：
    1. $\Omega \in \mathcal{F}$；
    2. 若 $A \in \mathcal{F}$，则其补集 $A^c \in \mathcal{F}$；
    3. 若可数个事件 $A_1, A_2, \dots \in \mathcal{F}$，且两两互不相容，则它们的并集 $\bigcup_{i} A_i \in \mathcal{F}$。
    $\mathcal{F}$ 中的元素称为**可测事件**，我们只为这些事件分配概率。
- **概率测度 $P$**： 定义在 $\mathcal{F}$ 上的一个函数，为每个事件 $A \in \mathcal{F}$ 分配一个实数 $P(A)$，且满足：
    1. **非负性**： 对于任意 $A \in \mathcal{F}$，有 $P(A) \ge 0$。
    2. **规范性**： $P(\Omega) = 1$。
    3. **可列可加性**： 对于 $\mathcal{F}$ 中任意两两互不相容的事件序列 $A_1, A_2, \dots$，有 $P\big(\bigcup_{i} A_i\big) = \sum_{i} P(A_i)$。

### 随机试验

一个实验如果满足以下三个条件，则称之为**随机试验**：

- 可以在相同条件下重复进行；
- 每次试验的可能结果不止一个，并且能事先明确所有可能结果；
- 进行一次试验之前，不能确定哪一个结果会出现。

### 样本空间
随机试验**所有可能结果**的集合，记为 $S$ 或 $\Omega$。

- **例子**：抛一枚硬币的样本空间 $S = \{\text{正面},\ \text{反面}\}$。

### 样本点
样本空间中的**每一个元素**，即随机试验的一个可能的基本结果。

### 随机事件
样本空间 $S$ 的一个**子集**，简称**事件**。通常用大写字母 A, B, C... 表示。

- **基本事件**：由一个样本点组成的单点集。
- **必然事件**：整个样本空间 $S$。
- **不可能事件**：不包含任何样本点的空集 $\varnothing$。

### 概率
即**概率测度 $P$**，是满足概率空间公理的函数。

### 条件概率
设 $A, B$ 是两个事件，且 $P(B) > 0$，称

$$
P(A\mid B) = \frac{P(A\cap B)}{P(B)}
$$

为在事件 $B$ 发生的条件下，事件 $A$ 发生的**条件概率**。

### 独立性
如果两个事件 $A, B$ 满足：

$$
P(A\cap B) = P(A)P(B)
$$

则称事件 $A$ 与 $B$ **相互独立**。

### 随机变量
定义在样本空间 $S$ 上的实值函数 $X = X(\omega),\ \omega \in S$。它表示随机试验的结果。

- **离散型随机变量**：所有可能取的值是有限个或可列无限个。
- **连续型随机变量**：可能取的值充满一个区间。

### 概率分布
描述随机变量取各个值的可能性大小的函数。

- **概率质量函数**：对于**离散型**随机变量 $X$，函数 $p(x_i) = P(X = x_i)$ 为其概率质量函数。
- **概率密度函数**：对于**连续型**随机变量 $X$，如果存在非负可积函数 $f(x)$，使得对任意实数 $a, b$ ($a < b$)，有 $P(a < X \le b) = \int_a^b f(x)\,dx$，则 $f(x)$ 称为 $X$ 的概率密度函数。

### 分布函数
设 $X$ 是一个随机变量，$x$ 是任意实数，函数
$$
F(x) = P(X \le x)
$$
称为 $X$ 的**分布函数**，也称为累积分布函数。

分布函数 $F$ 满足以下必要性质：

1. 极限性质：

$$
\lim_{x\to -\infty} F(x) = 0,\qquad\lim_{x\to +\infty} F(x) = 1.
$$

2. 非减性：对于任意 $x_1 < x_2$，有

$$
F(x_1) \le F(x_2).
$$

3. 右连续性：对于任意实数 $x_0$，有

$$
\lim_{x\to x_0^+} F(x) = F(x_0).
$$

补充说明：分布函数的左极限 $F(x_0^-)=\lim_{x\to x_0^-}F(x)$ 一般与 $F(x_0)$ 不相等，二者的差表示 $X$ 在点 $x_0$ 的质量（点概率）：

$$
P(X = x_0) = F(x_0) - F(x_0^-).
$$

### 数学期望
随机变量 $X$ 的**平均值**或“中心位置”的度量。

- **离散型**：$E(X) = \sum_{i} x_i p(x_i)$，若级数绝对收敛。
- **连续型**：$E(X) = \int_{-\infty}^{\infty} x f(x)\,dx$，若积分绝对收敛。

### 方差
度量随机变量与其数学期望的偏离程度。

$$
D(X) = \operatorname{Var}(X) = E\big( [X - E(X)]^2 \big) = E(X^2) - [E(X)]^2
$$

方差的算术平方根称为**标准差**。

### 矩与中心矩

对于随机变量 $X$，定义其 **$k$ 阶（原点）矩**为：

$$
M_k := E[X^k].
$$

特别地，$M_1 = E[X]$ 就是期望（通常简记为 $\mu$）。

**$k$ 阶中心矩**定义为：

$$
C_k := E\big[(X - \mu)^k\big].
$$

中心矩度量随机变量相对于其均值的偏离程度。

**MGF（矩母函数）**：

$$
M(t) = E[\exp(tX)].
$$

矩的存在性：若 $E[|X|^k] < \infty$，则称 $k$ 阶矩存在。高阶矩可能不存在（如 Cauchy 分布连一阶矩都不存在）。

??? note "矩的深入理解与应用"
    
    矩在整个课程中会反复出现，这里预览一些核心思想：（以下都是机翻，还没整理）

    **1. 矩是分布的摘要**
    
    - **均值与方差**：我们已经广泛使用了均值和方差。在高层次上，中心矩可以告诉你概率质量如何围绕均值分布。
    
    - **偏度（Skewness）**：对于任何对称分布，第三中心矩为零。一般来说，如果第三中心矩非零，说明分布是偏斜的，其符号告诉你它偏向均值的哪一侧。
      
      例如，高斯分布的第三中心矩为零。
    
    - **峰度（Kurtosis）**：标准高斯分布的第四中心矩为 3。第四中心矩大于 3 的分布称为**尖峰态（leptokurtic）**（比高斯在均值附近更尖峰），小于 3 的称为**扁峰态（platykurtic）**（比高斯更平坦）。
    
    - **矩的比较**：解释高阶矩的一个典型方法是将它们与相应的高斯矩进行比较。
    
    **2. 矩可用于推理收敛性**
    
    我们不会在课程中过多涉及，但证明中心极限定理的一个典型方法是证明：
    
    $$
    \frac{\sqrt{n}(\hat{\mu} - \mu)}{\sigma}
    $$
    
    的所有矩收敛到标准正态分布的相应矩。然后诉诸这样一个事实：矩收敛 + 一些正则性条件 $\Longrightarrow$ 分布收敛。
    
    从概念上讲，这"降低"了证明数字序列（即矩）收敛以证明分布收敛。
    
    **3. 矩给出估计量**
    
    我们稍后会再次看到这一点，但作为一个简单的例子：
    
    假设 $X_1, \dots, X_n$ 从高斯分布 $\mathcal{N}(\mu, \sigma^2)$ 中独立同分布抽取，我想估计高斯的参数 $\mu$ 和 $\sigma$。一个简单（且非常通用）的策略是匹配矩。我们可以计算：
    
    $$
    E(X) = \mu, \quad E(X^2) = \sigma^2 + \mu^2.
    $$
    
    样本矩为：
    
    $$
    \bar{E}(X) = \frac{1}{n}\sum_{i=1}^n X_i, \quad \bar{E}(X^2) = \frac{1}{n}\sum_{i=1}^n X_i^2,
    $$
    
    因此我们可以尝试求解方程：
    
    $$
    \bar{E}(X) = \mu, \quad \bar{E}(X^2) = \sigma^2 + \mu^2,
    $$
    
    以获得 $\mu$ 和 $\sigma$ 的估计。这是提出参数估计量的一般方法之一，即"匹配"样本矩（易于估计）与未知分布的矩。
    
    **4. 矩给出尾界**
    
    我们已经看到了 Chebyshev 不等式：
    
    $$
    P(|X - \mu| \ge t\epsilon) \le \frac{1}{t^2}.
    $$
    
    为了推导 Chebyshev 不等式，我们对数据取平方并使用了 Markov 不等式。我们也可以取更高的幂来得到：
    
    $$
    P(|X - \mu| \ge t) = P(|X - \mu|^k \ge t^k) \le \frac{E|X - \mu|^k}{t^k}.
    $$
    
    因此，如果我们的分布具有较小的高阶中心矩，那么通过在上述表达式中选择更高的 $k$ 值，我们通常会获得更紧的控制。
    
    **Chernoff 方法**：这种技术的一个更常用（特别是用于显示指数集中）的改进称为 **Chernoff 技术**。假设我们首先对随机变量进行中心化（减去其均值）。然后观察到：
    
    $$
    P(X \ge t) = P(\exp(uX) \ge \exp(ut)) \le \frac{E\exp(uX)}{\exp(ut)}.
    $$
    
    在上述表达式中，$E\exp(uX)$ 就是 $X$ 的 MGF，$u$ 是我们可以选择的自由参数以使界变紧。
    
    **Chernoff 技术的说明**：对于均值为 0 的高斯随机变量，我们可以计算 MGF：
    
    $$
    E(\exp(uX)) = \exp(\sigma^2 u^2/2),
    $$
    
    因此 Chernoff 方法给出：
    
    $$
    P(X \ge t) \le \frac{\exp(\sigma^2 u^2/2)}{\exp(ut)} = \exp(\sigma^2 u^2/2 - ut).
    $$
    
    右侧通过选择 $u = t/\sigma^2$ 最小化，这给出界：
    
    $$
    P(X \ge t) \le \exp(-t^2/(2\sigma^2)).
    $$
    
    这是显示高斯具有指数尾的一种更简单的方法。我还要补充一个重要说明：
    
    **亚高斯随机变量**：上述练习的一个简单结论是，如果某个（不一定是高斯的）随机变量 $Y$ 的 MGF 被高斯的 MGF 支配，即如果某个（不一定是高斯的）随机变量 $Y$ 满足：
    
    $$
    E(\exp(uY)) \le \exp(\sigma^2 u^2/2),
    $$
    
    对于某个 $\sigma$ 值，那么它也会满足尾界：
    
    $$
    P(Y \ge t) \le \exp(-t^2/(2\sigma^2)).
    $$
    
    事实证明，一大类随机变量实际上满足上述条件：它们被称为**亚高斯随机变量**，包括有界随机变量等。
    
    **高层次理解**：MGF 可用于理解随机变量的尾部，重要的是，如果 MGF 较小，则随机变量具有较轻的尾部。因此，矩再次帮助我们理解分布的尾部。

## 一些常见的分布
下面列出一些常见的离散与连续分布及其概率质量函数（pmf）或概率密度函数（pdf）：

### 离散分布

- **离散均匀分布（Discrete uniform）**：在 $k$ 个类别 $\{x_1, x_2, \dots, x_k\}$ 上
    $$
    p_X(x) = \frac{1}{k},\qquad x\in\{x_1,\dots,x_k\}.
    $$
    - 期望：$E(X) = \frac{1}{k}\sum_{i=1}^k x_i$
    - 方差：$\operatorname{Var}(X) = \frac{1}{k}\sum_{i=1}^k x_i^2 - \left(\frac{1}{k}\sum_{i=1}^k x_i\right)^2$

- **Bernoulli 分布**：表示一次带偏置的掷币（取值 $0,1$），记作 $\operatorname{Ber}(p)$：
    $$
    p_X(x)=p^x(1-p)^{1-x},\qquad x\in\{0,1\}.
    $$
    - 期望：$E(X) = p$
    - 方差：$\operatorname{Var}(X) = p(1-p)$

- **Binomial 分布**：$n$ 次独立伯努利试验中成功次数，记作 $\operatorname{Bin}(n,p)$：
    $$
    p_X(x)=\binom{n}{x}p^x(1-p)^{n-x},\qquad x=0,1,\dots,n.
    $$
    - 期望：$E(X) = np$
    - 方差：$\operatorname{Var}(X) = np(1-p)$

- **几何分布**（第一成功次数，取值从 1 开始），记作 $\operatorname{Geom}(p)$：
    $$
    p_X(x)=p(1-p)^{x-1},\qquad x=1,2,\dots.
    $$
    - 期望：$E(X) = \frac{1}{p}$
    - 方差：$\operatorname{Var}(X) = \frac{1-p}{p^2}$

- **泊松分布（Poisson）**：平均值为 $\lambda$，记作 $\operatorname{Pois}(\lambda)$：
    $$
    p_X(x)=\frac{\lambda^x e^{-\lambda}}{x!},\qquad x=0,1,2,\dots.
    $$
    - 期望：$E(X) = \lambda$
    - 方差：$\operatorname{Var}(X) = \lambda$

### 连续分布

- **连续均匀分布（Uniform）**：在区间 $[a,b]$ 上：
    
    \begin{equation}
    f_X(x)=\begin{cases}\dfrac{1}{b-a},&x\in[a,b], \\ 0,&\text{otherwise.}\end{cases}
    \end{equation}
    
    - 期望：$E(X) = \frac{a+b}{2}$
    - 方差：$\operatorname{Var}(X) = \frac{(b-a)^2}{12}$

- **正态（高斯）分布**：具有位置参数 $\mu$（均值）和尺度参数 $\sigma$（标准差），记作 $\mathcal{N}(\mu,\sigma^2)$，其 pdf 为
    $$
    f_X(x)=\frac{1}{\sqrt{2\pi}\,\sigma}\exp\left(-\frac{(x-\mu)^2}{2\sigma^2}\right).
    $$
    - 期望：$E(X) = \mu$
    - 方差：$\operatorname{Var}(X) = \sigma^2$

- **Cauchy 分布**：具有位置参数 $x_0$ 和尺度参数 $\gamma > 0$，记作 $\operatorname{Cauchy}(x_0, \gamma)$，其 pdf 为
    $$
    f_X(x) = \frac{1}{\pi\gamma\left[1 + \left(\frac{x-x_0}{\gamma}\right)^2\right]}.
    $$
    - 期望：**不存在**（积分不收敛）
    - 方差：**不存在**
    
    **注**：标准 Cauchy 分布对应 $x_0=0, \gamma=1$。Cauchy 分布是一个经典例子，说明并非所有分布都有有限的期望和方差。其"厚尾"特性使得一阶矩及更高阶矩均不存在。

## 随机变量的变换

### 问题描述
设随机变量 $X$ 有 pdf/pmf $f_X$ 和 CDF $F_X$，对于某个函数 $r$，定义 $Y = r(X)$（如 $Y = X^2$ 或 $Y = e^X$）。如何计算 $Y$ 的 pdf/pmf 或 CDF？

### 离散情况
若 $X$ 是离散型随机变量，则 $Y$ 也是离散型，其 pmf 为：

$$
p_Y(y) = P(Y = y) = P(r(X) = y) = P(X \in r^{-1}(y)) = \sum_{x: r(x)=y} p_X(x).
$$

**示例**：若 $X \in \{-1, 0, 1\}$，概率分别为 $1/4, 1/2, 1/4$，令 $Y = X^2$，则 $Y$ 取值 $\{0, 1\}$，且 $p_Y(0) = 1/2$，$p_Y(1) = 1/2$。

### 连续情况（可逆变换）
若变换 $r$ 可逆，记 $s = r^{-1}$，则 $Y$ 的 pdf 为：

$$
f_Y(y) = f_X(s(y)) \left|\frac{ds(y)}{dy}\right|,
$$

其中 $\left|\frac{ds(y)}{dy}\right|$ 称为**雅可比（Jacobian）**，修正变换导致的区间伸缩。

**示例**：若 $X \sim U[0,1]$，$Y = X^2$。由 $s(y) = \sqrt{y}$，得：

$$
f_Y(y) = f_X(\sqrt{y}) \cdot \frac{1}{2\sqrt{y}} = \frac{1}{2\sqrt{y}}, \quad y \in [0, 1].
$$

**直观解释**：对于很小的 $\Delta$，有

$$
P(Y \in [y_0 - \Delta, y_0 + \Delta]) \approx f_Y(y_0) \cdot 2\Delta.
$$

而 $Y \in [y_0 - \Delta, y_0 + \Delta]$ 等价于 $X \in [s(y_0 - \Delta), s(y_0 + \Delta)]$，即

$$
P(Y \in [y_0 - \Delta, y_0 + \Delta]) = P\left(X \in \left[s(y_0) - \Delta\frac{ds(y_0)}{dy}, s(y_0) + \Delta\frac{ds(y_0)}{dy}\right]\right) \approx f_X(s(y_0)) \cdot 2\Delta\left|\frac{ds(y_0)}{dy}\right|.
$$

比较两式即得上述公式。

## 期望的不等式

在概率论中，经常需要对某些期望进行上界估计。以下两个不等式非常有用：

### Cauchy-Schwarz 不等式

对于任意两个随机变量 $X, Y$（假设相应的期望存在），有：

$$
E[XY] \le \sqrt{E[X^2]E[Y^2]}.
$$

**应用**：可以用来证明相关系数 $\rho(X,Y) = \frac{\operatorname{Cov}(X,Y)}{\sigma_X \sigma_Y}$ 满足 $|\rho(X,Y)| \le 1$。

??? Info "Answer"

    使用 Cauchy-Schwarz 不等式验证两个随机变量之间的相关系数被限制在 $[-1, 1]$ 之间。

    设 $X, Y$ 是两个随机变量，均值分别为 $\mu_X, \mu_Y$，标准差分别为 $\sigma_X, \sigma_Y$。定义中心化随机变量：

    $$
    X' = X - \mu_X, \quad Y' = Y - \mu_Y.
    $$

    则协方差 $\operatorname{Cov}(X,Y) = E[X'Y']$，且 $E[(X')^2] = \sigma_X^2$，$E[(Y')^2] = \sigma_Y^2$。

    对 $X'$ 和 $Y'$ 应用 Cauchy-Schwarz 不等式：

    $$
    |E[X'Y']| \le \sqrt{E[(X')^2]E[(Y')^2]} = \sqrt{\sigma_X^2 \sigma_Y^2} = \sigma_X \sigma_Y.
    $$

    因此：

    $$
    |\operatorname{Cov}(X,Y)| \le \sigma_X \sigma_Y.
    $$

    两边同时除以 $\sigma_X \sigma_Y$（假设方差非零），得：

    $$
    \left|\frac{\operatorname{Cov}(X,Y)}{\sigma_X \sigma_Y}\right| \le 1,
    $$

    即 $|\rho(X,Y)| \le 1$，也就是 $-1 \le \rho(X,Y) \le 1$。

    **等号成立的条件**：当且仅当 $X'$ 和 $Y'$ 线性相关时等号成立，即存在常数 $a$ 使得 $Y' = aX'$（或 $Y - \mu_Y = a(X - \mu_X)$）。此时 $\rho = \pm 1$（符号取决于 $a$ 的正负）。

### Jensen 不等式

首先回顾**凸函数**的定义：函数 $g$ 是凸函数，当且仅当对任意 $x, y$ 和 $\alpha \in [0,1]$，有：

$$
g(\alpha x + (1-\alpha)y) \le \alpha g(x) + (1-\alpha)g(y).
$$

几何上，凸函数的任意两点之间的连线完全位于函数曲线的上方。

**Jensen 不等式**：对于凸函数 $g$ 和随机变量 $X$（假设相应的期望存在），有：

$$
g(E[X]) \le E[g(X)].
$$

**直观理解**：Jensen 不等式实际上是凸性定义在概率测度下的推广。

**常见应用**：

- 由于 $g(x) = x^2$ 是凸函数，有 $(E[X])^2 \le E[X^2]$，即 $\operatorname{Var}(X) = E[X^2] - (E[X])^2 \ge 0$。
- 由于 $g(x) = e^x$ 是凸函数，有 $e^{E[X]} \le E[e^X]$（矩母函数的性质）。
- 由于 $g(x) = -\log x$ 是凸函数（$x > 0$），有 $-\log(E[X]) \le E[-\log X]$，即 $\log(E[X]) \ge E[\log X]$。

## 矩母函数

### 定义
随机变量 $X$ 的**矩母函数（Moment Generating Function, MGF）**定义为：

$$
M_X(t) = E[e^{tX}].
$$

一般情况下，MGF 可能不存在（与期望类似），并且对于较大的 $t$ 值可能发散。我们通常只在 $t$ 的某个邻域（包含 0）内考虑 MGF。

### 为什么叫"矩母函数"

MGF 的名称来源于它在 $t=0$ 处的导数可以给出各阶矩。具体地：

$$
M'_X(0) = \left.\frac{d}{dt}E[e^{tX}]\right|_{t=0} = E\left[\frac{d}{dt}e^{tX}\right]_{t=0} = E[X e^{tX}]|_{t=0} = E[X].
$$

类似地，$k$ 阶导数给出 $k$ 阶矩：

$$
M_X^{(k)}(0) = E[X^k].
$$

因此，MGF 包含了随机变量的所有矩信息。

### 重要性质

**性质 1：独立随机变量和的 MGF**

若 $X_1, \dots, X_n$ 独立，且 $Y = \sum_{i=1}^n X_i$，则：

$$
M_Y(t) = \prod_{i=1}^n M_{X_i}(t).
$$

**证明**：由独立性，$E[e^{t(X_1+\cdots+X_n)}] = E[e^{tX_1} \cdots e^{tX_n}] = E[e^{tX_1}] \cdots E[e^{tX_n}]$。

这个性质使得计算独立随机变量和的各阶矩变得非常容易。

**性质 2：MGF 的唯一性**

若两个随机变量 $X$ 和 $Y$ 的 MGF 在 0 的某个邻域内存在且相等，则 $X$ 和 $Y$ 具有相同的分布。

这意味着 MGF 完全决定了分布。

### 例子

**例 1：Bernoulli 分布**

设 $X \sim \operatorname{Ber}(p)$，计算其 MGF 并用它求期望。

直接计算：

$$
M_X(t) = E[e^{tX}] = p e^t + (1-p) e^0 = pe^t + (1-p).
$$

求一阶导数并在 $t=0$ 处求值：

$$
M'_X(0) = (pe^t)|_{t=0} = p.
$$

即 $E[X] = p$，与我们已知的结果一致。

**例 2：指数分布**

设 $X$ 服从参数为 $\lambda$ 的指数分布（均值为 $1/\lambda$），其 pdf 为：

$$
f_X(x) = \lambda e^{-\lambda x}, \quad x \ge 0.
$$

计算 MGF（当 $t < \lambda$ 时）：

$$
M_X(t) = \int_0^\infty e^{tx} \lambda e^{-\lambda x}\,dx = \int_0^\infty \lambda e^{(t-\lambda)x}\,dx = \frac{\lambda}{\lambda - t}, \quad t < \lambda.
$$

注意：当 $t \ge \lambda$ 时，积分发散，MGF 不存在。

