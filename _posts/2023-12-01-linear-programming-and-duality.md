---
title: 线性规划和其对偶性
tags: [optimization, math]
categories: [Note]
math: true
---

线性规划和其对偶问题作为一个基本概念，在运筹学，数值优化等领域中都给出了具有启发性的见解。

<!--more-->

## 线性规划

考虑以下线性规划原始问题 (primal problem)

$$
\begin{aligned}
    \min_{x} \ & c^\top x \\
    \text{s.t.} \ & A x \geq b
\end{aligned} \tag{1}
$$

建立拉格朗日函数

$$
\mathcal{L}=c^\top x + \lambda^\top (b - A x)
$$

根据Karush-Kuhn-Tucker(KKT)给出的最优解的必要条件，如果 $x^\star$ 是上述LP问题的最优解,则存在 $\lambda^\star$ 满足一下方程组

$$
\begin{aligned}
    c - A^\top \lambda^* & = 0\\
    A x^* & \geq b \\
    \lambda^*(b - A x^*) & = 0 \\
    \lambda^* &\geq 0
\end{aligned}
$$

从 complementary condition $\lambda^\star(b-Ax^\star)=0$ 可推得 $b^\top \lambda \leq c^\top x$. 因此以 $\lambda$ 为优化变量构建对偶问题 (dual problem) 如下

$$
\begin{aligned}
    \max_\lambda \ &b^\top \lambda \\
    \text{s.t.} \ &A^\top \lambda = c \\
    & \lambda^* \geq 0
\end{aligned}
$$

其中拉格朗日函数写作

$$
\mathcal{L} = -b^\top \lambda + \mu^\top (A^\top \lambda - c) - v^\top \lambda
$$

如果 $\lambda^\star$ 是最优解，那么存在 $(\mu^*,v^*)$ 使得以下方程组成立

$$
\begin{aligned}
    -b + A \mu^* - v^* &= 0 \\
    A^\top \lambda^* & = c, \lambda^* \geq 0 \\
    v^{*\top}\lambda^* &= 0 \\
    v^* & \geq 0
\end{aligned}
$$

通过观察可发现原始问题和对偶问题的 KKT 条件等价.

不同的线性规划表达有不同的转换形式

| Primal              | Dual | Note |
| :---------------- | :------: | ----: |
| $\min c^\top x, Ax\geq b, x\geq 0$       |   $\max b^\top y, A^\top y\leq c, y\geq 0 $   | symmetric |
| $\min c^\top x, Ax\geq b$         |   $\max b^\top y, A^\top y = c, y\geq 0$   | asymmetric |
| $\min c^\top x, Ax=b,x\geq 0$ | $\max b^\top y, A^\top y\leq c$

**弱对偶性(weak duality)**:
对于每个原始问题和对偶问题的可行解 $x,y$

$$
c^\top x \leq b^\top y
$$

如果原始问题无界,那么对偶问题无解.

**强对偶性(strong duality)**:
如果两个问题的其中自由有一个最优解,那么另外一个也有最优解并且弱对偶性给出的边界是紧的,也就是

$$
\max_x c^\top x = \min_y b^\top y
$$
