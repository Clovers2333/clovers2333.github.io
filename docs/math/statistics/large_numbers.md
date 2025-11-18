# Laws of Large Numbers and Central Limit Theorem

## Basic Inequalities

### Markov 不等式

对于非负随机变量 $X$（假设 $E[X]$ 存在），对任意 $t \ge 0$：

$$
P(X > t) \le \frac{E[X]}{t}.
$$

等价形式：$P(X > t \cdot E[X]) \le \frac{1}{t}$。

**直观含义**：正随机变量不太可能远大于其期望值。

**证明**：对于 $X \ge 0$，

$$
E[X] = \int_0^\infty x f_X(x)\,dx = \int_0^t x f_X(x)\,dx + \int_t^\infty x f_X(x)\,dx \ge \int_t^\infty x f_X(x)\,dx \ge t \int_t^\infty f_X(x)\,dx = t P(X \ge t).
$$

### Chebyshev 不等式

设 $E[X] = \mu_X$，$\operatorname{Var}(X) = \sigma_X^2$（假设均存在），则对任意 $k \ge 0$：

$$
P(|X - \mu_X| > k\sigma_X) \le \frac{1}{k^2}.
$$

**直观含义**：随机变量偏离均值超过 $k$ 个标准差的概率至多为 $1/k^2$。例如，偏离超过 3 个标准差的概率至多 $1/9 \approx 11\%$。

**证明**：对非负随机变量 $(X - \mu_X)^2$ 应用 Markov 不等式：

$$
P(|X - \mu_X| \ge k\sigma_X) = P((X - \mu_X)^2 \ge k^2\sigma_X^2) \le \frac{E[(X - \mu_X)^2]}{k^2\sigma_X^2} = \frac{\sigma_X^2}{k^2\sigma_X^2} = \frac{1}{k^2}.
$$

**应用示例**：若 $n$ 个独立同分布随机变量的平均值 $\bar{Y}$ 满足 $E[\bar{Y}] = 0$，$\operatorname{Var}(\bar{Y}) = 1/n$，则：

$$
P\left(|\bar{Y}| > \frac{k}{\sqrt{n}}\right) \le \frac{1}{k^2}.
$$

这表明 $n$ 个独立随机变量的平均值通常距其期望约 $1/\sqrt{n}$ 的距离。

**关于 Chebyshev 不等式的两个值得注意的方面**：

1. **典型偏离量**为 $\sigma_X$ 量级，**概率界**为 $1/k^2$。
2. 通过更精细的假设（如矩母函数或更高阶矩的存在），可以改进概率界，但偏离量始终在 $\sigma_X$ 量级。例如，**指数集中不等式**可以将概率改进到 $\exp(-k^2)$ 的形式。

## Exponential Concentration Inequalities

### Mill's 不等式

首先考虑随机变量 $X_1, \dots, X_n \sim \mathcal{N}(\mu, \sigma^2)$ 独立同分布的正态情况。

**高斯线性组合的性质**：高斯随机变量的线性组合仍是高斯分布，因此样本均值

$$
Y = \frac{1}{n}\sum_{i=1}^n X_i
$$

也服从正态分布，特别地，$Y \sim \mathcal{N}(\mu, \sigma^2/n)$。

**Mill's 不等式**：设 $Z \sim \mathcal{N}(0,1)$，则对任意 $t > 0$：

$$
P(|Z| > t) \le \sqrt{\frac{2}{\pi}} \frac{\exp(-t^2/2)}{t}.
$$

**关键观察**：偏离量仍在标准差量级，但概率是**指数衰减**的，远强于 Chebyshev 不等式的 $1/t^2$。

**应用于样本均值**：若 $Y \sim \mathcal{N}(\mu, \sigma^2/n)$，则标准化变量

$$
Z := \frac{Y - \mu}{\sigma/\sqrt{n}}
$$

服从 $\mathcal{N}(0,1)$。应用 Mill's 不等式得：

