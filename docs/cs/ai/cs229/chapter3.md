# Generalized Linear Models

## Introduction

到目前为止，我们已经学习了回归问题和分类问题的例子。在回归问题中，我们有 $y|x;\theta \sim \mathcal{N}(\mu, \sigma^2)$，而在分类问题中，我们有 $y|x;\theta \sim \text{Bernoulli}(\phi)$，其中 $\mu$ 和 $\phi$ 是关于 $x$ 和 $\theta$ 的适当函数。在本章中，我们将展示这两种方法都是一个更广泛的模型家族——广义线性模型（GLMs）的特例。我们还将展示GLM家族中的其他模型如何推导并应用于其他分类和回归问题。

## The Exponential Family

为了构建广义线性模型，我们首先需要定义指数族分布。如果一个分布的概率密度函数（或概率质量函数）可以写成以下形式，则称其属于指数族：

$$p(y; \eta) = b(y) \exp(\eta^T T(y) - a(\eta))$$

其中：

- $\eta$ 是分布的**自然参数**（也称为**规范参数**）
- $T(y)$ 是**充分统计量**（即将数据压缩到最简形式，对于我们考虑的分布，通常 $T(y) = y$）
- $a(\eta)$ 是**对数配分函数**，$e^{-a(\eta)}$ 本质上起到归一化常数的作用，确保分布 $p(y; \eta)$ 对 $y$ 的积分或求和为1
- $b(y)$ 是基函数

固定选择 $T$、$a$ 和 $b$ 定义了一个由参数 $\eta$ 参数化的分布族；当我们改变 $\eta$ 时，就得到这个族内的不同分布，而 $\eta$ 是绑定参数 $\theta$ 的，所以本质上我们就是通过改变 $\theta$ 来满足 $y$ 的理想分布。

### Bernoulli Distribution as an Exponential Family

伯努利分布 $\text{Bernoulli}(\phi)$ 定义了 $y \in \{0, 1\}$ 上的分布，使得 $p(y=1; \phi) = \phi$，$p(y=0; \phi) = 1 - \phi$。我们可以将其写为：

$$p(y; \phi) = \phi^y (1-\phi)^{1-y}$$

通过一系列代数变换，我们可以将其重写为指数族的形式：

\begin{aligned}
p(y; \phi) &= \exp(y \log \phi + (1-y) \log(1-\phi)) \\
&= \exp\left(\left(\log \frac{\phi}{1-\phi}\right) y + \log(1-\phi)\right)
\end{aligned}

因此，伯努利分布的自然参数为 $\eta = \log(\phi/(1-\phi))$。有趣的是，如果我们反解这个定义，得到 $\phi = 1/(1+e^{-\eta})$，这正是我们熟悉的sigmoid函数！这将在我们推导逻辑回归作为GLM时再次出现。

对于伯努利分布作为指数族，我们有：

- $T(y) = y$
- $a(\eta) = -\log(1-\phi) = \log(1+e^\eta)$
- $b(y) = 1$

### Gaussian Distribution as an Exponential Family

回忆高斯分布的概率密度函数：

$$p(y; \mu) = \frac{1}{\sqrt{2\pi}} \exp\left(-\frac{1}{2}(y - \mu)^2\right)$$

为了简化推导，我们设 $\sigma^2 = 1$（在线性回归中，$\sigma^2$ 的值不影响我们对 $\theta$ 和 $h_\theta(x)$ 的最终选择）。

通过代数变换，我们可以将高斯分布重写为：

$$p(y; \mu) = \frac{1}{\sqrt{2\pi}} \exp\left(-\frac{1}{2}y^2\right) \cdot \exp\left(\mu y - \frac{1}{2}\mu^2\right)$$

因此，高斯分布也属于指数族，其中：

- $\eta = \mu$
- $T(y) = y$
- $a(\eta) = \mu^2/2 = \eta^2/2$
- $b(y) = (1/\sqrt{2\pi}) \exp(-y^2/2)$

除了伯努利分布和高斯分布外，指数族还包括多项式分布（我们稍后会看到）、泊松分布（用于建模计数数据）、伽马分布和指数分布（用于建模非负连续随机变量，如时间间隔）、贝塔分布和狄利克雷分布（用于概率分布上的分布）等等。

## Constructing Generalized Linear Models

