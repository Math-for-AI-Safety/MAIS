# Opposing staircases

*Open problem MAIS-O7 · headline problem 7 of the survey [MAIS-P1](../papers/P1/) · canonically formalized in [MAIS-A7](../agendas/A7/) as [Conjecture 3.10](../agendas/A7/MAIS-A7.tex#L367) · Status: open; [full solution pending review (issue #5)](https://github.com/Math-for-AI-Safety/MAIS/issues/5).*

*Tags: generalization · developmental interpretability · singular learning theory · training dynamics · algebraic geometry. Difficulty: ★★.*

*Authored by: Claude Fable 5 directed by Lionel Levine · Audited by: GPT 5.6 Sol.*

Gradient flow on a deep linear network descends a staircase: it lingers at a saddle where only the $k$ strongest modes of the target are fitted, then drops to the next. Watanabe's free-energy asymptotics assign a complexity invariant to each stage — the local learning coefficient $\lambda$ of Lau, Furman, Wang, Murfet, and Wei [[LFWMW23]](../references/LFWMW23.md), which controls generalization on the Bayesian side. The conjecture: as the loss falls down its staircase, $\lambda$ climbs an opposing one, step for step. Proved, it would let singular geometry predict the stages of training in advance — the central hope of developmental interpretability.

The setting: a two-layer linear network trained by gradient flow on the square loss against a rank-$r$ target with distinct singular values, hidden width $H>r$. From the small aligned initialization of Saxe, McClelland, and Ganguli [[SMG14]](../references/SMG14.md), the mode strengths decouple and grow logistically, and the flow visits saddle sets $C_0, C_1,\dots,C_r$ in order (all saddles except the last, which is the optimal set), where $C_k$ consists of the critical points realizing only the $k$ strongest modes; the losses along the chain fall, $L_0>L_1>\dots>L_r$. At a saddle the usual learning coefficient is undefined (a saddle has descent directions), so the agenda uses the **two-sided** exponent: the volume of nearby parameters whose loss lies within $\varepsilon$ of the saddle's, above or below, shrinks like $\varepsilon^{\lambda(w)}$ up to a logarithmic factor. Let $\lambda_k=\inf_{w\in C_k}\lambda(w)$.

**Conjecture ([MAIS-A7, Conjecture 3.10](../agendas/A7/MAIS-A7.tex#L367)).** In this setting, $\lambda_0<\lambda_1<\dots<\lambda_r$ while $L_0>L_1>\dots>L_r$: along the saddle chain that gradient flow visits, the loss falls and the learning coefficient rises, step for step. ($C_k$ is noncompact, so attainment of the infimum is part of the claim.)

In the transformer experiments of Hoogland, Wang, Farrugia-Roberts, Carroll, Wei, and Murfet [[HWFC+25]](https://arxiv.org/abs/2402.02364), the estimated local learning coefficient climbs as the loss falls through most training stages — most, not all — and on the Bayesian side, the exact free-energy asymptotics of Aoyagi and Watanabe [AW05] for the deep linear model give the nonnegative increments the conjecture demands, computed at the rank-$k$ models' own optimal sets (whether those coefficients equal the two-sided saddle values is itself part of the problem). The deep linear network is the one model where both staircases are explicit, so it is where the correspondence should first be proved or refuted. For the exact setup, the two-sided definition, and the time–sample dictionary the conjecture would calibrate, see [MAIS-A7](../agendas/A7/).

## References

- [[SMG14]](../references/SMG14.md) A. M. Saxe, J. L. McClelland, and S. Ganguli, *Exact solutions to the nonlinear dynamics of learning in deep linear neural networks*, ICLR 2014. [arXiv:1312.6120](https://arxiv.org/abs/1312.6120)
- [[AW05]](../references/AW05.md) M. Aoyagi and S. Watanabe, *Stochastic complexities of reduced rank regression in Bayesian estimation*, Neural Networks 18 (2005), 924–933.
- [[LFWMW23]](../references/LFWMW23.md) E. Lau, Z. Furman, G. Wang, D. Murfet, and S. Wei, *The local learning coefficient: a singularity-aware complexity measure*, 2023. [arXiv:2308.12108](https://arxiv.org/abs/2308.12108)
- [HWFC+25] J. Hoogland, G. Wang, M. Farrugia-Roberts, L. Carroll, S. Wei, and D. Murfet, *Loss landscape degeneracy and stagewise development in transformers*, Transactions on Machine Learning Research (2025). [arXiv:2402.02364](https://arxiv.org/abs/2402.02364)

*Related: [MAIS-O77](MAIS-O77.md) (the λ-stratification it presupposes) · [MAIS-O78](MAIS-O78.md) (training time versus sample size) · [MAIS-O63](MAIS-O63.md) (learning coefficients of a real task).*