$$
P\left(|Y| > \frac{t\sigma}{\sqrt{n}}\right) \le \sqrt{\frac{2}{\pi}} \frac{\exp(-t^2/2)}{t}.
$$

与 Chebyshev 不等式对比：

$$
P\left(|Y| > \frac{t\sigma}{\sqrt{n}}\right) \le \frac{1}{t^2},
$$

对于大 $t$ 值，指数界远优于多项式界。

??? note "Mill's 不等式的证明"
    使用标准正态密度 $f_Z(x) = \frac{1}{\sqrt{2\pi}}\exp(-x^2/2)$：

    $$
    \begin{align}
    P(|Z| > t) &= \frac{2}{\sqrt{2\pi}} \int_t^\infty \exp\left(-\frac{x^2}{2}\right)dx \\
    &= \sqrt{\frac{2}{\pi}} \int_t^\infty \frac{t}{x} \exp\left(-\frac{x^2}{2}\right)dx \\
    &\le \sqrt{\frac{2}{\pi}} \int_t^\infty t \exp\left(-\frac{x^2}{2}\right)dx \\
    &= \sqrt{\frac{2}{\pi}} \frac{1}{t} \int_{x^2/2}^{\infty} \exp(-u)\,du = \sqrt{\frac{2}{\pi}} \frac{\exp(-t^2/2)}{t}.
    \end{align}
    $$

### Hoeffding's 不等式

Mill's 不等式的主要缺点是仅适用于高斯随机变量。**Hoeffding's 不等式**是一个更通用的指数集中不等式，适用于有界随机变量。

**Hoeffding's 不等式**：设 $X_1, \dots, X_n$ 独立，且 $a_i \le X_i \le b_i$，$E[X_i] = 0$。则对任意 $t > 0$：

$$
P\left(\frac{1}{n}\sum_{i=1}^n X_i \ge t\right) \le \exp\left(-\frac{2n^2t^2}{\sum_{i=1}^n(b_i - a_i)^2}\right).
$$

等价形式：

$$
P\left(\left|\frac{1}{n}\sum_{i=1}^n X_i\right| \ge t\sqrt{\frac{\sum_{i=1}^n(b_i - a_i)^2}{n^2}}\right) \le 2\exp(-2t^2).
$$

**关键观察**：

- 虽然不再要求标准差的精确值，但若 $a_i < X_i < b_i$，则 $\operatorname{Var}(X_i) \le (b_i - a_i)^2$。
- 偏离量仍在 $1/\sqrt{n}$ 量级，但右侧是 $\exp(-t^2)$ 而非 $1/t^2$。

**应用示例**：设有 $n$ 次抛硬币，每次随机变量在 $-1$ 和 $1$ 之间。由 Hoeffding's 不等式：

$$
P\left(|Y| \ge \frac{2t}{\sqrt{n}}\right) \le 2\exp(-2t^2).
$$

这个不等式与 Chebyshev 不等式在左侧相似（偏离量为 $1/\sqrt{n}$ 量级），但右侧是 $\exp(-t^2)$ 而非 $1/t^2$，在实际应用中更有用。

