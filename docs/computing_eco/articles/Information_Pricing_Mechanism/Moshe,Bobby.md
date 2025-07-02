# Optimal Mechanisms for Selling Information

> Author: Moshe Babaioff, Robert Kleinberg, Renato Paes Leme
> 
> Year: 2012
> 
> Link: [Optimal Mechanisms for Selling Information](Moshe,Bobby.pdf)
>
> 研究目标：
>
>   1. 介绍在不同情况下的最优机制
>   2. 证明最优机制的计算复杂度
>   3. 证明表示最优机制的信息复杂度

## 1. 独立信号下的机制设计

当 $\omega$ 和 $\theta$ 独立时，卖方关于 $\theta$ 的信念和买方关于 $\omega$ 的信念是共同知识。

### 1.1 一轮最优机制的存在性

我们首先介绍一种简单的一轮交互式机制：

!!! Note "协议树"
    
    **深度3的树**：
    
    1. **第一层**：买方节点（报告类型 $\theta$）。
    
    2. **第二层**：转移节点（支付 $t_\theta$）
        - **定价映射机制（Pricing Mappings）**：转移节点（支付 $t_\theta$），后接卖方节点（发送信号 $Y_\theta$）。
        - **定价结果机制（Pricing Outcomes）**：卖方节点（发送信号 $Y_\theta$），后接转移节点（支付 $t_\theta$）。
    
    3. **第三层**：叶子节点（买方基于后验选择行动）。
   
!!! Theorem "定理 1.1"

    考虑 $\theta$ 和 $\omega$ 独立的情况 $(u, \mu)$。如果可以从承诺买方中提取收益 $R$，那么使用定价映射机制也可以从非承诺买方中提取收益 $R$。

???+Example "证明"

    **(1) 原始协议的表示**

    **设定**：

    - 原始协议是一个多轮交互树，包含买方节点、卖方节点和转移节点。

    - 买方策略 $\phi^\theta$ 是类型 $\theta$ 的最优策略，生成叶子分布 $Z(\omega, \theta)$。

    - 收益 $R = \mathbb{E}_{\omega,\theta}[\tau(Z(\omega, \theta))]$，其中 $\tau(\cdot)$ 是路径上的累计支付。

    **(2) 构造简化协议**

    **1. 第一层（根节点）**：

    - 买方节点：买方报告类型 $\hat{\theta} \in \Theta$（可能谎报）。

    **2. 第二层**：

    - 转移节点：对每个报告 $\hat{\theta}$，支付金额为原始协议中类型 $\hat{\theta}$ 的期望支付：

    \begin{align*}
    t_{\hat{\theta}} = \mathbb{E}_\omega[\tau(Z(\omega, \hat{\theta}))]
    \end{align*}

    **3. 第三层**：

    - 卖方节点：根据真实状态 $\omega$，按原始协议的叶子分布 $Z(\omega, \hat{\theta})$ 生成信号（即复制原始协议的最终信息结构）。

    **(3) 等价性验证**

    需证明简化协议满足：

    - **收益不变**：卖方的期望收益仍为 $R$。

    - **激励相容**：买方诚实报告 $\theta$ 是最优策略。

    **a. 收益不变**：

    - 简化协议中，类型 $\theta$ 的买方支付 $t_\theta = \mathbb{E}_\omega[\tau(Z(\omega, \theta))]$，与原始协议一致。

    - 由于独立性，$\omega$ 的分布与 $\theta$ 无关，期望收益保持不变。

    **b. 激励相容 (IC)**：

    - **诚实报告的效用**：买方报告真实类型 $\theta$，支付 $t_\theta$，获得原始协议的信息和行动效用。

    - **谎报的效用**：若买方谎报为 $\theta'$，其支付为 $t_{\theta'}$，但信息结构 $Z(\omega, \theta')$ 是为类型 $\theta'$ 设计的，对类型 $\theta$ 可能次优。

    - **关键点**：由于 $\omega \perp \theta$，类型 $\theta$ 买方谎报时，$\omega$ 的分布仍为 $p(\omega)$（与真实类型无关），因此其效用与原始协议中模仿类型 $\theta'$ 策略 $\phi^{\theta'}$ 相同（其实 $t_{\theta'}$ 就是按照 $\phi^{\theta'}$ 来计算的）。但原始协议中 $\phi^\theta$ 已是最优策略，故谎报无利可图。

    **c. 非承诺买方的自愿性**：

    - 简化协议中，买方在支付 $t_\theta$ 后立即获得信号，无法中途退出（因协议结束），故非承诺性不影响参与。

    **(4) 独立性的关键作用**

    - **支付计算**：期望支付 $t_\theta = \mathbb{E}_\omega[\tau(Z(\omega, \theta))]$ 仅依赖 $\theta$，因 $\omega$ 的分布与 $\theta$ 无关。

    - **信号生成**：卖方节点按 $Z(\omega, \theta)$ 生成信号，但独立性保证不同 $\theta$ 的信号价值可分离。

