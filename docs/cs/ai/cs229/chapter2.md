# Classification and logistic regression

## Logistic regression

对于分类问题，我们可以使用线性回归模型，但是线性回归模型的输出值为连续值，而分类问题的输出值为离散值。因此，我们需要将线性回归模型的输出值映射到离散值。

$$h(x) = g(\theta^T x) = \frac{1}{1 + e^{-\theta^T x}}$$

其中 $g(z) = \frac{1}{1 + e^{-z}}$ 称为 sigmoid / logistic 函数，在这里我们给出 $g(z)$ 的一阶导数：

$$g'(z) = g(z)(1 - g(z))$$

那么我们如何确定 $\theta$ 呢？—— 和线性回归一样，我们使用最大似然估计来确定。

### 最大似然估计

假设：

\begin{align*}
P(y = 1 | x; \theta) &= h_\theta(x) \\
P(y = 0 | x; \theta) &= 1 - h_\theta(x)
\end{align*}

这可以更紧凑地写为：

$$p(y | x; \theta) = (h_\theta(x))^y (1 - h_\theta(x))^{1-y}$$

假设 $n$ 个训练样本是独立生成的，我们可以写出参数的似然函数：

\begin{align*}
L(\theta) &= p(\vec{y} | X; \theta) \\
&= \prod_{i=1}^n p(y^{(i)} | x^{(i)}; \theta) \\
&= \prod_{i=1}^n (h_\theta(x^{(i)}))^{y^{(i)}} (1 - h_\theta(x^{(i)}))^{1-y^{(i)}}
\end{align*}

与之前一样，最大化对数似然会更容易：

\begin{align*}
\ell(\theta) &= \log L(\theta) \\
&= \sum_{i=1}^n y^{(i)} \log h(x^{(i)}) + (1 - y^{(i)}) \log(1 - h(x^{(i)}))
\end{align*}

### 梯度上升法

如何最大化似然函数？类似于线性回归的推导，我们可以使用梯度上升法。用向量表示，我们的更新公式为 $\theta := \theta + \alpha \nabla_\theta \ell(\theta)$。（注意这里是正号而不是负号，因为我们是在最大化而不是最小化函数）。

让我们从一个训练样本 $(x, y)$ 开始，求导数来推导随机梯度上升规则：

\begin{align*}
\frac{\partial}{\partial \theta_j} \ell(\theta) &= \left(y \frac{1}{g(\theta^T x)} - (1 - y) \frac{1}{1 - g(\theta^T x)} \right) \frac{\partial}{\partial \theta_j} g(\theta^T x) \\
&= \left(y \frac{1}{g(\theta^T x)} - (1 - y) \frac{1}{1 - g(\theta^T x)} \right) g(\theta^T x)(1 - g(\theta^T x)) \frac{\partial}{\partial \theta_j} \theta^T x \\
&= \left(y(1 - g(\theta^T x)) - (1 - y)g(\theta^T x) \right) x_j \\
&= (y - h_\theta(x)) x_j
\end{align*}

因此，随机梯度上升的更新规则为：

$$\theta_j := \theta_j + \alpha (y^{(i)} - h_\theta(x^{(i)})) x_j^{(i)}$$

对于批量梯度上升，我们对所有样本求和：

$$\theta_j := \theta_j + \alpha \sum_{i=1}^n (y^{(i)} - h_\theta(x^{(i)})) x_j^{(i)}$$

有趣的是，这个更新规则与线性回归的形式相同，但 $h_\theta(x)$ 的定义不同。在线性回归中，$h_\theta(x) = \theta^T x$，而在逻辑回归中，$h_\theta(x) = g(\theta^T x)$，在逻辑回归中，$h$ 是非线性的。

## 决策边界

在逻辑回归中，我们通常使用 0.5 作为阈值来进行分类：

- 如果 $h_\theta(x) \geq 0.5$，则预测 $y = 1$
- 如果 $h_\theta(x) < 0.5$，则预测 $y = 0$

