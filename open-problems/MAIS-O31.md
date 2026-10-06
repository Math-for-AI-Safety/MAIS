# What one intervenable variable reveals about a causal chain

*Open problem MAIS-O31 · posed in [MAIS-A2](../agendas/A2/) as [Question 4.10](../agendas/A2/MAIS-A2.tex#L332) · Status: open; [full solution pending review (issue #8)](https://github.com/Math-for-AI-Safety/MAIS/issues/8).*

*Tags: interpretability · world-model discovery · eliciting latent knowledge · black-box evaluation · probability. Difficulty: ★★.*

*Authored by: Claude Fable 5 directed by Lionel Levine · Audited by: GPT 5.6 Sol.*

Take the simplest causal structure there is, a chain $C_m\to C_{m-1}\to\dots\to C_1$ of binary variables, and an agent paid to guess the far end $C_1$ while observing nothing. An analyst may tamper with the chain at a single link $C_j$ and watch how the agent's guess responds. How much of the chain's probability law does that one dial recover?

This is a smallest instance of world-model extraction from behavior: reading what an agent believes off what it does, with no access to its internals. Richens and Everitt [[RE24]](../references/RE24.md) proved that an analyst who can intervene on every chance variable recovers the whole model from the agent's optimal policies, for almost every parameter choice. An auditor of a real system controls fewer dials, and what a restricted intervention set identifies is open in general ([MAIS-O30](MAIS-O30.md)); the chain with one dial is the case meant to be solved first.

The framework: a model is the chain graph together with its conditional probability tables — the marginal of the root $C_m$ and a $2\times2$ transition table at each of the $m-1$ edges, hence $2(m-1)+1$ parameters $\theta$, all bounded away from degeneracy in the margin class $\mathcal{M}(\mathsf{s},\lambda)$, where the skeleton $\mathsf{s}$ records the data $(\mathbf{C},\mathbf{O},\mathbf{Z},u)$ fixed in the question below. Conditions (M2)–(M3) on the utility say the guess always matters: the utility gap is bounded away from zero and takes both signs. Since the agent observes nothing, a policy is a single number in $[0,1]$, the probability of guessing $1$. The analyst may apply any mixture $\sigma\in\Sigma_W$ of local interventions (fix, flip, or leave each variable of $W$) and observe an optimal policy for the resulting task; a parameter is **$\Sigma_W$-identifiable** if it is constant across all models in the class sharing such an optimal-policy family.

**Question ([MAIS-A2, Question 4.10](../agendas/A2/MAIS-A2.tex#L332)).** Let $\mathbf{C}=\lbrace C_1,\dots,C_m\rbrace $ with graph the directed path $C_m\to C_{m-1}\to\dots\to C_1$, observation set $\mathbf{O}=\emptyset$, utility parents $\mathbf{Z}=\lbrace C_1\rbrace $, and $u$ with margin (M2)–(M3). For $W=\lbrace C_j\rbrace $, a single intervenable variable: which of the $2(m-1)+1$ table parameters are $\Sigma_W$-identifiable for almost every $\theta$ (comparison class: the models of $\mathcal{M}(\mathsf{s},\lambda)$ carrying this chain graph, so that all the parameters are defined)?

The agenda records a heuristic, labeled as such: mixtures at $C_j$ should reveal the composite transfer map from $C_j$ to $C_1$ — a product of $2\times2$ stochastic matrices — and the observational marginal of $C_1$, but not the individual factors nor anything upstream of $C_j$. Neither half is proved, and the negative half needs a genuine indistinguishability construction: two chains, agreeing in transfer map and marginal, sharing every optimal policy. For the full margin quantifiers and the restricted-intervention formalism this instantiates, see [MAIS-A2](../agendas/A2/).

## References

- [[RE24]](../references/RE24.md) J. Richens and T. Everitt, *Robust agents learn causal world models*, ICLR 2024. [arXiv:2402.10877](https://arxiv.org/abs/2402.10877)

*Related: [MAIS-O30](MAIS-O30.md) (the general restricted-intervention problem) · [MAIS-O34](MAIS-O34.md) (another small exact case: two variables, all interventions) · [MAIS-O23](MAIS-O23.md) (identifiability with the full intervention set).*