这意味着在独立信号下，买方承诺与否不影响卖方的最优收益。

同时，它也意味着所有的最优机制都可以映射到定价映射机制，我们只需要在这个结构上加上个人理性和激励相容条件就可以具体化地求解。

换句话说，这其实也是显示原理在这里的体现：因为输入是任意机制、买家的占优策略均衡；而输出的则是以协议树为基础的最优机制——所有人诚实报告自己的类型，即满足激励相容条件，同时收益相同。

我们接下来将形式化地表示这个机制，然后用线性规划和凸优化等技术将其求解，然后讨论它的计算复杂性。

### 1.2 定价映射机制

基于定理1.1，我们专注于定价映射机制。机制表示为合同菜单 $\{(Y_\theta, t_\theta)\}_{\theta \in \Theta}$，其中：

- $Y_\theta \in S_\theta$：随机信号变量。根据 seller 持有的 $\omega$、枚举的 $\theta$ 以及独立的随机元 $r$ 构造。 $\psi_\theta^\omega \in \Delta(S_\theta)$，当真实状态为 $\omega$ 时，seller 按照 $\psi_\theta^\omega$ 的分布采样得到 $Y_\theta$。
- $t_\theta$：合同价格

类型 $\theta$ 买方选择合同 $(Y_\theta, t_\theta)$ 的效用为：

\begin{align*}
\mathbb{E}_{Y_\theta}[\max_a \mathbb{E}_\omega[u(\theta, \omega, a)|Y_\theta]] - t_\theta
\end{align*}

