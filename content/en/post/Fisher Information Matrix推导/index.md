---
title: Fisher Information Matrix推导
subtitle: 在本篇博文中，我对实高斯矢量参数以及复高斯矢量参数情况下的FIM进行了相应的推导，得到了FIM的形式
summary: 在本篇博文中，我对实高斯矢量参数以及复高斯矢量参数情况下的FIM进行了相应的推导，得到了FIM的形式
date: 2022-06-09
math: true
diagram: true
highlight: true
---

# 前言
在[上一篇文章](https://blog.csdn.net/qq_41562431/article/details/125149116)中，我对Cramer-Rao Lower Bound在单个参数以及矢量参数这两种不同的情况下分别进行了推导。对于矢量参数而言，CRLB表述为：$C_\theta \geq FIM^{-1}$。在本文中，将对FIM分别在实数矢量以及复数矢量下的形式进行推导。

# 实高斯矢量参数的FIM
- 假定我们有一组数据集$X=(x_1,x_2,\ldots,x_n)$，我们希望通过某种估计方法对$k$个参数$\theta=(\theta_1, \theta_2, \ldots,\theta_k)$进行估计。
- 对于数据集中的每一个样本，都是一个实高斯随机变量，那么对于矢量$X$: $X\sim N(\mu(\theta),C(\theta))$，具有概率密度：$$p(X|\theta)=\frac{1}{(2\pi)^{\frac{n}{2}}[det(C(\theta))]^{\frac{1}{2}}}exp(-\frac{1}{2}(X-\mu(\theta))^TC(\theta)^{-1}(X-\mu(\theta)))$$
> 出于书写方便清晰，后面的推导我将$\mu(\theta)$和$C(\theta)$简写为$\mu$以及$C$，请不要忘了他们是参数矢量$\theta$的函数
- 对概率密度取对数后求一阶偏导，我们得到：$$logp(X|\theta)=-\frac{n}{2}log(2\pi)-\frac{1}{2}log(det(C))-\frac{1}{2}(X-\mu)^TC^{-1}(X-\mu) \\ \frac{\partial logp(X|\theta)}{\partial \theta_i}=0-\frac{1}{2}tr\bigl(C^{-1}\frac{\partial C}{\partial \theta_i}\bigr)+\frac{1}{2}\frac{\partial \mu^T}{\partial \theta_i}C^{-1}(X-\mu)-\frac{1}{2}(X-\mu)^T\frac{\partial C^{-1}(X-\mu)}{\partial \theta_i}$$
> 这里我们用到了矩阵的行列式求导，下面对其进行推导：
> - $$\frac{\partial |C|}{\partial \theta}=\sum_{j}\sum_i\frac{\partial |C|}{\partial c_{ij}}\frac{\partial c_{ij}}{\partial \theta}$$
> - 由于$$|C|=\sum_{i=1}^nc_{ij}M_{ij}$$其中$M_{ij}$为代数余子式，故$$\frac{\partial |C|}{\partial c_{ij}}=M_{ij}$$
> - 因此$$\frac{\partial |C|}{\partial \theta_i}=|C|\sum_{j}\sum_i\frac{M_{ij}}{|C|}\frac{\partial c_{ij}}{\partial \theta}$$
> - 由逆矩阵的知识我们知道：$\frac{M_{ij}}{|C|}=C^{-1}_{ji}$，而$\frac{\partial c_{ij}}{\partial \theta}$为$\frac{\partial C}{\partial \theta}$的第$(i,j)$个元素，因此第一个对于$i$的求和相当于是计算矩阵乘法的某一项，即：$$\frac{\partial |C|}{\partial \theta_i}=|C|\sum_j(C^{-1}\frac{\partial C}{\partial \theta})_{jj}$$
> - 对于第二个对$j$求和，可以看成是求该矩阵的迹，于是$$\frac{\partial |C|}{\partial \theta}=|C|tr(C^{-1}\frac{\partial C}{\partial \theta})$$
- 对于第二个等式的最后一项，我们有：$$\frac{\partial C^{-1}(X-\mu)}{\partial \theta_i}=-C^{-1}\frac{\partial C}{\partial \theta_i}C^{-1}(X-\mu)-C^{-1}\frac{\partial \mu}{\partial \theta_i}$$
- 于是我们得到：$$\frac{\partial logp(X|\theta)}{\partial \theta_i}=-\frac{1}{2}tr\bigl(C^{-1}\frac{\partial C}{\partial \theta_i}\bigr)+\frac{1}{2}\frac{\partial \mu^T}{\partial \theta_i}C^{-1}(X-\mu)+\frac{1}{2}(X-\mu)^TC^{-1}\frac{\partial C}{\partial \theta_i}C^{-1}(X-\mu)+\frac{1}{2}(X-\mu)^TC^{-1}\frac{\partial \mu}{\partial \theta_i}$$
> 这里我们用到了逆矩阵的导数，下面对其进行推导：
> - 由于$CC^{-1}=I$，两边同时求导，得到：$$\frac{\partial C}{\partial \theta}C^{-1}+C\frac{\partial C^{-1}}{\partial \theta}=0$$
> - 于是很轻易的我们就得到： $$\frac{\partial C^{-1}}{\partial \theta}=-C^{-1}\frac{\partial C}{\partial \theta}C^{-1}$$
- 注意到：$$\frac{1}{2}(X-\mu)^TC^{-1}\frac{\partial \mu}{\partial \theta_i}=\frac{1}{2}\frac{\partial \mu^T}{\partial \theta_i}C^{-1}(X-\mu)$$
- 于是偏导数可以简化为：$$\frac{\partial logp(X|\theta)}{\partial \theta_i}=-\frac{1}{2}tr\bigl(C^{-1}\frac{\partial C}{\partial \theta_i}\bigr)+\frac{\partial \mu^T}{\partial \theta_i}C^{-1}(X-\mu)+\frac{1}{2}(X-\mu)^TC^{-1}\frac{\partial C}{\partial \theta_i}C^{-1}(X-\mu)$$
- 由Fisher Information Matrix的定义，对于其第$(i,j)$个元素，其表达式为：$$\begin{aligned} F_{ij}&=E[\frac{\partial logp(X|\theta)}{\partial \theta_i}\frac{\partial logp(X|\theta)}{\partial \theta_j}] \\ &=E \left\{\Big [-\frac{1}{2}tr\bigl(C^{-1}\frac{\partial C}{\partial \theta_i}\bigr)+\frac{\partial \mu^T}{\partial \theta_i}C^{-1}(X-\mu)+\frac{1}{2}(X-\mu)^TC^{-1}\frac{\partial C}{\partial \theta_i}C^{-1}(X-\mu)\Big] \cdot \Big [-\frac{1}{2}tr\bigl(C^{-1}\frac{\partial C}{\partial \theta_j}\bigr)+\frac{\partial \mu^T}{\partial \theta_j}C^{-1}(X-\mu)+\frac{1}{2}(X-\mu)^TC^{-1}\frac{\partial C}{\partial \theta_j}C^{-1}(X-\mu)\Big]\Bigg\}\right. \end{aligned}$$
> 这个式子乘出来有9项，是不是感觉很吓人？别怕，It just some notations。让我们来一项一项看看(顺序为乘法左侧的第一项分别与右侧的三项轮流，然后是左侧第二项，以此类推)。为了方便推导，我们记 $y=X-\mu$。
> - 第一项是$E[-\frac{1}{2}tr\bigl(C^{-1}\frac{\partial C}{\partial \theta_i}\bigr) \times -\frac{1}{2}tr\bigl(C^{-1}\frac{\partial C}{\partial \theta_j}\bigr)]$，常数项，没啥好说的
> - 第二项是$E[-\frac{1}{2}tr\bigl(C^{-1}\frac{\partial C}{\partial \theta_i}\bigr) \times \frac{\partial \mu^T}{\partial \theta_j}C^{-1}y]$，由于此项为$y$的一阶矩，故为0
> - 第三项是$E[-\frac{1}{2}tr\bigl(C^{-1}\frac{\partial C}{\partial \theta_i}\bigr)\times\frac{1}{2}y^TC^{-1}\frac{\partial C}{\partial \theta_j}C^{-1}y]$，这一项稍微复杂一些。我们知道$E(Y^TX)=tr(E(XY^T))$，因此该项可以变换为$-\frac{1}{4}tr\bigl(C^{-1}\frac{\partial C}{\partial \theta_i}\bigr)tr(C^{-1}\frac{\partial C}{\partial \theta_j}C^{-1}E[yy^T])=-\frac{1}{4}tr\bigl(C^{-1}\frac{\partial C}{\partial \theta_i}\bigr)tr(C^{-1}\frac{\partial C}{\partial \theta_j})$
> - 第四项与第二项一样为$y$的一阶矩，故为0
> - 第五项为：$$\begin{aligned} E\Big[\frac{\partial \mu^T}{\partial \theta_i}C^{-1}y\frac{\partial \mu^T}{\partial \theta_j}C^{-1}y\Big] &= E\Big[\frac{\partial \mu^T}{\partial \theta_i}C^{-1}y[\frac{\partial \mu^T}{\partial \theta_j}C^{-1}y]^T\Big] \\ &= \frac{\partial \mu^T}{\partial \theta_i}C^{-1}E\Big[yy^T\Big]C^{-1}\frac{\partial \mu}{\partial \theta_j} \\ &= \frac{\partial \mu^T}{\partial \theta_i}C^{-1}\frac{\partial \mu}{\partial \theta_j}  \end{aligned}$$
> - 第六项为$y$的三阶矩，故也为0
> - 第七项与第三项一模一样，为$-\frac{1}{4}tr\bigl(C^{-1}\frac{\partial C}{\partial \theta_i}\bigr)tr(C^{-1}\frac{\partial C}{\partial \theta_j})$
> - 第八项为$y$的三阶矩，故也为0
> - 第九项为$\frac{1}{4}E\Big[y^TC^{-1}\frac{\partial C}{\partial \theta_i}C^{-1}yy^TC^{-1}\frac{\partial C}{\partial \theta_j}C^{-1}y\Big]$。这里需要使用一个引理，这里不加证明的给出：$E[y^TAyy^TBy]=tr(AC)tr(BC)+2\cdot tr(ACBC)$，要求A与B为对称矩阵。通过这个引理，我们将第九项变换为：$\frac{1}{4}\Big[tr(C^{-1}\frac{\partial C}{\partial \theta_i}C^{-1}C)tr(C^{-1}\frac{\partial C}{\partial \theta_j}C^{-1}C)+ tr(C^{-1}\frac{\partial C}{\partial \theta_i}C^{-1}CC^{-1}\frac{\partial C}{\partial \theta_j}C^{-1}C)\Big]=\frac{1}{4}\Big[ tr(C^{-1}\frac{\partial C}{\partial \theta_i})tr(C^{-1}\frac{\partial C}{\partial \theta_j}) + 2\cdot tr(C^{-1}\frac{\partial C}{\partial \theta_i}C^{-1}\frac{\partial C}{\partial \theta_j})\Big]$
- 于是我们最终得到：$$\begin{aligned} F_{ij} &= \frac{1}{4}tr\bigl(C^{-1}\frac{\partial C}{\partial \theta_i}\bigr)tr(C^{-1}\frac{\partial C}{\partial \theta_j})-\frac{1}{4}tr\bigl(C^{-1}\frac{\partial C}{\partial \theta_i}\bigr)tr(C^{-1}\frac{\partial C}{\partial \theta_j})+\frac{\partial \mu^T}{\partial \theta_i}C^{-1}\frac{\partial \mu}{\partial \theta_j}-\frac{1}{4}tr\bigl(C^{-1}\frac{\partial C}{\partial \theta_i}\bigr)tr(C^{-1}\frac{\partial C}{\partial \theta_j})+\frac{1}{4}\Big[ tr(C^{-1}\frac{\partial C}{\partial \theta_i})tr(C^{-1}\frac{\partial C}{\partial \theta_j}) + 2\times tr(C^{-1}\frac{\partial C}{\partial \theta_i}C^{-1}\frac{\partial C}{\partial \theta_j})\Big] \\ &= \frac{\partial \mu^T}{\partial \theta_i}C^{-1}\frac{\partial \mu}{\partial \theta_j} + \frac{1}{2}\cdot tr(C^{-1}\frac{\partial C}{\partial \theta_i}C^{-1}\frac{\partial C}{\partial \theta_j})\end{aligned}$$

# 复高斯矢量参数的FIM
- 假定我们有一组数据集$\widetilde{X}=(\widetilde{x}_1,\widetilde{x}_2,\ldots,\widetilde{x}_n)$，我们希望通过某种估计方法对$k$个参数$\theta=(\theta_1, \theta_2, \ldots,\theta_k)$进行估计。
> 对于这$k$个参数，既可以是复数参数也可以是实数参数。我们都知道，对于复数可以以实部和虚部进行表示，因此一个复数参数可以写成两个实数参数。因此为了不产生混淆，后续所有的$\theta$均为实数参数矢量。
- 对于数据集中的每一个样本，都是一个复高斯随机变量，那么对于矢量$\widetilde{X}$: $\widetilde{X}\sim N(\widetilde{\mu}(\theta),\widetilde{C}(\theta))$，具有概率密度：$$p(\widetilde{X}|\theta)=\frac{1}{\pi^n det\widetilde{C}(\theta)}exp\Big(-(\widetilde{X}-\widetilde{\mu}(\theta))^H\widetilde{C}^{-1}(\theta)(\widetilde{X}-\widetilde{\mu}(\theta))\Big)$$
> 出于书写方便清晰，后面的推导我将$\widetilde{\mu}(\theta)$和$\widetilde{C}(\theta)$简写为$\widetilde{\mu}$以及$\widetilde{C}$，请不要忘了他们是参数矢量$\theta$的函数
- 对概率密度取对数，我们得到：$$logp(\widetilde{X}|\theta)=-nlog(\pi)-log(det(\widetilde{C}))-(\widetilde{X}-\widetilde{\mu})^H\widetilde{C}^{-1}(\widetilde{X}-\widetilde{\mu})$$
- 进一步求一阶偏导，我们得到：$$\frac{\partial logp(\widetilde{X}|\theta)}{\partial \theta_i}=-tr\Big(\widetilde{C}^{-1}\frac{\partial \widetilde{C}}{\partial \theta_i}\Big)+\frac{\partial \widetilde{\mu}^H}{\partial \theta_i}\widetilde{C}^{-1}(\widetilde{X}-\widetilde{\mu})-(\widetilde{X}-\widetilde{\mu})^H\frac{\partial \widetilde{C}^{-1}(\widetilde{X}-\widetilde{\mu})}{\partial \theta_i}$$
- 对于上式的最后一项，我们进一步拆开：$$\frac{\partial \widetilde{C}^{-1}(\widetilde{X}-\widetilde{\mu})}{\partial \theta_i}=-\widetilde{C}^{-1}\frac{\partial \widetilde{C}}{\partial \theta_i}\widetilde{C}^{-1}(\widetilde{X}-\widetilde{\mu})-\widetilde{C}^{-1}\frac{\partial \widetilde{\mu}}{\partial\theta_i}$$
- 最终我们得到如下结果：$$\frac{\partial logp(\widetilde{X}|\theta)}{\partial \theta_i}=-tr\Big(\widetilde{C}^{-1}\frac{\partial \widetilde{C}}{\partial \theta_i}\Big)+\frac{\partial \widetilde{\mu}^H}{\partial \theta_i}\widetilde{C}^{-1}(\widetilde{X}-\widetilde{\mu})+(\widetilde{X}-\widetilde{\mu})^H\widetilde{C}^{-1}\frac{\partial \widetilde{\mu}}{\partial\theta_i}+(\widetilde{X}-\widetilde{\mu})^H\widetilde{C}^{-1}\frac{\partial \widetilde{C}}{\partial \theta_i}\widetilde{C}^{-1}(\widetilde{X}-\widetilde{\mu})$$
>在推导一阶偏导的时候，我们同样用到了矩阵行列式的导数以及矩阵的逆的导数，在实高斯部分已经讲过了，这里就不再赘述。
- 由Fisher Information Matrix的定义，对于其第$(i,j)$个元素，其表达式为：$$\begin{aligned} F_{ij}&=E\Big[\frac{\partial logp(\widetilde{X}|\theta)}{\partial \theta_i}\frac{\partial logp(\widetilde{X}|\theta)}{\partial \theta_j}\Big] \\ &= E\left\{ \Bigg[ -tr\Big(\widetilde{C}^{-1}\frac{\partial \widetilde{C}}{\partial \theta_i}\Big)+\frac{\partial \widetilde{\mu}^H}{\partial \theta_i}\widetilde{C}^{-1}(\widetilde{X}-\widetilde{\mu})+(\widetilde{X}-\widetilde{\mu})^H\widetilde{C}^{-1}\frac{\partial \widetilde{\mu}}{\partial\theta_i}+(\widetilde{X}-\widetilde{\mu})^H\widetilde{C}^{-1}\frac{\partial \widetilde{C}}{\partial \theta_i}\widetilde{C}^{-1}(\widetilde{X}-\widetilde{\mu})\Bigg] \cdot \Bigg[ -tr\Big(\widetilde{C}^{-1}\frac{\partial \widetilde{C}}{\partial \theta_j}\Big)+\frac{\partial \widetilde{\mu}^H}{\partial \theta_j}\widetilde{C}^{-1}(\widetilde{X}-\widetilde{\mu})+(\widetilde{X}-\widetilde{\mu})^H\widetilde{C}^{-1}\frac{\partial \widetilde{\mu}}{\partial\theta_j}+(\widetilde{X}-\widetilde{\mu})^H\widetilde{C}^{-1}\frac{\partial \widetilde{C}}{\partial \theta_j}\widetilde{C}^{-1}(\widetilde{X}-\widetilde{\mu})\Bigg] \Bigg\}\right. \end{aligned}$$ 
> 好家伙，乘出来足足16项！不过在实高斯部分我们已经看到了，是有很多部分是0，可以直接消去的。在这里我们记$\widetilde{y}=\widetilde{X}-\widetilde{\mu}$，这里我们不再对每一项进行推导，仅给出在简化过程中会用到的一些点。
> - 我们知道，关于$\widetilde{y}$的一阶矩和3阶矩均为0。
> - 同样的，我们需要使用$E[y^Hy]=tr(E[yy^H])$
> - 在实高斯情况下第九项中用到的引理在这里还需要再次使用，只不过形式稍有改变：$E[y^HAyy^HBy]=tr(AC)tr(BC)+tr(ACBC)$，同样要求A与B为对称矩阵。
> - 还有一个与实高斯情况下有所不同，那就是这里我们的$\widetilde{C}=E[\widetilde{y}\widetilde{y}^H]$，而$E[\widetilde{y}\widetilde{y}^T]=0$，并以此可以推出$E[\widetilde{y}^*\widetilde{y}^H]=(E[\widetilde{y}\widetilde{y}^T])^*=0$。
- 在这里我不道德的要求各位自己对这16项进行简化，我仅给出简化后的结果：$$F_{ij}=tr\Big(\widetilde{C}^{-1}\frac{\partial \widetilde{C}}{\partial \theta_i}\Big)tr\Big(\widetilde{C}^{-1}\frac{\partial \widetilde{C}}{\partial \theta_i}\Big)-tr\Big(\widetilde{C}^{-1}\frac{\partial \widetilde{C}}{\partial \theta_i}\Big)tr\Big(\widetilde{C}^{-1}\frac{\partial \widetilde{C}}{\partial \theta_i}\Big)\\-tr\Big(\widetilde{C}^{-1}\frac{\partial \widetilde{C}}{\partial \theta_i}\Big)tr\Big(\widetilde{C}^{-1}\frac{\partial \widetilde{C}}{\partial \theta_i}\Big) + \frac{\partial \widetilde{\mu}^H}{\partial \theta_i}\widetilde{C}^{-1}\frac{\partial \widetilde{\mu}}{\partial \theta_j}+\frac{\partial \widetilde{\mu}^H}{\partial \theta_j}\widetilde{C}^{-1}\frac{\partial \widetilde{\mu}}{\partial \theta_i}\\+tr\Big(\widetilde{C}^{-1}\frac{\partial \widetilde{C}}{\partial \theta_i}\Big)tr\Big(\widetilde{C}^{-1}\frac{\partial \widetilde{C}}{\partial \theta_i}\Big) + tr\Big(\widetilde{C}^{-1}\frac{\partial \widetilde{C}}{\partial \theta_i}\widetilde{C}^{-1}\frac{\partial \widetilde{C}}{\partial \theta_j}\Big)$$
- 注意到：$$\frac{\partial \widetilde{\mu}^H}{\partial \theta_i}\widetilde{C}^{-1}\frac{\partial \widetilde{\mu}}{\partial \theta_j} = \Big(\frac{\partial \widetilde{\mu}^H}{\partial \theta_j}\widetilde{C}^{-1}\frac{\partial \widetilde{\mu}}{\partial \theta_i}\Big)^H$$ 即两项互为共轭，因此有：$$\frac{\partial \widetilde{\mu}^H}{\partial \theta_i}\widetilde{C}^{-1}\frac{\partial \widetilde{\mu}}{\partial \theta_j}+\frac{\partial \widetilde{\mu}^H}{\partial \theta_j}\widetilde{C}^{-1}\frac{\partial \widetilde{\mu}}{\partial \theta_i}=2Re\left\{  \frac{\partial \widetilde{\mu}^H}{\partial \theta_i}\widetilde{C}^{-1}\frac{\partial \widetilde{\mu}}{\partial \theta_j}\Bigg\}\right. $$
- 最终我们得到了：$$F_{ij}=tr\Big(\widetilde{C}^{-1}\frac{\partial \widetilde{C}}{\partial \theta_i}\widetilde{C}^{-1}\frac{\partial \widetilde{C}}{\partial \theta_j}\Big)+2Re\left\{  \frac{\partial \widetilde{\mu}^H}{\partial \theta_i}\widetilde{C}^{-1}\frac{\partial \widetilde{\mu}}{\partial \theta_j}\Bigg\}\right. $$

# 参考文献
[1] Kay S M. Fundamentals of statistical signal processing: estimation theory[M]. Prentice-Hall, Inc., 1993.