??? note "Hoeffding's 不等式的证明"
    证明的关键是使用 **Hoeffding's 引理**和矩母函数技术。

    **Hoeffding's 引理**：若 $X$ 满足 $a \le X \le b$ 且 $E[X] = 0$，则对任意 $s > 0$：

    $$
    E[e^{sX}] \le \exp\left(\frac{s^2(b-a)^2}{8}\right).
    $$

    **证明主定理**：设 $S_n = \sum_{i=1}^n X_i$，我们要证明 $P(S_n \ge nt) \le \exp\left(-\frac{2n^2t^2}{\sum_{i=1}^n(b_i - a_i)^2}\right)$。

    对任意 $s > 0$，使用 Markov 不等式：

    $$
    P(S_n \ge nt) = P(e^{sS_n} \ge e^{snt}) \le \frac{E[e^{sS_n}]}{e^{snt}}.
    $$

    由独立性：

    $$
    E[e^{sS_n}] = E\left[\prod_{i=1}^n e^{sX_i}\right] = \prod_{i=1}^n E[e^{sX_i}].
    $$

    应用 Hoeffding's 引理到每个 $X_i$（已中心化，即 $E[X_i] = 0$）：

    $$
    E[e^{sX_i}] \le \exp\left(\frac{s^2(b_i - a_i)^2}{8}\right).
    $$

    因此：

    $$
    P(S_n \ge nt) \le \frac{\exp\left(\frac{s^2}{8}\sum_{i=1}^n(b_i - a_i)^2\right)}{e^{snt}} = \exp\left(\frac{s^2}{8}\sum_{i=1}^n(b_i - a_i)^2 - snt\right).
    $$

    对 $s$ 求最优值（令导数为 0）：

    $$
    \frac{d}{ds}\left[\frac{s^2}{8}\sum_{i=1}^n(b_i - a_i)^2 - snt\right] = \frac{s}{4}\sum_{i=1}^n(b_i - a_i)^2 - nt = 0,
    $$

    得 $s^* = \frac{4nt}{\sum_{i=1}^n(b_i - a_i)^2}$。代入得：

    $$
    P(S_n \ge nt) \le \exp\left(-\frac{2n^2t^2}{\sum_{i=1}^n(b_i - a_i)^2}\right).
    $$

    **Hoeffding's 引理的证明概要**：利用凸性，对 $a \le X \le b$ 且 $E[X] = 0$，函数 $e^{sX}$ 的期望可以通过在端点 $a, b$ 处的值界定。详细计算涉及指数函数的凸性和对数和不等式，最终得到 $E[e^{sX}] \le \exp(s^2(b-a)^2/8)$。

## Sample Sizes and Exponential Concentration

### 样本量计算示例

通过一些基本计算来熟悉集中不等式的应用。假设我对 $X_1, \dots, X_n$ 进行独立同分布采样，每个取值为 $0$ 或 $+1$（概率未知）。为了估计 $P(X=1)$（即 $X$ 的期望值），我使用估计量：

$$
\hat{\mu} = \frac{1}{n}\sum_{i=1}^n X_i.
$$

$\hat{\mu}$ 的方差为：

$$
\operatorname{Var}(\hat{\mu}) = \frac{\mu(1-\mu)}{n} \le \frac{1}{4n}.
$$

**问题**：假设我希望 95% 确信样本均值在真实均值的 0.01 以内，需要多少样本？

**方法 1：使用 Chebyshev 不等式**

回顾 Chebyshev 不等式：

$$
P\left(|\hat{\mu} - \mu| \ge \frac{t}{2\sqrt{n}}\right) \le \frac{1}{t^2}.
$$

我们需要：

$$
\frac{t}{2\sqrt{n}} \le 0.01, \quad \frac{1}{t^2} \le 0.05.
$$

特别地，选择 $t = \sqrt{1/0.05} \approx 4.47$，得到 $n \ge \left(\frac{t}{0.02}\right)^2 \approx 50000$ 个样本。

**方法 2：使用 Hoeffding's 不等式**

回顾 Hoeffding's 不等式：

$$
P\left(|\hat{\mu} - \mu| \ge \frac{t}{\sqrt{n}}\right) \le 2\exp(-t^2/2).
$$

我们需要：

$$
\frac{t}{\sqrt{n}} \le 0.01, \quad 2\exp(-t^2/2) \le 0.05.
$$

在这种情况下，使用：

$$
t = \sqrt{-\frac{\ln 0.025}{2}} \approx 1.36,
$$

得到 $n \ge 18500$ 个样本。

**对比**：使用 Hoeffding's 不等式大约减少了 3 倍的样本量！如果改变要求（更高的置信度或更小的误差），差异会更加显著。

