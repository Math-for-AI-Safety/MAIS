# Boundary-state residual of max-margin gradient descent

*Open problem MAIS-O89 · posed in [MAIS-A8](../agendas/A8/) as [Problem 7.1](../agendas/A8/MAIS-A8.tex#L462) · Status: open; [full solution pending review (issue #14)](https://github.com/Math-for-AI-Safety/MAIS/issues/14).*

*Tags: generalization · goal misgeneralization · training dynamics · optimization · dynamical systems. Difficulty: ★.*

*Authored by: Claude Fable 5 directed by Lionel Levine · Audited by: GPT 5.6 Sol.*

By the implicit-bias theorem of Soudry, Hoffer, Nacson, Gunasekar, and Srebro [[SHNGS18]](../references/SHNGS18.md), gradient descent on separable data converges in direction to the max-margin classifier. That direction predicts a trained linear classifier only to leading order, and the bounded residual it leaves behind is usually beneath notice. On the coin line — the agenda's one-dimensional toy of goal misgeneralization — the residual controls half the test return.

The setting is the agenda's coin line with the relative encoding: an agent at $p$ and a coin at $c$ on $\lbrace 0,\dots,L\rbrace $, features $x_{\mathrm r}(p,c)=(1,c-p)$, and a linear model $f_w=w\cdot x_{\mathrm r}$ trained by gradient descent on the logistic loss at zero diversity, meaning every training state has the coin at the right end and label "step right" (exact training conventions in [MAIS-A8](../agendas/A8/)). The agenda's Proposition 4.4 shows that from any initialization, at any sufficiently small step size, $w_k/\lVert w_k\rVert\to(1,1)/\sqrt2$, so the trained policy eventually steps right when $c\ge p$ and left when $c\le p-2$ — nearly the intended goal. At the boundary states $c=p-1$ the limit direction is silent: there $(1,1)\cdot x_{\mathrm r}=0$, and the verdict falls to the residual. The stakes are concrete: a policy that deterministically steps right one step past the coin enters a two-cycle and never collects it, while a left step collects it immediately. Nor is the boundary rare: since the limiting policy steps left whenever $c\le p-2$, every episode that starts with the coin behind the agent funnels into a boundary state, and that is about half of all test episodes.

**Problem ([MAIS-A8, Problem 7.1](../agendas/A8/MAIS-A8.tex#L462)).** In this setting, the boundary logit at $c=p-1$ is $d_k=w_{0,k}-w_{1,k}$; the leading maximum-margin term cancels there. Determine whether $d_k$ converges as a function of the initialization and step size, and compute its limit $d_\infty$. Then compute the exact return of the limiting logistic policy, which steps right at a boundary state with probability $\sigma(d_\infty)$. This convention includes $d_\infty=0$ without a separate tie rule.

Here $w_{0,k},w_{1,k}$ are the two coordinates of the step-$k$ iterate, $\sigma(z)=1/(1+e^{-z})$, and the return is the test return of the agenda's coin line: the probability of collecting the coin within the horizon when start and coin positions are independent and uniform. The refined residual analysis of Ji and Telgarsky [[JT19]](../references/JT19.md) for separable logistic regression is the natural tool, and no networks are involved: this is a starter problem about a two-parameter convex iteration. For the max-margin computation it refines, see [MAIS-A8](../agendas/A8/).

## References

- [[SHNGS18]](../references/SHNGS18.md) D. Soudry, E. Hoffer, M. S. Nacson, S. Gunasekar, and N. Srebro, *The implicit bias of gradient descent on separable data*, Journal of Machine Learning Research 19 (2018), no. 70, 1–57. [arXiv:1710.10345](https://arxiv.org/abs/1710.10345)
- [[JT19]](../references/JT19.md) Z. Ji and M. Telgarsky, *The implicit bias of gradient descent on nonseparable data*, Conference on Learning Theory 2019, pp. 1772–1798. [arXiv:1803.07300](https://arxiv.org/abs/1803.07300)

*Related: [MAIS-O85](MAIS-O85.md) (defers to this residual when the margin score vanishes) · [MAIS-O84](MAIS-O84.md) (the other finite-time question in the linear model) · [MAIS-O8](MAIS-O8.md) (the headline selection problem).*
