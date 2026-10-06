# Citing MAIS

MAIS is a pre-publication research outlet: documents here are drafts with status labels, released early so that others can build on them. Nothing here is certified by peer review unless its own page says otherwise. Cite accordingly — by permanent identifier, with a date.

## The repository as a whole

```bibtex
@misc{mais,
  author       = {Levine, Lionel},
  title        = {Math for {AI} Safety},
  year         = {2026},
  howpublished = {\url{https://github.com/Math-for-AI-Safety/MAIS}},
  note         = {Living repository of open problems, research agendas, and papers}
}
```

## An individual problem, agenda, or paper

Every document carries a permanent identifier (MAIS-O$n$ for open problems, MAIS-A$n$ for agendas, MAIS-P$n$ for papers) that survives revision, promotion, and resolution — cite the identifier, not the file path or a section number. Each document's byline and credit lines state its authorship, including AI contributors and their roles; cite what is printed. For example:

```bibtex
@misc{MAIS-O60,
  author       = {{Claude Fable 5}},
  title        = {Open problem {MAIS-O60}: Does a single {ReLU} neuron align to one frequency?},
  year         = {2026},
  howpublished = {\url{https://github.com/Math-for-AI-Safety/MAIS/blob/main/open-problems/MAIS-O60.md}},
  note         = {In \emph{Math for AI Safety}. Resolved in the negative by
                  Gautam Neelakantan Memana, August 2026}
}
```

Documents evolve; for an immutable reference, cite a permalink (press `y` on any GitHub file page to pin the URL to the current commit) or give an access date.

## Contributions by others

Solutions, corrections, and papers contributed by others belong to their authors. Cite the contributor's own document — their arXiv posting, paper, or note — as the primary source, with the MAIS identifier locating the problem it addresses:

```bibtex
@misc{memana2026,
  author = {Neelakantan Memana, Gautam},
  title  = {A simple dead-neuron counterexample of {MAIS}-60},
  year   = {2026},
  note   = {Resolves MAIS-O60 in the negative. Posted at
            \url{https://github.com/Math-for-AI-Safety/MAIS/issues/1}}
}
```

The problem page's Resolution section records the pointer either way, so a reader following the MAIS identifier will find the contributor's work.