**关键启示**：方差的微小不精确性（仅有上界）对样本量要求可能产生巨大影响。这就是为什么在实践中，有限样本的紧集中不等式非常重要。而大样本理论（假设 $n \to \infty$）虽然在理论上总是正确的，但对有限样本可能过于宽松。

### 置信区间

Hoeffding's 不等式为我们提供了构造二项参数 $p$ 的置信区间的简单方法。

固定 $\alpha > 0$，令

$$
t = \sqrt{\frac{1}{2n}\log(2/\alpha)}.
$$

由 Hoeffding's 不等式：

$$
P(|\hat{\mu} - p| \ge t) \le 2\exp(-2nt^2) = \alpha.
$$

定义 $C = (\hat{\mu} - t, \hat{\mu} + t)$，则：

$$
P(p \notin C) \le \alpha.
$$

因此，随机区间 $C$ 以至少 $1-\alpha$ 的概率包含真实参数值 $p$；我们称 $C$ 为 **$1-\alpha$ 置信区间**。

**注意**：$C$ 是随机的（依赖于数据），而 $p$ 是固定的未知参数。置信区间的含义是：如果我们重复实验多次，大约 $(1-\alpha) \times 100\%$ 的区间会包含真实值 $p$。

## 弱大数定律

弱大数定律（Weak Law of Large Numbers, WLLN）本质上保证了独立同分布随机变量的平均值"收敛"到期望值。

**弱大数定律**：假设 $X_1, \dots, X_n$ 独立同分布，且 $\operatorname{Var}(X) < \infty$。则对任意 $\epsilon > 0$：

$$
P\left(\left|\frac{1}{n}\sum_{i=1}^n X_i - E[X]\right| \ge \epsilon\right) \to 0,
$$

当 $n \to \infty$ 时。

这种收敛类型非常常见，被称为**依概率收敛**（convergence in probability）。因此弱大数定律告诉我们：样本均值依概率收敛到总体均值。之所以称为"弱"定律，是因为依概率收敛通常被称为弱收敛。我们将很快更系统地讨论收敛性。

**证明**：证明是 Chebyshev 不等式的简单应用。对任意 $\epsilon > 0$，由 Chebyshev 不等式：

$$
P\left(\left|\frac{1}{n}\sum_{i=1}^n X_i - E[X]\right| \ge \epsilon\right) \le \frac{\operatorname{Var}\left(\frac{1}{n}\sum_{i=1}^n X_i\right)}{\epsilon^2} = \frac{\operatorname{Var}(X)}{n\epsilon^2} \to 0,
$$

当 $n \to \infty$ 时。

## Stochastic convergence

弱大数定律是我们试图推理随机变量序列的极限行为的一个例子：

$$
Y_n = \frac{1}{n}\sum_{i=1}^n X_i.
$$

在统计学中，我们通常使用数据来估计参数，在这种情况下，我们的估计形成一个随机变量序列（随着收集更多数据）。我们希望推理估计量的极限行为（它们是否"收敛"到真实值，它们在真实值周围的分布是什么，等等）。这就是我们所说的**大样本理论**（large sample theory）。

假设我们有随机变量序列 $X_1, \dots, X_n$，以及另一个随机变量 $X$。设 $F_n$ 表示 $X_n$ 的 CDF，$F$ 是 $X$ 的 CDF。

两种最基本的随机收敛形式是：

### 1. 依概率收敛（Convergence in Probability）

若对每个 $\epsilon > 0$，有

$$
P(|X_n - X| \ge \epsilon) \to 0,
$$

当 $n \to \infty$ 时，则称序列**依概率收敛**到 $X$。

**重要例子**：弱大数定律。

### 2. 依分布收敛（Convergence in Distribution）

若对 $F$ 的所有连续点 $t$，有

$$
\lim_{n\to\infty} F_n(t) = F(t),
$$

则称序列**依分布收敛**到 $X$。

