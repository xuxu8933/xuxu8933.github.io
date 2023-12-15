---
title: 逆向优化
categories: [Note]
tags: [optimization, math]
math: true
---


优化问题使用目标函数和约束条件作为输入而问题的优化解为最优决策. 相对的, 逆向优化问题把决策作为输入并将目标函数或者约束条件作为输出. 具体来说, 逆向优化问题求解优化问题模型, 也称为前向模型(forward model)的参数使得给定的决策是优化问题模型的近似或者最优解. 求解参数本身也可以构建为一个优化模型, 并称为逆向模型(inverse model).
<!--more-->

很多问题可以归纳为逆向问题. 例如, 在控制领域中, 受控器模型的参数辨识(identification of plant parameters)或者目标函数的权重参数选择(selection of weighting parameters). 逆向优化可应用于多种领域, 例如物流, 医疗和电力系统, 其中使用观测的决策数据来估测一个决策模型并重现这些观测值. 决策数据可以是路线选择,电力消耗模式或者医疗诊断, 并且逆向诱惑模型可以用来估测路线偏好,效用函数或者治疗目标.

## 问题描述

### 前向模型

$$ \textbf{FOP}(\theta): \qquad
\begin{aligned}
  \min_x \ & f(x,\theta) \\
  \text{s.t.} \ & x \in \mathcal{X(\theta)}
\end{aligned} \tag{1}
$$

模型通过参数 $\theta$ 表征并且可以用来调整目标函数和约束条件.

设

$$
\mathcal{X}^{opt}(\theta):=\arg\min_x \{ f(x,\theta) | x\in \mathcal{X(\theta)}\}
$$

为*最优解集合(optimal solution set)*.

### 逆向模型

考虑一个数据集 $\{\hat{x}_i\}$ for $i=1,\dots,N$. 这些数据为不同条件下前向模型得出的决策. 将这些不同条件的前向模型标为 $\textbf{FOP}_i(\theta)$, 并且对应的最优解集合为 $\mathcal{X}_i^{opt}(\theta)$. 给出数据集 $\hat{x}_i$, 逆向优化估测一个参数向量 $\theta^\star$ 使得前向模型 $\text{FOP}_i(\theta^\star)$ 对于数据 $\hat{x}_i$ 的拟合 得到优化. 一个估参 $\theta^\star$ 可以被认为是完美拟合 $\hat{x}_i$ 如果 $\hat{x}_i\in\mathcal{X}_i^{opt}(\theta^\star) $. 这样的完美拟合数据的参数集合称为*逆向可行集合(inverse feasible set)*.

$$
\Theta_i^{inv}(\hat{x}_i):=\{\theta|\hat{x}_i \in \mathcal{X}_i^{opt}(\theta) \}
$$

根据是否能严格保证参数在给定数据下逆向可行性, 逆向模型的构建可以分为两种: *经典建模(classic)*和*数据驱动建模(data-driven)*.

#### 经典模型
  
- 假定生成数据的决策过程已知, 也就是已知描述决策过程的前向模型.
- 参数 $\theta\in\Theta$ 被逆向可行集严格约束  $\theta\in\Theta_i^{inv}(\hat{x}_i), \forall i\in\{1,\dots,N \} $, 也就是对于所以条件下的前向模型和决策数据逆向可行(inverse-feasible).
- 构建以下优化问题:

  $$
  \min_\theta \bigg\{ h(\theta) \bigg| \theta\in\Theta_i^{inv}(\hat{x}_i), \forall i\in \{1,\dots,N \}, \theta\in\Theta \bigg\}
  $$

  其中 $ h(\theta) $是一个根据具体应用定义的目标函数, 比如 $h(\theta)=\| \theta - \hat{\theta} \|_p $, $\hat{\theta}$ 是一个固定的先验或者估计值.

- 逆向可行性使问题成为双级规划问题(bilevel program), 可变换为单级优化(single-level)便于求解.
- 多用于设计应用(design problem).

#### 数据驱动模型

- 考虑观测的数据集包涵测量噪声, 决策模型的不一致或者模型没有观测的因素.
- 对于所有数据集, 参数的逆向可行性无法严格约束.
- 定义损失函数(loss function) $l(x_i, \mathcal{X}_i^{opt}(\theta))$ 用来惩罚逆向可行性的违背程度, 构建以下优化问题.

  $$
  \min_\theta \bigg\{ \kappa h(\theta) + \frac{1}{N}\sum_{i=1}^N l(\hat{x}_i, \mathcal{X}_i^{opt}(\theta)) \bigg| \theta\in\Theta \bigg\}
  $$

- 直观的测量办法是计算模型到观测值的误差

  $$
  l_D(\hat{x},\mathcal{X}^{opt}(\theta)):=\min_{x\in\mathcal{X}^{opt}(\theta)} \| x-\hat{x}\|_2
  $$

- 多用于估计应用(estimation problem).

## 结论

逆向问题将观察的数据作为问题的输入而输出则为前向模型的参数值. 通过数据训练模型也是神经网络或者加强学习中的重要概念.

## 参考

Chan, T. C., Mahmood, R., & Zhu, I. Y. (2021). Inverse optimization: Theory and applications. arXiv preprint arXiv:2109.03920
