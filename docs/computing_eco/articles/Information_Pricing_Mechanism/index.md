# 信息定价中的机制设计

## 前置知识

!!! Definition "个人理性"

    个人理性（Individual Rationality, IR）：

    个人理性指参与拍卖的竞标者自愿参与时，其预期收益（或效用）至少不低于不参与时的保留效用（通常假设为0）。即竞标者不会因参与拍卖而遭受损失。

!!! Definition "激励相容"

    激励相容（Incentive Compatibility, IC）：

    **定义：**
    一个机制是激励相容的，如果竞标者通过真实报告其私人信息（如估值）能获得最大效用，即说真话是最优策略。

    **分类：**

    - 占优策略激励相容（DSIC）：无论其他竞标者如何行为，说真话都是最优策略（如Vickrey拍卖）。
    - 贝叶斯激励相容（BIC）：在贝叶斯纳什均衡下，说真话是最优策略（需考虑其他竞标者的分布信念）。

!!! Definition "显示原理"

    显示原理（Revelation Principle）：

    给定任意一个机制及其占优策略均衡（或贝叶斯纳什均衡），都可以找到一个激励相容的直接机制，使得该机制均衡下的结果和原机制均衡下对应的结果一致。

## 问题背景

### 1. 模型设定 (Setting)

#### 1.1 基本框架

研究场景包含一个卖断卖方和一个买方，双方关于世界状态 $\omega \in \Omega$ 和买方私有类型 $\theta \in \Theta$ 拥有不对称信息：

- **卖方**: 观测到信号 $\omega$ (如用户 demographics)。

- **买方**: 观测到信号 $\theta$ (如用户浏览历史)，并基于此选择行动 $a \in A$ (如广告投放)。

- **联合分布**: $(\omega, \theta)$ 服从联合概率分布 $\mu \in \Delta(\Omega \times \Theta)$。

- **效用函数**: 买方收益为 $u(\theta, \omega, a)$，其目标是最大化期望收益。

#### 1.2 信息价值量化

买方在未获取卖方信息时的期望收益为:

$$V_0(\theta) = \max_a \mathbb{E}_\omega [u(\theta, \omega, a) | \theta]$$

若买方完全获知 $\omega$，期望收益提升至:

$$V_1(\theta) = \mathbb{E}_\omega [\max_a u(\theta, \omega, a) | \theta]$$

**信息剩余价值**定义为:

$$\xi(\theta) = V_1(\theta) - V_0(\theta)$$

卖方目标是通过机制设计最大化从 $\xi(\theta)$ 中提取的收益。

### 2. 机制设计基础

#### 2.1 密封信封机制 (Sealed Envelope Mechanism)

- **操作**: 卖方将 $\omega$ 写入信封，以固定价格 $t$ 出售。

- **买方决策**: 当且仅当 $\xi(\theta) \geq t$ 时购买。

- **收益**: $t \cdot \mathbb{P}_{\theta \sim \mu}[\xi(\theta) \geq t]$。

- **局限性**:

    - 卖方未利用 $\omega$ 调整定价，可能损失收益。
    - 若定价依赖 $\omega$ (如 $t(\omega)$)，买方可能通过价格反推 $\omega$，导致信息泄露。

#### 2.2 通用交互协议 (Generic Interactive Protocol)

- **协议树**: 由卖方节点 (基于 $\omega$ 发送信号)、买方节点 (策略性选择行动)、支付节点 (资金转移) 构成。

- **买方策略类型**:
    - **承诺型**: 必须完成协议。
    - **非承诺型**: 可中途退出 (如拒绝支付)。

- **效用计算**: 买方通过贝叶斯更新后验信念，最大化 $U(\theta, \phi) = \mathbb{E}[u(\theta, \omega, a)|Z] - \tau(Z)$，其中 $Z$ 为协议终止节点，$\tau(Z)$ 为累计支付。 

## Articles

接下来将详细介绍两篇论文：

- Moshe, Bobby 的 Optimal Mechanisms for Selling Information，主要聚焦于当用户和信息卖方私有信息独立/不独立时，如何设计最优机制。
- Yiling, Haifeng, Shuran 的 Selling Information Through Consulting，主要聚焦于如何避免用户中途退出，并且不适用过高的押金。

论文名称 | 笔记链接 | 原论文链接 | 讲义
--- | --- | --- | ---
Moshe, Bobby-Optimal Mechanisms for Selling Information(2012) | [笔记](Moshe,Bobby.md) | [原论文](Moshe,Bobby.pdf) | [讲义](slides.pdf)
Yiling, Haifeng, Shuran-Selling Information Through Consulting(2020) | [笔记](Yiling,Haifeng,Shuran.md) | [原论文](Yiling,Haifeng,Shuran.pdf) | ---

## Table of Contents

### 1. Introduction

#### 1.1 问题背景

#### 1.2 Main Results

### 2. Preliminaries

#### 2.1 个人理性、激励相容、显示原理

#### 2.2 Basic Setup

#### 2.3 The Design Space

#### 2.4 Information Revelation via Signaling Schemes

#### 2.5 Player Utilities and the Revenue Maximization Problem

#### 2.6 Envelope

### 3. Independent Signals

#### 3.1 Public Budget

##### 3.1.1 Pricing Mappings

##### 3.1.2 CM-dirP

#### 3.2 Private Budget - CM-depR

#### 3.3 Computing Optimal Consulting Mechanisms

### 4. Correlated Signals

#### 4.1 Pricing Outcomes

Commited?

Not Commited?

#### 4.2 CM-ProbR

#### 4.3 Computing the Optimal CM-probR

#### 4.4 Other Properties

### 5. Open Problems