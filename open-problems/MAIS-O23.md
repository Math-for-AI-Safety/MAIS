# Do margins imply behavioral identifiability of causal models?

*Open problem MAIS-O23 · posed in [MAIS-A2](../agendas/A2/) as [Question 4.1](../agendas/A2/MAIS-A2.tex#L260) · Status: open; [full solution pending review (issue #6)](https://github.com/Math-for-AI-Safety/MAIS/issues/6).*

*Tags: interpretability · world-model discovery · eliciting latent knowledge · black-box evaluation · algebraic geometry · probability. Difficulty: ★★★.*

*Authored by: Claude Fable 5 directed by Lionel Levine · Audited by: GPT 5.6 Sol.*

If two agents act optimally in every locally tampered version of an environment, masked observations included, must they hold the same causal model of it? Richens and Everitt [[RE24]](../references/RE24.md) answered yes for Lebesgue-almost-every parameter choice, and world-model discovery from behavior rests on this. But the exceptional null set is described only implicitly, as the vanishing locus of polynomials arising in their proof, and an auditor cannot check membership in an unnamed null set.

The setting is a finite causal influence diagram with binary variables: a **skeleton** $\mathsf{s}=(\mathbf{C},\mathbf{O},\mathbf{Z},u)$ lists the binary chance variables $\mathbf{C}$, the subset $\mathbf{O}\subseteq\mathbf{C}$ the agent observes, the utility parents $\mathbf{Z}\subseteq\mathbf{C}$, and a known utility $u$; a **model** $M=(G,\theta)$ is a directed acyclic graph on $\mathbf{C}$ with conditional probability tables. The agent sees the values of a visible subset $\mathbf{O}'\subseteq\mathbf{O}$ (the rest are masked), picks a binary action $d$, and is scored $u(d,z)$ where $z$ is the value of $\mathbf{Z}$. A **local intervention** overrides one variable's sampled value — fixing it, negating it, or leaving it alone — while every other mechanism runs unchanged, and $P_M(c;\sigma)$ is the distribution over configurations $c$ when the pattern of overrides is drawn from a probability mixture $\sigma$.

The **margin class** $\mathcal{M}(\mathsf{s},\lambda)$ imposes six explicit conditions (M1)–(M6) ruling out the obvious degeneracies: table entries in $[\lambda,1-\lambda]$, utility gaps $|g(z)|\ge\lambda$ where $g(z)=u(1,z)-u(0,z)$, both actions competitive given any observation, every edge of strength at least $\lambda$, every variable relevant to the score and at least one hidden, and $u$ sensitive to each utility parent. All behavioral information is carried by the **behavioral transform** $\boldsymbol\Delta_M=\bigl(\Delta_M^{\mathbf{O}'}\bigr)_{\mathbf{O}'\subseteq\mathbf{O}}$, where $\Delta_M^{\mathbf{O}'}(\sigma,w)=\sum_{c\ :\ c_{\mathbf{O}'}=w}P_M(c;\sigma)\ g(c_{\mathbf{Z}})$ for mixtures $\sigma$ of local interventions: a policy is optimal for the task $(\sigma,\mathbf{O}')$ precisely when it plays $1$ where $\Delta_M^{\mathbf{O}'}(\sigma,\cdot)$ is positive and $0$ where it is negative, so optimal behavior is the sign data of the transform.

**Question ([MAIS-A2, Question 4.1](../agendas/A2/MAIS-A2.tex#L260)).** Fix a skeleton $\mathsf{s}$ and $\lambda\in(0,\tfrac12)$. If $M,M'\in\mathcal{M}(\mathsf{s},\lambda)$ satisfy $\boldsymbol\Delta_M=\boldsymbol\Delta_{M'}$, must $M=M'$? Equivalently (by the agenda's Proposition 3.2): can two distinct models in the margin class share a common family of optimal policies for every observation mask?

The agenda deliberately offers no conjecture. A counterexample — two distinct models in a margin class with identical masked behavior everywhere — would be as valuable as a proof, since it would reveal a cancellation mechanism that any finite-sample theory must condition away. Classical causal discovery met this issue once before: *strong faithfulness* replaces a measure-zero unfaithfulness assumption by an explicit margin, and Uhler, Raskutti, Bühlmann, and Yu [[URBY13]](../references/URBY13.md) showed the margin excludes a surprisingly large parameter set — a warning that "almost every" and "every, given margins" can be far apart. For the margin conditions in full, the equivalence of behavior with the transform, and the finite-sample program this question underpins, see [MAIS-A2](../agendas/A2/).

## References

- [[RE24]](../references/RE24.md) J. Richens and T. Everitt, *Robust agents learn causal world models*, ICLR 2024. [arXiv:2402.10877](https://arxiv.org/abs/2402.10877)
- [[URBY13]](../references/URBY13.md) C. Uhler, G. Raskutti, P. Bühlmann, and B. Yu, *Geometry of the faithfulness assumption in causal inference*, Ann. Statist. 41 (2013), 436–463. [arXiv:1207.0547](https://arxiv.org/abs/1207.0547)

*Related: [MAIS-O24](MAIS-O24.md) (explicit polynomial margins that would settle it) · [MAIS-O27](MAIS-O27.md) (its quantitative refinement, the regret floor) · [MAIS-O34](MAIS-O34.md) (the two-variable case, solvable exactly).*
