# Resolution of MAIS-O60

*Resolution page for [MAIS-O60](../MAIS-O60.md): Does a single ReLU neuron align to one frequency? · Status: **resolved in the negative** (August 2026).*

*Resolved by: [Gautam Neelakantan Memana](https://people.math.wisc.edu/~neelakantanm/), with a strengthening by GPT 5.6 Sol · Checked by: Claude Fable 5.*

**The dead-neuron counterexample (Memana).** Take $u(0)=v(0)=\varepsilon/2$ and $u(a)=v(a)=-B$ for $a\neq 0$, with $B>\varepsilon/2$; take $w(0)=-A$ with $A^2 > \tfrac{p}{2(p-1)}\varepsilon^2$ and $w=0$ elsewhere. Exactly one gate is active, with the negative weight $-A$ on its example's correct answer, and the flow reduces to two variables: the active preactivation $s$ and the logit gap $g$, which obey the conservation law $g^2-\tfrac{p}{2(p-1)}s^2 = \mathrm{const} > 0$. The gap stays bounded below, so $\dot s$ is bounded away from zero and $s$ hits the kink transversely in finite time; choosing the zero Clarke slope there freezes the weights forever. The frozen centered triple is a multiple of the point mass $e_0-\tfrac1p\mathbf 1$, whose Fourier energy is exactly $\tfrac{2}{p-1}$ in *every* nonzero frequency class — flat, so not $(\delta,[\rho_\zeta])$-pure for any $\delta < 1-\tfrac{2}{p-1}$. The trajectory to the kink lives in an open chamber where the flow is smooth and the hit is transverse, so an open neighborhood of initializations does the same, and open sets have positive Gaussian probability.

**The strengthening (Sol).** The failure is not an artifact of the dead-neuron loophole or of the freedom to choose Clarke trajectories. There is an open set of active initializations on which the Clarke trajectory is *unique* (via local semiconvexity of the loss at positive-slope kinks), stays active forever, captures the cross-gates onto sliding kink faces one by one, memorizes a single table entry with $\lVert\theta\rVert\to\infty$ — and its normalized limit has the same flat $\tfrac{2}{p-1}$ spectrum. Companion theorems give an open set where *every* Clarke trajectory dies in finite time, and carry the failure to $C^\infty$ dead-zone smoothings of the ReLU and to fixed-step full-batch gradient descent. The result does not extend to everywhere-positive activations such as softplus, where both mechanisms are unavailable.

**Full details** are published as [MAIS-P3](../../papers/P3/), available on [arXiv:2608.04451](https://arxiv.org/abs/2608.04451). The proofs were independently verified by Claude Fable 5, and the constructions reproduce numerically.

**Successor problem** [MAIS-O92: The outcome law of one rectifier neuron](../MAIS-O92.md) asks whether alignment is *typical*, and with what probability each outcome occurs from a Gaussian initialization.

## Documents

The published solution is Gautam Neelakantan Memana, *A Counterexample to Fourier Alignment in Single-Neuron Modular Addition*, [MAIS-P3](../../papers/P3/), August 2026 — [arXiv:2608.04451](https://arxiv.org/abs/2608.04451).

The component manuscripts remain attached to [issue #1](https://github.com/Math-for-AI-Safety/MAIS/issues/1), where the discussion lives:

- Gautam Neelakantan Memana, *A simple dead-neuron counterexample of MAIS-60*, August 1, 2026 — [PDF](https://github.com/user-attachments/files/30615776/MAIS_60-2.pdf).
- GPT-5.6 Sol, *Strengthened counterexamples to single-frequency alignment in MAIS-O60*, written in conversation with G. N. Memana, August 1, 2026 — [PDF](https://github.com/user-attachments/files/30615797/MAIS_O60_strengthened_counterexamples.pdf).

*Related: [MAIS-O60](../MAIS-O60.md) (the problem page) · [MAIS-P3](../../papers/P3/) (the published solution) · [MAIS-O92](../MAIS-O92.md) (the successor: the outcome law this resolution leaves open).*
