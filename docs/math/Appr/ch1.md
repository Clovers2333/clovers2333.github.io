# Chapter 1. An Introduction to Approximation Algorithms

## 1.1 Basic Introduction

!!! Theorem "Theorem 1.1"

    If $P \neq NP$, then we can't simultaneously have algorithms that:

    - (1) Find optimal solutions
    - (2) Run in polynomial time
    - (3) For any instance

我们可以对三个条件分别进行松弛：

- 松弛第二个可以得到启发式算法；
- 松弛第三个可以得到特定情况下的多项式解；
- 松弛第一个则是这本书的主题——近似算法。

!!! Definition "Definition 1.1"

    对于优化问题的 $\alpha$-近似算法是一个多项式时间算法，它对问题的所有实例都能产生一个解，该解的值在最优解值的 $\alpha$ 倍因子范围内。

!!! Definition "Definition 1.2"

    多项式时间近似方案（PTAS）是一族算法 $\{A_\epsilon\}$，其中对于每个 $\epsilon > 0$，都存在一个算法，使得 $A_\epsilon$ 是一个 $(1 + \epsilon)$-近似算法（对于最小化问题）或者 $(1 - \epsilon)$-近似算法（对于最大化问题）。

许多问题都有多项式时间近似方案。在后面的章节中，我们将遇到背包问题和欧几里得旅行商问题，它们都有 PTAS。

然而，存在一类不那么容易的问题。这类问题被称为 MAX SNP；虽然我们不会定义它，但它包含许多有趣的优化问题，例如最大可满足性问题和最大割问题，我们将在本书后面讨论。

!!! Theorem "Theorem 1.3"

    对于任何 MAX SNP-hard 问题，不存在多项式时间近似方案，除非 P = NP。

!!! Theorem "Theorem 1.4"

    设 $n$ 表示输入图中顶点的数量，对于任何常数 $\epsilon > 0$，不存在 $O(n^{\epsilon-1})$-近似算法求解最大团问题，除非 P = NP。

为了看出这个定理有多强，我们观察到，对于这个问题很容易得到一个 $n^{-1}$-近似算法：只需输出一个单独的顶点。

而定理指出，找到比这个完全平凡的近似算法仅仅略好一点的解就意味着 P = NP！

## 1.2 The set cover problem

给定一个元素的全集 $E = \{e_1, \ldots, e_n\}$，这些元素的一些子集 $S_1, S_2, \ldots, S_m$，其中每个 $S_j \subseteq E$，以及每个子集 $S_j$ 的非负权重 $w_j \geq 0$。

目标是找到子集的最小权重集合，使得该集合覆盖 $E$ 中的所有元素；即，我们希望找到 $I \subseteq \{1, \ldots, m\}$，使得 $\sum_{j \in I} w_j$ 最小，并且 $\bigcup_{j \in I} S_j = E$。

如果每个子集 $j$ 的 $w_j = 1$，问题称为无权重集合覆盖问题。

在集合覆盖问题的情况下，我们需要决定在解决方案中使用哪些子集 $S_j$。我们创建一个决策变量 $x_j$ 来表示这种选择，即如果集合 $S_j$ 包含在解决方案中，我们希望 $x_j$ 为1，否则为0。

因此我们可以将集合覆盖问题建模为整数线性规划问题：

\begin{align}
\text{minimize} \quad & \sum_{j=1}^m w_j x_j \\
\text{subject to} \quad & \sum_{j: e_i \in S_j} x_j \geq 1, \quad i = 1, \ldots, n, \ \ (1.1) \\
& x_j \in \{0, 1\}, \quad j = 1, \ldots, m.
\end{align}

设 $Z_{IP}^*$ 表示给定集合覆盖问题实例的此整数规划的最优值。由于整数规划完全建模了问题，我们有 $Z_{IP}^* = \text{OPT}$，其中 OPT 是集合覆盖问题的最优解的值。

一般来说，整数规划不能在多项式时间内求解，因为集合覆盖问题是 NP-hard 的；然而，线性规划是多项式时间可解的：

\begin{align}
\text{minimize} \quad & \sum_{j=1}^m w_j x_j \\
\text{subject to} \quad & \sum_{j: e_i \in S_j} x_j \geq 1, \quad i = 1, \ldots, n, \ \ (1.2) \\
& x_j \geq 0, \quad j = 1, \ldots, m.
\end{align}

线性规划是原始整数规划的**松弛**，线性规划的任何最优解都将具有值 $Z_{LP}^* \leq Z_{IP}^* = \text{OPT}$。

## 1.3 A deterministic rounding algorithm

假设我们求解了集合覆盖问题的线性规划松弛。设 $x^*$ 表示 LP 的最优解。那么我们如何恢复集合覆盖问题的解？

这里有一个非常简单的方法：给定 LP 解 $x^*$，当且仅当 $x_j^* \geq 1/f$ 时，我们在解中包含子集 $S_j$，其中 $f$ 是任何元素出现的集合的最大数量。

