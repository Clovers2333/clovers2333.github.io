# Estimation

## Statistical Estimation

统计学（和机器学习）的核心任务是基于样本来理解/估计底层总体的某些性质。形式上，典型的设定如下：

$$
X_1, \ldots, X_n \sim F,
$$

我们能对 $F$ 推断什么？

为了从少量样本中对 $F$ 做出有意义的推断，我们通常以某种自然的方式限制 $F$。在这种情况下，我们用 $\mathcal{F}$ 表示可能分布 $F$ 的集合。这被称为**统计模型（statistical model）**。广义上，有两种可能性：

### Parametric Model

在参数模型中，可能分布的集合 $\mathcal{F}$ 可以用有限个参数来描述。以下是几个例子：

**Gaussian model**

这是一个简单的双参数模型。这里我们假设：

$$
\mathcal{F} = \left\{ f(x; \mu, \sigma) = \frac{1}{\sigma\sqrt{2\pi}} \exp\left\{-\frac{(x-\mu)^2}{2\sigma^2}\right\}, \mu \in \mathbb{R}, \sigma > 0 \right\}.
$$

**Bernoulli model**

这是一个单参数模型，其中：

$$
\mathcal{F} = \{P(X=1) = p, P(X=0) = 1-p, 0 \leq p \leq 1\}.
$$

### Non-parametric Model

非参数模型是指 $\mathcal{F}$ 不能由有限个参数参数化的模型。以下是几个常见的例子：

**Estimating the CDF**

这里模型由任何有效的 CDF 组成，即一个在 0 和 1 之间、单调递增、右连续且在 $-\infty$ 处等于 0、在 $\infty$ 处等于 1 的函数。我们给定样本 $X_1, \ldots, X_n \sim F$，目标是估计 $F$。

**Density estimation**

在密度估计中，我们给定样本 $X_1, \ldots, X_n \sim f_X$，其中 $f_X$ 是我们想要估计的未知密度。事实证明，所有可能密度的类太大，这个问题无法很好地提出，因此我们需要对密度假设一些平滑性。一个典型的假设集如下：

$$
\mathcal{F} = \left\{ f: \int (f''(x))^2 dx < \infty, \int f(x)dx = 1, f(x) \geq 0 \right\}.
$$

## Point Estimation

点估计在统计中是指计算未知感兴趣量的单个"最佳猜测"值。感兴趣的量可以是参数或例如密度函数。

通常，我们用 $\hat{\theta}$ 或 $\hat{\theta}_n$ 表示点估计量。**点估计量**是数据 $X_1, \ldots, X_n$ 的函数：

$$
\hat{\theta}_n = g(X_1, \ldots, X_n),
$$

因此 $\hat{\theta}_n$ 是一个随机变量。

### Bias and Variance

估计量的**偏差（bias）**定义为：

$$
b(\hat{\theta}_n) = \mathbb{E}_{\theta}[\hat{\theta}_n] - \theta.
$$

类似地，估计量的**方差（variance）**为：

$$
v(\hat{\theta}_n) = \mathbb{E}_{\theta}(\hat{\theta}_n - \mathbb{E}_{\theta}[\hat{\theta}_n])^2.
$$

在经典统计中，通常起点是识别**无偏（unbiased）**估计量，然后找到具有小（或最小）方差的无偏估计量。在现代统计中，我们经常使用有偏估计量，因为方差的减少往往证明偏差是合理的。

### Consistency

如果估计量在概率意义下收敛到真实参数，我们称参数的估计量是**相合的（consistent）**，即对于任意 $\epsilon$：

$$
\mathbb{P}_{\theta}(|\hat{\theta}_n - \theta| \geq \epsilon) \to 0,
$$

当 $n \to \infty$ 时。

## The Bias-Variance Decomposition

评估估计量质量的一种方法是通过其**均方误差（mean squared error, MSE）**：

$$
\text{MSE} = \mathbb{E}_{\theta}(\theta - \hat{\theta}_n)^2.
$$

MSE 可以分解为平方偏差和方差之和，即：

$$
\begin{align}
\text{MSE} &= \mathbb{E}_{\theta}(\theta - \hat{\theta}_n)^2 \\
&= \mathbb{E}_{\theta}(\theta - \mathbb{E}_{\theta}[\hat{\theta}_n] + \mathbb{E}_{\theta}[\hat{\theta}_n] - \hat{\theta}_n)^2 \\
&= b(\hat{\theta}_n)^2 + v(\hat{\theta}_n).
\end{align}
$$

