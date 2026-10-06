# When does the localized posterior estimate the local learning coefficient?

*Open problem MAIS-O69 · posed in [MAIS-A6](../agendas/A6/) as [Problem 6.1](../agendas/A6/MAIS-A6.tex#L471) · Status: open; [full solution pending review (issue #10)](https://github.com/Math-for-AI-Safety/MAIS/issues/10).*

*Tags: interpretability · generalization · singular learning theory · developmental interpretability · statistics · probability. Difficulty: ★★.*

*Authored by: Claude Fable 5 directed by Lionel Levine · Audited by: GPT 5.6 Sol.*

Local learning coefficients are estimated in practice by sampling from a posterior pinned near the point of interest — inside networks with billions of parameters. But the theorem underpinning the estimator cuts the wrong way: with any fixed localization strength, the Gaussian pin is just another smooth positive prior, so the estimator converges to the global coefficient, not necessarily the local one. How fast must the pin tighten with the sample size?

The target of the estimate is Watanabe's **local learning coefficient** [W09]: at a parameter $w^\ast $ where the population loss $K$ (the average Kullback–Leibler divergence from the truth to the model) vanishes, $\lambda(w^\ast )$ measures how plentiful the almost-true parameters are nearby — inside a small fixed ball around $w^\ast $, the volume of $\lbrace K \le \varepsilon\rbrace $ scales as $\varepsilon^{\lambda(w^\ast )}$ up to logarithmic factors. The global coefficient is the minimum of $\lambda$ over the whole zero set $W_0$ of $K$. Bayesian learning favors the exact fits of smallest coefficient, so local coefficients are a handle on which internal mechanism a network learns — the reason developmental interpretability estimates them at scale.

The estimator of Lau, Furman, Wang, Murfet, and Wei [[LFWMW23]](../references/LFWMW23.md), idealized here by exact expectations: given empirical loss $L_n(w) = -\frac1n \sum_i \log p(Y_i \mid X_i, w)$, a point $w^\ast $, and a localization strength $\gamma > 0$, the **localized tempered posterior** is proportional to $\exp(-n\beta^\ast  L_n(w) - \frac{\gamma}{2}\Vert w - w^\ast \Vert ^2)$ with $\beta^\ast  = 1/\log n$, and the estimate is $\hat\lambda_n(w^\ast ;\gamma) = \bigl(\mathbb{E}^{\beta^\ast ,\gamma}[\ n L_n(w)\ ] - n L_n(w^\ast )\bigr)/\log n$. The problem poses the simplest model with two components of unequal local complexity, so that global and local answers differ.

**Problem ([MAIS-A6, Problem 6.1](../agendas/A6/MAIS-A6.tex#L471)).** Take $d = 2$, $W = \lbrace w \in \mathbb{R}^2 : \Vert w\Vert  \le 3\rbrace $, no inputs, model $y \sim N(\mu(w), 1)$ with

$$\mu(w) = (w_1 - 1)\ (w_1^2 + w_2^2)^2,$$

and truth $y \sim N(0,1)$, so that $K(w) = \tfrac12 \mu(w)^2$ and $W_0 = \lbrace w_1 = 1\rbrace  \cup \lbrace (0,0)\rbrace $, with local coefficients $\lambda(w) = \tfrac12$ on the segment and $\lambda((0,0)) = \tfrac14$ at the isolated point. Let $w^\ast  = (1,0)$. Determine the sequences $\gamma_n \to \infty$ for which $\hat\lambda_n(w^\ast ; \gamma_n) \to \tfrac12$ in probability, and identify the limit at the transitional scaling.

The two sides of the dichotomy are known. Watanabe's WBIC theorem [W13] says that with a fixed smooth positive prior in place of the Gaussian pin (same tempering $\beta^\ast = 1/\log n$), the estimator converges to the smallest local coefficient among the zeros of $K$ in the prior's domain. For fixed $\gamma$, the Gaussian factor is itself such a prior on all of $W$, so the theorem forces $\hat\lambda_n \to \tfrac14$, the global coefficient: the estimator leaks to the more singular point at the origin. For a hard restriction to a small ball around $w^\ast $, the only zeros of $K$ in the domain lie on the segment, and the same theorem gives $\hat\lambda_n \to \tfrac12$. The problem asks for the boundary between the two regimes — the growth rate of $\gamma_n$ at which Gaussian localization starts to localize — and the limit on the boundary itself. Precise definitions and both endpoint arguments are worked out in [MAIS-A6](../agendas/A6/).

## References

- [[LFWMW23]](../references/LFWMW23.md) E. Lau, Z. Furman, G. Wang, D. Murfet, and S. Wei, *The local learning coefficient: a singularity-aware complexity measure*, 2023. [arXiv:2308.12108](https://arxiv.org/abs/2308.12108)
- [[W13]](../references/W13.md) S. Watanabe, *A widely applicable Bayesian information criterion*, Journal of Machine Learning Research 14 (2013), 867–897.
- [[W09]](../references/W09.md) S. Watanabe, *Algebraic Geometry and Statistical Learning Theory*, Cambridge University Press, 2009.

*Related: [MAIS-O70](MAIS-O70.md) (ground-truth local pairs to calibrate the estimator against) · [MAIS-O61](MAIS-O61.md) (running the estimator on small trained networks) · [MAIS-O63](MAIS-O63.md) (the exact values the estimator would be checked against on modular addition).*
