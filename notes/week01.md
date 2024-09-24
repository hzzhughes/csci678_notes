# Week 01

## Basics

>Some symbols and definitions

设有样本空间 $\cal{Z}$ ，iid分布 $\cal{P}$ 及训练集 $z_{1:n}:=\{z_1,\ldots,z_n\mid z_i\in\cal{Z}, z_i\sim\cal{P},iid\}$

则学习器（learner）可依此得到预测器（predictor）$\hat{y}\in \cal{D}$并称其所在的空间 $\cal{D}$ 为决策空间（decision space）

>PS: 为什么学习器的输出要是确定性的函数而不可以是预测目标y基于特征x的条件概率分布？
>
>追注：好像还真是可以的，也许这就是将训练集抽象为 $\cal Z$ 而不再是$
\cal X$ 与 $\cal Y$ 的道理

进而有损失函数 $l:\cal{D}\times\cal{Z}\mapsto\Bbb{R}$

及泛化损失（generalized loss） $L(\hat y):=\Bbb E_{z\sim\cal{P}}[l(\hat y,z)]$

同时，我们不再对样本分布 $\cal{P}$ 作出假设，而是将我们的学习目标定为拟合一族特定的类 $\cal{F}\subseteq\cal{D}$ ，并定义当 $\cal{F}=\cal{D}$ 时，称学习器是*proper*的，反之则是*improper*的

且另通过*excess risk*: $\Bbb E[L(\hat{y})]-\inf_{f\in\cal{F}}L(f)$ 来衡量我们的学习器 $\hat{y}$ 与reference space中最佳学习器的差异
>PS: Prior knowledge is therefore no longer incorporated in the distribution but in $\cal{F}$. And this approach is so called *agnostic* or *distribution free*
>
>另注：我觉得这一步不是很好理解。为什么要建立reference space？
>
>试解：也许用例子尝试理解。如在线性回归时应有 ${\cal F}=\{f\mid f(x)=\theta^T x\}\subset\cal Y^X$ ，而在生存分析时则有 ${\cal F}=\{f\mid f\ is\ non-decreasing \}\subset\cal Y^X$ ，对于神经网络则有 ${\cal F}=\{f\mid f(x)=z_1\circ\ldots\circ z_n (x),z_i=g_i(w_i^T·+b_i) for\ i=1,\ldots, n\}\subset\cal Y^X$ 。
>但是这么一想模型的失误（例如给非线性的数据关系分配线性模型的reference class）就被包括在了 $\inf_{f\in\cal{F}}L(f)$ 中，而不是excess risk。那么excess risk所包含的就是算法的不足（无法达到最优拟合）。
>
>追注：实际上，excess risk的另一个名称就是泛化误差（generalization  error），而模型reference类${\cal F}$内生的误差则被称作表现误差（representation error）。除此两者，还有来自将模型转化为实际算法时产生的优化误差（optimization error）
>
>乱语：但是如果 $\cal{F}$ 不在空间 $\cal{D}$ 内又如何呢？可能的学习器$f$就不能是不在 $\cal{Z}$ 上定义的函数？计算的误差不仅可来自对于 $\cal{P}$ 的无知，也可能来自数据 $\cal Z$ 的选取不当。

**定义 (learnable)** 若存在某种算法可使学习器得到的预测器能以任意精度接近reference space $\cal F$中的最佳预测器, 则称 $\cal F$ 是*可学习的（learnable）*

>PS: This is still a very abstract definition, but we'll soon see a more rigorous one(PAC learnable) in following text

## PAC Setting

在PAC中，我们假设学习器进行受监督二分类任务（supervised binary classification problem），并存在 $f^\star\in\cal{F}$ 满足 $\cal{P}(y=f^\star(x)\mid x)=1$ （即存完美预测器）

易见，此时 $\inf_{f\in\cal{F}}L(f)=0$

故而excess risk即 $\Bbb E[L(\hat{y})]$

此时我们有：

**定义 （PAC learnable）** 对任意给定的误差 $\epsilon$ 及置信水平 $\delta$ ，若存在某种算法可使学习器在给予量达 $poly(1/\epsilon,1/\delta)$ 的训练样本输入后，输出的预测器 $\hat y$ 可满足 ${\cal P} (L(\hat y)\leq\epsilon)\geq 1-\delta$ ，则称 $\cal F$ 是 PAC可学习的（PAC-learnable）

## The Value of Game and No Free Lunchh Theorem

此处引入博弈论中的斯塔克伯格先行一步模型，定义 $\cal V^{iid}(F,n)$ 为先选择训练策略 $\pi:\cal Z^n \mapsto D$ 后，再由环境选择最劣数据概率分布 $\cal P$ 时关于excess risk的博弈结果：

$$
\cal V^{iid}(F,n) = \inf_\pi \sup_P(\Bbb E[\it L(\hat y)]-\inf_{f\in\cal F}L(f))
$$

此时，
我们将用于定义类 $\cal F$ 可学习的标准定为:

$$
\lim \sup_{n\rightarrow\infin}\cal V^{iid}(F,n)=0
$$

**定理 （No Free Lunch）**
*设对某二分类问题有
$\cal |X| \geq 2n$，
$\cal Y=\{0,1\}$，
及
$l(\hat y, (x,y))=\Bbb I\{\hat y(x) \neq y\}$。
则有
$\cal V^{iid}(\cal Y^X,n)\geq 1/4$
，
亦即，类 $\cal Y^X$ 不是可学习的。*

*证：*

## A Harder Setup: Online Learning

## An Even Harder Setup: Online Learning with Partial Information