由于 $g(z) \geq 0.5$ 当且仅当 $z \geq 0$，所以决策规则等价于：

- 如果 $\theta^T x \geq 0$，则预测 $y = 1$
- 如果 $\theta^T x < 0$，则预测 $y = 0$

决策边界是满足 $\theta^T x = 0$ 的点的集合，这在二维空间中是一条直线，在高维空间中是一个超平面。

## 牛顿法最大化对数似然

除了梯度上升法外，我们还可以使用牛顿法来最大化对数似然函数 $\ell(\theta)$。牛顿法通常比梯度上升法收敛更快，尤其是在接近最优解时。

### 牛顿法基本原理

牛顿法最初是用来寻找函数的零点的。假设我们有一个函数 $f: \mathbb{R} \mapsto \mathbb{R}$，希望找到 $\theta$ 使得 $f(\theta) = 0$，其中 $\theta \in \mathbb{R}$ 是一个实数。牛顿法执行以下更新：

$$\theta := \theta - \frac{f(\theta)}{f'(\theta)}$$

这个方法的直观解释是：我们在当前的猜测值 $\theta$ 处，用一个与 $f$ 相切的线性函数来近似 $f$，然后求解这个线性函数等于零的点，将其作为 $\theta$ 的下一个猜测值。

牛顿法通常比梯度下降/上升法收敛更快。如果初始点选择得当，牛顿法可以在很少的迭代次数内达到高精度的解。

### 用牛顿法最大化函数

如果我们想要最大化一个函数 $\ell$，我们知道在函数的极大值点处，其一阶导数 $\ell'(\theta) = 0$。因此，我们可以将牛顿法应用于函数 $f(\theta) = \ell'(\theta)$，寻找 $f(\theta) = 0$ 的点。

根据牛顿法的更新规则，我们得到：

$$\theta := \theta - \frac{\ell'(\theta)}{\ell''(\theta)}$$

这就是用牛顿法最大化函数的更新规则。（思考题：如果我们想要最小化而不是最大化一个函数，这个更新规则会如何变化？）

### 多维牛顿法

在逻辑回归中，$\theta$ 是一个向量而不是标量，因此我们需要使用多维牛顿法。对于多维情况，更新规则变为：

$$\theta := \theta - H^{-1} \nabla_\theta \ell(\theta)$$

其中：

- $\nabla_\theta \ell(\theta)$ 是对数似然函数的梯度向量
- $H$ 是对数似然函数的海森矩阵（Hessian matrix），即二阶导数矩阵，其中 $H_{ij} = \frac{\partial^2 \ell(\theta)}{\partial \theta_i \partial \theta_j}$

### 逻辑回归中的牛顿法

对于逻辑回归，我们需要计算对数似然函数的海森矩阵。下面是详细的推导过程：

#### 海森矩阵的推导

1. **从梯度出发，求二阶导数**：

    我们已经知道对数似然函数关于 $\theta_j$ 的一阶导数，然后我们再次对 $\theta_j$ 求导：
   
$$\frac{\partial \ell(\theta)}{\partial \theta_j} = \sum_{i=1}^n (y^{(i)} - h_\theta(x^{(i)})) x_j^{(i)}$$
   
$$\frac{\partial}{\partial \theta_k} \left( \frac{\partial \ell(\theta)}{\partial \theta_j} \right) = \frac{\partial}{\partial \theta_k} \left[ \sum_{i=1}^n (y^{(i)} - h_\theta(x^{(i)})) x_j^{(i)} \right]$$

2. **对单个样本 $i$ 求导**：
   
    由于求和符号外的求导可以移到求和符号内，我们先考虑单个样本的情况（这里我们使用了链式法则，并注意到 $y^{(i)}$ 是常数，对 $\theta_k$ 求导为零）：
   
$$\frac{\partial}{\partial \theta_k} \left[ (y^{(i)} - h_\theta(x^{(i)})) x_j^{(i)} \right] = -x_j^{(i)} \cdot \frac{\partial h_\theta(x^{(i)})}{\partial \theta_k}$$
   
3. **利用Sigmoid函数的导数性质**：
   
    回忆 $h_\theta(x^{(i)}) = g(\theta^T x^{(i)})$，其中 $g$ 是Sigmoid函数。利用链式法则，并且套入 $g'(z) = g(z)(1-g(z))$：
   
\begin{aligned}
\frac{\partial h_\theta(x^{(i)})}{\partial \theta_k} &= \frac{\partial g(\theta^T x^{(i)})}{\partial \theta_k} \\
&= g'(\theta^T x^{(i)}) \cdot \frac{\partial \theta^T x^{(i)}}{\partial \theta_k} \\
&= g'(\theta^T x^{(i)}) \cdot x_k^{(i)} \\
&= h_\theta(x^{(i)})(1 - h_\theta(x^{(i)}))x_k^{(i)}
\end{aligned}

4. **代入得到海森矩阵的元素**：
   
    将步骤3的结果代入步骤2：
   
\begin{aligned}
\frac{\partial}{\partial \theta_k} \left[ (y^{(i)} - h_\theta(x^{(i)})) x_j^{(i)} \right] &= -x_j^{(i)} \cdot h_\theta(x^{(i)})(1 - h_\theta(x^{(i)}))x_k^{(i)} \\
&= -x_j^{(i)}x_k^{(i)}h_\theta(x^{(i)})(1 - h_\theta(x^{(i)}))
\end{aligned}
   
\begin{aligned}
H_{jk} = \frac{\partial^2 \ell(\theta)}{\partial \theta_j \partial \theta_k} = -\sum_{i=1}^n x_j^{(i)}x_k^{(i)}h_\theta(x^{(i)})(1 - h_\theta(x^{(i)}))
\end{aligned}

5. **矩阵形式表示**：
   
    上述结果可以用矩阵形式更紧凑地表示为：
   
$$H = -\sum_{i=1}^n x^{(i)} (x^{(i)})^T h_\theta(x^{(i)}) (1 - h_\theta(x^{(i)}))$$

这个海森矩阵是负定的（negative definite），这意味着对数似然函数是凹函数（concave function），保证了牛顿法能够找到全局最大值。

#### 牛顿法更新规则

有了海森矩阵，牛顿法的更新规则为：

$$\theta := \theta - H^{-1} \nabla_\theta \ell(\theta)$$

代入海森矩阵和梯度的表达式：

$$\theta := \theta - \left( -\sum_{i=1}^n x^{(i)} (x^{(i)})^T h_\theta(x^{(i)}) (1 - h_\theta(x^{(i)})) \right)^{-1} \sum_{i=1}^n (y^{(i)} - h_\theta(x^{(i)})) x^{(i)}$$

简化后：

$$\theta := \theta + \left( \sum_{i=1}^n x^{(i)} (x^{(i)})^T h_\theta(x^{(i)}) (1 - h_\theta(x^{(i)})) \right)^{-1} \sum_{i=1}^n (y^{(i)} - h_\theta(x^{(i)})) x^{(i)}$$

### 牛顿法的优缺点

**优点**：

- 收敛速度快，通常只需要很少的迭代次数
- 在接近最优解时，具有二次收敛性

**缺点**：

- 每次迭代需要计算海森矩阵的逆，计算复杂度为 $O(d^3)$，其中 $d$ 是特征维度
- 对于大规模问题，计算和存储海森矩阵可能非常昂贵
- 如果海森矩阵不是正定 / 负定的，算法可能不收敛

在实践中，对于中小规模问题，牛顿法是一个很好的选择；而对于大规模问题，通常使用拟牛顿法（如BFGS、L-BFGS）或随机梯度上升法。
