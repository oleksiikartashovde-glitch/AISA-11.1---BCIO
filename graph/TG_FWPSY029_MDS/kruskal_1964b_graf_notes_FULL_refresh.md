# MDS Graf Corpus Note B — Computational Core

## Scope
This file belongs to the Graf layer and is written as a dense corpus-facing reference for the computational and metric side of the MDS family. All major computational constructs are tied to their graph IDs, and all major formulas or procedures are tied to their MET IDs inline.

## Optimization method framework
**Method of Steepest Descent** — `TG:FWPSY031`

`TG:FWPSY031` is the framework node for the iterative numerical method used to reduce stress.

Current DREF profile of `TG:FWPSY031`:
- `MF:0000008.06.33.bc.be.p.ma.md.se.so.sd.d`
- `IAO:0000030.08.00.bc.be.p.ma.md.se.so.sd`

The MET nodes directly supporting this framework are:
- **Gradient Update For Stress Minimization** — `TG:THMET079`
- **Step-Size Update Rule** — `TG:THMET080`

### Procedure index
**Gradient Update For Stress Minimization** — `TG:THMET079`
Use the negative gradient of stress and update coordinates iteratively in configuration space.

**Step-Size Update Rule** — `TG:THMET080`
Update step size from previous step size using angle factor, relaxation factor, and good luck factor.

## Space layer
**Configuration Space** — `TG:CTPSY063`

`TG:CTPSY063` is the construct node for configuration space.

Current DREF profile of `TG:CTPSY063`:
- `BCIO:006110.00.05.ma.d`
- `IAO:0000030.08.00.bc.be.p.ma.md.se.so.sd`

### Local minimum state
**Local Minimum in Stress Optimization** — `TG:CTPSY065`

`TG:CTPSY065` is the construct node for the local-minimum state and stopping-state logic.

Current DREF profile of `TG:CTPSY065`:
- `BCIO:006110.00.05.ma.d`
- `IAO:0000030.08.00.bc.be.p.ma.md.se.so.sd`

## Missing-data handling layer
**Missing Dissimilarity Handling in Nonmetric MDS** — `TG:CTPSY066`

`TG:CTPSY066` is the graph construct for missing-data handling.

Current DREF profile of `TG:CTPSY066`:
- `BCIO:050745.00.07.ma`
- `IAO:0000030.08.00.bc.be.p.ma.md.se.so.sd`

## Metric-family framework
**Minkowski r-Metric in Multidimensional Scaling** — `TG:FWPSY032`

`TG:FWPSY032` is the framework node for the family of distance metrics beyond the default Euclidean case.

Current DREF profile of `TG:FWPSY032`:
- `BCIO:050745.00.07.ma`
- `IAO:0000030.08.00.bc.be.p.ma.md.se.so.sd`

### City-block special case
**City-Block Metric in Multidimensional Scaling** — `TG:CTPSY067`

`TG:CTPSY067` is the construct node for the city-block special case within the Minkowski family.

Current DREF profile of `TG:CTPSY067`:
- `BCIO:050745.00.07.ma`
- `BCIO:006110.00.05.ma.d`

## MET family root
**Nonmetric Multidimensional Scaling Metrics** — `TG:THMET072`

The family currently contains:
- `TG:THMET073` — Raw Stress
- `TG:THMET074` — Normalized Stress
- `TG:THMET075` — Monotone Regression Fitted Distance
- `TG:THMET076` — Minkowski R-Metric Distance
- `TG:THMET077` — Euclidean Distance In MDS
- `TG:THMET078` — City-Block Distance In MDS
- `TG:THMET079` — Gradient Update For Stress Minimization
- `TG:THMET080` — Step-Size Update Rule

## Distance formulas
### Minkowski R-Metric Distance — `TG:THMET076`
`d_ij = ( Σ |x_il - x_jl|^r )^(1/r)`

### Euclidean Distance In MDS — `TG:THMET077`
`d_ij = sqrt( Σ (x_il - x_jl)^2 )`

### City-Block Distance In MDS — `TG:THMET078`
`d_ij = Σ |x_il - x_jl|`

## Stress formulas
### Raw Stress — `TG:THMET073`
`S* = Σ(d_ij - d_hat_ij)^2`

### Normalized Stress — `TG:THMET074`
`S = sqrt( Σ(d_ij - d_hat_ij)^2 / Σ(d_ij)^2 )`

## Fitted-distance procedure
### Monotone Regression Fitted Distance — `TG:THMET075`
Fit a monotone sequence `d_hat_ij` preserving dissimilarity rank order and use fitted values for stress computation.

## Dense routing matrix for computational passages
- gradient method, steepest descent, iterative improvement → `TG:FWPSY031`
- configuration as a point in nt-dimensional search space → `TG:CTPSY063`
- local minima, no-small-movement improvement state, local minimum criterion → `TG:CTPSY065`
- missing entries, half-matrices, observed-pairs-only sums → `TG:CTPSY066`
- Minkowski family, r-metric generalization, metric choice across r values → `TG:FWPSY032`
- city-block / Manhattan special case → `TG:CTPSY067`
- general MDS metric family reference → `TG:THMET072`
- raw stress formula → `TG:THMET073`
- normalized stress formula → `TG:THMET074`
- monotone fitted distance procedure → `TG:THMET075`
- Minkowski formula → `TG:THMET076`
- Euclidean formula → `TG:THMET077`
- city-block formula → `TG:THMET078`
- gradient update rule → `TG:THMET079`
- step-size update rule → `TG:THMET080`
