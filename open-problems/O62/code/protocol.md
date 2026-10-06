# Search protocol

This protocol records the widths attempted, solver, initialisation and seeds,
number of attempts, tolerances, stopping rules, and screening stages.

The search implementation and logs referenced below are in the complete
evidence archive attached to
[issue #2](https://github.com/Math-for-AI-Safety/MAIS/issues/2); this directory carries
the standalone verifier and exported numerical fits.

Everything below is read off the code, the logs, or the interpreter that
produced the results. Where a fact could not be established it is stated as not
established, with whatever *can* be checked in its place.

Three things to read before the detail:

1. The environment was not recorded at run time. It is recovered from the
   unmodified virtual environment: **Python 3.11.15, numpy 2.4.6,
   scipy 1.17.1**.
2. Three runs used non-default restart counts. Three `p=7, H=12` configurations
   (3,600 restarts) had no log and are **excluded from every count**; the
   accounting is 18 configurations / 12,100 restarts, all log-confirmed.
3. The shipped `p=5, H=8` sample is **seeded and rerunnable**, with the seed
   recorded per fit. Reruns reproduce it exactly *provided the BLAS thread count
   is pinned*; unpinned, floating-point noise can change whether a borderline
   seed is accepted, so compare by residual and gauge-invariant norm rather than
   by bytes. Every solution verifies from its JSON regardless of any rerun — see
   "Reproducibility".

---

## Solver

`scipy.optimize.least_squares`, `method='lm'` (MINPACK Levenberg–Marquardt),
with an analytic Golub–Pereyra Jacobian, on a variable-projection objective: the
output weights `V` are eliminated by least squares at every evaluation, so only
the units `(x_j, y_j)` are searched.

- residual vector: `T - N pinv(N) T` flattened, plus `H` gauge rows
  `0.1 * (||p_j||^2 - 1)` fixing the per-unit scale
- `N[a*p+b, j] = (x_j(a) + y_j(b))^2`, `T[a*p+b, c] = [a+b == c]`
- tolerances `xtol = ftol = gtol = 1e-15`
- `method='trf'` is substituted automatically when `p^3 + H < 3pH`; this did not
  trigger at `p=5, H=8` or at `p=7, H<=13`
- source: `modadd/core.py` (`Model.resid`, `Model.jac`, `Model.fit`)

### Environment

`requirements.txt` gives only the intended floor (`numpy>=2.0`, `scipy>=1.13`,
`mpmath>=1.3`). The versions that actually ran are **Python 3.11.15, numpy
2.4.6, scipy 1.17.1**.

The direct evidence is the shipped `p=5, H=8` sample itself: it is the output of
a replay run under this interpreter (see "Reproducibility"), so the environment
is demonstrated by a live rerun rather than inferred. Two independent reruns
under it agreed bit for bit.

Supporting: the `numpy` and `scipy` `dist-info` directories are dated
2026-07-28 and have not been upgraded since, and `venv/pyvenv.cfg` still records
the path the repository occupied before it was moved, so the environment has not
been rebuilt.

The BLAS backend is Accelerate, and its thread count must be pinned for exact
reruns — see "Reproducibility" for the reason and the exact invocation.

---

## Stopping rules and acceptance

Two stages, both in `modadd/core.py`:

- **screen**: `max_nfev = 200` (or `300`), `tol = 1e-10`
- **polish**: repeated LM re-entry from its own output, `max_nfev = 8000`,
  `tol = 1e-15`, up to 4 rounds (8 in the seeded drivers), stopping early when a
  round fails to improve the residual by a relative `1e-3`

**Acceptance criterion: a point is recorded as a fit iff residual `< 1e-9` *and*
`||V|| < 1e4`.**

The `||V||` condition is not cosmetic. A small residual carried by a diverging
`||V||` is a border-rank artefact — a degeneration approaching the target
without reaching it — and residual `< 1e-10` alone admits those
(`experiments/c3_sample.py`, `NOTES` §17). Every negative result below should be
read against both conditions, not the residual alone.

---

## Screening, and the effective number of trials

`core.multistart` / `multistart_par` are coarse-to-fine: every restart gets the
cheap screen, and only the best `polish_frac` of them (2% or 3%, floor of 3) is
polished to convergence.

At `p=7, H=12`, counting **only** configurations whose restart count is
confirmed by a log header (see "Restart counts" below — three configurations
recorded solely in `NOTES` §5, 3,600 restarts, are excluded):

| | count | source |
|---|---|---|
| configurations | **18** | `t1_H12.log` (11 restricted + 1 unrestricted), `t1_H12_deep.log`, `t1a_p7_H12.log` (6) |
| total restarts | **12,100** | `4,400 + 400 + 2,500 + 4,800` |
| restarts polished to convergence | **271** | `88 + 12 + 75 + 96`, from each driver's `polish_frac` |
| **effective trials** (see below) | **~10,600** | `0.88 x 12,100` |

Separately, and *fully polished*, the two seedings of §3.3 were run at
`(7,12)` for **4,400 restarts** with no screening at all
(`results/calib_p7new.json`, `results/calibration_full.log`). Those preserve
seeded-strategy coverage after the exclusion above, and they need no retention
correction. Combined effective trials at `(7,12)`: **~15,000**
(`10,600 + 4,400`).

**The polished count is not the effective trial count, because the screen is
calibrated.** Running the identical protocol at `p=5, H=8` with *every* restart
polished (`experiments/calibrate.py`, 15,700 restarts, 180 successes) puts 158
of the 180 successes — 88% — inside the top 2% of the screen ordering; success
ranks are 0, 1, 2, 3, … for almost every strategy. So screening discards about
12% of what it would otherwise find, and the effective trial count over the
log-confirmed screened runs is close to `0.88 x 12,100 ~ 10,600`.

One strategy is a clear exception and should be quoted separately: **one unit
fully constant retains only 23 of 40 (57%)**. It is also the strategy with the
highest raw success rate, so for that stratum the screen does real damage.

Caveat: the 88% is measured at `(5,8)`. Whether the screen orders `(7,12)` as
faithfully is untested.

### Configuration and restart accounting at `p=7, H=12`

Configurations as run, reconstructed from the drivers as committed, counting the
unrestricted ansatz once — its 400-restart and 2,500-restart runs are two runs
of one configuration:

- unrestricted, 400 @ 3%, and unrestricted deep, 2,500 @ 3%
  (`run_unrestricted.py`) — **counted**
- 11 restricted configurations, 400 each @ 2%
  (`run_t1_p7.py`, `structured_configs(12)`) — **counted**
- 6 census-seeded configurations, 800 each @ 2% (`t1a_p7.py`) — **counted**
- 3 seeded dead-unit configurations, 1,200 each @ 2% (`seeded_p7.py`) —
  **excluded, no log** (see below)

Counted total: **18 configurations, 12,100 restarts**. Including the excluded
three would give 21 / 15,700, the figure quoted before this audit.

Restart counts, confirmed from log headers, with three non-default:

| log | recorded | driver default |
|---|---|---|
| `results/t1_H10.log` | `restarts=600` | 400 — non-default |
| `results/t1_H11.log` | `restarts=600` | 400 — non-default |
| `results/t1_H12_deep.log` | `restarts=2500` | 400 — non-default |
| `results/t1_H12.log` | 400 (11 restricted + unrestricted) | default |
| `results/t1_H13.log` | `restarts=400` | default |
| `results/t1a_p7_H12.log` | 800, seed noise 0.25 | default |

**Every counted restart is log-confirmed.** The three seeded dead-unit
configurations (`A_j=0`, `B_j=0`, two dead) at 1,200 restarts each have no log
file — their count appears only in `NOTES` §5's table — so they are **excluded
from the accounting entirely**, taking the totals from 21 configurations /
15,700 restarts down to **18 / 12,100**. `NOTES` §5 retains the rows, annotated
as excluded. Nothing is lost from the negative result: the same seeded stratum
is covered by the fully-polished 4,400-restart run at `(7,12)`, which is
log-backed.

---

## Widths attempted

| `p` | widths | outcome |
|---|---|---|
| 3 | 3, 4 | 3 floors at `6.83e-1`; 4 fits, and an exact rational fit is now known |
| 5 | 7, 8, 9 | 7 no fit (6,100 restarts, all polished); 8 fits numerically; 9 by the Fourier construction |
| 7 | 10, 11, 12, 13 | 10 and 11 no fit; 12 no fit found; 13 fits |

Restart counts at `p=7`, `H != 12`: 600 at `H=10`, 600 at `H=11`, 400 at `H=13`
(`run_t1_p7.py`, `run_unrestricted.py`). The screening caveat above applies to
all of them.

### `p=5, H=7`: the floor itself

The proved Alder–Strassen / Winograd floor is `(3p-1)/2`, which at `p=5` is
exactly 7. So `H_min(5)` is 7 or 8, and until now the deciding width had no
search on record: every `p=5` run in the repo was at `H=8` or `H=9`, and the
only occurrence of `(5,7)` was in `tests/test_gates.py`, checking the analytic
Jacobian against finite differences rather than searching. The `p=7` ladder took
this step (widths 10 and 11, 600 restarts each) before concluding at 12; the
`p=5` ladder had skipped it.

`experiments/calib_p5_H7.py`, 6,100 restarts, **every restart polished**, using
the four strategies with the highest measured rates at `H=8` at the same restart
counts:

| strategy | N | successes | best residual | `\|\|V\|\|` at best |
|---|---|---|---|---|
| unrestricted | 2500 | 0 | `1.671e-04` | `3.94e9` |
| one dead unit `A_j = 0` | 1200 | 0 | `8.917e-01` | `6.42e7` |
| one dead unit `B_j = 0` | 1200 | 0 | `8.917e-01` | `4.04e8` |
| one unit fully constant | 1200 | 0 | `1.293` | `4.56e6` |

`S = 0 / N = 6,100`. Matching each strategy to its own measured `H=8` rate
predicts 110 successes, giving `P(0) ~ 1.7e-48`; the rule of three gives
`p_(5,7) < 4.9e-4`. The best point reached is `1.7e-4`, against `5.1e-15` for
the same strategy at `H=8` — eleven orders of magnitude, so width 7 is not a
near-miss. The two mirror strategies agree on a residual floor of `8.917e-01` to
four figures.

**How far this goes.** It is a quantified negative, not a proof, and the
findability assumption behind it is weaker here than for the `p=7` ladder. A
width-7 fit would sit exactly at the proved floor, so it would be an extremal
object; the `H=8` solutions form a 6-dimensional family modulo gauge, and there
is no reason to expect a width-7 variety, if nonempty, to be as large. It could
be isolated, and random-restart descent reaches isolated points far less readily
than families. The predicted-110 figure transfers the `H=8` rates unchanged and
therefore inherits that assumption in full.

---

## Initialisation and seeds

- **unstructured**: i.i.d. Gaussian, scale `0.8`, `numpy.random.default_rng`
- **seeded, dead unit**: one unit hard-constrained with `x_j` constant
  (`A_j = 0`) or `y_j` constant (`B_j = 0`); the rest unstructured
  (`experiments/seeded_p7.py`)
- **census-seeded**: live units initialised single-frequency on an assigned
  orbit with `g_j = 0` exactly, then all weights released so mixing can develop;
  only the dead unit is a hard constraint; seed noise `0.25`
  (`experiments/t1a_p7.py`)
- **restricted**: units hard-constrained to a single frequency via the basis
  `Bx_j`, `By_j` (`modadd/core.py`, `basis_x`, `basis_y`)

Seeds by driver: `run_t1_p7.py` uses `101` (restricted) and `202`
(unrestricted); `run_unrestricted.py` defaults to `202`; `seeded_p7.py` uses
`771`; `t1a_p7.py` uses `900 + short_orbit`; `c3_sample.py` starts at `1000` and
increments until the requested number of fits accumulates.

Per-solution seed attribution for `results/fits_p5_H8.npy` was not recorded:
`c3_sample.distinct_fits` increments its seed in a loop and stores only the
accepted parameter vectors. The exported provenance gives the seed range
(`1000..1039`).

---

## Acceptance thresholds: three criteria, and which applies where

Three different acceptance pairs appear across this work. They are listed here
because the headline comparison mixes runs made under two of them.

| used by | residual | `\|\|V\|\|` |
|---|---|---|
| the `p=7, H=12` searches, and `c3_sample` (`run_t1_p7`, `seeded_p7`, `t1a_p7`) | `< 1e-9` | `< 1e4` |
| the calibration (`calibrate.py`, at both `(5,8)` and the new `(7,12)` run) | `< 1e-11` | `< 1e3` |
| `verify.py`, the shipping gate | `res_inf <= 1e-12` | — |

**This was checked rather than assumed**, by re-tallying the stored per-restart
residuals and `||V||` under both criteria:

| | strict `1e-11 / 1e3` | loose `1e-9 / 1e4` |
|---|---|---|
| `p=5, H=8` | 180 / 15,700 (`1.1465e-2`) | **192 / 15,700 (`1.2229e-2`)** |
| `p=7, H=12`, new seedings | 0 / 4,400 | 0 / 4,400 |
| `p=5, H=7` | 0 / 6,100 | 0 / 6,100 |

Two things follow. **The negatives are threshold-independent** — every null is
`S = 0` under both criteria, so no width-12 or width-7 claim depends on where
the line is drawn. And **the calibration rate was understated**: measured at the
strict criterion it is `1.15e-2`, but the criterion the `(7,12)` searches
actually used gives `1.22e-2` (Wilson 95% `[1.06e-2, 1.41e-2]`).

**`1.15e-2` is the rate quoted throughout this document and in
`CALIBRATION.md`.** It is the conservative figure, and it is the one to cite
when the rate is stated on its own. The loose-criterion figure appears in
exactly two places, both below: the calibration table, which carries both
columns, and the single sentence under "What the searches do and do not
establish" that compares the rate to the width-12 null — where the two must be
measured the same way or the comparison is not like-for-like.

---

## Reproducibility

**The shipped `p=5, H=8` sample is the replay output of**

```
VECLIB_MAXIMUM_THREADS=1 OMP_NUM_THREADS=1 OPENBLAS_NUM_THREADS=1 \
MKL_NUM_THREADS=1 NUMEXPR_NUM_THREADS=1 \
python -m experiments.recover_seeds 10 60
```

seeded and rerunnable under the recorded environment, with the seed printed and
recorded per fit (`1001, 1003, 1005, 1006, 1007, 1011, 1013, 1016, 1017, 1018`;
19 seeds tried from `seed0 = 1000`). A rerun recovers the same fits; compare by
residual and gauge-invariant norm rather than by bytes.

**Pin the BLAS thread count.** numpy here is built against Accelerate, whose
reduction order varies between runs when multithreaded. That perturbs results in
the last bits, which is enough to change which restarts survive `multistart`'s
top-5% screen and therefore whether a borderline seed is accepted at all:
unpinned, seed `1000` is accepted on some runs and not others, shifting the
accepted set by one. With the thread count pinned as above, two independent
reruns agreed **bit for bit** — identical seeds, identical parameters, maximum
coordinate difference `0.000e+00`. The same effect explains a `12` vs `13`
success-count difference seen across two runs of the reference implementation.

Measured on the shipped sample: residuals `5.5394e-15` – `3.5565e-14`, `||V||`
`13.1135` – `138.9695`, and all ten pairwise distinct in gauge-invariant norm at
tolerance `1e-6` (norms `6.0792, 6.3433, 6.4030, 6.8407, 6.9150, 7.3346, 7.4906,
8.1002, 9.7802, 9.8433`) — distinctness verified directly rather than assumed,
since `c3_sample.distinct_fits` does not itself check it.

**Disclosure.** An earlier exported sample, whose generating command was not
recorded, could not be regenerated and was replaced by this seeded, rerunnable
sample; the replacement does not affect any claim, since solutions are verified
from their JSON independently of how they were found.

For the record, the two samples are genuinely different points of the
6-dimensional solution family, not the same fits in different gauges — their
gauge-invariant norms differ (withdrawn sample: `5.82, 6.22, 6.41, 6.49, 6.87,
6.99, 8.12, 8.97, 9.19, 13.93`). `results/fits_p5_H8.npy` is **not** tracked in git — `.gitignore` excludes
`results/*.npy` — and it is not one of the shipped solutions. It **is** included
in the evidence bundle, at `results/fits_p5_H8.npy`, as provenance for
`p05_H08_012`, which was derived from it by norm minimisation. So the array is
present alongside this document for anyone who wants to inspect the withdrawn
sample or re-derive that solution; but `p05_H08_012` verifies from its own JSON,
so no shipped claim depends on it.

`NOTES` §C3 and the README quote residuals `5.5e-15 .. 3.6e-14`, matching this
sample at both ends, so their analysis was computed on it. Their `||V||` minimum
of 15.8 is this sample's *second* smallest; the smallest is 13.11. **Any
statement of the form "the ten fits" must say which sample is meant.**

---

## Calibration: `P(search succeeds | a fit exists)`

Run and complete (`experiments/calibrate.py`; full report in `CALIBRATION.md`).
The control is the identical protocol at `p=5, H=8`, one unit below the Fourier
width, where exact fits are known, run for 15,700 restarts with every restart
polished to convergence so that `S/N` is a true per-restart rate. (That budget
was chosen to match the `(7,12)` sweep as originally counted; after the
log-confirmation audit the `(7,12)` figure is 12,100 screened restarts plus
4,400 fully polished, so the two are comparable in order of magnitude but are no
longer the same number. Nothing in the comparison depends on their being equal —
the quantity carried across is the per-restart rate, not the budget.)

| | strict `1e-11 / 1e3` | loose `1e-9 / 1e4` |
|---|---|---|
| `p=5, H=8` | 180 successes / 15,700 restarts | 192 successes / 15,700 restarts |
| per-restart rate | **`p_5 = 1.15e-2`** | `1.22e-2` |
| Wilson 95% | `[9.9e-3, 1.33e-2]` | `[1.06e-2, 1.41e-2]` |
| rule-of-three ceiling at `p=7, H=12` | `p_7 < 2.0e-4` (combined ~15,000 effective trials) | same |
| ratio | `~57x`; `~50x` at the Wilson lower end | `~61x`; `~53x` at the Wilson lower end |

The strict column is the one quoted elsewhere. The loose column exists because
the `(7,12)` searches used that criterion, so it is the like-for-like pair —
see "What the searches do and do not establish".

(For reference, the ceiling computed on the 12,100 log-confirmed screened
restarts alone is `2.5e-4` nominal / `2.8e-4` on the 10,600 effective, giving
ratios of `46x` / `41x`. All three are far above the `9.5x` quoted before the
audit — dropping the unlogged restarts *tightens* the bound rather than
loosening it, because the earlier figure was computed against the 2,500-restart
unrestricted run alone rather than the full log-confirmed count.)

**Run-to-run variation in these counts.** The calibration was run with the BLAS
thread count unpinned, so success counts carry a run-to-run wobble of order a
few — the same effect documented under "Reproducibility" (it is what produced a
`12` vs `13` difference across two runs of the reference implementation). This
does not touch any conclusion: the gap between `p_5 = 1.15e-2` and the
`p_(7,12) < 2.0e-4` ceiling is a factor of ~57, and a few counts move it
negligibly. The nulls are unaffected for a stronger reason — the best points
found at `(7,12)` and `(5,7)` sat at residual `~1e-8` and `~1e-4` with
`||V|| ~ 1e7`–`1e9`, orders of magnitude away from either acceptance boundary,
so no borderline flip could have concealed a fit. Reruns intended to reproduce
exact counts should pin the thread count as shown under "Reproducibility".

Strategies that can succeed run `8.8e-3` to `3.3e-2`. The strata that are
structurally empty (two dead units; all five orbit-pure and restricted
configurations) return exactly zero, which is the negative control, and it
passes: the acceptance criterion does not fire on border-rank points.

Additionally, the two seedings above were run at `p=7, H=12` at full scale with
every restart polished: **4,400 restarts, zero successes**, every point on a
border-rank path at `||V|| ~ 1e6`–`1e8`. Matching each seeding to its own
measured `p=5` rate predicts ~82 successes.

---

## What the searches do and do not establish

**Supported at `p=7, H=12`:** 18 log-confirmed configurations, 12,100 restarts,
of which 271 were polished to convergence under a screen (`max_nfev =
200`–`300`, `tol = 1e-10`) measured to retain 88% of fits at `(5,8)`, giving
roughly 10,600 effective trials; none produced a point meeting the acceptance
criterion (residual `< 1e-9`, `||V|| < 1e4`). A further **4,400 restarts were
polished without any screening**, also with no success, bringing the combined
effective count to roughly **15,000**. By the rule of three on that combined
count, `p_(7,12) < 2.0e-4` conditional on a fit existing.

This is the one place the two are compared, so both sides are measured the same
way. The `(7,12)` searches accepted at residual `< 1e-9` with `||V|| < 1e4`;
`(5,8)` scored under that same criterion gives `p_5 = 1.22e-2` (192 of 15,700),
a ratio of **61x**, or **53x** at the lower end of the Wilson interval
`[1.06e-2, 1.41e-2]`. Under the stricter criterion used to report `p_5`
elsewhere in this document (`1.15e-2`) the ratio is 57x, so quoting the
conservative rate costs nothing in the conclusion. See "Acceptance thresholds".

**Supported at `p=5, H=7`:** 6,100 restarts, every one polished, no success, best
residual eleven orders of magnitude above the `H=8` fits.

**Not supported:** that no such fit exists, at either width. Measuring the
success rate at `p=5, H=8` establishes findability *in that reference class*.
Transferring it to `p=7, H=12` — 252 parameters against 120, and a variety that
if nonempty is lower-dimensional — is an assumption, not a theorem; transferring
it to `p=5, H=7`, where any fit would be extremal and possibly isolated, is a
stronger assumption still. Any sentence resting on these numbers should say "as
findable as the `p=5, H=8` case" rather than "findable", and should state the
assumption rather than imply it.