更正式地，设 $f_i = |\{j : e_i \in S_j\}|$ 是元素 $e_i$ 出现的集合数，$i = 1, \ldots, n$；则 $f = \max_{i=1,\ldots,n} f_i$。设 $I$ 表示此解中子集的索引。

实际上，我们将分数解 $x^*$ 舍入为整数解 $\hat{x}$：当 $x_j^* \geq 1/f$ 时设 $\hat{x}_j = 1$，否则 $\hat{x}_j = 0$。我们将证明 $\hat{x}$ 是整数规划的可行解，$I$ 确实索引一个集合覆盖。

!!! Lemma "Lemma 1.5"

    子集集合 $S_j$，$j \in I$，是一个集合覆盖。

!!! Info "Proof"

    考虑引理指定的解，如果此解包含某个包含 $e_i$ 的子集，则称元素 $e_i$ 被*覆盖*。我们证明每个元素 $e_i$ 都被覆盖。

    因为最优解 $x^*$ 是线性规划的可行解，我们知道对于元素 $e_i$ 有 $\sum_{j:e_i \in S_j} x_j^* \geq 1$。

    由 $f_i$ 和 $f$ 的定义，和式中有 $f_i \leq f$ 项，所以至少有一项必须至少为 $1/f$。因此，对于某个 $j$ 使得 $e_i \in S_j$，有 $x_j^* \geq 1/f$。所以 $j \in I$，元素 $e_i$ 被覆盖。 $\square$

我们还可以证明此舍入过程产生一个近似算法。

!!! Theorem "Theorem 1.6"

    舍入算法是集合覆盖问题的 $f$-近似算法。

!!! Info "Proof"

    显然算法在多项式时间内运行。由我们的构造，对于每个 $j \in I$ 有 $1 \leq f \cdot x_j^*$。由此，以及每项 $f w_j x_j^*$ 对 $j = 1, \ldots, m$ 非负的事实，我们得到

    \begin{align*}
    \sum_{j \in I} w_j &\leq \sum_{j=1}^m w_j \cdot (f \cdot x_j^*) \\
    &= f \sum_{j=1}^m w_j x_j^* \\
    &= f \cdot Z_{LP}^* \\
    &\leq f \cdot \text{OPT},
    \end{align*}

    其中最后的不等式来自前面论证的 $Z_{LP}^* \leq \text{OPT}$。 $\square$

在顶点覆盖问题的特殊情况下，对每个边 $i \in E$，$f_i = 2$，因为每条边恰好与两个顶点相邻。因此，舍入算法给出顶点覆盖问题的 2-近似算法。

## 1.4 Rounding a dual solution

我们在这里将引出线性规划对偶的概念。

假设每个元素 $e_i$ 被单独收取某个非负价格 $y_i \geq 0$ 用于被集合覆盖。直观来讲，如果某些元素用低权重子集就可以覆盖，而其他元素可能需要高权重子集来覆盖，那么我们期望对前者收取较小的价格，对后者收取较大的价格。

为了使价格合理，子集 $S_j$ 中元素价格的总和不能超过该集合的权重，因为我们可以通过支付权重 $w_j$ 来覆盖所有这些元素。因此，对于每个子集 $S_j$，我们有以下价格限制：

\begin{align*}
\sum_{i: e_i \in S_j} y_i \leq w_j.
\end{align*}

我们可以通过以下线性规划找到元素可以被收取的最高总价格：

\begin{align}
\text{maximize} \quad & \sum_{i=1}^n y_i \\
\text{subject to} \quad & \sum_{i: e_i \in S_j} y_i \leq w_j, \quad j = 1, \ldots, m, \ \ (1.3) \\
& y_i \geq 0, \quad i = 1, \ldots, n. \\
\text{minimize} \quad & \sum_{j=1}^m w_j x_j \\
\text{subject to} \quad & \sum_{j: e_i \in S_j} x_j \geq 1, \quad i = 1, \ldots, n, \ \ (1.2) \\
& x_j \geq 0, \quad j = 1, \ldots, m.
\end{align}

线性规划 (1.3) 是集合覆盖线性规划松弛 (1.2) 的**对偶**线性规划。一般来说，我们可以为任何给定的线性规划导出对偶线性规划。

一般情况来讲，对偶的限制会对应原始的变量，而原始的限制会对应对偶的变量。

### 1.4.1 Weak duality

对偶线性规划有许多非常有趣和有用的性质。因为对于任何 $e_i$，$\sum_{j: e_i \in S_j} x_j \geq 1$，我们可以写出下面的式子：

\begin{align*}
\sum_{i=1}^n y_i \leq \sum_{i=1}^n y_i \sum_{j: e_i \in S_j} x_j,
\end{align*}

然后重写右侧的不等式，我们有

\begin{align*}
\sum_{i=1}^n y_i \sum_{j: e_i \in S_j} x_j = \sum_{j=1}^m x_j \sum_{i: e_i \in S_j} y_i.
\end{align*}

