# Does a single ReLU neuron align to one frequency?

*Open problem MAIS-O60 · posed in [MAIS-A5](../agendas/A5/) as [Problem 6.2](../agendas/A5/MAIS-A5.tex#L320) · Status: **[resolved in the negative](O60/MAIS-O60-resolution.md)** by Gautam Neelakantan Memana (August 2026).*

*Tags: interpretability · mechanistic interpretability · training dynamics · monosemanticity · dynamical systems · probability · harmonic analysis. Difficulty: ★★.*

*Authored by: Claude Fable 5 directed by Lionel Levine · Audited by: GPT 5.6 Sol · Resolved by: Gautam Neelakantan Memana, with a strengthening by GPT 5.6 Sol.*

Strip the network down to a single rectifier neuron on addition mod $p$. Trained wide networks on this task end up with neurons each committed to one Fourier frequency, as Nanda, Chan, Lieberum, Smith, and Steinhardt observed [[NCLSS23]](../references/NCLSS23.md); here the question is isolated in one neuron: started from a unit-scale Gaussian, does its direction converge, and is the limit a single frequency?

The parameters are three functions $u_1, v_1, w_1 \in \mathbb{R}^{C_p}$ — a single neuron, width $m = 1$ in the agenda's notation — and the logits are $f_\theta(a,b)(c) = \mathrm{ReLU}(u_1(a)+v_1(b))\ w_1(c)$. Training is gradient flow on the cross-entropy loss over the full addition table, with no weight decay (coefficient $\lambda = 0$), from independent standard Gaussian coordinates (initialization scale $\tau = 1$); since the rectifier is only piecewise smooth, the flow is a differential inclusion and the statement is asserted for every measurable selection of Clarke trajectories. The frequency classes of $C_p$ are $[\rho_\zeta] = \lbrace \rho_\zeta, \rho_{-\zeta}\rbrace $ where $\rho_\zeta(a) = e^{2\pi i \zeta a/p}$, and the triple $(u_1,v_1,w_1)$ is **$(\delta,[\rho_\zeta])$-pure** if, after centering, at least a $(1-\delta)$ fraction of its combined squared norm lies in the isotypic component of $[\rho_\zeta]$ — that is, along the functions $\cos(2\pi\zeta \cdot/p)$ and $\sin(2\pi\zeta \cdot/p)$.

**Problem ([MAIS-A5, Problem 6.2](../agendas/A5/MAIS-A5.tex#L320)).** Let $G = C_p$, $\sigma = \mathrm{ReLU}$, $m = 1$, $\lambda = 0$, $\tau = 1$, and condition on the event that the neuron is active at initialization — $u_1(a) + v_1(b) > 0$ for some $(a,b)$. (On the complementary event the gradient vanishes almost surely; equality at a ReLU kink is a Gaussian null event.) Prove or refute: almost surely on the active event, the normalized weights $(u_1, v_1, w_1)/\Vert \cdot\Vert $ converge, and the limit is $(\delta, [\rho_\zeta])$-pure for every $\delta > 0$, for some frequency $\zeta$.

In words: with a single neuron there is no competition for neurons, only the question of whether the rectifier dynamics themselves concentrate a random initial spectrum onto one frequency. For the Clarke-trajectory convention and the surrounding results, see [MAIS-A5](../agendas/A5/).

## Resolution

**The answer is no**: the almost-sure claim fails, for every prime $p \ge 5$, on explicit positive-probability events. Resolved by Gautam Neelakantan Memana ([issue #1](https://github.com/Math-for-AI-Safety/MAIS/issues/1), August 2026), with a strengthening by GPT-5.6 Sol: an open set of initializations from which the sole active gate dies in finite time and the weights freeze at a spectrally flat state, and a second open set — on which the Clarke trajectory is unique — where the neuron survives, memorizes a single table entry, and diverges in norm with the same flat limiting spectrum. Links to the solution documents are on the [resolution page](O60/MAIS-O60-resolution.md).

**What survives.** Both counterexamples occupy constructed corners of initialization space; the typical behavior from unit-scale Gaussian initialization — and whether alignment occurs with any positive probability at all — is untouched, and is now posed as [MAIS-O92](MAIS-O92.md). The nearest positive result remains the controlled-initialization leakage estimate of He, Wang, Chen, and Yang [[HWCY26]](../references/HWCY26.md).

## References

- [[HWCY26]](../references/HWCY26.md) J. He, L. Wang, S. Chen, and Z. Yang, *On the mechanism and dynamics of modular addition: Fourier features, lottery ticket, and grokking*, preprint, 2026. [arXiv:2602.16849](https://arxiv.org/abs/2602.16849)
- [[NCLSS23]](../references/NCLSS23.md) N. Nanda, L. Chan, T. Lieberum, J. Smith, and J. Steinhardt, *Progress measures for grokking via mechanistic interpretability*, ICLR, 2023. [arXiv:2301.05217](https://arxiv.org/abs/2301.05217)

*Related: [MAIS-O92](MAIS-O92.md) (the successor: the outcome law this resolution leaves open) · [MAIS-O59](MAIS-O59.md) (two quadratic neurons: alignment plus competition) · [MAIS-O53](MAIS-O53.md) (multiplicities when many neurons compete) · [MAIS-O5](MAIS-O5.md) (the headline selection law).*
