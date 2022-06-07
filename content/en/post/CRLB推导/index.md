---
title: CRLB推导
subtitle: 在本篇博文中，我针对单个参数以及多个参数估计的CRLB进行了推导
summary: 在本篇博文中，我针对单个参数以及多个参数估计的CRLB进行了推导
date: 2022-06-07
math: true
diagram: true
highlight: true
---

[toc]
# Why we need CRLB
- 假定我们有n个样本数据$(x_1, x_2,...x_n)$，这些数据是独立同分布的，具有概率密度$p(x|\theta)$。其中$\theta$是确定但未知的一个参数(Deterministic but unknown)。
- 我们希望从样本数据中得到对参数$\theta$的一个估计$\hat{\theta}=T(x_1, x_2,...,x_n)$。
- 通常用于衡量估计好坏的指标是均方误差MSE：$E[(\hat{\theta}-\theta)^2]$。
- 我们希望我们得到的估计量都是最小方差无偏估计量(MVUE)，但是这往往是过于理想化的，甚至是永远不可实现的。尽管如此，我们依旧可以从MVUE中得到关于估计量的一些知识。
- 我们想要知道，最好的MVUE的表现能有多好，换句话说，均方误差最小能够有多小，即均方误差的下界$E[(\hat{\theta}-\theta)^2]\geq C(\theta,n)$。

> 下面通过一个简单的例子来找找感觉。
> - 假设我们每次采样只采一个数据$x$，该数据服从以待估计量$\theta$为均值，$\sigma^2$为方差的高斯分布。
> - 可以写出$x$的概率密度为$p(x|\theta)=\frac{1}{\sqrt{2\pi}\sigma}e^{-\frac{1}{2\sigma^2}(x-\theta)^2}$。
> - 对等式两边取对数，可以得到：$log p(x|\theta)=-log(\sqrt{2\pi}\sigma)-\frac{1}{2\sigma^2}(x-\theta)^2$
> - 再对取对数之后的结果对$\theta$求导数：$\frac{\partial}{\partial \theta}logp(x|\theta)=\frac{1}{\sigma^2}(x-\theta)$
> - 求二阶偏导：$\frac{\partial^2}{\partial \theta^2}logp(x|\theta)=-\frac{1}{\sigma^2}$
> - 即$\sigma^2=\frac{1}{-\frac{\partial^2}{\partial \theta^2}logp(x|\theta)}$
> - 在这个简单的例子中，$x$的方差$\sigma^2$就是我们估计的均方误差
> - 通过这个例子，我们能够感觉到，我们可以通过对我们的概率模型进行一些操作（求导），来得到和估计误差的一个关系，下面将详细描述这样的关系。

# CRLB for single parameter
- 假定我们有一组n个样本的数据$x=(x_1,x_2,...,x_n)^T$，我们希望通过这组数据对某个未知参数$\theta\in R$进行估计，即$\hat{\theta}=\hat{\theta}(x)$，$\hat{\theta}(x)$是我们的估计函数。
- 我们要求$\hat{\theta}$是一个无偏估计量，即$E[\hat{\theta}-\theta]=0$。
- 于是我们得到：$$\int_{R^n}(\hat{\theta}-\theta)p(x|\theta)dx=0 \\ \Rightarrow\frac{\partial}{\partial \theta}\int_{R^n}(\hat{\theta}-\theta)p(x|\theta)dx=0 \\ \Rightarrow -\int_{R^n}p(x|\theta)dx+\int_{R^n}(\hat{\theta}-\theta)\frac{\partial}{\partial \theta}p(x|\theta)dx=0 \\ \Rightarrow\int_{R^n}(\hat{\theta}-\theta)\frac{\partial}{\partial \theta}p(x|\theta)dx=1$$