最后，注意由于 $y$ 是对偶线性规划的可行解，我们知道对于任何 $j$，$\sum_{i: e_i \in S_j} y_i \leq w_j$，所以

\begin{align*}
\sum_{j=1}^m x_j \sum_{i: e_i \in S_j} y_i \leq \sum_{j=1}^m x_j w_j.
\end{align*}

所以我们证明了：

\begin{align*}
\sum_{i=1}^n y_i \leq \sum_{j=1}^m w_j x_j;
\end{align*}

即，对偶线性规划的任何可行解的值不大于原始线性规划的任何可行解的值。特别地，对偶线性规划的任何可行解的值不大于原始线性规划的最优解的值，所以对于任何可行的 $y$，$\sum_{i=1}^n y_i \leq Z_{LP}^*$。这被称为线性规划的**弱对偶性**性质。

由于我们之前论证了 $Z_{LP}^* \leq \text{OPT}$，我们有对于任何可行的 $y$，$\sum_{i=1}^n y_i \leq \text{OPT}$。这是一个非常有用的性质，将帮助我们设计近似算法。

### 1.4.2 Strong duality

另外，有一个相当令人惊讶的线性规划的**强对偶性**性质。强对偶性说明，只要原始和对偶线性规划都存在可行解，它们的最优值就相等。因此，如果 $x^*$ 是集合覆盖线性规划松弛的最优解，$y^*$ 是对偶线性规划的最优解，则

\begin{align*}
\sum_{j=1}^m w_j x_j^* = \sum_{i=1}^n y_i^*.
\end{align*}

来自对偶线性规划解的信息有时可以用来导出好的近似算法。设 $y^*$ 是对偶 LP (1.3) 的最优解，考虑选择所有对应的对偶不等式为紧的子集的解；即，对于子集 $S_j$，不等式以等式成立，$\sum_{i: e_i \in S_j} y_i^* = w_j$。设 $I'$ 表示此解中的支撑集（即取等号的集合）。我们将证明此算法也是集合覆盖问题的 $f$-近似算法。

!!! Lemma "Lemma 1.7"

    子集集合 $S_j$，$j \in I'$，是一个集合覆盖。

!!! Info "Proof"

    假设存在某个未覆盖的元素 $e_k$。那么对于包含 $e_k$ 的每个子集 $S_j$，必须有

    \begin{align*}
    \sum_{i: e_i \in S_j} y_i^* < w_j. \ \ (1.4)
    \end{align*}

    因为如果取等那么就会取这个子集，从而覆盖 $e_k$。

    设 $\epsilon$ 是涉及 $e_k$ 的所有约束的右侧和左侧之间的最小差值；即，$\epsilon = \min_{j: e_k \in S_j} \left( w_j - \sum_{i: e_i \in S_j} y_i^* \right)$。由不等式 (1.4)，我们知道 $\epsilon > 0$。

    现在考虑新的对偶解 $y'$，其中 $y_k' = y_k^* + \epsilon$，$y'$ 的每个其他分量与 $y^*$ 相同。那么 $y'$ 是对偶可行解，因为对于每个包含 $e_k$ 的 $j$，

    \begin{align*}
    \sum_{i: e_i \in S_j} y_i' = \sum_{i: e_i \in S_j} y_i^* + \epsilon \leq w_j,
    \end{align*}

    由 $\epsilon$ 的定义。对于每个不包含 $e_k$ 的 $j$，

    \begin{align*}
    \sum_{i: e_i \in S_j} y_i' = \sum_{i: e_i \in S_j} y_i^* \leq w_j,
    \end{align*}

    如之前一样。此外，$\sum_{i=1}^n y_i' > \sum_{i=1}^n y_i^*$，这与 $y^*$ 的最优性矛盾。因此，必须是所有元素都被覆盖，$I'$ 是一个集合覆盖。 $\square$

!!! Theorem "Theorem 1.8"

    上述描述的对偶舍入算法是集合覆盖问题的 $f$-近似算法。

