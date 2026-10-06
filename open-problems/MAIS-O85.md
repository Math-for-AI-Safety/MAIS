# Which feature maps make the max-margin classifier misgeneralize?

*Open problem MAIS-O85 · posed in [MAIS-A8](../agendas/A8/) as [Problem 5.9](../agendas/A8/MAIS-A8.tex#L388) · Status: open; [full solution pending review (issue #13)](https://github.com/Math-for-AI-Safety/MAIS/issues/13).*

*Tags: generalization · goal misgeneralization · proxy goals · simplicity bias · convex geometry · optimization. Difficulty: ★★.*

*Authored by: Claude Fable 5 directed by Lionel Levine · Audited by: GPT 5.6 Sol.*

On the coin line at zero training diversity, the encoding decides: linear gradient descent on the features $(1,p,c)$ provably learns "move right," while on $(1,c-p)$ it provably learns to approach the coin. One-hot features (indicator vectors of the two positions, concatenated) and every monomial encoding $\psi_k(p,c)=(p^ic^j)_{i+j\le k}$ side with the proxy. What property of a feature map is responsible?

The setup, from the agenda: an agent at $p$ and a coin at $c$ on $\lbrace 0,\dots,L\rbrace $; at diversity $\varepsilon=0$ the training states are $(p,L)$ for $0\le p\le L-1$, all labeled $+1$ (step right), so "move right" and "go to the coin" fit equally well. By the max-margin theorem of Soudry, Hoffer, Nacson, Gunasekar, and Srebro [[SHNGS18]](../references/SHNGS18.md), gradient descent on the logistic loss over features $\psi(p,c)$ converges in direction to the maximum-margin separator, the least-norm linear functional with margin at least one on every training feature. The selected extrapolation is therefore read off from one number: the sign of the max-margin score at the probe state $s^{\ast }=(\lceil L/2\rceil,0)$, where the coin lies at the left end and the two policies disagree. Positive means the proxy; negative means the goal. An encoding is **separable** if some linear functional is positive on all its training features.

**Problem ([MAIS-A8, Problem 5.9](../agendas/A8/MAIS-A8.tex#L388)).** Give a structural criterion on a feature map $\psi:\lbrace 0,\dots,L\rbrace ^2\to\mathbb R^D$ deciding whether the maximum-margin direction at $\varepsilon=0$ is positive or negative at the probe. Restrict to separable encodings. If the maximum-margin direction vanishes at the probe, classify the result as undetermined at leading order and pass to the residual of [MAIS-O89](MAIS-O89.md).

The residual in the last clause is the bounded remainder of the gradient-descent iterate once its divergent max-margin component is removed; at a state where the max-margin score is zero, that remainder decides the sign, and [MAIS-O89](MAIS-O89.md) asks for its limit.

The known cases are data for the criterion. For monomial encodings the agenda gives a three-line Karush–Kuhn–Tucker argument: every training feature $\psi_k(p,L)$ has positive inner product with the probe feature, so the proxy wins for all $k$ and $L$. The relative encoding $(1,c-p)$ hard-wires the comparison the task turns on, and the goal wins. The closest prior result: Nagarajan, Andreassen, and Neyshabur [[NAN21]](https://arxiv.org/abs/2010.15775) prove that geometric skews of the training data can make a max-margin linear classifier rely on fully dispensable spurious coordinates. A criterion separating the coin-line encodings — some geometric condition on how $\psi$ couples training states to unseen ones — would say in advance which representations of state misgeneralize, before any training is run. For the worked examples and the KKT certificates, see [MAIS-A8](../agendas/A8/).

## References

- [[SHNGS18]](../references/SHNGS18.md) D. Soudry, E. Hoffer, M. S. Nacson, S. Gunasekar, and N. Srebro, *The implicit bias of gradient descent on separable data*, Journal of Machine Learning Research 19 (2018), no. 70, 1–57. [arXiv:1710.10345](https://arxiv.org/abs/1710.10345)
- [NAN21] V. Nagarajan, A. Andreassen, and B. Neyshabur, *Understanding the failure modes of out-of-distribution generalization*, International Conference on Learning Representations, 2021. [arXiv:2010.15775](https://arxiv.org/abs/2010.15775)

*Related: [MAIS-O89](MAIS-O89.md) (the residual this problem defers to at a vanishing probe score) · [MAIS-O8](MAIS-O8.md) (the network version of the same selection question) · [MAIS-O82](MAIS-O82.md) (the width-$\infty$ proxy-selection conjecture).*