> 这里有一个小技巧，在后面的推导中会频繁用到：$\frac{\partial}{\partial \theta}p(x|\theta)=(\frac{\partial}{\partial \theta}logp(x|\theta))p(x|\theta)$
- 接前面的推导：$$  \Rightarrow\int_{R^n}(\hat{\theta}-\theta)(\frac{\partial}{\partial \theta}logp(x|\theta))p(x|\theta)dx=1 \\ \Rightarrow E[(\hat{\theta}-\theta)\frac{\partial}{\partial \theta}logp(x|\theta)]=1$$
>这里是最重要的一步，利用Cauchy-Schwarz inequality：$(E[XY])^2\leq E[X^2]E[Y^2]$
- 我们得到：$$1\leq E[(\hat{\theta}-\theta)^2]E[(\frac{\partial}{\partial \theta}logp(x|\theta))^2] \\ \Rightarrow E[(\hat{\theta}-\theta)^2] \geq \frac{1}{E[(\frac{\partial}{\partial \theta}logp(x|\theta))^2]}$$
> 这里的$E[(\frac{\partial}{\partial \theta}logp(x|\theta))^2]$被称为Fisher information(费雪信息)，通过前面的推导，我们得到了这样的一个关系，即我们得到的估计量的均方误差不会小于Fisher information的倒数，即我们得到了MSE的下界。这个下界称为Cramer-Rao Lower Bound(CRLB)。他描述了无偏估计量能够取得的最小的误差，无论采用何种估计方法，都无法将误差降到比这个下界更低的值。
- 前面推导过程中得到的Fisher information是关于$logp(x|\theta)$的一阶偏导的形式。但是在实际计算的时候，一阶偏导的形式可能会比较复杂(参考前面的简单例子)，因此能不能将Fisher information进一步写成关于$logp(x|\theta)$的二阶偏导形式？
- 我们有：$$\int_{R^n}p(x|\theta)dx=1$$
- 求对等式两边求$\theta$的一阶偏导，我们得到：$$\frac{\partial}{\partial \theta}\int_{R^n}p(x|\theta)dx=0 \\ \Rightarrow \int_{R^n}(\frac{\partial}{\partial \theta}logp(x|\theta))p(x|\theta)dx=0$$
- 进一步求二阶偏导，我们有：$$\frac{\partial}{\partial \theta}\int_{R^n}(\frac{\partial}{\partial \theta}logp(x|\theta))p(x|\theta)dx=0$$
- 利用函数乘积求导的方法，我们得到：$$\int_{R^n}(\frac{\partial^2}{\partial \theta^2}logp(x|\theta))p(x|\theta)dx + \int_{R^n}(\frac{\partial}{\partial \theta}logp(x|\theta))^2 p(x|\theta)dx=0$$
- 最终我们得到了Fisher information关于概率模型二阶偏导的形式如下：$$E[(\frac{\partial}{\partial \theta}logp(x|\theta))^2]=-E[\frac{\partial^2}{\partial \theta^2}logp(x|\theta))]$$
- 于是CRLB可以写成：$$E[(\hat{\theta}-\theta)^2] \geq \frac{1}{-E[\frac{\partial^2}{\partial \theta^2}logp(x|\theta))]}$$

