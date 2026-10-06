# The outcome law of one rectifier neuron

*Open problem MAIS-O92 · posed in [MAIS-A5](../agendas/A5/) as [Problem 6.4](../agendas/A5/MAIS-A5.tex#L337) · Status: open.*

*Tags: interpretability · mechanistic interpretability · training dynamics · monosemanticity · dynamical systems · probability · harmonic analysis. Difficulty: ★★★.*

*Authored by: Claude Fable 5 directed by Lionel Levine.*

A single ReLU neuron trained on addition mod $p$ can end in at least three ways: it can die, freezing at a spectrally flat state; it can survive, memorize one entry of the addition table, and diverge with the same flat spectrum; or it can align to a single Fourier frequency, the monosemantic outcome that wide trained networks exhibit neuron by neuron. The first two outcomes occur with positive probability — that is the negative resolution of [MAIS-O60](MAIS-O60.md) by Gautam Neelakantan Memana and GPT-5.6 Sol. What nobody knows is the *law*: with what probabilities does a unit-scale Gaussian initialization produce each outcome? Whether interpretable structure is typical or merely possible is precisely the question this one-neuron model isolates.

The setting is that of [MAIS-O60](MAIS-O60.md): parameters $u_1,v_1,w_1\in\mathbb{R}^{C_p}$, logits $f_\theta(a,b)(c)=\mathrm{ReLU}(u_1(a)+v_1(b))\,w_1(c)$, gradient flow on the full-table cross-entropy with no weight decay, standard Gaussian initialization conditioned on initial activity, Clarke trajectories at the kinks. Say a trajectory **dies** if it reaches a completely inactive state in finite time and is constant thereafter; **aligns** if $\theta(t)/\lVert\theta(t)\rVert$ converges to a limit that is $(\delta,[\rho_\zeta])$-pure for every $\delta>0$ and some frequency $\zeta$; and **memorizes** if the normalized limit exists but is not pure.

**Problem ([MAIS-A5, Problem 6.4](../agendas/A5/MAIS-A5.tex#L337)).** In the setting of [MAIS-O60](MAIS-O60.md), determine the outcome law: (1) does alignment occur with positive probability? (2) determine, or bound as $p\to\infty$, the conditional probabilities of death, memorization, and alignment; (3) how does the law depend on the initialization scale $\tau$? (4) for an everywhere-positive activation — softplus $\sigma_\beta(x)=\beta^{-1}\log(1+e^{\beta x})$, where no gate can die and the known counterexample mechanisms are unavailable — does the alignment conclusion of [MAIS-O60](MAIS-O60.md) hold almost surely?

Part (1) alone would be substantial progress: the known counterexamples occupy constructed corners of initialization space and say nothing about typical behavior. The controlled-initialization analysis of He, Wang, Chen, and Yang [[HWCY26]](../references/HWCY26.md) is the nearest tool for the alignment basin; the resolution constructions of [MAIS-O60](MAIS-O60.md) delimit the death and memorization basins. For the Clarke conventions and the surrounding selection-law program, see [MAIS-A5](../agendas/A5/).

## References

- [M26] G. N. Memana, *A simple dead-neuron counterexample of MAIS-60*, note posted to [MAIS issue #1](https://github.com/Math-for-AI-Safety/MAIS/issues/1), August 2026.
- [[HWCY26]](../references/HWCY26.md) J. He, L. Wang, S. Chen, and Z. Yang, *On the mechanism and dynamics of modular addition: Fourier features, lottery ticket, and grokking*, preprint, 2026. [arXiv:2602.16849](https://arxiv.org/abs/2602.16849)

*Related: [MAIS-O60](MAIS-O60.md) (the resolved yes/no question this quantifies) · [MAIS-O59](MAIS-O59.md) (two quadratic neurons: alignment plus competition) · [MAIS-O61](MAIS-O61.md) (a pilot measurement of the wide-network law) · [MAIS-O5](MAIS-O5.md) (the headline selection law).*