这个分解的一个简单推论是：如果 $b(\hat{\theta}_n) \to 0$ 且 $v(\hat{\theta}_n) \to 0$，则估计量 $\hat{\theta}_n$ 是相合的。这是因为如果偏差和方差都趋于 0，那么我们有二次均值收敛，这反过来意味着依概率收敛。

??? note "偏差-方差分解的推导"
    
    $$
    \begin{align}
    \text{MSE} &= \mathbb{E}_{\theta}(\theta - \hat{\theta}_n)^2 \\
    &= \mathbb{E}_{\theta}\left[(\theta - \mathbb{E}_{\theta}[\hat{\theta}_n] + \mathbb{E}_{\theta}[\hat{\theta}_n] - \hat{\theta}_n)^2\right] \\
    &= \mathbb{E}_{\theta}\left[(\theta - \mathbb{E}_{\theta}[\hat{\theta}_n])^2 + (\mathbb{E}_{\theta}[\hat{\theta}_n] - \hat{\theta}_n)^2 + 2(\theta - \mathbb{E}_{\theta}[\hat{\theta}_n])(\mathbb{E}_{\theta}[\hat{\theta}_n] - \hat{\theta}_n)\right] \\
    &= (\theta - \mathbb{E}_{\theta}[\hat{\theta}_n])^2 + \mathbb{E}_{\theta}[(\mathbb{E}_{\theta}[\hat{\theta}_n] - \hat{\theta}_n)^2] + 2(\theta - \mathbb{E}_{\theta}[\hat{\theta}_n])\mathbb{E}_{\theta}[\mathbb{E}_{\theta}[\hat{\theta}_n] - \hat{\theta}_n] \\
    &= b(\hat{\theta}_n)^2 + v(\hat{\theta}_n) + 2(\theta - \mathbb{E}_{\theta}[\hat{\theta}_n]) \cdot 0 \\
    &= b(\hat{\theta}_n)^2 + v(\hat{\theta}_n).
    \end{align}
    $$
    
    其中交叉项为零是因为 $\mathbb{E}_{\theta}[\mathbb{E}_{\theta}[\hat{\theta}_n] - \hat{\theta}_n] = \mathbb{E}_{\theta}[\hat{\theta}_n] - \mathbb{E}_{\theta}[\hat{\theta}_n] = 0$。

**示例**：假设 $X_1, \ldots, X_n \sim \text{Ber}(p)$，我们的估计量为：

$$
\hat{p}_n = \frac{1}{n}\sum_{i=1}^n X_i.
$$

这个估计量的偏差是多少？它的方差是多少？这个估计量是相合的吗？

??? note "答案"
    
    **偏差**：
    
    $$
    b(\hat{p}_n) = \mathbb{E}[\hat{p}_n] - p = \mathbb{E}\left[\frac{1}{n}\sum_{i=1}^n X_i\right] - p = \frac{1}{n}\sum_{i=1}^n \mathbb{E}[X_i] - p = \frac{1}{n} \cdot np - p = 0.
    $$
    
    所以 $\hat{p}_n$ 是无偏的。
    
    **方差**：
    
    $$
    v(\hat{p}_n) = \text{Var}(\hat{p}_n) = \text{Var}\left(\frac{1}{n}\sum_{i=1}^n X_i\right) = \frac{1}{n^2}\sum_{i=1}^n \text{Var}(X_i) = \frac{1}{n^2} \cdot n \cdot p(1-p) = \frac{p(1-p)}{n}.
    $$
    
    **相合性**：
    
    因为 $b(\hat{p}_n) = 0$ 且 $v(\hat{p}_n) = \frac{p(1-p)}{n} \to 0$ 当 $n \to \infty$，所以 $\hat{p}_n$ 是 $p$ 的相合估计量。
    
    或者，我们可以直接应用弱大数定律：$\hat{p}_n = \frac{1}{n}\sum_{i=1}^n X_i \xrightarrow{P} \mathbb{E}[X_1] = p$。

## Asymptotic Normality

我们研究的估计量通常具有**渐近正态性（asymptotic normality）**。这意味着：

$$
\frac{\hat{\theta}_n - \theta}{\sqrt{v(\hat{\theta}_n)}}
$$

在分布意义下收敛到 $N(0,1)$。我们将这一性质称为渐近正态性。

## Confidence Sets

一般地，对于参数 $\theta$，我们定义一个 $1-\alpha$ **置信集（confidence set）** $C_n$ 为任何满足以下性质的集合：

$$
\mathbb{P}_{\theta}(\theta \in C_n) \geq 1 - \alpha.
$$