# CRLB for multiple parameters (vector)
> 前面我们考虑仅有一个参数需要估计的情况，下面将其推广至具有多个待估计参数的情形(矢量形式)
- 同样的，假定我们有一组n个样本的数据$x=(x_1,x_2,\ldots,x_n)^T$，我们希望通过这组数据对$k$个未知参数$\theta = (\theta_1, \theta_2,\ldots,\theta_k)^T$进行估计，得到的估计为$\hat{\theta}=(\hat{\theta}_1(x), \hat{\theta}_2(x), \ldots,\hat{\theta}_k(x))^T$ 对于每个位置参数，我们同样考虑为无偏估计，即$E[\hat{\theta_i}] = \theta_i$。
- 对于$k$个估计值中的任何一个，其必须满足自身的CRLB，即$$E[(\hat{\theta}_k-\theta)^2] \geq \frac{1}{-E[\frac{\partial^2}{\partial \theta_k^2}logp(x|\theta_k))]}$$
> 但是仅仅对于单个参数考虑是不够的，我们还需要考虑各个参数之间的相互影响。
- 我们定义相关矩阵(协方差矩阵)如下：$$C_\theta=E[(\hat{\theta}-\theta)(\hat{\theta}-\theta)^T]$$ 该矩阵一定是一个正定矩阵，即$C_\theta\geq0$。
> 关于正定矩阵的定义如下：$$A\in R^{n\times n}, A\geq 0 \iff \forall\alpha\in R^n,\alpha^TA\alpha\geq 0$$
> 证明$C_\theta$是正定的：$$\begin{aligned} \forall \alpha\in R^k, \alpha^TC_\theta\alpha&=\alpha^TE[(\hat{\theta}-\theta)(\hat{\theta}-\theta)^T]\alpha \\ &= 
E[\alpha^T(\hat{\theta}-\theta)(\hat{\theta}-\theta)^T\alpha] \\ &=E[(\alpha^T(\hat{\theta}-\theta))^2] \\&\geq 0\end{aligned}$$
- 我们定义概率模型取对数之后关于$\theta$的梯度向量为：$\nabla_\theta logp(x|\theta)$，其中$\nabla_\theta=(\frac{\partial}{\partial \theta_1}, \frac{\partial}{\partial \theta_k}, \ldots, \frac{\partial}{\partial \theta_k})^T$
- 我们定义Fisher Information Matrix(FIM)如下：$$FIM=E[(\nabla_\theta logp(x|\theta))(\nabla_\theta logp(x|\theta))^T]$$ 同样的，FIM也是一个正定矩阵。
> 从前面单个参数的情况下我们得到了误差方差的下界是Fisher information的倒数。而在多个参数的情况下，误差方差变成了协方差矩阵$C_\theta$，而Fisher information也变成了FIM。因此参照单个参数的结论，我们很容易能够照猫画虎的得到多个参数下的结论：$C_\theta \geq FIM^{-1}$，这是一个很自然的推广，下面将对其进行相应的证明。
- 我们想要证明$C_\theta \geq FIM^{-1}$，也就是希望证明$C_\theta - FIM\geq0$，即证明矩阵$C_\theta-FIM$是一个正定矩阵。
- 我们首先定义一个新的向量：$$\begin{aligned}Y&=(\hat{\theta_1}-\theta_1, \hat{\theta_2}-\theta_2, \ldots,\hat{\theta_k}-\theta_k, \frac{\partial}{\partial \theta_1}logp(x|\theta), \frac{\partial}{\partial \theta_2}logp(x|\theta), \ldots, \frac{\partial}{\partial \theta_k}logp(x|\theta))^T \\ &=(\hat{\theta}-\theta, \nabla_\theta logp(x|\theta))^T \end{aligned}$$
- 计算该向量的相关矩阵：$$\begin{aligned} E[YY^T]&=E[\bigl(
\begin{matrix} \hat{\theta}-\theta \\ \nabla_\theta logp(x|\theta)\end{matrix}\bigr) ((\hat{\theta}-\theta)^T, (\nabla_\theta logp(x|\theta))^T)] \\ &= \bigl(\begin{matrix} E[(\hat{\theta}-\theta)(\hat{\theta}-\theta)^T] & E[(\hat{\theta}-\theta)(\nabla_\theta logp(x|\theta))^T] \\ E[(\nabla_\theta logp(x|\theta))(\hat{\theta}-\theta)^T] & E[\nabla_\theta logp(x|\theta)(\nabla_\theta logp(x|\theta))^T]\end{matrix} \bigr) \\ &=\bigl( \begin{matrix} C_\theta & E[(\hat{\theta}-\theta)(\nabla_\theta logp(x|\theta))^T] \\ E[(\nabla_\theta logp(x|\theta))(\hat{\theta}-\theta)^T] & FIM\end{matrix}\bigr) \end{aligned}$$
同样的，$E[YY^T]$也是一个正定矩阵， 它的主对角线上的元素分别是$k$个参数的相关矩阵以及FIM
- 下面考虑非主对角线上的两个矩阵，以$E[(\hat{\theta}-\theta)(\nabla_\theta logp(x|\theta))^T]$为例，考虑其第$i,j$个元素：$$\begin{aligned}E[(\hat{\theta_i}-\theta_i)(\frac{\partial}{\partial \theta_j}logp(x|\theta))] &=\int_{R^n}(\hat{\theta_i}-\theta_i)(\frac{\partial}{\partial \theta_j}logp(x|\theta))p(x|\theta)dx \\ &= \int_{R^n}(\hat{\theta_i}-\theta_i)(\frac{\partial}{\partial \theta_j}p(x|\theta))dx \\ &= \int_{R^n}\hat{\theta_i}(\frac{\partial}{\partial \theta_j}p(x|\theta))dx -\int_{R^n}\theta_i(\frac{\partial}{\partial \theta_j}p(x|\theta))dx \\ &= \frac{\partial}{\partial \theta_j}\int_{R^n}\hat{\theta_i}p(x|\theta))dx-0\\ &= \frac{\partial}{\partial \theta_j}E[\hat{\theta_i}]\\&=\frac{\partial}{\partial \theta_j}\theta_i\\&=\left\{ \begin{matrix}
 1 \qquad if \enspace i=j \\
 0 \qquad if \enspace i\neq j
\end{matrix}\right. \end{aligned}$$
> $$\begin{aligned}\int_{R^n}\theta_i(\frac{\partial}{\partial \theta_j}p(x|\theta))dx &= \theta_i\frac{\partial}{\partial \theta_j} \int_{R^n}p(x|\theta))dx \\&=\theta_i\frac{\partial}{\partial \theta_j}1 \\&=0 \end{aligned}$$
- 通过上述推导，我们可以得到结论：$E[(\hat{\theta}-\theta)(\nabla_\theta logp(x|\theta))^T]$是一个单位阵，而$E[(\nabla_\theta logp(x|\theta))(\hat{\theta}-\theta)^T]$是其转置，故也是一个单位阵。
- 因此$E[YY^T]$变成了如下形式：$$E[YY^T]=\bigl( \begin{matrix} C_\theta & I \\ I& FIM\end{matrix}\bigr)$$
- 通过Schur-Complement，我们可以得到：$$\bigl( \begin{matrix} I & -FIM^{-1} \\ 0& I\end{matrix}\bigr) \bigl( \begin{matrix} C_\theta & I \\ I& FIM\end{matrix}\bigr)\bigl( \begin{matrix} I &  0\\ -FIM^{-1}& I\end{matrix}\bigr) = \bigl( \begin{matrix} C_\theta-FIM^{-1} & 0 \\ 0& FIM\end{matrix}\bigr) \geq0$$
>这里需要证明当$A\geq 0$时，对任意矩阵$B$, 有$B^TAB \geq 0$：$\forall \alpha, \alpha^TB^TAB\alpha=(B\alpha)^TA(B\alpha)=\beta^TA\beta\geq 0$ 
- 由于该矩阵是一个分块对角阵，其为正定矩阵的充要条件是对角线上的矩阵都是正定的，因此我们得到：$C_\theta-FIM^{-1}\geq 0$ ，证明完毕。