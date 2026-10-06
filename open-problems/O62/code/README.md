# Computational artifacts for MAIS-O62

[Progress report](../MAIS-O62-progress-1.md) · [PDF](https://github.com/user-attachments/files/30774709/MAIS_O62_Bound_Version_2.pdf) · [MAIS-O62](../../MAIS-O62.md) · [Issue #2](https://github.com/Math-for-AI-Safety/MAIS/issues/2)

Machine-readable parameters for the numerical fits quoted in *A Width Bound for
Quadratic Network Modular Addition*, plus a standalone verifier. Nothing here
imports the search code; `verify.py` and `refine.py` depend only on
`requirements.txt`.

The complete research archive, including the search implementation, calibration
runs, and logs referenced by `protocol.md`, is attached to
[issue #2](https://github.com/Math-for-AI-Safety/MAIS/issues/2).

```bash
pip install -r requirements.txt
python verify.py solutions/          # table + per-solution diagnostics
python refine.py solutions/p05_H08_012.json
```

`verify.py` exits nonzero if any solution fails, so it works as a CI check.

## The object

For odd `p` and width `H`, a parameter is `w = (x_j, y_j, V_j)` with
`x_j, y_j, V_j` in `R^p`. The network computes, for `a, b, c` in `Z/pZ`,

```
f_w(a,b)_c = sum_{j=1}^{H} V[c,j] * (x[j,a] + y[j,b])^2
```

against the target `T(a,b)_c = 1` if `(a+b) mod p == c` else `0`. An **exact
fit** has `f_w = T` at all `p^3` triples.

## Index conventions

| array | shape | indices |
|---|---|---|
| `x`, `y` | `H x p` | row `j` = unit, column = input residue |
| `V` | `p x H` | row `c` = **output** coordinate, column `j` = unit |

`V` is `p x H`, matching `V_{c,j}` in the note — **not** `H x p`. A transposed
`V` is the most likely silent failure mode, so `verify.py` checks the shapes
before doing anything else and aborts with a specific message on mismatch.

## File format

One JSON file per solution. Floats appear twice: `x`/`y`/`V` carry a decimal
form at 17 significant digits for reading, and `x_hex`/`y_hex`/`V_hex` carry
the IEEE-754 hex form. **`verify.py` reads the hex fields.** The decimal fields
are never used for computation.

Round-trip is exact: reading the hex and re-serialising with the documented
writer (`json.dumps(obj, indent=2, sort_keys=False) + "\n"`) reproduces each
file byte for byte. `verify.py` checks this per solution and reports it in the
`RT` column.

`gauge` is `"as-found"` for every file: parameters are exactly as the search
left them, with no normalisation applied, so the recorded provenance means what
it says. `verify.py` reports norms both as-found and normalised.

## Residual

Both are printed, computed over **all `p^3` equations** — no subsampling:

```
res_2   = sqrt( sum_{a,b,c} ( f_w(a,b)_c - T(a,b)_c )^2 )
res_inf = max_{a,b,c}  | f_w(a,b)_c - T(a,b)_c |
```

**Arithmetic: IEEE double (binary64) throughout `verify.py`.** `refine.py` uses
mpmath at the stated working precision, and is the only place any other
precision appears.

The residual is invariant under the gauge group below, so unlike a parameter
norm it is a meaningful number to quote without further qualification.

## Gauge

Two per-unit operations leave `f_w` pointwise unchanged:

- **scale** `(x_j, y_j, V_j) -> (t x_j, t y_j, t^-2 V_j)`, `t != 0`
- **shift** `(x_j, y_j) -> (x_j + s, y_j - s)`, `s` constant

Along a scaling orbit `||w|| -> infinity` in both directions with a unique
minimum, so *the norm of a solution* must mean the orbit minimum. That minimum
is closed-form. The shift optimum is `s = (ybar_j - xbar_j)/2` and does not
depend on `t`, so shift and scale do not interact; minimising
`t^2 u^2 + t^-4 v^2` (with `u^2 = ||(x_j,y_j)||^2`, `v = ||V_j||`) gives
`t^6 = 2 v^2 / u^2`, at which

```
||V_j||^2 = (1/2) * ||(x_j, y_j)||^2        <- the minimising gauge
```

This is what `verify.py` reports as `|w| gauge`. Note that
`||(x_j,y_j)||^2 = ||V_j||` is *not* scale-covariant and is **not** the
minimiser; it is not used here.

`verify.py` self-tests the gauge code by recomputing the residual after
normalising and reporting the change, which should be at machine-epsilon level.

## Thresholds

| quantity | flag | default | meaning |
|---|---|---|---|
| residual tolerance | `--tol` | `1e-12` | `res_inf` above this fails the run |
| rank cutoff | `--rank-cutoff` | `1e-10` | singular values below `cut * sigma_max` count as zero |
| dead unit | `--dead-tol` | `1e-8` | `\|\|A_j\|\|` or `\|\|B_j\|\|` below this (normalised gauge) is zero |
| annihilator support | `--supp-tol` | `1e-8` | `\|s_j\|` below this is outside the support |

## Per-unit diagnostics

With `ghat(k) = sum_a g(a) w^{-ka}`, `w = e^{2 pi i / p}` (numpy's FFT
convention), and `A_j = (xhat_j(k))_{k != 0}`, `B_j = (yhat_j(l))_{l != 0}`:

- `||A_j||`, `||B_j||` where `||A_j||^2 = sum_{k != 0} |xhat_j(k)|^2`
- **dead** iff `A_j = 0` or `B_j = 0` (equivalently `x_j` or `y_j` constant)
- offset `g_j = xhat_j(0) + yhat_j(0)`, mass `kappa_j = sum_{a,b} (x_j(a)+y_j(b))^2`
- the identity `kappa_j = g_j^2 + ||A_j||^2 + ||B_j||^2`, with the discrepancy
  printed (it holds to `~4e-15` on every exported solution)

## Annihilator

`s_j = sum_c V[c,j]` is printed for every unit, with its support and whether the
supporting units are dead. The note claims `s` is supported on a single unit and
that unit is dead. **On all 13 exported solutions this holds**: support is
exactly one unit, and that unit is dead. `verify.py` prints the verdict per
solution rather than asserting it.

## Jacobian rank

The Jacobian of `F: w -> (f_w(a,b)_c)`, from `R^{3pH}` to `R^{p^3}`, is reported
with its rank, the cutoff used, and the singular values bracketing the cutoff.

| | `d = 3pH` | equations `p^3` | rank | nullity | corank |
|---|---|---|---|---|---|
| `p=5, H=8` | 120 | 125 | **98** | 22 | 27 |
| `p=7, H=13` | 273 | 343 | **235** | 38 | 108 |

At `p=5, H=8` the ratio across the cutoff is `~1e14`, so the cutoff is not doing
delicate work; the spectrum is printed so this can be seen rather than trusted.
Pass `--full-spectrum` for all singular values.

The rank deficiency of 27 is the reason **standard certification does not
apply** here. Interval-Newton, Krawczyk and alpha-theory all require a square
system with invertible Jacobian. The fit conditions are not a complete
intersection: 27 of the 125 equations hold without being implied by the other
98, so a certificate for any square subsystem says nothing about the remainder.

## What this does not show

**No file here claims that `H_min(5) <= 8` or that these numerics prove
`H_min(7) <= 13`.** The correct framing is: *numerical candidates at the stated
residual, not certified exact fits.*

`H_min(7) <= 13` **is** proved — by the exact Fourier construction of width
`2p-1`, which is independent of everything in this directory. The two must be
kept separate: the `p=7, H=13` solution here is a numerical fit that happens to
sit at a width already known to be achievable by construction.

`refine.py` is evidence, not proof. On `p05_H08_012` the residual falls in step
with the working precision:

| dps | `res_2` |
|---|---|
| 30 | `2.26e-30` |
| 60 | `1.82e-60` |
| 120 | `2.23e-120` |

with no floor, which is consistent with a true nearby solution. It is not a
proof of one. In bilinear complexity, approximate algorithms of a given length
can exist with error tending to zero when no exact algorithm of that length
does — border rank exceeding rank — and that is exactly the possibility these
numbers cannot exclude.

## Contents

```
solutions/p05_H08_001..012.json   twelve p=5, H=8 candidates
solutions/p07_H13_001.json        one p=7, H=13 candidate
verify.py                         standalone verifier (numpy only)
refine.py                         high-precision Newton (mpmath)
protocol.md                       search protocol -- SEE ITS OPEN ITEMS
requirements.txt
```

Ten of the `p=5` solutions (`001`-`010`) are the C3 sample; `011` is an earlier
independent hit; `012` is derived from `009` by minimising the parameter norm
along the solution family, and is the smallest-norm fit known
(`||w|| = 5.5131`). Each file's `provenance` block records which. All twelve are
distinct points, not gauge copies of one another.
