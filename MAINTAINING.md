# Maintaining

*Maintainer playbook · Authored by Claude Fable 5.1, directed by Lionel Levine · October 2026 · Status: **provisional** — a first version of the workflow, to be refined as maintainers use it. Propose changes by pull request.*

*How a maintainer handles one issue from arrival to record. Readers and contributors don't need this page: [CONTRIBUTING.md](CONTRIBUTING.md) is theirs. Conventions for files, identifiers, and builds are in [HOUSEKEEPING.md](HOUSEKEEPING.md).*

Almost every issue in this repository is a submission waiting for a verdict: a claimed solution, a partial result, a correction, a proposed problem, a reference summary. The job of a maintainer is to move each one from `unscreened` to `recorded` (or to a clear no), in public, without waiting for anyone else. This page is the whole procedure. If a step here leaves you unsure, ask on the thread; if the page is wrong, fix the page.

## Who does what

Each research agenda MAIS-A1 … A8 has an **area editor**, who owns every issue labeled with that agenda. Any maintainer may merge a pull request that adds or updates a resolution page, a progress page, or a reference summary. Changes to an agenda, a paper, or the verbatim statement of a problem are reviewed by Lionel Levine before merging. Direct pushes to `main` are not how changes land: every change, including a maintainer's own, arrives as a pull request.

## The six steps

### 1. Claim it

Assign yourself, set the stage label to `in-review`, and say so on the thread within a day: who you are and that you'll read the submission and report back. Say nothing yet about the mathematics. A claim is an acknowledgment, not a judgment, and a thread that has been acknowledged by a named human is already in better shape than one that hasn't.

### 2. Screen it

Ten minutes. Three questions: is it in good faith, could a mathematician follow what is being claimed, and does it bear on the problem *as stated* on its page? Submissions whose authors disclose that an AI system produced the argument (label `ai-generated`) get this screen before anything expensive. A post that passes moves to `screened`. One that doesn't gets a short, kind reply saying what's missing, and stays `screened`-pending until the author responds.

### 3. Verify it

Read the argument yourself first. Then run the automated referee pipeline, which lives in the private `MAIS-referee` repository (ask a maintainer for access), and any machine checks that apply: a formalization another volunteer has posted, numerics attached to the thread, a verifier script. Record what was checked and what was not. Silence is never verification: every claim in the submission either gets a disposition or is listed as unchecked.

Two traps that have already caught people:

- **Quantifiers.** A counterexample must meet the statement's quantifiers. If the problem asks about behavior with positive probability, a single trajectory refutes nothing; an open set of initial conditions does. Read the verbatim statement on the problem page, not the summary in the issue.
- **Overlap.** Several threads often address one problem, or one thread claims something another thread already posted. Read both. Priority goes by posting date, and the record can credit both.

### 4. Decide

Five outcomes, each with a label:

| Outcome | Label | What happens next |
|---|---|---|
| The problem is resolved as stated | `verified` | Step 5, a resolution page |
| Real progress, problem still open | `verified` | Step 5, a progress page |
| Promising but has a gap or needs clarification | `needs-revision` | Reply with the specific requests; wait |
| Does not resolve the problem as stated | `not-a-resolution` | See the publicity rule below |
| Not a submission about a MAIS problem | `discussion` | Reply, point to CONTRIBUTING.md, close |

**The publicity rule.** Positive outcomes, and the author-facing summary of the review, go on the thread. A negative outcome goes to the author privately first, by email or direct message, and is posted only with their consent. Public descriptions of the review process stay at the level of "an automated adversarial review from several independent angles": never list its components.

### 5. Record it

Open a pull request. The conventions are in [HOUSEKEEPING.md](HOUSEKEEPING.md); the two existing pages are the templates:

- **Resolution:** [open-problems/O60/MAIS-O60-resolution.md](open-problems/O60/MAIS-O60-resolution.md). The problem page's status line becomes `resolved in the negative/affirmative by <resolver> (month year)`, its credit block gains `Resolved by:`, a `## Resolution` section states the answer and sketches the argument so the page stands without external links, and the registry row gains a `— **resolved**` marker.
- **Progress:** [open-problems/O62/MAIS-O62-progress-1.md](open-problems/O62/MAIS-O62-progress-1.md). A progress page in the problem's folder, linked from the problem page, stating what is proved, what is numerical, and what remains open.

Both carry a byline naming who checked the work. Credit the authors exactly as printed on their submission, AI coauthors included. Link to their document where they posted it; don't copy it into the repository. The problem keeps its identifier forever.

Pull-request checklist, which is also the pull-request template:

- [ ] Identifier unchanged; no renumbering
- [ ] Status line and credit block updated on the problem page
- [ ] Registry row in `open-problems/README.md` updated
- [ ] Credit matches the submission's printed byline
- [ ] Links point to landing pages, not PDFs (except links labeled "PDF")
- [ ] Checker named in the page's byline
- [ ] Issue number in the pull-request description

### 6. Close it

After the merge, comment on the thread with a link to the page, set the stage label to `recorded`, and close the issue. A problem that stays open after verified progress keeps its thread for further discussion; say which in the closing comment.

## Other kinds of issue

- **Corrections** (`correction`). Typos and broken links: fix and merge. Anything touching a problem's mathematical content is conservative by policy: the identifier survives a repair, and a genuinely different problem gets a new number instead. These go to Lionel for review.
- **New problems** (`new-problem`). Compare against the registry first; a restatement keeps the existing identifier. If it is new, it gets the next free O-number, a page in the house format, a registry row, and jump-list entries, all in one commit, credited to the proposer.
- **Reference summaries** (`reference`). Check the summary against the paper. A page in `references/` and its registry row land in one commit, with the summarizer credited in the page's byline.

## What to do first

When the queue is long, take it in this order:

1. Human-authored submissions with an external artifact: a preprint, a code repository, a preregistered protocol.
2. Submissions on which a machine check has already been posted by someone else. Half the verification exists; your job is the hypotheses it lists as assumed.
3. Corrections and proposed problems, which may change a statement.
4. AI-generated candidate solutions with no independent check. Screen first; full verification only if the screen passes. Group threads by problem so one reader handles related claims together.

## Labels

Every open issue carries one label from each of the first three families.

- **Kind:** `candidate-solution` · `progress` · `correction` · `new-problem` · `reference` · `discussion`
- **Stage:** `unscreened` → `screened` → `in-review` → `needs-revision` / `verified` / `not-a-resolution` → `recorded`
- **Area:** `A1` … `A8` · `meta`
- **Provenance:** `ai-generated`, applied only when the submitter says so
