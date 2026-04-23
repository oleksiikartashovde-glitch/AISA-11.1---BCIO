# MDS Graf Corpus Note A — Theoretical Core

## Scope
This file belongs to the Graf layer. It is a dense corpus-facing reference for the theoretical side of the MDS family already instantiated in the graph. Every major concept is tied directly to a graph entity. Where the concept has a formula-bearing or procedure-bearing metric realization, the relevant MET index is named inline next to the concept.

## Root framework shell
**Nonmetric Multidimensional Scaling** — `TG:FWPSY029`

`TG:FWPSY029` is the root framework shell of the MDS family. In Graf terms, this node should be read as the highest-level container for the following core commitments: objects are represented as points; interpoint distances are compared to experimentally obtained dissimilarities; the relation between distance and dissimilarity is not fixed by a prespecified formula; the central adequacy requirement is monotone fit rather than formula identity; the task is to recover a configuration rather than to assign manually interpreted coordinates in advance.

Current DREF profile of `TG:FWPSY029`:
- `BCIO:006110.00.05.ma.d`
- `BCIO:050745.00.07.ma`
- `MF:0000008.06.33.bc.be.p.ma.md.se.so.sd.d`

Within Graf, `TG:FWPSY029` should absorb the following retained framework patterns:
1. object-to-point representation,
2. distance–dissimilarity correspondence at the root shell level,
3. configuration recovery as a framework task.

It should not absorb the already separated entities `TG:THPSY198`, `TG:THPSY199`, `TG:CTPSY061`, `TG:CTPSY062`, `TG:FWPSY030`, `TG:PRPSY014`, `TG:PRPSY013`, `TG:FWPSY031`, `TG:FWPSY032`, `TG:CTPSY066`.

## Hypothesis layer
**Nonmetric Hypothesis** — `TG:THPSY198`

`TG:THPSY198` is the hypothesis node that carries the monotone-fit commitment. It should be used whenever the corpus fragment says that dissimilarities and distances must preserve rank order or be monotonically related, but need not stand in a fixed linear, polynomial, or otherwise predetermined functional relation.

Current DREF profile of `TG:THPSY198`:
- `BCIO:050745.00.07.ma`
- `BCIO:006110.00.05.ma.d`

## Fit criterion layer
**Stress as Goodness of Fit** — `TG:THPSY199`

`TG:THPSY199` is the theory-level fit criterion node. It is the correct Graf destination whenever the text is about evaluating how well a candidate configuration matches the observed dissimilarity structure.

Current DREF profile of `TG:THPSY199`:
- `IAO:0000030.08.00.bc.be.p.ma.md.se.so.sd`
- `BCIO:006110.00.05.ma.d`

The formula-bearing metric realizations of this criterion are:
- **Raw Stress** — `TG:THMET073`
- **Normalized Stress** — `TG:THMET074`

### Formula index
**Raw Stress** — `TG:THMET073`
`S* = Σ(d_ij - d_hat_ij)^2`

**Normalized Stress** — `TG:THMET074`
`S = sqrt( Σ(d_ij - d_hat_ij)^2 / Σ(d_ij)^2 )`

## Solution object layer
**Best-Fitting Configuration** — `TG:CTPSY061`

`TG:CTPSY061` is the recovered solution object. It should be used when the corpus fragment is talking about the resulting configuration of points, the minimum-stress solution, the final arrangement of objects, or the recovered point structure taken as an object of description.

Current DREF profile of `TG:CTPSY061`:
- `BCIO:006110.00.05.ma.d`
- `IAO:0000007.01.02.bc.be.p.ma.md.se.so.sd`

## Monotone fitting construct layer
**Monotone Regression of Distance upon Dissimilarity** — `TG:CTPSY062`

`TG:CTPSY062` is the graph construct for the monotone fitting step.

Current DREF profile of `TG:CTPSY062`:
- `IAO:0000030.08.00.bc.be.p.ma.md.se.so.sd`
- `BCIO:050745.00.07.ma`

Linked metric realization:
- **Monotone Regression Fitted Distance** — `TG:THMET075`

### Procedure index
**Monotone Regression Fitted Distance** — `TG:THMET075`
Fit a monotone sequence `d_hat_ij` preserving dissimilarity rank order and use fitted values for stress computation.

## Dimensionality layer
**Dimensionality Choice in Multidimensional Scaling** — `TG:FWPSY030`

`TG:FWPSY030` is the framework node for the dimension-choice problem.

Current DREF profile of `TG:FWPSY030`:
- `BCIO:006110.00.05.ma.d`
- `IAO:0000030.08.00.bc.be.p.ma.md.se.so.sd`

It should absorb:
- embedding-dimension selection shell,
- fit-versus-dimension comparison shell,
- interpretability-based dimensionality check.

### Stress-elbow principle
**Stress-Elbow Heuristic for Dimensionality Selection** — `TG:PRPSY014`

`TG:PRPSY014` is the principle node for elbow logic.

Current DREF profile of `TG:PRPSY014`:
- `IAO:0000030.08.00.bc.be.p.ma.md.se.so.sd`
- `BCIO:006110.00.05.ma.d`

## Principle layer
**Rank Order Alone Can Constrain the Solution** — `TG:PRPSY013`

`TG:PRPSY013` is the principle node for ordinal sufficiency.

Current DREF profile of `TG:PRPSY013`:
- `BCIO:050745.00.07.ma`
- `MF:0000008.06.33.bc.be.p.ma.md.se.so.sd.d`

## Internal routing matrix for Graf
- overall method, objects as points, correspondence between distances and dissimilarities → `TG:FWPSY029`
- monotonicity or no fixed formula requirement → `TG:THPSY198`
- evaluating fit or defining goodness of fit → `TG:THPSY199`
- resulting recovered solution → `TG:CTPSY061`
- monotone fitted values or monotone regression step → `TG:CTPSY062` and `TG:THMET075`
- choosing number of dimensions → `TG:FWPSY030`
- elbow-based stopping for dimensions → `TG:PRPSY014`
- ordinal sufficiency or rank-order-only constraint → `TG:PRPSY013`

## Fast inline index
`TG:FWPSY029` root framework  
`TG:THPSY198` monotone hypothesis  
`TG:THPSY199` fit criterion  
`TG:CTPSY061` best-fitting configuration  
`TG:CTPSY062` monotone regression construct  
`TG:FWPSY030` dimensionality framework  
`TG:PRPSY014` elbow principle  
`TG:PRPSY013` rank-order principle  
`TG:THMET073` raw stress  
`TG:THMET074` normalized stress  
`TG:THMET075` fitted monotone distance
