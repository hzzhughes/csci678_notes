# Week 02

## Uniform Convergence and Rademacher Complexity

Since we have already seen that the learnability of one reference class $\cal F$ can be induced from whether the value of game $\cal V^{iid}(\cal Y^X,n)$ converges to 0 w.r.t. $n$ (though not vice versa), we can further explore other sufficient conditions for learnability by some upper bound on $\cal V^{iid}(\cal Y^X,n)$

### Empirical process and uniform convergence

So here we begin with one of these upper bounds we call *uniform convergence*.

**Definition (Uniform Convergence):**
We say that reference class $\cal F$ satisfies *uniform convergence* if

$$
\lim \sup_{n\rightarrow\infin}
\sup_{\cal P}
\Bbb E[\sup_{f\in\cal F}(L(f)-\frac{1}{n}\sum_{t=1}^n l(f,z_t))]=0
$$

**Theorem:**
*If uniform convergence holds for $\cal F$ , then $\cal F$ is learnable*

*Proof:*
We start by constructing one concrete and yet intuitive training algorithm which we call *Empirical Risk Minimizer* (ERM). The ERM predictor is defined as follow:
$$
\hat y_{ERM}\in \arg\min_{f\in\cal F}\frac{1}{n}\sum_{t=1}^n l(f,z_t)
$$

>PS: Though sometimes ERM algorithm can even form an NP-hard problem, we'll not talk about how to implement it practically here. This is because we focus more on its statistical learnability (but not its computational one)
>
>PPS: One more discussion on ERM may be why we do not choose other algorithm that may include regularization. The answer is such an algorithm can also be regarded as an ERM.

Recall that $\cal V^{iid}(F,n) = \inf_\pi \sup_P(\Bbb E[\it L(\hat y)]-\inf_{f\in\cal F}L(f))$ , it's easy to prove:

$$
\cal V^{iid}(\cal Y^X,n) \leq \sup_P(\Bbb E[\it L(\hat y)]-\inf_{f\in\cal F}L(f))
$$

since obviously ERM is just one possible training algorithm $\pi$.

Next, we have

$$
\begin{align*}
\cal V^{iid}(\cal Y^X,n)
&\leq \sup_P\Bbb E\left[\it L(\hat y_{ERM})\right]-L(f^\star)\\

&= \sup_P\Bbb E\left[\it L(\hat y_{ERM})-\frac{1}{n}\sum_{t=1}^n l(f^\star,z_t)\right]
&(by\ definition\ of\ L)\\

&\leq \sup_P\Bbb E\left[\it L(\hat y_{ERM})-\frac{1}{n}\sum_{t=1}^n l(y_{ERM},z_t)\right]
&(by\ definition\ of\ \hat y_{ERM})\\

&\leq \sup_P\Bbb E\left[\sup_f\left(\it L(\hat f)-\frac{1}{n}\sum_{t=1}^n l(f,z_t)\right)\right]
\end{align*}
$$

And clearly we can see the term used in the definition of uniform is an upper bound of $\cal V^{iid}(\cal Y^X,n)$.

*Q.E.D.*

### Symmetrization and Rademacher complexity

Here we define Radenacher complexity $\cal R^{iid}(H)$ to measure how representative a function class $\cal H\subset\Bbb R^Z$ can be

**Definition (conditional Rademacher complexity):**
We define the *conditional Rademacher complexity* $\cal\hat R^{iid}(H;z_{1:n})$ of function class $\cal H\subset\Bbb R^Z$ on arbitrary input sequence $z_1,\ldots,z_n$ as

$$
\cal\hat R^{iid}(H;z_{1:n}) =
\frac{1}{n}\Bbb E_{\epsilon_{1:n}}\left[\sup_{h\in H}\sum_{t=1}^n\epsilon_t h(z_t)\right]
$$

where $\epsilon_t$ is *Rademacher random variable* that takes value on 1 and -1 with equal probability.

> PS: the size of input sequence $n$ can still be interpreted as the size of training set in the sense of statistical learning.

**Definition (Rademacher complexity):**
The (unconditional) Rademacher complexity $\cal R^{iid}(H)$ of $\cal H$ with respect to a distribution $\cal P$ supported on $\cal Z$ is defined as
$$
\cal R^{iid}(H) =
\frac{1}{n}\Bbb E_{z_{1:n},\epsilon_{1:n}}\left[\sup_{h\in H}\sum_{t=1}^n\epsilon_t h(z_t)\right]
$$

>PS: Though not clearly stated as input, Rademacher complexity is still subject to the value of $n$.

The intuition is that since more representative classes usually perform better on a given amount of data points, we can just measures how well can a function class perform on $n$ data points with completely random labels.

**Theorem:**
*For any data-generating distribution $\cal P$ and any class $\cal F$ ,we have*
$$
\cal\Bbb E\left[\sup_{f\in F}\left(\it L(\hat f)-\frac{1}{n}\sum_{t=1}^n l(f,z_t)\right)\right]
\leq 2R^{iid}(l(F))
$$

*where $\cal l(F)=\{h_f\in\Bbb R^Z:f\in F,h_f(z)=l(f,z),\forall z\}$*