!!! Info "Proof"

    因为在上述定理中我们可以选择支撑集来作为覆盖集，所以当我们选择集合 $S_j$ 包含在覆盖中时，我们可以通过对其每个元素 $e_i$ 收费 $y_i^*$ 来"支付"它。
    
    每个元素最多为包含它的每个集合收费一次（因此总共最多被收费 $f$ 次），所以总成本最多为 $f \sum_{i=1}^n y_i^*$，或 $f$ 倍对偶目标函数值。

    更正式地，由于 $j \in I'$ 当且仅当 $w_j = \sum_{i: e_i \in S_j} y_i^*$，我们有集合覆盖 $I'$ 的成本是

    \begin{align*}
    \sum_{j \in I'} w_j &= \sum_{j \in I'} \sum_{i: e_i \in S_j} y_i^* \\
    &= \sum_{i=1}^n |\{j \in I' : e_i \in S_j\}| \cdot y_i^* \\
    &\leq \sum_{i=1}^n f_i y_i^* \\
    &\leq f \sum_{i=1}^n y_i^* \\
    &\leq f \cdot \text{OPT}.
    \end{align*}

    第二个等式来自交换求和顺序时，$y_i^*$ 的系数当然等于该项总共出现的次数。最后的不等式来自前面讨论的弱对偶性性质。 $\square$

实际上，可以证明此算法不会比前一节的算法做得更好；准确地说，我们可以证明如果 $I$ 是前一节原始舍入算法返回的解，则 $I \subseteq I'$。这可以通过线性规划的**互补松弛性**来证明。

!!! Definition "互补松弛性"

    我们之前为集合覆盖线性规划松弛的任何可行解 $x$ 和对偶线性规划的任何可行解 $y$ 证明了以下不等式串：

    \begin{align*}
    \sum_{i=1}^n y_i \leq \sum_{i=1}^n y_i \sum_{j: e_i \in S_j} x_j = \sum_{j=1}^m x_j \sum_{i: e_i \in S_j} y_i \leq \sum_{j=1}^m x_j w_j.
    \end{align*}

    此外，我们声明强对偶性意味着对于最优解 $x^*$ 和 $y^*$，$\sum_{i=1}^n y_i^* = \sum_{j=1}^m w_j x_j^*$。因此，对于任何最优解 $x^*$ 和 $y^*$，上述不等式链中的两个不等式实际上必须是等式。

    这只有在以下情况下才能发生：当 $y_i^* > 0$ 时，则 $\sum_{j: e_i \in S_j} x_j^* = 1$，当 $x_j^* > 0$ 时，则 $\sum_{i: e_i \in S_j} y_i^* = w_j$。即，当线性规划变量（原始或对偶）非零时，对偶或原始中的相应约束是紧的。这些条件被称为**互补松弛性条件**。

    如果 $x^*$ 和 $y^*$ 是最优解，互补松弛性条件必须成立。反之也成立：如果 $x^*$ 和 $y^*$ 是可行的原始和对偶解，则如果互补松弛性条件成立，两个目标函数的值相等，因此解必须是最优的。

!!! Info "Proof"

    在集合覆盖问题的情况下，如果对于任何原始最优解 $x^*$，$x_j^* > 0$，则对于任何对偶最优解 $y^*$，$S_j$ 的相应对偶不等式必须是紧的。回想在前一节的算法中，当 $x_j^* \geq 1/f$ 时我们将 $j \in I$。因此，$j \in I$ 意味着 $j \in I'$，所以 $I' \supseteq I$。

## 1.5 Constructing a dual solution: primal-dual method

前面两节算法的一个缺点是它们需要求解线性规划。虽然线性规划可以高效求解，在实践中算法也很快，但专用算法通常要快更多。

我们不需要实际求解对偶LP，而是可以构造一个具有相同性质的可行对偶解。在这种情况下，构造对偶解比求解对偶LP要快得多，因此导致更快的算法。

例如针对集合覆盖问题，我们不需要求解对偶线性规划，而是可以依据 Theorem 1.7 的证明方法，直接写出一个构造对偶解的算法：

!!! Algorithm "Algorithm 1.1: 集合覆盖问题的原始-对偶算法"

    ```
    y ← 0
    I ← ∅
    while 存在 e_i ∉ U_{j∈I} S_j do
        增加对偶变量 y_i 直到存在某个 ℓ 使得 e_i ∈ S_ℓ 且
            ∑_{j: e_j ∈ S_ℓ} y_j = w_ℓ
        I ← I ∪ {ℓ}
    ```

!!! Theorem "Theorem 1.9"

    算法1.1是集合覆盖问题的 $f$-近似算法。

这种类型的算法通过与其他组合算法中使用的原始-对偶方法的类比称为**原始-对偶算法**。线性规划问题、网络流问题和最短路径问题（以及其他）都有原始-对偶优化算法；我们将在第7.3节看到最短 $s$-$t$ 路径问题的原始-对偶算法示例。

原始-对偶算法从对偶可行解开始，使用对偶信息推断原始的（可能不可行的）解。如果原始解确实不可行，则修改对偶解以增加对偶目标函数的值。原始-对偶方法在设计近似算法方面非常有用，我们将在第7章中广泛讨论。

## 1.6 A greedy algorithm

本节中将展示一种贪心算法，给出了性能保证通常显著优于 $f$ 的近似算法。

我们提出一个非常自然的集合覆盖问题贪心算法：集合是按轮次选择的。在每一轮中，我们选择给我们最大"性价比"的集合；即，**使其权重与它包含的当前未覆盖元素数量的比值最小的集合**。在平局的情况下，我们选择实现最小比值的任意集合。

我们继续选择集合直到所有元素都被覆盖。显然，这将产生一个多项式时间算法，因为最多有 $m$ 轮，每轮我们计算 $O(m)$ 个比值，每个在常数时间内计算。算法1.2给出了正式描述。

!!! Algorithm "Algorithm 1.2: 集合覆盖问题的贪心算法"

    ```
    I ← ∅
    Ŝ_j ← S_j  ∀j
    while I is not a set cover do
        ℓ ← arg min_{j: Ŝ_j ≠ ∅} w_j/|Ŝ_j|
        I ← I ∪ {ℓ}
        Ŝ_j ← Ŝ_j - S_ℓ  ∀j
    ```

在陈述定理之前，我们需要一些记号和一个有用的数学事实。设 $H_k$ 表示第 $k$ 个**调和数**：即，$H_k = 1 + \frac{1}{2} + \frac{1}{3} + \cdots + \frac{1}{k}$。注意 $H_k \approx \ln k$。

以下事实是我们在本书过程中将多次使用的。它可以用简单的代数操作证明。

!!! Fact "Fact 1.10"

    给定正数 $a_1, \ldots, a_k$ 和 $b_1, \ldots, b_k$，则

    $$\min_{i=1,\ldots,k} \frac{a_i}{b_i} \leq \frac{\sum_{i=1}^k a_i}{\sum_{i=1}^k b_i} \leq \max_{i=1,\ldots,k} \frac{a_i}{b_i}.$$

!!! Theorem "Theorem 1.11"

    算法1.2是集合覆盖问题的 $H_n$-近似算法。

!!! Warning "Warning"

    本节以下的证明都还没看，留坑。

!!! Info "Proof"

    算法分析的基本直觉如下。设 OPT 表示集合覆盖问题最优解的值。我们知道最优解用权重 OPT 的解覆盖所有 $n$ 个元素；因此，必须存在某个子集，它以平均权重最多 $\text{OPT}/n$ 覆盖其元素。

    类似地，在覆盖了 $k$ 个元素后，最优解可以用权重 OPT 的解覆盖剩余的 $n - k$ 个元素，这意味着存在某个子集，它以平均权重最多 $\text{OPT}/(n - k)$ 覆盖其剩余的未覆盖元素。

    因此，一般地，贪心算法为覆盖第 $k$ 个未覆盖元素支付大约 $\text{OPT}/(n - k + 1)$，给出性能保证 $\sum_{k=1}^n \frac{1}{n-k+1} = H_n$。

    我们现在正式化这个直觉。设 $n_k$ 表示在第 $k$ 次迭代开始时仍未覆盖的元素数量。如果算法进行 $\ell$ 次迭代，则 $n_1 = n$，我们设 $n_{\ell+1} = 0$。选择任意迭代 $k$。设 $I_k$ 表示在迭代1到 $k-1$ 中选择的集合索引，对于每个 $j = 1, \ldots, m$，设 $\hat{S}_j$ 表示在此迭代开始时 $S_j$ 中未覆盖元素的集合；即，$\hat{S}_j = S_j - \bigcup_{p \in I_k} S_p$。那么我们声称，对于第 $k$ 次迭代中选择的集合 $j$，

    $$w_j \leq \frac{n_k - n_{k+1}}{n_k} \text{OPT}. \ \ (1.5)$$

    给定声称的不等式(1.5)，我们可以证明定理。设 $I$ 包含我们最终解中集合的索引。则

    \begin{align*}
    \sum_{j \in I} w_j &\leq \sum_{k=1}^{\ell} \frac{n_k - n_{k+1}}{n_k} \text{OPT} \\
    &\leq \text{OPT} \cdot \sum_{k=1}^{\ell} \left( \frac{1}{n_k} + \frac{1}{n_k - 1} + \cdots + \frac{1}{n_{k+1} + 1} \right) \ \ (1.6) \\
    &= \text{OPT} \cdot \sum_{i=1}^n \frac{1}{i} \\
    &= H_n \cdot \text{OPT},
    \end{align*}

    其中不等式(1.6)来自对于每个 $0 \leq i < n_k$，$\frac{1}{n_k} \leq \frac{1}{n_k-i}$ 的事实。

    为了证明声称的不等式(1.5)，我们首先论证在第 $k$ 次迭代中，

    $$\min_{j: \hat{S}_j \neq \emptyset} \frac{w_j}{|\hat{S}_j|} \leq \frac{\text{OPT}}{n_k}. \ \ (1.7)$$

    如果我们设 $O$ 包含最优解中集合的索引，则不等式(1.7)来自事实1.10，观察到

    $$\min_{j: \hat{S}_j \neq \emptyset} \frac{w_j}{|\hat{S}_j|} \leq \frac{\sum_{j \in O} w_j}{\sum_{j \in O} |\hat{S}_j|} = \frac{\text{OPT}}{\sum_{j \in O} |\hat{S}_j|} \leq \frac{\text{OPT}}{n_k},$$

    其中最后的不等式来自这样的事实：由于 $O$ 是集合覆盖，集合 $\bigcup_{j \in O} \hat{S}_j$ 必须包含所有剩余的 $n_k$ 个未覆盖元素。

    设 $j$ 为使此比值最小的子集索引，所以 $\frac{w_j}{|\hat{S}_j|} \leq \frac{\text{OPT}}{n_k}$。如果我们将子集 $S_j$ 添加到我们的解中，则会有 $|\hat{S}_j|$ 个更少的未覆盖元素，所以 $n_{k+1} = n_k - |\hat{S}_j|$。因此，

    $$w_j \leq \frac{|\hat{S}_j| \text{OPT}}{n_k} = \frac{n_k - n_{k+1}}{n_k} \text{OPT}. \qquad \square$$

我们可以通过在分析中使用线性规划松弛的对偶来略微改进算法的性能保证。设 $g$ 为任何子集 $S_j$ 的最大大小；即，$g = \max_j |S_j|$。回想 $Z_{LP}^*$ 是集合覆盖问题线性规划松弛的最优值。以下定理立即意味着贪心算法是 $H_g$-近似算法，因为 $Z_{LP}^* \leq \text{OPT}$。

!!! Theorem "Theorem 1.12"

    算法1.2返回由 $I$ 索引的解，使得 $\sum_{j \in I} w_j \leq H_g \cdot Z_{LP}^*$。

!!! Info "Proof"

    为了证明定理，我们将构造一个**不可行对偶解** $y$，使得 $\sum_{j \in I} w_j = \sum_{i=1}^n y_i$。然后我们将显示 $y' = \frac{1}{H_g} y$ 是可行对偶解。由弱对偶性定理，$\sum_{i=1}^n y_i' \leq Z_{LP}^*$，所以 $\sum_{j \in I} w_j = \sum_{i=1}^n y_i = H_g \sum_{i=1}^n y_i' \leq H_g \cdot \text{OPT}$。

    我们将在证明结尾看到我们选择将不可行对偶解 $y$ 除以 $H_g$ 的原因。

    **对偶拟合**这一名称已被赋予这种构造不可行对偶解的技术，该解的值等于所构造的原始解的值，以及通过单个值缩放对偶解使其可行。我们将在第9.4节回到这种技术。

    为了构造不可行对偶解 $y$，假设我们选择在迭代 $k$ 中将子集 $S_j$ 添加到我们的解中。然后对于每个 $e_i \in \hat{S}_j$，我们设 $y_i = w_j/|\hat{S}_j|$。由于每个 $e_i \in \hat{S}_j$ 在迭代 $k$ 中未覆盖，并且在算法的剩余迭代中被覆盖（因为我们将子集 $S_j$ 添加到解中），对偶变量 $y_i$ 被设为恰好一次的值；特别地，它在覆盖元素 $e_i$ 的迭代中被设置。

    此外，$w_j = \sum_{i: e_i \in \hat{S}_j} y_i$；即，在第 $k$ 次迭代中选择的子集 $S_j$ 的权重等于在第 $k$ 次迭代中覆盖的未覆盖元素的对偶 $y_i$ 的和。这立即意味着 $\sum_{j \in I} w_j = \sum_{i=1}^n y_i$。

    剩下的是证明对偶解 $y' = \frac{1}{H_g} y$ 是可行的。我们必须显示对于每个子集 $S_j$，$\sum_{i: e_i \in S_j} y_i' \leq w_j$。选择任意子集 $S_j$。设 $a_k$ 为在第 $k$ 次迭代开始时此子集中仍未覆盖的元素数量，所以 $a_1 = |S_j|$，$a_{\ell+1} = 0$。设 $A_k$ 为在第 $k$ 次迭代中覆盖的 $S_j$ 的未覆盖元素，所以 $|A_k| = a_k - a_{k+1}$。如果在第 $k$ 次迭代中选择子集 $S_p$，则对于在第 $k$ 次迭代中覆盖的每个元素 $e_i \in A_k$，

    $$y_i' = \frac{w_p}{H_g |\hat{S}_p|} \leq \frac{w_j}{H_g a_k},$$

    其中 $\hat{S}_p$ 是第 $k$ 次迭代开始时 $S_p$ 的未覆盖元素集合。不等式成立是因为如果在第 $k$ 次迭代中选择 $S_p$，它必须使其权重与它包含的未覆盖元素数量的比值最小。因此，

    \begin{align*}
    \sum_{i: e_i \in S_j} y_i' &= \sum_{k=1}^{\ell} \sum_{i: e_i \in A_k} y_i' \\
    &\leq \sum_{k=1}^{\ell} (a_k - a_{k+1}) \frac{w_j}{H_g a_k} \\
    &\leq \frac{w_j}{H_g} \sum_{k=1}^{\ell} \frac{a_k - a_{k+1}}{a_k} \\
    &\leq \frac{w_j}{H_g} \sum_{k=1}^{\ell} \left( \frac{1}{a_k} + \frac{1}{a_k - 1} + \cdots + \frac{1}{a_{k+1} + 1} \right) \\
    &\leq \frac{w_j}{H_g} \sum_{i=1}^{|S_j|} \frac{1}{i} \\
    &= \frac{w_j}{H_g} H_{|S_j|} \\
    &\leq w_j,
    \end{align*}

    其中最后的不等式来自 $|S_j| \leq g$ 的事实。这里我们看到将对偶解按 $H_g$ 缩放的原因，因为我们知道对于所有集合 $j$，$H_{|S_j|} \leq H_g$。 $\square$

事实证明，在比 P ≠ NP 稍强的假设下，对于集合覆盖问题，不可能有性能保证优于 $H_n$ 的近似算法。

!!! Theorem "Theorem 1.13"

    如果对于某个常数 $c < 1$，无权重集合覆盖问题存在 $c \ln n$-近似算法，那么对于每个 NP-完全问题存在 $O(n^{O(\log \log n)})$-时间确定性算法。

!!! Theorem "Theorem 1.14"

    存在某个常数 $c > 0$，使得如果无权重集合覆盖问题存在 $c \ln n$-近似算法，则 P = NP。

我们将在第16章更详细地讨论这类结果；在定理16.32中，我们展示了如何导出这些结果的稍弱版本。这类结果有时称为**困难性定理**，因为它们表明为具有某些性能保证的特定问题提供近最优解是 NP-困难的。

集合覆盖问题的 $f$-近似算法意味着顶点覆盖问题特殊情况的2-近似算法。目前没有已知常数性能保证更好的算法。此外，已经证明了两个困难性定理，定理1.15和1.16如下。

!!! Theorem "Theorem 1.15"

    如果顶点覆盖问题存在 $\alpha$-近似算法，其中 $\alpha < 10\sqrt{5} - 21 \approx 1.36$，则 P = NP。

以下定理提到了一个称为**唯一博弈猜想**的猜想，我们将在第13.3节和第16.5节中更多讨论。该猜想大致是说特定问题（称为唯一博弈）是 NP-困难的。

!!! Theorem "Theorem 1.16"

    假设唯一博弈猜想，如果顶点覆盖问题存在常数 $\alpha < 2$ 的 $\alpha$-近似算法，则 P = NP。

因此，假设 P ≠ NP 和唯一博弈问题的 NP-完全性，我们基本上找到了顶点覆盖问题的最佳近似算法。

## 1.7 随机化舍入算法

在本节中，我们考虑为集合覆盖问题设计近似算法的最后一种技术。虽然算法比前一节的贪心算法更慢且没有更好的保证，但我们将它包括在这里是因为它引入了在近似算法中使用随机化的概念，这个想法我们将在第5章深入讨论。

与第1.3节的算法一样，该算法将求解集合覆盖问题的线性规划松弛，然后将分数解舍入为整数解。然而，该算法不是确定性地这样做，而是使用一种称为**随机化舍入**的技术随机地进行。

设 $x^*$ 是 LP 松弛的最优解。我们希望将 $x^*$ 的分数值舍入为 0 或 1，以获得集合覆盖问题整数规划表述的解 $\hat{x}$，而不会过多增加成本。随机化舍入的核心思想是我们将分数值 $x_j^*$ 解释为 $\hat{x}_j$ 应该设为 1 的概率。

设 $X_j$ 是随机变量，如果子集 $S_j$ 包含在解中则为 1，否则为 0。那么解的期望值为

$$E\left[\sum_{j=1}^m w_j X_j\right] = \sum_{j=1}^m w_j \Pr[X_j = 1] = \sum_{j=1}^m w_j x_j^* = Z_{LP}^*,$$

让我们现在计算给定元素 $e_i$ 未被此过程覆盖的概率。这是包含 $e_i$ 的子集都不包含在解中的概率，即

$$\prod_{j: e_i \in S_j} (1 - x_j^*).$$

我们可以使用 $1 - x \leq e^{-x}$（对任何 $x$，其中 $e$ 是自然对数的底）这一事实来限制这个概率。那么

\begin{align*}
\Pr[e_i \text{ not covered}] &= \prod_{j: e_i \in S_j} (1 - x_j^*) \\
&\leq \prod_{j: e_i \in S_j} e^{-x_j^*} \\
&= e^{-\sum_{j: e_i \in S_j} x_j^*} \\
&\leq e^{-1},
\end{align*}

其中最后的不等式来自 LP 约束 $\sum_{j: e_i \in S_j} x_j^* \geq 1$。虽然 $e^{-1}$ 是给定元素未被覆盖概率的上界，但可以任意接近这个界，所以在最坏情况下，这种随机化舍入过程很可能不会产生集合覆盖。

那么这个概率需要多小才能很可能产生集合覆盖？"很可能"的"正确"概念又是什么？

假设对于任何常数 $c$，我们可以设计一个多项式时间算法，其失败概率最多为 $n^{-c}$；那么我们说我们有一个**高概率**工作的算法。

如果可以设计一个随机化过程，使得对于某个常数 $c \geq 2$，$\Pr[e_i \text{ not covered}] \leq \frac{1}{n^c}$，那么

$$\Pr[\text{存在未覆盖元素}] \leq \sum_{i=1}^n \Pr[e_i \text{ not covered}] \leq \frac{1}{n^{c-1}},$$

我们将有高概率的集合覆盖。实际上，可以通过以下方式实现这样的界：对于每个子集 $S_j$，我们想象一个以概率 $x_j^*$ 正面朝上的硬币，我们掷硬币 $c \ln n$ 次。如果在 $c \ln n$ 次试验中的任何一次中正面朝上，我们在解中包含 $S_j$，否则不包含。因此，$S_j$ 不被包含的概率是 $(1 - x_j^*)^{c \ln n}$。此外，

\begin{align*}
\Pr[e_i \text{ not covered}] &= \prod_{j: e_i \in S_j} (1 - x_j^*)^{c \ln n} \\
&\leq \prod_{j: e_i \in S_j} e^{-x_j^*(c \ln n)} \\
&= e^{-(c \ln n) \sum_{j: e_i \in S_j} x_j^*} \\
&\leq \frac{1}{n^c},
\end{align*}

如所需。

现在我们只需要证明在产生集合覆盖的条件下，算法有良好的期望值。

!!! Theorem "Theorem 1.17"

    该算法是一个随机化 $O(\ln n)$-近似算法，它以高概率产生集合覆盖。

!!! Warning "Warning"

    这个证明也没看。

!!! Info "Proof"

    设 $p_j(x_j^*)$ 是给定子集 $S_j$ 作为 $x_j^*$ 的函数包含在解中的概率。根据算法的构造，我们知道 $p_j(x_j^*) = 1 - (1 - x_j^*)^{c \ln n}$。

    观察到如果 $x_j^* \in [0,1]$ 且 $c \ln n \geq 1$，那么我们可以在 $x_j^*$ 处用 $c \ln n$ 界定导数 $p_j'$：

    $$p_j'(x_j^*) = (c \ln n)(1 - x_j^*)^{(c \ln n)-1} \leq (c \ln n).$$

    那么由于 $p_j(0) = 0$，函数 $p_j$ 的斜率在区间 $[0,1]$ 上被 $c \ln n$ 界定，所以 $p_j(x_j^*) \leq (c \ln n)x_j^*$ 在区间 $[0,1]$ 上。

    如果 $X_j$ 是随机变量，如果子集 $S_j$ 包含在解中则为 1，否则为 0，那么随机过程的期望值为

    \begin{align*}
    E\left[\sum_{j=1}^m w_j X_j\right] &= \sum_{j=1}^m w_j \Pr[X_j = 1] \\
    &\leq \sum_{j=1}^m w_j (c \ln n) x_j^* \\
    &= (c \ln n) \sum_{j=1}^m w_j x_j^* = (c \ln n) Z_{LP}^*.
    \end{align*}

    然而，我们希望在产生集合覆盖的条件下界定解的期望值。设 $F$ 是过程获得的解是可行集合覆盖的事件，设 $\bar{F}$ 是此事件的补集。我们从前面的讨论知道 $\Pr[F] \geq 1 - \frac{1}{n^{c-1}}$，我们也知道

    $$E\left[\sum_{j=1}^m w_j X_j\right] = E\left[\sum_{j=1}^m w_j X_j \mid F\right] \Pr[F] + E\left[\sum_{j=1}^m w_j X_j \mid \bar{F}\right] \Pr[\bar{F}].$$

    由于对所有 $j$，$w_j \geq 0$，

    $$E\left[\sum_{j=1}^m w_j X_j \mid \bar{F}\right] \geq 0.$$

    因此，

    \begin{align*}
    E\left[\sum_{j=1}^m w_j X_j \mid F\right] &= \frac{1}{\Pr[F]} \left( E\left[\sum_{j=1}^m w_j X_j\right] - E\left[\sum_{j=1}^m w_j X_j \mid \bar{F}\right] \Pr[\bar{F}] \right) \\
    &\leq \frac{1}{\Pr[F]} \cdot E\left[\sum_{j=1}^m w_j X_j\right] \\
    &\leq \frac{(c \ln n) Z_{LP}^*}{1 - \frac{1}{n^{c-1}}} \\
    &\leq 2c(\ln n) Z_{LP}^*
    \end{align*}

    对于 $n \geq 2$ 和 $c \geq 2$。 $\square$

虽然在这种情况下有更简单更快的近似算法实现更好的性能保证，但我们将在第5章看到，有时随机化算法比确定性算法更简单地描述和分析。实际上，我们在本书中提出的大多数随机化算法都可以**去随机化**：即，可以创建它们的确定性变体，实现随机化算法的期望性能保证。