假设我们想要构建一个模型来估计基于某些特征 $x$（如商店促销、最近的广告、天气、星期几等）在给定小时内到达商店的顾客数量 $y$（或网站的页面浏览量）。我们知道泊松分布通常是访客数量的良好模型。幸运的是，泊松分布是指数族分布，因此我们可以应用广义线性模型（GLM）。

更一般地，考虑一个分类或回归问题，我们希望预测某个随机变量 $y$ 作为 $x$ 的函数。为了推导这个问题的GLM，我们对 $y$ 的条件分布和我们的模型做出以下三个假设：

1. $y | x; \theta \sim \text{ExponentialFamily}(\eta)$。即，给定 $x$ 和 $\theta$，$y$ 的分布遵循某个参数为 $\eta$ 的指数族分布。

2. 给定 $x$，我们的目标是预测 $T(y)$ 的期望值。在大多数例子中，$T(y) = y$，所以这意味着我们希望学习的假设 $h$ 输出的预测 $h(x)$ 满足 $h(x) = E[y|x]$。（注意，这个假设在逻辑回归和线性回归中都得到满足。例如，在逻辑回归中，$h_\theta(x) = P(y=1|x;\theta) = 0 \cdot P(y=0|x;\theta) + 1 \cdot P(y=1|x;\theta) = E[y|x;\theta]$。）

3. 自然参数 $\eta$ 和输入 $x$ 线性相关：$\eta = \theta^T x$。（如果 $\eta$ 是向量值，则 $\eta_i = \theta_i^T x$。）

第三个假设可能看起来是最不合理的，它更像是GLM设计中的一个"设计选择"，而不是一个假设，这也正是“广义线性”中“广义”的由来。这三个假设/设计选择将使我们能够推导出一类优雅的学习算法——GLMs，它们具有易于学习等许多理想特性。此外，由此产生的模型通常非常有效地建模不同类型的 $y$ 分布；例如，我们很快就会看到逻辑回归和普通最小二乘法都可以作为GLMs推导出来。

### Ordinary Least Squares

为了证明普通最小二乘法是GLM家族的特例，考虑目标变量 $y$（在GLM术语中也称为响应变量）是连续的，我们将 $y$ 给定 $x$ 的条件分布建模为高斯分布 $\mathcal{N}(\mu, \sigma^2)$（其中 $\mu$ 可能依赖于 $x$）。

因此，我们让上面的 $\text{ExponentialFamily}(\eta)$ 分布是高斯分布。如前所述，在高斯分布作为指数族分布的表述中，我们有 $\mu = \eta$。所以：

$$h_\theta(x) = E[y|x;\theta] = \mu = \eta = \theta^T x$$

第一个等式来自假设2；第二个等式来自 $y|x;\theta \sim \mathcal{N}(\mu, \sigma^2)$，因此其期望值由 $\mu$ 给出；第三个等式来自假设1（以及我们之前的推导，表明 $\mu = \eta$ 在高斯分布的指数族表述中）；最后一个等式来自假设3。

### Logistic Regression

现在考虑逻辑回归。这里我们关注二元分类，所以 $y \in \{0, 1\}$。给定 $y$ 是二值的，选择伯努利分布族来建模 $y$ 给定 $x$ 的条件分布是很自然的。

在我们对伯努利分布作为指数族的表述中，我们有 $\phi = 1/(1+e^{-\eta})$。此外，如果 $y|x;\theta \sim \text{Bernoulli}(\phi)$，则 $E[y|x;\theta] = \phi$。所以，按照与普通最小二乘法类似的推导：

$$h_\theta(x) = E[y|x;\theta] = \phi = \frac{1}{1+e^{-\eta}} = \frac{1}{1+e^{-\theta^T x}}$$

因此，这给出了形式为 $h_\theta(x) = 1/(1+e^{-\theta^T x})$ 的假设函数。如果你之前想知道我们是如何得出逻辑函数 $1/(1+e^{-z})$ 的形式的，这就是答案之一：一旦我们假设 $y$ 条件于 $x$ 服从伯努利分布，它就作为GLM定义和指数族分布的结果自然产生。

引入一些术语，函数 $g$ 给出分布均值作为自然参数的函数（$g(\eta) = E[T(y);\eta]$）被称为**规范响应函数**。其逆函数 $g^{-1}$ 被称为**规范连接函数**。因此，高斯族的规范响应函数就是恒等函数；而伯努利族的规范响应函数是逻辑函数。

