# Week 02

## Uniform Convergence and Rademacher Complexity

Since we have already seen that the learnability of one reference class $\cal F$ can be induced from whether the value of game $\cal V^{iid}(\cal Y^X,n)$ converges to 0 w.r.t. $n$ (though not vice versa),
 we can further explore other sufficient conditions for learnability by some upper bound on $\cal V^{iid}(\cal Y^X,n)$

### Empirical process and uniform convergence

So here we begin with one of these upper bounds and what we called *uniform convergence*.

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

**Definition (conditional Rademacher complexity):**
We define the *conditional Rademacher complexity* $\cal\hat R^{iid}(H;z_{1:n})$ of function class $\cal H\subset\Bbb R^Z$ on arbitrary input sequence $z_1,\ldots,z_n$ as

$$
\cal\hat R^{iid}(H;z_{1:n}) =
\frac{1}{n}\Bbb E_{\epsilon_{1:n}}\left[\sup_{h\in H}\sum_{t=1}^n\epsilon_t h(z_t)\right]
$$

where $\epsilon_t$ is *Rademacher random variable* that takes value on 1 and -1 with equal probability.

**Definition (Rademacher complexity):**
The (unconditional) Rademacher complexity $\cal R^{iid}(H)$ of $\cal H$ with respect to a distribution $\cal P$ supported on $\cal Z$ is defined as

$$
\cal R^{iid}(H) =
\frac{1}{n}\Bbb E_{z_{1:n},\epsilon_{1:n}}\left[\sup_{h\in H}\sum_{t=1}^n\epsilon_t h(z_t)\right]
$$

>PS: Though not clearly stated as input, Rademacher complexity is still subject to the value of $n$.