*Proof:* By definition of $L(f)$ , we rewrite it as $\cal\Bbb E_{z_1',\ldots,z_n'\sim P}[\frac{1}{n}\sum_{t=1}^n l(f,z_t)]$ and arrive at
$$
\cal\Bbb E\left[\sup_{f\in F}\left(\it L(\hat f)-\frac{1}{n}\sum_{t=1}^n l(f,z_t)\right)\right]
=\frac{1}{n}\cal\Bbb E_{z_{1:n}}\left[\sup_{f\in F}\Bbb E_{z_{1:n}'}\left[\sum_{t=1}^n (l(f,z_t')-l(f,z_t))\right]\right]
$$
Further, by change the order of expectation and supreme, we have the upper bound (which is symmetric)
$$
\frac{1}{n}\cal\Bbb E_{z_{1:n}}\left[\sup_{f\in F}\Bbb E_{z_{1:n}'}\left[\sum_{t=1}^n (l(f,z_t')-l(f,z_t))\right]\right]
\leq
\frac{1}{n}\cal\Bbb E_{z_{1:n},z_{1:n}'}\left[\sup_{f\in F}\sum_{t=1}^n (l(f,z_t')-l(f,z_t))\right]
$$
Then, by property of symmetricity, we have:
$$
\frac{1}{n}\cal\Bbb E_{z_{1:n},z_{1:n}'}\left[\sup_{f\in F}\sum_{t=1}^n (l(f,z_t')-l(f,z_t))\right]
=
\frac{1}{n}\cal\Bbb E_{z_{1:n},z_{1:n}',\epsilon_{1:n}}\left[\sup_{f\in F}\sum_{t=1}^n \epsilon_t(l(f,z_t')-l(f,z_t))\right]
$$

> PS: Though this part is hard to understand, we can use some extra tools to prove this. It's easy to prove that $\epsilon_t(l(f,z_t')-l(f,z_t))=l(f,z_t'')-l(f,z_t''')$ where $z_t''=\Bbb I(\epsilon_t=1)z_t'+\Bbb I(\epsilon_t=-1)z_t$ and $z_t'''=\Bbb I(\epsilon_t=1)z_t+\Bbb I(\epsilon_t=-1)z_t'$ . Clearly, we still have $z_{1:n},z_{1:n}',z_{1:n}'',z_{1:n}'''$ all follows the same distribution while keeping $z_t''$ and $z_t'''$ independent to each other. Then just by replacing $z_{1:n}',z_{1:n}$ in left of the equation above with $z_{1:n}'',z_{1:n}'''$ , we prove the equation.

Next, by splitting the supreme into two parts, we have another upper bound
$$
\begin{align*}
\cal\Bbb E_{z_{1:n},z_{1:n}',\epsilon_{1:n}}\left[\sup_{f\in F}\sum_{t=1}^n \epsilon_t(l(f,z_t')-l(f,z_t))\right]
&\leq
\cal\Bbb E_{z_{1:n},z_{1:n}',\epsilon_{1:n}}\left[\sup_{f\in F}\sum_{t=1}^n \epsilon_t l(f,z_t')+\sup_{f\in F}\sum_{t=1}^n -\epsilon_t l(f,z_t)\right]\\
&=
\cal\Bbb E_{z_{1:n},z_{1:n}',\epsilon_{1:n}}\left[\sup_{f\in F}\sum_{t=1}^n \epsilon_t l(f,z_t')\right]+\Bbb E_{z_{1:n},z_{1:n}',\epsilon_{1:n}}\left[\sup_{f\in F}\sum_{t=1}^n -\epsilon_t l(f,z_t)\right]\\
&=\cal 2nR^{iid}(l(F))
\end{align*}
$$
*Q.E.D.*

### Erasing the loss for supervised learning

Note that we used term $\cal R^{iid}(l(F))$ instead of $\cal R^{iid}(F)$ in the upper bound we just obtained, here we'll prove they are not that different at least for supervised learning problems where $\cal Z=X\times Y$ and $\cal F \sub Y^X$ .

**Lemma:** *For a binary classification problem with $\cal Y=\{-1,+1\}$ and 0-1 loss, one has $\cal \hat R^{iid}(l(F);z_{1:n})=\frac{1}{2}\cal \hat R^{iid}(F;z_{1:n})$ for any sequence $z_{1:n}$ , and thus $\cal R^{iid}(l(F))=\frac{1}{2}\cal R^{iid}(F)$ .*

*Proof.*

*Q.E.D.*

**Lemma (Contraction lemma):** *For a regression problem with $\cal Y \sub \Bbb R$ and loss $l(f,(x,y))=l'(f(x),y)$ for some loss $l'(f(x),y)$ that is G-Lipschitz in the first parameter (which means $\lvert l'(y_1,y)-l'(y_2,y)\rvert \leq G\lvert y_1-y_2\rvert$ holds for all $y_1,y_2,y$ )*

*Proof.*

*Q.E.D.*

## Finite Class

**Definition (σ-sub-Gaussion):** 

## Infinite Class: Classification
