# Contributing

Each document in this repository has its own author list and its own status, noted at the top of the file. Contributions of every size are welcome! Notice a typo? Have you solved one of the open problems, or do you have an idea for tackling one of them? Want your own open problem listed here? [Open an **issue**](https://github.com/Math-for-AI-Safety/MAIS/issues/new/choose) (a public comment thread attached to this repository) or email me: [lionel.levine@cornell.edu](mailto:lionel.levine@cornell.edu)

Step-by-step instructions: [submitting a solution](contributing/solutions.md) · [discussion and progress on a problem](contributing/discussion.md) · [submitting a correction](contributing/corrections.md) · [proposing a new problem or agenda](contributing/new-problems.md).

## Proposing a new problem

If you have an open problem that fits the collection — a precisely stated math problem motivated by AI safety — describe it in an issue or by email. If it fits, we'll add it to [`open-problems/`](open-problems/) with you as an author. An accepted problem receives a permanent unique MAIS-O identifier, which names the problem itself and never changes as it moves into agendas and papers.

## Rough ideas are welcome!

The [Polymath projects](https://polymathprojects.org/) ran on a rule worth keeping: share rough ideas instead of waiting to polish them. The same rule applies here. Contributions should aim for the house style in [STYLE.md](STYLE.md). If you use LLMs in your writing process, provide them with STYLE.md so that they write in the house style.

## Corrections and small improvements

Typos, broken references, clearer phrasings, missing citations: edit the file directly on GitHub — the pencil icon on any file page opens an editor, and **Propose changes** turns your edit into a pull request, with the machinery of forks and branches handled invisibly. Merged corrections are acknowledged in the relevant document.

## Substantial contributions

Progress on an open problem, a new result, a counterexample, a new section or worked example: **say so before investing serious work**, in an issue or by email. This protects you from duplicating effort or working at cross purposes with the authors, it lets others see that the problem is being actively worked on, and it puts the coauthorship question on the table from the start.

**Finding a solution in the literature is solving the problem.** The problems here were checked against the literature when they were released, but "open" always means open to our knowledge. If you discover that a listed problem was already solved — whether or not anyone here is working on it — that discovery is a contribution in its own right: the problem page will record the problem as resolved by the prior work, identified by you, with credit.

## Coauthorship

A substantial contribution to a document earns an invitation to join that document's author list. Expanding an open problem into an agenda, developing an agenda, or helping an agenda spin off a paper can all be the kind of contribution that makes a coauthor.

What counts as substantial is judged per document by its current authors, in conversation with the contributor, and settled before the contribution is merged.

## GitHub for mathematicians

GitHub organizes collaboration around two objects with opaque names. An **issue** is a public comment thread attached to the repository — the place to ask a question, report an error, float an idea, or announce that you are working on something.  A **pull request** is a proposed edit: a marked-up copy of one or more files, submitted for the authors to review and merge — the manuscript with margin notes. Both require only a free account, created in a minute at [github.com/signup](https://github.com/signup). To follow what happens here, press **Watch** at the top of the [repository page](https://github.com/Math-for-AI-Safety/MAIS) for notifications of new threads, or subscribe to the [feed of changes](https://github.com/Math-for-AI-Safety/MAIS/commits/main.atom).

## Writing math in Markdown

Two formats live in this repo: open problems are Markdown, so they render right on GitHub; agendas and papers are LaTeX, each committed with its compiled PDF (see the folder READMEs for how to rebuild one).

For the Markdown: GitHub renders LaTeX in `.md` files — `$...$` inline, `$$...$$` for displays, and most of `amsmath` works. Writing

```
The board is the torus $\Lambda = (\mathbb{Z}/N\mathbb{Z})^2$, and we ask for
$$\Pr[\text{the board resembles a smiley face}] \ge 1 - 10^{-100}.$$
```

renders as: The board is the torus $\Lambda = (\mathbb{Z}/N\mathbb{Z})^2$, and we ask for

$$\Pr[\text{the board resembles a smiley face}] \ge 1 - 10^{-100}.$$

Three gotchas. For a multi-line display (`aligned`, `cases`, a matrix) — or any display that misrenders — a fenced ` ```math ` block is more robust:

````
```math
d(x,y) = \begin{cases} 0 & \text{if } x = y, \\ 1 & \text{otherwise.} \end{cases}
```
````

```math
d(x,y) = \begin{cases} 0 & \text{if } x = y, \\ 1 & \text{otherwise.} \end{cases}
```

Inside a table, a bare `|` reads as a column divider, so write `\vert` (e.g. `$\vert x \vert$` for $\vert x \vert$). And a literal dollar sign is `\$`.

One more trap: GitHub applies Markdown's backslash-punctuation escapes *inside* math, silently deleting the backslash from `\{`, `\}`, `\|`, `\,`, `\;` and letting a bare `*` open italics. Write `\lbrace`, `\rbrace`, `\Vert`, `\ ` (backslash-space), and `\ast` instead, and put any display containing `\\` line breaks in a fenced ` ```math ` block, where these escapes do not apply.

## License

Everything here is under [CC BY 4.0](LICENSE). By contributing, you agree that your contribution is released under the same license.