**重要例子**：中心极限定理（Central Limit Theorem, CLT）。我们将在后续更详细地讨论这一点，但粗略地说，中心极限定理表明独立同分布随机变量的平均值（经过适当重新缩放）依分布收敛到标准正态分布，即：

$$
\frac{\sqrt{n}(\hat{\mu} - \mu)}{\sigma} \xrightarrow{d} \mathcal{N}(0, 1).
$$

**这是一个真正令人惊叹的结果**：你对随机变量除了前两阶矩需要存在之外不做任何假设，却得出结论说平均值趋近于高斯分布。

??? note "其他收敛类型与例子"
    
    除了依概率收敛和依分布收敛之外，还有其他重要的收敛概念。这里提供一些例子来说明它们之间的区别和联系。

    **例 1：**
    
    设 $X_n \sim \mathcal{N}(0, n^{-1})$，对于 $n = 1, 2, \dots$。则 $X_n$ 的分布随着 $n$ 增加而越来越集中在 0 附近。我们想说 $X_n$ 收敛到 0，但 $P(X_n = 0) = 0$ 对所有 $n$ 成立（但是没关系，依分布收敛是定义在连续点上的，所以还是依分布收敛）。
    
    这是依概率收敛合适的一个例子。定义 $X$ 为（非）随机变量，总是等于 0，我们观察到：
    
    $$
    P(|X_n - X| \ge \epsilon) \le 2\exp(-n\epsilon^2/2) \to 0,
    $$
    
    当 $n \to \infty$ 时。因此序列依概率收敛。
    
    **例 2：依分布收敛但不依概率收敛**
    
    假设 $X_n$ 对 $n = 1, 2, \dots$ 都是独立同分布的 $\mathcal{N}(0, 1)$ 随机变量，$X \sim \mathcal{N}(0, 1)$。在这种情况下，很容易看出不存在依概率收敛，而正确的收敛概念是依分布收敛。
    
    **重要说明**：具体来说，依分布收敛要弱得多：它只是关于分布的陈述，而依概率收敛是关于随机变量值的陈述。证明依概率收敛蕴含依分布收敛并不太困难（见 Wasserman 的书）。
    
    **比依概率收敛更强的收敛概念**：
    
    **1. 二次均值收敛（Convergence in Quadratic Mean）**
    
    若
    
    $$
    E(X_n - X)^2 \to 0,
    $$
    
    当 $n \to \infty$ 时，则称序列 $X_n$ **依二次均值收敛**到 $X$。
    
    这又是一次关于随机变量序列值的收敛。事实上，二次均值收敛 $\Longrightarrow$ 依概率收敛，因为根据 Chebyshev 不等式我们知道：
    
    $$
    P(|X_n - X| \ge \epsilon) \le \frac{E(X_n - X)^2}{\epsilon^2} \to 0,
    $$
    
    当 $n \to \infty$ 时。
    
    **2. $L_1$ 收敛（Convergence in $L_1$）**
    
    若
    
    $$
    E|X_n - X| \to 0,
    $$
    
    当 $n \to \infty$ 时，则称序列 $X_n$ **依 $L_1$ 收敛**到 $X$。
    
    二次均值收敛 $\Longrightarrow$ $L_1$ 收敛。要证明这一点，我们可以使用 Cauchy-Schwarz 不等式：
    
    $$
    E|X_n - X| \le \sqrt{E(X_n - X)^2} \to 0,
    $$
    
    当 $n \to \infty$ 时。
    
    **3. 几乎必然收敛（Almost-Sure Convergence）**
    
    几乎必然收敛要求：
    
    $$
    P\left(\lim_{n\to\infty} X_n = X\right) = 1.
    $$
    
    在与 WLLN 相同的条件下，我们实际上有样本均值几乎必然收敛到总体均值。这实际上更难证明，但值得尝试解释它。在大数定律的设定中，我们定义：
    
    $$
    X = E[Y], \quad X_n = \frac{1}{n}\sum_{i=1}^n Y_i.
    $$
    
    **直观理解**：
    
    - **依概率收敛**粗略地告诉我们，在某个点之后，大多数 $X_n$ 的值都非常接近 $X$。我们仍然可能有"失败"的情况，即我们得到一个突然错误的平均值，但这些情况很少见（并且在序列的后面越来越少）。
    
    - **几乎必然收敛**说的是，在某个点之后不再有失败，序列 $X_n$ 的每个随机变量都接近 $X$。
    
    至少粗略地说，依概率收敛允许少数错误的样本平均值，只要它们不太可能，而几乎必然收敛则不允许。
    
    **收敛关系总结**：
    
    $$
    \text{几乎必然收敛} \Longrightarrow \text{依概率收敛} \Longrightarrow \text{依分布收敛}
    $$
    
    $$
    \text{二次均值收敛} \Longrightarrow \text{$L_1$ 收敛} \Longrightarrow \text{依概率收敛}
    $$