### Softmax Regression

让我们再看一个GLM的例子。考虑一个分类问题，其中响应变量 $y$ 可以取 $k$ 个值中的任意一个，所以 $y \in \{1, 2, \ldots, k\}$。例如，我们可能想将电子邮件分类为垃圾邮件、个人邮件和工作相关邮件，而不是仅仅分为垃圾邮件和非垃圾邮件。响应变量仍然是离散的，但现在可以取多于两个值。我们将其建模为服从多项式分布。

让我们推导用于建模这种多项式数据的GLM。为此，我们首先将多项式表示为指数族分布。

为了参数化 $k$ 个可能结果的多项式，我们可以使用 $k$ 个参数 $\phi_1, \ldots, \phi_k$ 指定每个结果的概率。然而，这些参数是冗余的，或者更正式地说，它们不是独立的（因为知道任意 $k-1$ 个 $\phi_i$ 唯一确定最后一个，因为它们必须满足 $\sum_{i=1}^k \phi_i = 1$）。所以，我们将用 $k-1$ 个参数 $\phi_1, \ldots, \phi_{k-1}$ 参数化多项式，其中 $\phi_i = p(y=i; \phi)$，且 $p(y=k; \phi) = 1 - \sum_{i=1}^{k-1} \phi_i$。为了表示方便，我们也记 $\phi_k = 1 - \sum_{i=1}^{k-1} \phi_i$，但要记住这不是一个参数，它完全由 $\phi_1, \ldots, \phi_{k-1}$ 确定。

为了将多项式表示为指数族分布，我们将 $T(y) \in \mathbb{R}^{k-1}$ 定义如下：

$$T(1) = \begin{bmatrix} 1 \\ 0 \\ 0 \\ \vdots \\ 0 \end{bmatrix}, T(2) = \begin{bmatrix} 0 \\ 1 \\ 0 \\ \vdots \\ 0 \end{bmatrix}, T(3) = \begin{bmatrix} 0 \\ 0 \\ 1 \\ \vdots \\ 0 \end{bmatrix}, \ldots, T(k-1) = \begin{bmatrix} 0 \\ 0 \\ 0 \\ \vdots \\ 1 \end{bmatrix}, T(k) = \begin{bmatrix} 0 \\ 0 \\ 0 \\ \vdots \\ 0 \end{bmatrix}$$

与我们之前的例子不同，这里我们没有 $T(y) = y$；此外，$T(y)$ 现在是 $k-1$ 维向量，而不是实数。

我们还引入一个非常有用的记号。指示函数 $1\{\cdot\}$ 取值为1如果其参数为真，否则为0（$1\{\text{True}\} = 1, 1\{\text{False}\} = 0$）。例如，$1\{2=3\} = 0$，$1\{3=5-2\} = 1$。所以，我们也可以将 $T(y)$ 和 $y$ 的关系写为 $(T(y))_i = 1\{y=i\}$。此外，我们有 $E[(T(y))_i] = P(y=i) = \phi_i$。

现在我们准备展示多项式是指数族的成员：

\begin{aligned}
p(y; \phi) &= \phi_1^{1\{y=1\}} \phi_2^{1\{y=2\}} \cdots \phi_k^{1\{y=k\}} \\
&= \phi_1^{1\{y=1\}} \phi_2^{1\{y=2\}} \cdots \phi_k^{1-\sum_{i=1}^{k-1} 1\{y=i\}} \\
&= \phi_1^{(T(y))_1} \phi_2^{(T(y))_2} \cdots \phi_k^{1-\sum_{i=1}^{k-1} (T(y))_i} \\
&= \exp((T(y))_1 \log(\phi_1) + (T(y))_2 \log(\phi_2) + \cdots + (1-\sum_{i=1}^{k-1} (T(y))_i) \log(\phi_k)) \\
&= \exp((T(y))_1 \log(\phi_1/\phi_k) + (T(y))_2 \log(\phi_2/\phi_k) + \cdots + (T(y))_{k-1} \log(\phi_{k-1}/\phi_k) + \log(\phi_k)) \\
&= b(y) \exp(\eta^T T(y) - a(\eta))
\end{aligned}

其中：