!!! Definition "定义 1.2"
    合同菜单有效当且仅当满足IR和IC约束：

    \begin{align*}
    \mathbb{E}_{Y_\theta}[\max_a \mathbb{E}_\omega[u(\theta, \omega, a)|Y_\theta]] - t_\theta &\geq \max_a \mathbb{E}_\omega[u(\theta, \omega, a)], \quad \forall \theta \ \ (IR)\\
    \mathbb{E}_{Y_\theta}[\max_a \mathbb{E}_\omega[u(\theta, \omega, a)|Y_\theta]] - t_\theta &\geq \mathbb{E}_{Y_{\theta'}}[\max_a \mathbb{E}_\omega[u(\theta, \omega, a)|Y_{\theta'}]] - t_{\theta'}, \quad \forall \theta \neq \theta' \ \ (IC)
    \end{align*}

### 1.3 线性规划重构

定义类型 $\theta$ 对后验 $q \in \Delta(\Omega)$ 的价值函数：

\begin{align*}
v_\theta(q) = \max_{a \in A} \sum_\omega q(\omega) u(\theta, \omega, a)
\end{align*}

其中：

\begin{align*}
q = \mathbb{P}[\omega|Y_\theta=s] = \frac{\psi_\theta^\omega(Y_\theta=s) \cdot \mu(\omega)}{\sum_{\omega'} \psi_\theta^{\omega'}(Y_\theta=s) \cdot \mu(\omega')}
\end{align*}

将每个 $Y_\theta$ 表示为后验分布上的概率分布 $x_\theta: \Delta(\Omega) \to [0,1]$，满足贝叶斯一致性条件：

!!! Note "观察 1.3"
    在合同菜单 $\{(Y_\theta, t_\theta)\}_{\theta \in \Theta}$ 中，如果存在两个不同的信号 $s, s' \in S_\theta$ 导致买方更新后的后验分布完全相同（即 $q_s = q_{s'}$），则可以通过合并这两个信号简化菜单，而不改变收益。
    
    具体操作：
    
    - 将原信号随机变量 $Y_\theta$ 替换为 $\tilde{Y}_\theta$，其中：
        - 所有原本输出 $s'$ 的情况改为输出 $s$；
        - 其他信号保持不变。
    
    - 新菜单 $\{(\tilde{Y}_\theta, t_\theta)\}$ 仍然是有效的（满足IR和IC约束），且收益与原菜单相同。

简化处理后，我们可以保证信号集中的每个信号的后验都不同。因此我们可以将 $Y_\theta$ 表示为有限后验集合 $Q$ 上的分布，每个 $q \in Q$ 形式为 $q \in \Delta(\Omega)$，概率为 $x_\theta(q)$。

即，我们把选择随机信号转化成了选择后验分布，发布一个 $Y_\theta = s$ 实际上就是发布了一个 $q$.

!!! Note "观察 1.4"
    设 $Y$ 是与 $\omega$ 相关的随机变量，给定 $\psi^\omega \in \Delta(S)$，$S = [k]$，$Y$ 通过首先采样 $\omega \sim \mu$ 然后从 $\psi^\omega$ 采样得到。考虑集合 $Q = \{q_1, \ldots, q_k\} \subset \Delta(\Omega)$ 和 $x \in \Delta(Q)$。对 $(Q, x)$ 表示与 $\omega$ 相关的随机变量分布当且仅当：
    
    \begin{align*}
    \sum_{q_i \in Q} x(q_i) \cdot q_i(\omega) = \mu(\omega) \quad \forall \omega \in \Omega \ \ (F)
    \end{align*}

我们上面提到，发布一个 $Y_\theta = s$ 实际上就是发布了一个 $q$；因此，每个 $Y_\theta$ 的**分布**就可以表示为一个展示 $q$ 概率的函数 $x_\theta: \Delta(\Omega) \to [0,1]$，满足：

\begin{align*}
\sum_{q \in Q} x_\theta(q) \cdot q(\omega) = \mu(\omega) \quad \forall \omega \in \Omega \ \ (F_\theta)
\end{align*}

同时，因为 $Y_\theta$ 的信号是有限的，所以 $x_\theta$ 的支撑集也是有限的。

由此我们可以通过选择 $x_\theta$ 来写出下面的线性规划：

**LP1**:

\begin{align*}
\max &\sum_\theta \mu(\theta) t_\theta \\
\text{s.t.} \quad &\sum_q v_\theta(q) x_\theta(q) - t_\theta \geq v_\theta(p), \quad \forall \theta \ \ (IR_\theta) \\
&\sum_q v_\theta(q) x_\theta(q) - t_\theta \geq \sum_q v_\theta(q) x_{\theta'}(q) - t_{\theta'}, \quad \forall \theta \neq \theta' \ \ (IC_{\theta,\theta'}) \\
&\sum_q x_\theta(q) \cdot q(\omega) = \mu(\omega), \quad \forall \theta, \omega \ \ (F_\theta)
\end{align*}

### 1.4 对偶问题

!!! Theorem "引理 1.5"
    对于任意给定的效用函数 $u: \Theta \times \Omega \times A \to \mathbb{R}$，存在一个有限的后验分布集合 $Q^* \subset \Delta(\Omega)$，使得：
    
    1. **最优性保留**：任何机制能实现的最大收益，均可通过仅使用 $Q^*$ 中的后验分布的机制实现。
    
    2. **高效表示**：每个 $q \in Q^*$ 可用多项式数量的比特表示（即数值精度可控）。