## Central Limit Theorem

### 定理陈述

设 $X_1, \dots, X_n$ 独立同分布，均值为 $\mu$，方差为 $\sigma^2$。定义：

$$
\hat{\mu}_n = \frac{1}{n}\sum_{i=1}^n X_i,
$$

和

$$
Z_n = \frac{\sqrt{n}(\hat{\mu}_n - \mu)}{\sigma},
$$

则 $Z_n$ **依分布收敛**到标准高斯分布 $\mathcal{N}(0,1)$。

### 初步验证

首先验证至少均值和方差是正确的：

$$
E[Z_n] = 0, \quad E[Z_n^2] = 1.
$$

这揭示了标准化的必要性：即减去 $\mu$，乘以 $\sqrt{n}$ 并除以 $\sigma$。

### 中心极限定理的两种解释

**解释 1：重复实验的视角**

假设我重复进行实验，即我抽取许多序列：

$$
\{X_1^1, \dots, X_n^1\}, \{X_1^2, \dots, X_n^2\}, \dots, \{X_1^k, \dots, X_n^k\},
$$

并计算它们的标准化平均值 $Z_n^1, \dots, Z_n^k$。

中心极限定理告诉我们，这些标准化平均值将（近似地）具有标准高斯分布。例如，你可以想象计算平均值的直方图，这将遵循高斯规律。

**解释 2：概率计算的视角**

假设在进行实验之前，我询问某个结果的概率：

$$
P(a \le Z_n \le b).
$$

中心极限定理告诉我们：

$$
P(a \le Z_n \le b) \approx \Phi(b) - \Phi(a),
$$

其中 $\Phi$ 是标准正态分布的 CDF。

### 扩展与相关结果

以下是一些我们在需要时会详细讨论的扩展：

**1. 收敛速度（Berry-Esseen 界）**

如果我们适当地定义平均值的 CDF 与高斯的 CDF 之间的距离，我们可以问对于**有限样本量** $n$，这两个 CDF 相距多远。

这些结果通常称为 **Berry-Esseen 界**。它们向我们保证，在一些重要情况下，收敛到正态性可以非常快速地发生。

**2. 多元中心极限定理（Multivariate CLT）**

如果我们对独立随机向量的集合求平均，那么它们将依分布收敛到多元高斯分布。

**3. Delta 方法（Delta Method）**

给定 $Y_n$ 依分布收敛到高斯分布，可以询问 $Y_n$ 的函数的情况。在某些正则性条件下，这些函数也收敛到高斯分布，delta 方法告诉我们如何计算新高斯分布的均值和方差。

这在实践中非常有用，因为它允许我们对各种估计量（不仅仅是样本均值）进行渐近推断。