\begin{aligned}
\eta &= \begin{bmatrix} \log(\phi_1/\phi_k) \\ \log(\phi_2/\phi_k) \\ \vdots \\ \log(\phi_{k-1}/\phi_k) \end{bmatrix} \\
a(\eta) &= -\log(\phi_k) \\
b(y) &= 1
\end{aligned}

连接函数由以下给出（对于 $i = 1, \ldots, k$）：

$$\eta_i = \log \frac{\phi_i}{\phi_k}$$

为了方便，我们也定义 $\eta_k = \log(\phi_k/\phi_k) = 0$。为了反转连接函数并导出响应函数，我们有：

\begin{aligned}
e^{\eta_i} &= \frac{\phi_i}{\phi_k} \\
\phi_k e^{\eta_i} &= \phi_i \\
\phi_k \sum_{i=1}^k e^{\eta_i} &= \sum_{i=1}^k \phi_i = 1
\end{aligned}

这意味着 $\phi_k = 1/\sum_{i=1}^k e^{\eta_i}$，可以代回方程 $\phi_k e^{\eta_i} = \phi_i$ 得到响应函数：

$$\phi_i = \frac{e^{\eta_i}}{\sum_{j=1}^k e^{\eta_j}}$$

这个函数映射从 $\eta$ 到 $\phi$ 被称为**softmax函数**。

为了完成我们的模型，我们使用前面给出的假设3，即 $\eta$ 线性依赖于 $x$。所以，我们有 $\eta_i = \theta_i^T x$（对于 $i = 1, \ldots, k-1$），其中 $\theta_1, \ldots, \theta_{k-1} \in \mathbb{R}^{d+1}$ 是我们模型的参数。为了表示方便，我们也定义 $\theta_k = 0$，使得 $\eta_k = \theta_k^T x = 0$，与前面给出的一致。因此，我们的模型假设 $y$ 给定 $x$ 的条件分布为：

$$p(y=i|x;\theta) = \phi_i = \frac{e^{\eta_i}}{\sum_{j=1}^k e^{\eta_j}} = \frac{e^{\theta_i^T x}}{\sum_{j=1}^k e^{\theta_j^T x}}$$

这个模型，适用于 $y \in \{1, \ldots, k\}$ 的分类问题，被称为**softmax回归**。它是逻辑回归的推广。

我们的假设将输出：

$$h_\theta(x) = E[T(y)|x;\theta] = \begin{bmatrix} \phi_1 \\ \phi_2 \\ \vdots \\ \phi_{k-1} \end{bmatrix} = \begin{bmatrix} \frac{\exp(\theta_1^T x)}{\sum_{j=1}^k \exp(\theta_j^T x)} \\ \frac{\exp(\theta_2^T x)}{\sum_{j=1}^k \exp(\theta_j^T x)} \\ \vdots \\ \frac{\exp(\theta_{k-1}^T x)}{\sum_{j=1}^k \exp(\theta_j^T x)} \end{bmatrix}$$

换句话说，我们的假设将输出 $p(y=i|x;\theta)$ 对于每个值 $i = 1, \ldots, k$ 的估计概率。（尽管 $h_\theta(x)$ 如上定义只有 $k-1$ 维，显然 $p(y=k|x;\theta)$ 可以通过 $1 - \sum_{i=1}^{k-1} \phi_i$ 获得。）

最后，让我们讨论参数拟合。与我们最初推导普通最小二乘法和逻辑回归类似，如果我们有训练集 $\{(x^{(i)}, y^{(i)}); i = 1, \ldots, n\}$，并希望学习这个模型的参数 $\theta_i$，我们首先写出对数似然函数：

\begin{aligned}
\ell(\theta) &= \sum_{i=1}^n \log p(y^{(i)}|x^{(i)};\theta) \\
&= \sum_{i=1}^n \log \prod_{l=1}^k \left( \frac{e^{\theta_l^T x^{(i)}}}{\sum_{j=1}^k e^{\theta_j^T x^{(i)}}} \right)^{1\{y^{(i)}=l\}}
\end{aligned}

为了得到上面的第二行，我们使用了 $p(y|x;\theta)$ 的定义，如方程(3.3)所示。现在我们可以通过最大化关于 $\theta$ 的 $\ell(\theta)$ 来获得参数的最大似然估计，使用梯度上升或牛顿法等方法。