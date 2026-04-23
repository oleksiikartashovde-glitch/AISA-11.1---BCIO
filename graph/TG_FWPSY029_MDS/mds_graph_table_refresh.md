# MDS graph table

| Graph ID | Type | Label | Role in Graf | DREF / MET anchors |
|---|---|---|---|---|
| `TG:FWPSY029` | `FW` | Nonmetric Multidimensional Scaling | root framework shell | `BCIO:006110.00.05.ma.d`, `BCIO:050745.00.07.ma`, `MF:0000008.06.33.bc.be.p.ma.md.se.so.sd.d` |
| `TG:THPSY198` | `TH` | Nonmetric Hypothesis | hypothesis node | `BCIO:050745.00.07.ma`, `BCIO:006110.00.05.ma.d` |
| `TG:THPSY199` | `TH` | Stress as Goodness of Fit | fit criterion node | `IAO:0000030.08.00.bc.be.p.ma.md.se.so.sd`, `BCIO:006110.00.05.ma.d` |
| `TG:CTPSY061` | `CT` | Best-Fitting Configuration | solution object | `BCIO:006110.00.05.ma.d`, `IAO:0000007.01.02.bc.be.p.ma.md.se.so.sd` |
| `TG:CTPSY062` | `CT` | Monotone Regression of Distance upon Dissimilarity | monotone regression construct | `IAO:0000030.08.00.bc.be.p.ma.md.se.so.sd`, `BCIO:050745.00.07.ma` |
| `TG:FWPSY030` | `FW` | Dimensionality Choice in Multidimensional Scaling | dimensionality choice framework | `BCIO:006110.00.05.ma.d`, `IAO:0000030.08.00.bc.be.p.ma.md.se.so.sd` |
| `TG:PRPSY014` | `PR` | Stress-Elbow Heuristic for Dimensionality Selection | dimension elbow heuristic | `IAO:0000030.08.00.bc.be.p.ma.md.se.so.sd`, `BCIO:006110.00.05.ma.d` |
| `TG:FWPSY032` | `FW` | Minkowski r-Metric in Multidimensional Scaling | metric-family framework | `BCIO:050745.00.07.ma`, `IAO:0000030.08.00.bc.be.p.ma.md.se.so.sd` |
| `TG:CTPSY067` | `CT` | City-Block Metric in Multidimensional Scaling | city-block special-case construct | `BCIO:050745.00.07.ma`, `BCIO:006110.00.05.ma.d` |
| `TG:CTPSY066` | `CT` | Missing Dissimilarity Handling in Nonmetric MDS | missing-data handling construct | `BCIO:050745.00.07.ma`, `IAO:0000030.08.00.bc.be.p.ma.md.se.so.sd` |
| `TG:FWPSY031` | `FW` | Method of Steepest Descent | optimization method framework | `MF:0000008.06.33.bc.be.p.ma.md.se.so.sd.d`, `IAO:0000030.08.00.bc.be.p.ma.md.se.so.sd` |
| `TG:CTPSY063` | `CT` | Configuration Space | configuration-space construct | `BCIO:006110.00.05.ma.d`, `IAO:0000030.08.00.bc.be.p.ma.md.se.so.sd` |
| `TG:CTPSY065` | `CT` | Local Minimum in Stress Optimization | local minimum construct | `BCIO:006110.00.05.ma.d`, `IAO:0000030.08.00.bc.be.p.ma.md.se.so.sd` |
| `TG:PRPSY013` | `PR` | Rank Order Alone Can Constrain the Solution | rank-order principle | `BCIO:050745.00.07.ma`, `MF:0000008.06.33.bc.be.p.ma.md.se.so.sd.d` |

## MET layer

| MET ID | Label | Role | Formula / procedure index |
|---|---|---|---|
| `TG:THMET072` | Nonmetric Multidimensional Scaling Metrics | metric family | family node |
| `TG:THMET073` | Raw Stress | metric / computational rule | `S* = Σ(d_ij - d_hat_ij)^2` |
| `TG:THMET074` | Normalized Stress | metric / computational rule | `S = sqrt( Σ(d_ij - d_hat_ij)^2 / Σ(d_ij)^2 )` |
| `TG:THMET075` | Monotone Regression Fitted Distance | metric / computational rule | monotone fitted `d_hat_ij` procedure |
| `TG:THMET076` | Minkowski R-Metric Distance | metric / computational rule | `d_ij = ( Σ |x_il - x_jl|^r )^(1/r)` |
| `TG:THMET077` | Euclidean Distance In MDS | metric / computational rule | `d_ij = sqrt( Σ (x_il - x_jl)^2 )` |
| `TG:THMET078` | City-Block Distance In MDS | metric / computational rule | `d_ij = Σ |x_il - x_jl|` |
| `TG:THMET079` | Gradient Update For Stress Minimization | metric / computational rule | negative-gradient coordinate update |
| `TG:THMET080` | Step-Size Update Rule | metric / computational rule | angle / relaxation / good-luck step update |