??? note "中心极限定理的证明"
    
    中心极限定理有多种证明方法。这里我们使用**特征函数（characteristic function）**方法，这是最常见且相对简洁的证明之一。
    
    **特征函数简介**
    
    随机变量 $X$ 的**特征函数**定义为：
    
    $$
    \varphi_X(t) = E[e^{itX}] = E[\cos(tX)] + iE[\sin(tX)],
    $$
    
    其中 $i = \sqrt{-1}$ 是虚数单位。
    
    特征函数的重要性质：
    
    1. **唯一性**：特征函数唯一决定分布。即若 $\varphi_{X_n}(t) \to \varphi_X(t)$ 对所有 $t$，则 $X_n \xrightarrow{d} X$。
    
    2. **独立和的特征函数**：若 $X, Y$ 独立，则 $\varphi_{X+Y}(t) = \varphi_X(t)\varphi_Y(t)$。
    
    3. **标准正态分布的特征函数**：若 $Z \sim \mathcal{N}(0,1)$，则 $\varphi_Z(t) = e^{-t^2/2}$。
    
    **证明中心极限定理**
    
    不失一般性，假设 $\mu = 0$，$\sigma^2 = 1$（通过标准化可以总是这样做）。
    
    定义 $Y_i = X_i/\sqrt{n}$，则：
    
    $$
    Z_n = \sum_{i=1}^n Y_i = \frac{1}{\sqrt{n}}\sum_{i=1}^n X_i.
    $$
    
    我们需要证明 $Z_n$ 的特征函数收敛到标准正态分布的特征函数 $e^{-t^2/2}$。
    
    **步骤 1**：计算 $Y_i$ 的特征函数
    
    $$
    \varphi_{Y_i}(t) = E[e^{itY_i}] = E\left[e^{itX_i/\sqrt{n}}\right] = \varphi_{X_i}(t/\sqrt{n}).
    $$
    
    **步骤 2**：由独立性，$Z_n$ 的特征函数为
    
    $$
    \varphi_{Z_n}(t) = \prod_{i=1}^n \varphi_{Y_i}(t) = \left[\varphi_{X}(t/\sqrt{n})\right]^n.
    $$
    
    **步骤 3**：对 $\varphi_X(t)$ 在 $t=0$ 附近进行 Taylor 展开
    
    由于 $E[X] = 0$ 且 $E[X^2] = 1$，我们有：
    
    $$
    \begin{align}
    \varphi_X(t) &= E[e^{itX}] = E\left[1 + itX - \frac{t^2X^2}{2} + O(t^3X^3)\right] \\
    &= 1 + it E[X] - \frac{t^2}{2}E[X^2] + O(t^3) \\
    &= 1 - \frac{t^2}{2} + O(t^3).
    \end{align}
    $$
    
    **步骤 4**：代入 $t/\sqrt{n}$
    
    $$
    \varphi_X(t/\sqrt{n}) = 1 - \frac{t^2}{2n} + O(n^{-3/2}).
    $$
    
    **步骤 5**：计算 $Z_n$ 的特征函数
    
    $$
    \varphi_{Z_n}(t) = \left[1 - \frac{t^2}{2n} + O(n^{-3/2})\right]^n.
    $$
    
    使用极限 $\lim_{n\to\infty}\left(1 + \frac{x}{n}\right)^n = e^x$：
    
    $$
    \lim_{n\to\infty} \varphi_{Z_n}(t) = \lim_{n\to\infty} \left[1 - \frac{t^2}{2n}\right]^n = e^{-t^2/2}.
    $$
    
    这正是标准正态分布的特征函数！
    
    **步骤 6**：结论
    
    由特征函数的唯一性（Lévy 连续性定理），$\varphi_{Z_n}(t) \to e^{-t^2/2}$ 对所有 $t$ 成立，因此 $Z_n \xrightarrow{d} \mathcal{N}(0,1)$。
    
    **关键洞察**：
    
    - 证明的核心是利用特征函数将卷积（独立和）转化为乘积。
    - Taylor 展开揭示了为什么只需要前两阶矩存在。
    - 指数极限 $(1 + x/n)^n \to e^x$ 是将有限样本连接到极限分布的关键。
    - 这个证明方法可以推广到更一般的情况（如 Lindeberg-Feller 定理）。

