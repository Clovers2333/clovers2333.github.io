# Linear regression 

## Maximum Likelihood Estimation

最大似然估计（Maximum Likelihood Estimation, MLE）是一种基于概率模型的参数估计方法，其核心原则是在给定观测数据的情况下，选择使得这些数据出现概率最大的参数值作为估计值。

### 似然函数

似然函数（Likelihood Function）：定义为参数 $\theta$ 的函数，表示在参数 $\theta$ 下观测到当前数据的概率。对于独立同分布的数据，似然函数为各数据点概率的乘积：

$$L(\theta; x_1, x_2, \ldots, x_n) = \prod_{i=1}^n P(x_i | \theta).$$

### 最大化似然

最大化似然：通过调整参数 $\theta$ ，寻找使似然函数最大化的 $\theta$ 值，即：

$$\hat{\theta}_{\text{MLE}} = \arg \max_{\theta} L(\theta; x_1, \ldots, x_n).$$

### 对数似然

在实际计算中，我们通常使用对数似然（Log-Likelihood）而不是直接使用似然函数，原因有：

1. 乘积运算转换为求和运算，计算更简便
2. 避免数值下溢问题（当概率值很小时，连乘可能导致数值接近零）
3. 对数函数是单调递增的，最大化对数似然等价于最大化似然函数

对数似然定义为：

$$\ell(\theta) = \log L(\theta) = \sum_{i=1}^n \log P(x_i | \theta)$$

## Cost Function

$$
\begin{align*}
h(x) &= \theta_0 + \theta_1 x_1 + \theta_2 x_2 + \cdots + \theta_d x_d = \theta^T x \\
J(\theta) &= \frac{1}{2} \sum_{i=1}^n (h(x^{(i)}) - y^{(i)})^2
\end{align*}
$$

其实会有一个疑问，为什么 cost function 是这个形式？

从概率模型的角度来看，线性回归可以被理解为一个概率问题。假设我们的数据满足以下模型：

$$y^{(i)} = \theta^T x^{(i)} + \epsilon^{(i)}$$

其中 $\epsilon^{(i)}$ 是误差项，它可能来自于未建模的特征或随机噪声。我们假设这些误差项 $\epsilon^{(i)}$ 是独立同分布的（IID），且服从均值为0、方差为 $\sigma^2$ 的高斯分布（正态分布）：

$$\epsilon^{(i)} \sim \mathcal{N}(0, \sigma^2)$$

这意味着误差项的概率密度函数为：

$$p(\epsilon^{(i)}) = \frac{1}{\sqrt{2\pi\sigma}} \exp\left(-\frac{(\epsilon^{(i)})^2}{2\sigma^2}\right)$$

由于 $y^{(i)} = \theta^T x^{(i)} + \epsilon^{(i)}$，我们可以得到 $y^{(i)}$ 在给定 $x^{(i)}$ 和参数 $\theta$ 条件下的概率分布：

$$p(y^{(i)}|x^{(i)}; \theta) = \frac{1}{\sqrt{2\pi\sigma}} \exp\left(-\frac{(y^{(i)} - \theta^T x^{(i)})^2}{2\sigma^2}\right)$$

这表明 $y^{(i)}|x^{(i)}; \theta \sim \mathcal{N}(\theta^T x^{(i)}, \sigma^2)$。

对于整个数据集，由于误差项的独立性假设，似然函数（likelihood function）可以写为：

$$L(\theta) = p(\vec{y}|X; \theta) = \prod_{i=1}^n p(y^{(i)}|x^{(i)}; \theta)$$

$$= \prod_{i=1}^n \frac{1}{\sqrt{2\pi\sigma}} \exp\left(-\frac{(y^{(i)} - \theta^T x^{(i)})^2}{2\sigma^2}\right)$$

根据最大似然估计（Maximum Likelihood Estimation）原则，我们应该选择能够最大化似然函数 $L(\theta)$ 的参数 $\theta$。

为了简化计算，我们通常最大化对数似然（log-likelihood）：

$$\ell(\theta) = \log L(\theta) = \sum_{i=1}^n \log\left(\frac{1}{\sqrt{2\pi\sigma}} \exp\left(-\frac{(y^{(i)} - \theta^T x^{(i)})^2}{2\sigma^2}\right)\right)$$

$$= \sum_{i=1}^n \left[\log\frac{1}{\sqrt{2\pi\sigma}} - \frac{(y^{(i)} - \theta^T x^{(i)})^2}{2\sigma^2}\right]$$

$$= n\log\frac{1}{\sqrt{2\pi\sigma}} - \frac{1}{2\sigma^2}\sum_{i=1}^n (y^{(i)} - \theta^T x^{(i)})^2$$

最大化对数似然等价于最小化：

$$\frac{1}{2\sigma^2}\sum_{i=1}^n (y^{(i)} - \theta^T x^{(i)})^2$$

忽略常数因子 $\frac{1}{2\sigma^2}$，我们得到了均方误差（MSE）损失函数：

$$J(\theta) = \frac{1}{2} \sum_{i=1}^n (h(x^{(i)}) - y^{(i)})^2$$

其中 $h(x^{(i)}) = \theta^T x^{(i)}$。

因此，线性回归中的均方误差损失函数实际上是基于高斯误差假设下的最大似然估计得到的。系数 $\frac{1}{2}$ 是为了在计算梯度时消除平方项的导数中的常数2，使表达式更简洁。

## LMS Algorithm

我们想要找到一个 $\theta$ 使得 $J(\theta)$ 最小，比较直观的想法就是每次往斜率相反的方向走，直到找到最低点。

$$
\theta_j := \theta_j - \alpha \frac{\partial}{\partial \theta_j} J(\theta)
$$

