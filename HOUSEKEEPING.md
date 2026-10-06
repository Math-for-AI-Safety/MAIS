# Housekeeping

Conventions that keep this repository consistent. Readers don't need this page; it's for anyone editing the repository itself.

## Identifiers and numbering

- Every document carries a unique permanent identifier: **MAIS-On** for open problems, **MAIS-An** for agendas, **MAIS-Pn** for papers. Numbers are assigned on arrival and never reused.
- The identifier names the mathematical content, not a file: a problem keeps its O-number when it is quoted in a paper, expanded into an agenda, or resolved.
- [open-problems/README.md](open-problems/README.md) is the full registry. A new problem lands as its page, its registry row, and its jump-list entries in the same commit. Before assigning a number, compare against the existing statements: a strict first case or a different deliverable may earn its own identifier; a restatement does not.
- The seed agendas A1–A8 expand the survey's headline problems O1–O8. The matching numbers record those particular relationships, not a general convention — a future agenda need not match its problem's number.
- **Resolved problems** keep their identifier and page forever. The status line becomes `Status: **resolved in the negative/affirmative** by <resolver> (month year)`; the credit block gains a `Resolved by:` entry; a `## Resolution` section after the verbatim statement states the answer, sketches the argument self-containedly (external links may rot; the page should not), records what remains open, and links the solution documents; the registry row gains a `— **resolved**` marker. A successor question, if any, is a new problem with a new number.
- **Per-problem folders, as needed.** When a problem accumulates material beyond its page — a resolution record, extended discussion — it gets a folder `open-problems/O$n$/` holding those pages (`MAIS-O60-resolution.md`), created only when first needed. The problem statement stays at `open-problems/MAIS-O$n$.md`, so no existing link moves; its status line and Resolution section link into the folder. Contributed solution documents are linked where their authors posted them (an issue attachment, arXiv, or a submitted MAIS paper — the paper becomes the primary link once it lands), not copied into the repository.
- **Draft problems** carry MAIS-D$n$ identifiers while unreleased; on release a draft receives the next free O-number (D-numbers are never public identifiers).

## Files and folders

- Folder names carry no MAIS- prefix (`agendas/A1/`, `papers/P2/`); filenames keep it (`MAIS-A1.tex`).
- Each agenda and paper folder is its landing page: a `README.md` with title, byline, abstract, and links, like an arXiv abstract page. Links elsewhere in the repository point to landing pages, not directly to PDFs — except links explicitly labeled "PDF".
- Each agenda and paper records its origin in a `PROVENANCE.md` beside its source.
- `references/` holds one summary landing page per cited external work, filed by citation key (`references/RE24.md`). Keys are alpha-style: surname initials + two-digit year for up to five authors ([RE24], [URBY13]); first four initials + "+" beyond five ([EHOS+22]); single authors get one initial ([C19]), more letters only to break a collision ([Dor26]/[Del26]). In-text citations render as bracketed links (`[[RE24]](…)`) targeting the reference page when one exists, else the paper. A new reference page updates its Cited-by list and the registry row in the same commit; a new citation of a summarized work links to its page. Every section of a reference page — the safety-relevance section especially — stands on its own merits for a reader (or LLM) who knows nothing of this repository: MAIS pointers come last and brief, never as the lead.
- Every document opens with a status line: kind + number · author · date · status.
- Each open-problem page follows its status line with two metadata lines: a flat tag list with star difficulty (`*Tags: … · …. Difficulty: ★★.*` — one merged list, no category labels; ★ a starter, ★★ a research project, ★★★ likely to need a new idea; *empirical* tags a computational deliverable) and a credit line (`*Authored by: … · Audited by: ….*`).

## Building

- Rebuild any document with LaTeX source by running `latexmk -pdf -interaction=nonstopmode -halt-on-error <FILE>.tex` from its folder. Only each `.tex` and its matching `.pdf` are committed; LaTeX auxiliary files are ignored.
- MAIS-P1 is a fixed PDF: a solo paper nearing completion, so its source is not a collaboration surface and is not in the repository.

## Issue workflow

- The maintainer's procedure for an issue, from arrival to record, is [MAINTAINING.md](MAINTAINING.md).
- Every open issue carries a kind label (`candidate-solution`, `progress`, `correction`, `new-problem`, `reference`, `discussion`), a stage label (`unscreened` → `screened` → `in-review` → `needs-revision` / `verified` / `not-a-resolution` → `recorded`), and an area label (`A1` … `A8`, or `meta`). `ai-generated` marks submissions whose authors disclose AI-produced arguments.
- All changes arrive as pull requests. Resolution pages, progress pages, and reference summaries may be merged by any maintainer; agendas, papers, and verbatim problem statements are reviewed by Lionel Levine.
