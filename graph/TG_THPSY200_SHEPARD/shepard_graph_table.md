# Shepard graph table

| Graph ID | Type | Label | Role in Graph | DREF / MET anchors |
|---|---|---|---|---|
| `TG:THPSY011` | `TH` | Universal Law of Generalization | root theory shell | `TG:CTPSY068`, `TG:THPSY200`, `TG:CTPSY069`, `TG:THPSY201`, `TG:PRPSY015` |
| `TG:THGEN002` | `TH` | Stimulus Generalization Theory | parent generalization theory shell | `TG:PRPSY015`, `TG:CTPSY068` |
| `TG:CTPSY068` | `CT` | Psychological Space | representational construct for distance-governed generalization | `BCIO:006110.00.05.ma.d`, `IAO:0000030.08.00.bc.be.p.ma.md.se.so.sd`, `MF:0000008.06.33.bc.be.p.ma.md.se.so.sd.d` |
| `TG:THPSY200` | `TH` | Exponential Decay of Generalization | law-form node for response generalization | `BCIO:006124.00.01.ma.e`, `BCIO:050962.00.01.ma.e`, `BCIO:006110.00.05.ma.d`, `TG:THMET081`, `TG:THMET082`, `TG:THMET084` |
| `TG:CTPSY069` | `CT` | Consequential Region | explanatory construct for consequential-class uncertainty | `BCIO:050618.01.04.ma`, `IAO:0000030.08.00.bc.be.p.ma.md.se.so.sd`, `MF:0000031.08.00.bc.be.p.ma.md.se.so.sd.d` |
| `TG:THPSY201` | `TH` | Probabilistic Geometry of Generalization | explanatory theory layer for region-based generalization | `BCIO:050962.00.01.ma.e`, `BCIO:006110.00.05.ma.d`, `IAO:0000030.08.00.bc.be.p.ma.md.se.so.sd` |
| `TG:PRPSY015` | `PR` | Generalization vs Failure of Discrimination | boundary principle separating two mechanisms | `BCIO:050745.00.07.ma`, `BCIO:006110.00.05.ma.d`, `IAO:0000030.08.00.bc.be.p.ma.md.se.so.sd` |

## Osgood insert

| Graph ID | Type | Label | Role in Graph | DREF / MET anchors |
|---|---|---|---|---|
| `TG:FWPSY021` | `FW` | Semantic Differential | Osgood measurement framework for bipolar scoring | `MET:0061.00.00.met`, `MET:0063.00.00.met` |
| `TG:FWPSY022` | `FW` | Semantic Space | coordinate space for semantic profile placement and semantic distance | `MET:0061.00.00.met`, `MET:0062.00.00.met` |
| `TG:DMPSY001` | `DM` | Evaluation | basic Osgood axis | `good-bad` |
| `TG:DMPSY002` | `DM` | Potency | basic Osgood axis | `strong-weak` |
| `TG:DMPSY003` | `DM` | Activity | basic Osgood axis | `active-passive` |
| `TG:CTPSY070` | `CT` | Evaluation–Potency–Activity Profile | minimal 3-axis semantic profile used by the current Shepard bridge | `MET:0061.00.00.met`, `BCIO:050745.00.07.ma`, `IAO:0000030.08.00.bc.be.p.ma.md.se.so.sd`, `MF:0000031.08.00.bc.be.p.ma.md.se.so.sd.d` |
| `TG:THMET083` | `THMET` | Osgood Distance Normalization | converts raw Osgood semantic distance into normalized Osgood distance | `MET:0062.00.00.met`, `BCIO:050745.00.07.ma`, `IAO:0000030.08.00.bc.be.p.ma.md.se.so.sd` |
| `TG:THMET084` | `THMET` | Osgood-Calibrated Alpha | converts normalized Osgood distance into bounded Shepard alpha | `MET:0062.00.00.met`, `MET:0051.02.00.met`, `BCIO:050745.00.07.ma` |

## MET layer

| MET ID | Label | Role | Formula / procedure index |
|---|---|---|---|
| `TG:THMET081` | Shepard Generalization Function | metric / computational rule | `g(d) = exp(-alpha_raw_final * d_norm)` |
| `TG:THMET082` | Alpha Estimation from Generalization and Distance | metric / computational rule | `alpha_raw = -ln(g) / d` or fit `ln(g_i) = -alpha_raw * d_i` |
| `TG:THMET083` | Osgood Distance Normalization | metric / computational rule | `D_norm = D_raw / 10.3923048454` |
| `TG:THMET084` | Osgood-Calibrated Alpha | metric / computational rule | `alpha = 0.01 + 6.8978 * D_norm` |
| `MET:0051.00.00.met` | Shepard-type transfer function | external MET anchor reused by runtime | family anchor |
| `MET:0051.01.00.met` | distance input | external MET anchor reused by runtime | distance component |
| `MET:0051.02.00.met` | alpha parameter | external MET anchor reused by runtime | alpha component |
| `MET:0051.03.00.met` | transfer function output | external MET anchor reused by runtime | output component |
| `MET:0061.00.00.met` | semantic differential profile allocation | Osgood profile placement in semantic space | factor-score and placement anchor |
| `MET:0062.00.00.met` | semantic meaning distance | Euclidean distance in semantic space | raw semantic distance anchor |
| `MET:0063.00.00.met` | semantic differential significance thresholds | threshold layer for semantic differential interpretation | significance anchor |

## Operational bridge

| Bridge block | Current operational role |
|---|---|
| Osgood profile | score each object on `good-bad`, `strong-weak`, `active-passive` |
| Osgood raw distance | compute `D_raw = sqrt((e1-e2)^2 + (p1-p2)^2 + (a1-a2)^2)` |
| Osgood normalized distance | compute `D_norm = D_raw / 10.3923048454` |
| Osgood-calibrated alpha | compute `alpha = 0.01 + 6.8978 * D_norm` |
| Shepard output | compute `g(d) = exp(-alpha_raw_final * d_norm)` |

## Runtime note

| Layer | Current Shepard support |
|---|---|
| Signal | `REQUEST_SIGNAL_GATEWAY.json` updated for Shepard and Osgood requests |
| Request pattern | `REFERENCE_RequestPatterns.json` updated for Shepard, Osgood, and Osgood-plus-Shepard compute routes |
| Semantic matching | `T_SEMANTIC_MATCHING_CORE_with_neighborhoods.json` updated with Shepard and Osgood aliases |
| Execution | `EXECUTION_CORE.json` updated for descriptor → profile → distance → alpha → g(d)` |
| Control | `OPERATIONAL_CONTROL_LAYER.json` updated with Shepard and Osgood runtime constraints |
| Dependencies | `MODULE_DEPENDENCY_GRAPH.json` updated for Osgood and Shepard execution path |
| Index | `bcio_index.json`, `bcio_by_module.json`, `bcio_hierarchy_index.json`, `bcio_clean.json` synced |
