# Chapter 1: Introduction

## 1.1 Definitions and terminology

- **Examples**：用于学习或评估的数据项或实例。在垃圾邮件问题中，这些 examples 对应于我们用于学习和测试的电子邮件消息集合。

- **Features**：与 example 相关联的属性集合，通常表示为向量。对于电子邮件消息，相关 features 可能包括消息长度、发送者姓名、header 的各种特征、消息正文中某些关键词的存在等。

- **Labels**：分配给 examples 的值或类别。在分类问题中，examples 被分配到特定类别，例如垃圾邮件二分类问题中的 SPAM 和 non-SPAM 类别。在回归中，项目被分配 real-valued labels。

- **Training sample**：用于训练 learning algorithm 的 examples。在垃圾邮件问题中，training sample 由一组 email examples 及其关联 labels 组成。training sample 在不同学习场景中会有所变化。

- **Validation sample**：在处理 labeled data 时用于调整 learning algorithm 参数的 examples。Learning algorithms 通常有一个或多个 free parameters，validation sample 用于为这些 model parameters 选择适当的值。

- **Test sample**：用于评估 learning algorithm 性能的 examples。test sample 与 training 和 validation data 分离，在学习阶段不可用。在垃圾邮件问题中，test sample 由一组 email examples 组成，learning algorithm 必须根据 features 预测 labels。这些预测然后与 test sample 的 labels 进行比较，以衡量算法性能。

- **Loss function**：衡量 predicted label 和 true label 之间差异或损失的函数。将所有 labels 的集合记为 $\mathcal{Y}$，可能预测的集合记为 $\mathcal{Y}'$，loss function $L$ 是映射 $L: \mathcal{Y} \times \mathcal{Y}' \rightarrow \mathbb{R}_+$。常见的 loss functions 包括 zero-one (or misclassification) loss 和 squared loss。

- **Hypothesis set**：将 features (feature vectors) 映射到 labels 集合 $\mathcal{Y}$ 的函数集合。在我们的例子中，这些可能是将 email features 映射到 $\mathcal{Y} = \{\text{SPAM}, \text{non-SPAM}\}$ 的函数集合。更一般地，hypotheses 可能是将 features 映射到不同集合 $\mathcal{Y}'$ 的函数，比如将 email feature vectors 映射到被解释为 scores 的 real numbers ($\mathcal{Y}' = \mathbb{R}$)，其中更高的 score 值更倾向于 SPAM。

## 1.2 Cross-validation

在实践中，可用的 labeled data 的数量通常太少，无法单独设置 validation sample，因为这会导致 training data 不足。因此我们提出 **n-fold cross-validation**，用于充分利用 labeled data 进行 **model selection**（算法自由参数的选择）和训练。

设 $\theta$ 表示算法的自由参数向量。对于 $\theta$ 的固定值，该方法首先将给定的样本 $S$（包含 $m$ 个 labeled examples）随机分割成 $n$ 个子样本或 fold。第 $i$ 个 fold 是一个 labeled sample $((x_{i1}, y_{i1}), \ldots, (x_{im_i}, y_{im_i}))$，大小为 $m_i$。然后，对于任何 $i \in [1, n]$，learning algorithm 在除第 $i$ 个 fold 之外的所有 fold 上进行训练以生成假设 $h_i$，并在第 $i$ 个 fold 上测试 $h_i$ 的性能。参数值 $\theta$ 基于假设 $h_i$ 的平均误差进行评估，这称为 **cross-validation error**。该量用 $\hat{R}_{\text{CV}}(\theta)$ 表示，定义为：

$$\hat{R}_{\text{CV}}(\theta) = \frac{1}{n} \sum_{i=1}^{n} \frac{1}{m_i} \sum_{j=1}^{m_i} L(h_i(x_{ij}), y_{ij})$$

一个很自然的问题是，我们应该如何选择 $n$ 的大小？

如果 $n$ 较大，最极端的情况是我们用 $n-1$ 个样本进行训练，1 个样本进行验证（leave-one-out cross-validation），不考虑过拟合的情况训练出来的模型相对真实情况的偏差（bias）会小，但是评估结果的方差（variance）会大；而如果 $n$ 较小，因为训练集变小了，训练出来的模型相对真实情况的偏差会大，但是评估结果的方差因为样本变多所以会小。

在机器学习应用中，$n$ 通常选择为 5 或 10。n-fold cross validation 在 model selection 中的使用如下：

1. 将完整的 labeled data 分为 training 和 test sample
2. 使用大小为 $m$ 的 training sample 计算候选 $\theta$ 值的 n-fold cross-validation error $\hat{R}_{\text{CV}}(\theta)$
3. 将 $\theta$ 设置为使 $\hat{R}_{\text{CV}}(\theta)$ 最小的值 $\theta_0$
4. 使用参数设置 $\theta_0$ 在完整的大小为 $m$ 的 training sample 上训练算法
5. 在 test sample 上评估其性能。

## 1.3 Learning scenarios

- **Supervised learning**：learner 接收一组 labeled examples 作为 training data，并对所有未见点进行预测。这是与 classification、regression 和 ranking problems 相关的最常见场景。前面讨论的垃圾邮件检测问题是 supervised learning 的一个实例。

- **Unsupervised learning**：learner 仅接收 unlabeled training data，并对所有未见点进行预测。由于在该设置中通常没有可用的 labeled example，因此很难定量评估 learner 的性能。Clustering 和 dimensionality reduction 是 unsupervised learning problems 的例子。

- **Semi-supervised learning**：learner 接收包含 labeled 和 unlabeled data 的 training sample，并对所有未见点进行预测。Semi-supervised learning 在 unlabeled data 容易获得但 labels 获取成本高昂的环境中很常见。各种类型的问题在应用中出现，包括 classification、regression 或 ranking tasks，都可以作为 semi-supervised learning 的实例。希望是 learner 可以访问的 unlabeled data 的分布可以帮助他获得比 supervised setting 更好的性能。分析在哪些条件下确实可以实现这一点是现代理论和应用机器学习研究的许多主题。

- **Transductive inference**：与 semi-supervised scenario 一样，learner 接收 labeled training sample 以及一组 unlabeled test points。然而，transductive inference 的目标是仅为这些特定的 test points 预测 labels。Transductive inference 似乎是一个更容易的任务，并且符合各种现代应用中遇到的场景。然而，如同在 semi-supervised setting 中，在这种设置中可以获得更好性能的假设是尚未完全解决的研究问题。

- **On-line learning**：与之前的场景相反，online scenario 涉及多轮，training 和 testing phases 是交错的。在每一轮中，learner 接收一个 unlabeled training point，做出预测，接收 true label，并产生损失。on-line setting 的目标是最小化所有轮次的累积损失。与刚才讨论的之前设置不同，在 on-line learning 中不做分布假设。实际上，instances 和它们的 labels 可能在此场景中被对抗性地选择。

- **Reinforcement learning**：training 和 testing phases 在 reinforcement learning 中也是交错的。为了收集信息，learner 主动与环境交互，在某些情况下影响环境，并为每个动作接收即时奖励。learner 的目标是在一系列动作和与环境的迭代中最大化他的奖励。然而，环境不提供长期奖励反馈，learner 面临 **exploration versus exploitation** 困境，因为他必须在探索未知动作以获得更多信息与利用已收集信息之间做出选择。

- **Active learning**：learner 自适应地或交互式地收集 training examples，通常通过查询 oracle 来请求新点的 labels。active learning 的目标是实现与标准 supervised learning scenario 相当的性能，但使用更少的 labeled examples。Active learning 通常用于 labels 获取成本高昂的应用中，例如 computational biology applications。

