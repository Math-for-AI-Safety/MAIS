# Explicit polynomial margins replacing generic causal identifiability

*Open problem MAIS-O24 · posed in [MAIS-A2](../agendas/A2/) as [Problem 4.2](../agendas/A2/MAIS-A2.tex#L266) · Status: open; [full solution pending review (issue #7)](https://github.com/Math-for-AI-Safety/MAIS/issues/7).*

*Tags: interpretability · world-model discovery · eliciting latent knowledge · black-box evaluation · algebraic geometry · probability · complexity theory. Difficulty: ★★.*

*Authored by: Claude Fable 5 directed by Lionel Levine · Audited by: GPT 5.6 Sol.*

The theorem behind behavioral world-model discovery is due to Richens and Everitt [[RE24]](../references/RE24.md): near-optimal adaptation to all local interventions and observation masks determines the agent's causal model. Their theorem holds for almost every parameter choice, with the exceptions described only as the vanishing locus of polynomials arising in the proof. Finite-sample statements need more: an explicit, efficiently checkable hypothesis in place of "almost every." This problem asks for the polynomials by name.

The objects: a skeleton $\mathsf{s}=(\mathbf{C},\mathbf{O},\mathbf{Z},u)$ fixes $m$ binary chance variables, an observation set $\mathbf{O}\subseteq\mathbf{C}$, utility parents $\mathbf{Z}\subseteq\mathbf{C}$, and a known utility $u$; a compatible model $M=(G,\theta)$ is a directed acyclic graph on the chance variables with its conditional probability tables, and $K(G)=\sum_i 2^{|\mathrm{Pa}_G(C_i)|}$ counts the free table entries ($K$ is its maximum over the margin class). The agent sees the observed variables, picks a binary action, and is scored by $u$; a policy is regret-$\delta$ if its expected score is within $\delta$ of optimal; a shifted task pairs a mixture of single-variable interventions with a mask hiding some of the observations.

Write $g(z):=u(1,z)-u(0,z)$ for the gap in score between the two actions. The margin class $\mathcal{M}(\mathsf{s},\lambda)$ imposes six explicit non-degeneracy conditions at scale $\lambda$: in gist, table entries, the gap $g$, and the effect of every edge and utility parent are bounded away from zero, plus structural requirements on which variables are observed; the exact quantifiers are in [MAIS-A2](../agendas/A2/). The behavioral transform $\boldsymbol\Delta_M$ is the finite family of $g$-weighted, observation-sliced interventional probabilities whose sign data is optimal behavior; the identified set $I_\delta(M)$ consists of the models sharing with $M$ a single policy family (one policy per shifted task) that is regret-$\delta$ for both; and the error $e(M;M')$ is $1$ if the graphs differ and otherwise the maximum entrywise table difference.

**Problem ([MAIS-A2, Problem 4.2](../agendas/A2/MAIS-A2.tex#L266)).** For each diagram shape $(\mathbf{C},\mathbf{O},\mathbf{Z})$ and each compatible graph $G$, exhibit a finite explicit list of rational polynomials $Q^G_1,\dots,Q^G_r$ in $(\theta,u)$. With $S=K(G)+2^{|\mathbf{Z}|}$, require the list length, degrees, coefficient bit lengths, and construction time to be polynomial in $S$ (using sparse monomial encoding). Find constants $a,b>0$ depending only on $(m,S)$ such that for all $\lambda,\mu\in(0,\tfrac12)$, on the subclass $\mathcal{M}(\mathsf{s},\lambda,\mu):=\lbrace M\in\mathcal{M}(\mathsf{s},\lambda): |Q^G_j(\theta,u)|\ge\mu \text{ for all } j,\text{ where } G \text{ is } M\text{'s own graph}\rbrace $:

1. $\boldsymbol\Delta_M=\boldsymbol\Delta_{M'}$ implies $M=M'$;
2. quantitatively, $M'\in I_\delta(M)$ implies $e(M;M')\le (K/\lambda\mu)^{a}\ \delta$ for all $\delta$ below an explicit threshold;
3. the excluded set is small: for each fixed graph $G$ and Lebesgue-almost-every $u$, $\mathrm{Leb}\bigl\lbrace \theta: |Q^G_j(\theta,u)|<\mu \text{ for some } j\bigr\rbrace \le S^{a}\mu^{b}$.

In words: identical behavior forces identical models on the subclass, nearly optimal behavior pins the model to within a constant times the regret, and the models sacrificed to get this are few. The rest of the agenda's finite-sample theory is posed relative to a solution. Classical causal discovery made the same move once before: strong faithfulness replaces a measure-zero assumption by an explicit margin, and Uhler, Raskutti, Bühlmann, and Yu [URBY13] showed the excluded set can be surprisingly large — a caution aimed directly at part 3.

## References

- [[RE24]](../references/RE24.md) J. Richens and T. Everitt, *Robust agents learn causal world models*, ICLR 2024. [arXiv:2402.10877](https://arxiv.org/abs/2402.10877)
- [[URBY13]](../references/URBY13.md) C. Uhler, G. Raskutti, P. Bühlmann, and B. Yu, *Geometry of the faithfulness assumption in causal inference*, Annals of Statistics 41 (2013), no. 2, 436–463.

*Related: [MAIS-O23](MAIS-O23.md) (the qualitative question the list would settle) · [MAIS-O25](MAIS-O25.md) (query complexity built on this subclass) · [MAIS-O2](MAIS-O2.md) (the sampled-action headline problem).*