我们通常将 $\mathbb{P}_{\theta}(\theta \in C_n)$ 称为置信集 $C_n$ 的**覆盖率（coverage）**。置信集 $C_n$ 是一个随机集合（因为 $\theta$ 是固定参数）。

关于覆盖率保证可以用以下方式理解：

你多次重复实验，每次构造一个不同的置信区间 $C_n$。那么这些不同集合中有 $1-\alpha$ 的比例将包含相应的真实参数。注意，真实参数不必是固定的，因此在某种意义上你进行的实验可以每次都不同。

我们已经看到了一种使用 Hoeffding 不等式为 Bernoulli 参数构造置信区间的方法。更一般地，我们总是可以使用浓度不等式来构造置信区间。这些置信区间通常较松，我们转而采用（渐近）置信区间。

### Asymptotic Confidence Intervals

通常情况是：

$$
\frac{\hat{\theta}_n - \theta}{\sqrt{v(\hat{\theta}_n)}}
$$

渐近地服从 $N(0,1)$。在这些情况下，我们有 $\hat{\theta}_n \approx N(\theta, v(\hat{\theta}_n))$。定义 $z_{\alpha/2} = \Phi^{-1}(1-\alpha/2)$。那么我们会构造一个置信区间：

$$
C_n = \left(\hat{\theta}_n - z_{\alpha/2}\sqrt{v(\hat{\theta}_n)}, \hat{\theta}_n + z_{\alpha/2}\sqrt{v(\hat{\theta}_n)}\right).
$$

我们现在需要验证：

$$
\mathbb{P}_{\theta}(\theta \in C_n) \to 1 - \alpha,
$$

当 $n \to \infty$ 时，这就是渐近置信区间的含义。

$$
\begin{align}
\mathbb{P}_{\theta}(\theta \in C_n) &= \mathbb{P}\left(\hat{\theta}_n - z_{\alpha/2}\sqrt{v(\hat{\theta}_n)} \leq \theta \leq \hat{\theta}_n + z_{\alpha/2}\sqrt{v(\hat{\theta}_n)}\right) \\
&= \mathbb{P}_{\theta}\left(-z_{\alpha/2} \leq \frac{\hat{\theta}_n - \theta}{\sqrt{v(\hat{\theta}_n)}} \leq z_{\alpha/2}\right) \\
&\to \mathbb{P}(-z_{\alpha/2} \leq Z \leq z_{\alpha/2}) = 1 - \alpha.
\end{align}
$$

### Example: Bernoulli Confidence Sets

**示例**：Bernoulli 置信集

我们之前使用 Hoeffding 不等式构造了置信集。它们的形式为：

$$
C_n = \left(\hat{p}_n - \sqrt{\frac{\log(2/\alpha)}{2n}}, \hat{p}_n + \sqrt{\frac{\log(2/\alpha)}{2n}}\right).
$$

如果我们改用正态近似：我们首先注意到我们的估计量的方差为：

$$
v(\hat{p}_n) = \frac{p(1-p)}{n}.
$$

然而，我们不能使用这个方差来创建我们的置信集，所以我们改为估计方差为：

$$
\hat{v}(\hat{p}_n) = \frac{\hat{p}_n(1-\hat{p}_n)}{n}.
$$

有了这个，我们将使用置信区间：

$$
C_n = \left(\hat{p}_n - z_{\alpha/2}\sqrt{\hat{v}(\hat{p}_n)}, \hat{p}_n + z_{\alpha/2}\sqrt{\hat{v}(\hat{p}_n)}\right).
$$

很容易验证这个区间总是比 Hoeffding 区间短，但它只是渐近正确的。

## Hypothesis Testing

通常，统计假设检验的进行方式是定义一个所谓的**零假设（null hypothesis）**。然后我们收集数据，通常我们要问的问题是数据是否提供了足够的证据来**拒绝（reject）**零假设。

**示例**：假设 $X_1, \ldots, X_n \sim \text{Ber}(p)$，我们想测试硬币是否公平。在这种情况下，零假设将是：

$$
H_0: p = 1/2.
$$

我们通常还会指定一个**备择假设（alternative hypothesis）**。在这种情况下，备择假设是：

$$
H_1: p \neq 1/2.
$$

通常，假设检验通过定义一个**检验统计量（test statistic）**来进行。在这种情况下，一个自然的统计量可能是：

$$
T = \left|\frac{1}{n}\sum_{i=1}^n X_i - p\right|.
$$

如果 $T$ 很大，拒绝零假设可能是有意义的。我们将在后面更精确地讨论这一点，特别是通过定义不同类型的错误，以及如何设置 $T$ 的阈值。