**DLP1**的对偶变量为 $g_\theta, h_{\theta,\theta'} \geq 0$ 和 $y_\theta \in \mathbb{R}^\Omega$：

\begin{align*}
\min &\sum_\theta \sum_\omega p(\omega) y_\theta(\omega) \\
\text{s.t.} \quad &-v_\theta(q) g_\theta + \sum_{\theta''} v_\theta(q) h_{\theta'',\theta} - \sum_{\theta'} v_\theta(q) h_{\theta,\theta'} + q^T y_\theta \geq 0, \quad \forall \theta, q \\
&g_\theta + \sum_{\theta'} h_{\theta,\theta'} - \sum_{\theta'} h_{\theta',\theta} \geq \mu(\theta), \quad \forall \theta
\end{align*}

具体的 DLP 书写过程可以看作是[拉格朗日对偶](../../../math/LinearProgramming/lagrange.md)的构造过程：


??? "对偶目标函数与约束的推导详解"

    

    ### 1. 拉格朗日对偶问题的基本框架

    对于原始线性规划 $LP_1$：

    \begin{align*}
    \max \sum_\theta \mu(\theta) t_\theta \quad s.t. \quad \text{IR, IC, 贝叶斯可行性}
    \end{align*}

    其拉格朗日目标函数通过引入乘子将约束整合到目标函数中：

    \begin{align*}
    \mathcal{L}(x, t; g, h, y) = \text{原始目标} + \text{乘子} \times \text{约束}
    \end{align*}

    ### 2. 拉格朗日目标函数的构造

    具体展开如下：

    \begin{align*}
    \mathcal{L} &= \sum_\theta \mu(\theta) t_\theta \\
    &+ \sum_\theta g_\theta \left( \sum_q v_\theta(q) x_\theta(q) - t_\theta - v_\theta(p) \right) \quad (\text{IR约束}) \\
    &+ \sum_{\theta, \theta' \neq \theta} h_{\theta, \theta'} \left( \sum_q v_\theta(q) x_\theta(q) - t_\theta - \sum_q v_\theta(q) x_{\theta'}(q) + t_{\theta'} \right) \quad (\text{IC约束}) \\
    &+ \sum_{\theta, \omega} y_\theta(\omega) \left( p(\omega) - \sum_q x_\theta(q) q(\omega) \right) \quad (\text{贝叶斯可行性})
    \end{align*}

    ### 3. 对偶问题的目标函数

    拉格朗日对偶问题的目标是：

    \begin{align*}
    \min_{g,h,y} \left( \max_{x,t} \mathcal{L}(x, t; g, h, y) \right)
    \end{align*}

    即先对原始变量 $x, t$ 最大化 $\mathcal{L}$，再对乘子 $g, h, y$ 最小化该最大值。

    **步骤1：整理 $\mathcal{L}$ 关于 $t_\theta$ 和 $x_\theta(q)$ 的项**

    将 $\mathcal{L}$ 按原始变量分组：

    \begin{align*}
    \mathcal{L} &= \sum_\theta t_\theta \left( \mu(\theta) - g_\theta - \sum_{\theta' \neq \theta} h_{\theta, \theta'} + \sum_{\theta' \neq \theta} h_{\theta', \theta} \right) \\
    &+ \sum_{\theta,q} x_\theta(q) \left( v_\theta(q) g_\theta + \sum_{\theta' \neq \theta} v_\theta(q) h_{\theta, \theta'} - \sum_{\theta' \neq \theta} v_\theta(q) h_{\theta', \theta} - \sum_\omega y_\theta(\omega) q(\omega) \right) \\
    &- \sum_\theta g_\theta v_\theta(p) + \sum_{\theta,\omega} y_\theta(\omega) p(\omega)
    \end{align*}

    **步骤2：关于 $t_\theta$ 和 $x_\theta(q)$ 的最大化**

    - 若 $t_\theta$ 的系数 $\left( \mu(\theta) - g_\theta - \sum_{\theta' \neq \theta} h_{\theta, \theta'} + \sum_{\theta' \neq \theta} h_{\theta', \theta} \right) > 0$，则 $\max_{t_\theta} \mathcal{L} = +\infty$（无界）。
    因此，对偶问题需强制该系数 $\leq 0$：

    \begin{align*}
    g_\theta + \sum_{\theta' \neq \theta} h_{\theta, \theta'} - \sum_{\theta' \neq \theta} h_{\theta', \theta} \geq \mu(\theta) \quad (\text{对偶约束1})
    \end{align*}

    此时 $t_\theta$ 的最优值为 0。

    - 类似地，$x_\theta(q)$ 的系数必须 $\leq 0$，否则 $\max_{x_\theta(q)} \mathcal{L} = +\infty$：

    \begin{align*}
    v_\theta(q) g_\theta + \sum_{\theta' \neq \theta} v_\theta(q) h_{\theta, \theta'} - \sum_{\theta' \neq \theta} v_\theta(q) h_{\theta', \theta} - q^T y_\theta \leq 0 \quad (\text{对偶约束2})
    \end{align*}

    此时 $x_\theta(q)$ 的最优值为 0。

    **步骤3：对偶目标函数的简化**

    当上述约束满足时，$\max_{x,t} \mathcal{L}$ 仅剩常数项：

    \begin{align*}
    -\sum_\theta g_\theta v_\theta(p) + \sum_{\theta,\omega} y_\theta(\omega) p(\omega)
    \end{align*}

    因此，对偶问题为最小化该表达式：

    \begin{align*}
    \min_{g,h,y} \left( -\sum_\theta g_\theta v_\theta(p) + \sum_{\theta,\omega} y_\theta(\omega) p(\omega) \right)
    \end{align*}

    由于 $v_\theta(p) = \max_a \sum_\omega p(\omega) u(\theta, \omega, a)$ 是常数，通常可忽略（或假设 $v_\theta(p) = 0$），最终目标简化为：

    \begin{align*}
    \min \sum_\theta p^T y_\theta
    \end{align*}

    ### 4. 对偶约束的最终形式

    将约束整理为标准形式：

    1. **对偶约束2 重写为：**

    \begin{align*}
    -v_\theta(q) g_\theta + \sum_{\theta' \neq \theta} [v_{\theta'}(q) h_{\theta', \theta} - v_\theta(q) h_{\theta, \theta'}] + q^T y_\theta \geq 0
    \end{align*}

    2. **对偶约束1 直接保留：**

    \begin{align*}
    g_\theta + \sum_{\theta' \neq \theta} (h_{\theta, \theta'} - h_{\theta', \theta}) \geq \mu(\theta)
    \end{align*}

### 分离Oracle

**Separation oracle**：第二族约束的大小是 $|\Theta|$，所以分离是trivial的。为了分离第一族约束，我们以不同的方式重写约束。注意 $v_\theta(\cdot)$ 是 $|A|$ 个线性函数的最大值。因此，我们可以将第一族的每个约束替换为以下 $|A|$ 个约束：

\begin{align*}
\sum_{\theta' \neq \theta} v_{\theta'}(q) h_{\theta',\theta} \geq -q^T y_\theta + \left( g_\theta + \sum_{\theta' \neq \theta} h_{\theta,\theta'} \right) \sum_\omega u(\theta, \omega, a) q(\omega)
\end{align*}

对于所有 $a \in A$，$\theta \in \Theta$ 和 $q \in Q^*$。现在，对于固定的 $a, \theta$，我们希望检查这个约束是否被 $q \in Q^*$ 中的所有 $q$ 满足，或者找到一个 $q$ 违反了约束。

将要求 $q \in Q^*$ 放松为 $q \in \Delta(\Omega)$，这等价于求解以下凸规划问题：

\begin{align*}
\min_{q \in \Delta(\Omega)} \sum_{\theta' \neq \theta} v_{\theta'}(q) h_{\theta',\theta} + q^T y_\theta - \left( g_\theta + \sum_{\theta' \neq \theta} h_{\theta,\theta'} \right) \sum_\omega u(\theta, \omega, a) q(\omega)
\end{align*}

!!! Theorem "引理 1.6"
    上述凸规划问题可在多项式时间内精确求解。

也就是说，这个最优机制问题可以在多项式时间内求解。

### 1.5 信息复杂度的相关结论

!!! Theorem "定理 1.7"
    设 $n = |\Theta|$，$m = |\Omega|$。用向量 $x$ 的支撑大小记为 $\|x\|_0$。规划 LP₁ 存在一个解，对于所有 $\theta$，$\|x_\theta\|_0 \leq m + n - 1$，且 $\sum_\theta \|x_\theta\|_0 \leq mn + \binom{n}{2}$。
    
    此外，存在支撑大小为 $n$ 的二次函数的前提，即使当 $m = 2$ 时也是必要的。

即，即使信息只有单个比特($m = 2$)，某些情况下仍需要 $\Omega(\log n)$ 比特的信号来实现收益最大化。

二次下界的事实即使对于 $m = 2$（例子 B.2）也成立，这在某种程度上是反直觉的：即使出售的信息是单个比特，也存在收益最大化需要使用由 $\Omega(\log n)$ 比特组成的信号的情况。

#### 1.5.1 密封信封机制的定义与收益下限

- **机制描述**：卖方将完整信息 $\omega$ 写入信封，以固定价格 $t$ 出售给买方。买方仅在 $\xi(\theta) \geq t$ 时购买。

- **收益下限**：对任意最优收益 $R$，存在至少一个类型 $\theta$ 满足：

\begin{align*}
\frac{R}{|\Theta|} \leq \mu(\theta) \cdot \xi(\theta), \quad \text{其中} \quad \xi(\theta) = \mathbb{E}_\omega\left[\max_a u(\theta, \omega, a)\right] - \max_a \mathbb{E}_\omega[u(\theta, \omega, a)]
\end{align*}

- **定价策略**：设 $t = \xi(\theta) - \epsilon$，则收益至少为 $\mu(\theta) \cdot \xi(\theta) \geq \frac{R}{|\Theta|}$。

- **紧性证明**：存在场景使得密封信封机制的收益不超过最优机制的 $\Omega\left(\frac{1}{|\Theta|}\right)$。
    - **实例**：设所有类型的 $\xi(\theta)$ 相同，但分布 $\mu(\theta)$ 不均匀，密封信封只能通过单一价格捕获部分类型的价值。

#### 1.5.2 协议树（最优机制）

当 $\omega$ 与 $\theta$ 独立时：

**协议复杂度**：存在规模为 $O(|\Omega| \cdot |\Theta| + |\Theta|^2)$ 的协议树。

- **叶子节点数**：由后验分布的支持集大小 $\sum_\theta \|x_\theta\|_0 \leq mn + \binom{n}{2}$ 决定（$m = |\Omega|, n = |\Theta|$）。
- **总节点数**：不超过叶子数的3倍（因单轮披露机制的树深度为3）。

#### 1.5.3 单比特信息下的高复杂度现象（Example B.2）

- **反直觉结论**：即使卖方信息仅为1比特（如"用户是否高收入"），最优机制可能需要 $\Omega(n^2)$ 种信号（即需 $\log n$ 比特编码）。

- **原因**：
    - 买方类型 $\theta$ 的多样性导致最优行动切换点增多。
    - 需通过精细的信号设计区分不同类型的激励相容约束。

- **实例构造**：设 $\Omega = \{0, 1\}$，设计效用函数 $u(\theta, \omega, a)$ 使得每个类型对 $\omega$ 的敏感度不同，迫使卖方使用多比特信号实现价格歧视。

## 2. 相关信号下承诺玩家的机制设计

### 2.1 问题背景与核心挑战

**场景设定**：

- 信号 $\omega$（卖方私有）与买方类型 $\theta$ 相关（即 $\mu(\omega, \theta)$ 不是独立分布）。

- 买方为承诺型（必须完成协议，不会中途退出）。

**关键问题**：

独立信号下的"固定价格合同菜单"不再最优，需引入结果依赖定价（Pricing Outcomes Mechanism）以充分利用相关性。

### 2.2 结果依赖定价机制（Pricing Outcomes Mechanism）

#### 2.2.1 机制设计

**合同菜单**：$\{(Y_\theta, t_\theta)\}_{\theta \in \Theta}$，其中：

- $Y_\theta$ 是基于 $\omega$ 的信号随机变量，取值于有限集 $S_\theta$。

- **支付函数** $t_\theta: S_\theta \to \mathbb{R}$：支付金额依赖信号实现值 $s \in S_\theta$（可正可负，负值表示卖方退款）。

**执行流程**：

1. 买方报告类型 $\theta$。

2. 卖方采样 $s \sim Y_\theta$ 并请求支付 $t_\theta(s)$。

3. 买方基于信号 $s$ 更新信念并选择行动。

#### 2.2.2 与独立信号机制的区别

| 特性 | 独立信号机制（Pricing Mappings） | 相关信号机制（Pricing Outcomes） |
|------|-----------------------------------|-----------------------------------|
| **支付时机** | 买方先支付固定价格 $t_\theta$，后接收信号 | 支付金额 $t_\theta(s)$ 依赖信号结果 $s$ |
| **价格歧视能力** | 弱（无法利用 $\omega$ 与 $\theta$ 的相关性） | 强（通过 $t_\theta(s)$ 实现跨类型歧视） |
| **经济学直觉** | 信息与支付解耦 | 支付与信号结果绑定，利用买方类型对信号的不同评估 |

#### 2.2.3 为什么需要结果依赖定价？

**相关性利用**：

不同买方类型 $\theta$ 对同一信号 $s$ 的后验信念 $q_s(\omega|\theta)$ 不同，因此愿意支付的金额不同。

- 卖方可通过设计 $t_\theta(s)$ 实现精确价格歧视。

**示例**：

设 $\omega$ 为"用户收入高/低"，$\theta$ 为"广告主类型"。

- 若信号 $s$ 提示"高收入"，奢侈品广告主（$\theta_1$）愿支付更高价格 $t_{\theta_1}(s)$，而普通广告主（$\theta_2$）支付 $t_{\theta_2}(s)$。

