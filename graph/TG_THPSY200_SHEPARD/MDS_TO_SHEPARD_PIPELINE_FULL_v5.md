# MDS_TO_SHEPARD_PIPELINE_FULL v5

## SECTION 1. PURPOSE
Convert pairwise dissimilarities, scale-specific distances, recovered MDS distances, or Osgood-based semantic profiles into:
1. object coordinates
2. recovered pairwise distances
3. fit diagnostics
4. Osgood semantic profiles
5. Osgood semantic distances
6. optional multi-scale distance profiles
7. optional aggregated distance outputs
8. alpha estimation, calibration, bounding, and normalization outputs
9. Shepard-stage similarity outputs

## SECTION 2. SCOPE
Applies only to:
- Multidimensional Scaling (MDS)
- Osgood semantic differential suboperation
- Shepard-stage distance use
- optional multi-scale distance profiling with optional aggregation
- Osgood-based alpha calibration for Shepard-stage computation

## SECTION 3. GRAPH ENTITIES USED
Existing graph base:
- `TG:FWPSY021` — Semantic Differential
- `TG:FWPSY022` — Semantic Space
- `TG:DMPSY001` — Evaluation
- `TG:DMPSY002` — Potency
- `TG:DMPSY003` — Activity
- `TG:THPSY200` — Exponential Decay of Generalization
- `TG:THMET081` — Shepard Generalization Function
- `TG:THMET082` — Alpha Estimation from Generalization and Distance

New Osgood → Shepard bridge entities:
- `TG:CTPSY070` — Evaluation–Potency–Activity Profile
- `TG:THMET083` — Osgood Distance Normalization
- `TG:THMET084` — Osgood-Calibrated Alpha

## SECTION 4. INPUTS
- `OBJECTS`
- `OBJECT_TYPE(oi) ∈ {TEXT, IMAGE, FIGURE, LABEL, LOGO, MIXED}`
- `DISSIMILARITY_SOURCE`
- `DISSIMILARITY_SOURCE_TYPE ∈ {MATRIX, PAIR_TABLE, ORDINAL_PAIRS, FUNCTION, OSGOOD_PROFILE}`
- `TARGET_DIMENSIONS = t`
- `SHARED_SPACE_FLAG`
- `COORDINATE_STRUCTURE ∈ {HOLISTIC_CONTINUOUS, ADDITIVE_FEATURES, UNCERTAIN}`
- `INITIALIZATION_MODE ∈ {RANDOM, PCA_INIT}`
- `MISSING_DATA_MODE ∈ {STOP, IGNORE_OBSERVED_ONLY}`
- `MDS_MODE ∈ {METRIC, NONMETRIC}`
- `STOPPING_RULES`
- `DISTANCE_BY_SCALE_MODE ∈ {DISABLED, ENABLED}`
- `SCALES`
- `AGGREGATION_MODE ∈ {NONE, MEAN, WEIGHTED_MEAN, MINKOWSKI, MAX}`
- `SHEPARD_DISTANCE_INPUT_MODE ∈ {SCALE_DISTANCE, AGGREGATED_DISTANCE, MDS_RECOVERED_DISTANCE, OSGOOD_DISTANCE}`
- `ALPHA_ESTIMATION_MODE ∈ {OSGOOD_CALIBRATED, SINGLE_PAIR, MULTI_PAIR_FIT, EXPLICIT_ONLY}`
- `ALPHA_BOUND_MODE ∈ {HARD_BOUNDED, ESTIMATE_THEN_CLIP}`
- `ALPHA_RANGE_RAW = [0.01, 6.9078]`
- `ALPHA_RANGE_NORM = [0,1]`
- `OSGOOD_INPUT_MODE ∈ {AUTO_DESCRIPTION, USER_TEXT, HYBRID}`
- `OSGOOD_PROFILE_MODE ∈ {BASIC_3_AXIS}`

## SECTION 5. REQUIRED INPUT FORMATS

### FORMAT A. MATRIX
Square symmetric matrix `Δ`:
- `δii = 0`
- `δij = δji`

### FORMAT B. PAIR_TABLE
Columns:
- `object_a`
- `object_b`
- `dissimilarity`

### FORMAT C. ORDINAL_PAIRS
Order relations only.

Allowed only in `NONMETRIC` mode.

### FORMAT D. FUNCTION
Deterministic rule computing `δij` from `(oi, oj)`.

### FORMAT E. SCALES
If `DISTANCE_BY_SCALE_MODE = ENABLED`, each scale must be explicitly declared with:
- `name`
- `definition`
- `normalization_rule`
- `metric`
- `weight`

No hidden scale inference is allowed.

### FORMAT F. OSGOOD_PROFILE
Minimal Osgood core uses three basic axes:
- `good-bad`
- `strong-weak`
- `active-passive`

Each axis is scored in:
- `[-3, +3]`

For image / figure / logo input:
- semantic description may be generated first
- then the object is scored on the three axes
- user corrections may refine the semantic description before scoring

## SECTION 6. INPUT VALIDATION
- matrix must be square, symmetric, zero diagonal
- if missing values and `MISSING_DATA_MODE = STOP` → stop
- if missing values and `MISSING_DATA_MODE = IGNORE_OBSERVED_ONLY` → restrict computations to observed pairs only
- no automatic imputation

If `DISTANCE_BY_SCALE_MODE = ENABLED`:
- every declared scale must have explicit normalization rule
- every declared scale must have explicit metric
- weights are required for `WEIGHTED_MEAN` and `MINKOWSKI`
- user-defined scales are allowed and must remain visible in outputs

If `OSGOOD_PROFILE_MODE = BASIC_3_AXIS`:
- each object must receive three scores
- each score must lie in `[-3, +3]`
- auto-description must remain auditable via descriptor output if requested

## SECTION 7. DISTANCE METRICS
- `HOLISTIC_CONTINUOUS` → `EUCLIDEAN`
- `ADDITIVE_FEATURES` → `CITY_BLOCK`
- `UNCERTAIN` → test `MINKOWSKI` family and choose by lowest normalized stress

Definitions:
- **EUCLIDEAN**: `dij = sqrt( Σl (xil - xjl)^2 )`
- **CITY_BLOCK**: `dij = Σl |xil - xjl|`
- **MINKOWSKI**: `dij = ( Σl |xil - xjl|^r )^(1/r)`

If `DISTANCE_BY_SCALE_MODE = ENABLED`, metric selection is applied scale-by-scale.

## SECTION 8. SCALE DISTANCE PROFILE
If `DISTANCE_BY_SCALE_MODE = ENABLED`, compute scale-specific distances before optional aggregation.

For each pair `(oi, oj)` and each scale `k`:
- compute `distance_raw_k`
- normalize to `distance_norm_k ∈ [0,1]`

This output is distinct from recovered MDS distance.

Scale families may include, but are not limited to:
- stimulus
- skill
- action
- procedure
- task
- goal
- context
- condition
- environment
- user-defined custom scales

## SECTION 9. OPTIONAL AGGREGATION
- **NONE**: no aggregation
- **MEAN**: `d_total = (1/n) Σ d_k`
- **WEIGHTED_MEAN**: `d_total = Σ w_k d_k`, with `Σ w_k = 1`
- **MINKOWSKI**: `d_total = ( Σ (w_k d_k)^r )^(1/r)`
- **MAX**: `d_total = max(d_1, d_2, ..., d_n)`

Aggregation is optional.
Scale outputs must remain visible even when aggregation is used.

## SECTION 10. ITERATIVE MDS LOOP
1. compute coordinates `xi`
2. compute distances `dij`
3. if `NONMETRIC`, fit monotone distances `d_hat_ij`
4. compute raw stress `S*`
5. compute normalized stress `S`
6. update coordinates to minimize `S`
7. repeat until stopping rules are met

## SECTION 11. FIT FUNCTIONS
- **Raw stress**: `S* = Σij (dij - d_hat_ij)^2`
- **Normalized stress**: `S = sqrt( Σij (dij - d_hat_ij)^2 / Σij dij^2 )`

If missing-data mode is `IGNORE_OBSERVED_ONLY`, all sums are restricted to observed pairs only.

## SECTION 12. DISTANCE NORMALIZATION
Recovered MDS distance normalization:
`d_norm(i,j) = d_ij / d_max`

Interpretation:
- `0` = coincidence / zero distance
- `1` = maximum distance in current analysis

Scale-profile and aggregated distances may also be normalized into `[0,1]`.

## SECTION 13. OSGOOD SUBOPERATION
Graph mapping:
- `TG:FWPSY021` — Semantic Differential
- `TG:FWPSY022` — Semantic Space
- `TG:CTPSY070` — Evaluation–Potency–Activity Profile
- `TG:THMET083` — Osgood Distance Normalization

### BASIC OSGOOD AXES
- `evaluation = good-bad` → `TG:DMPSY001`
- `potency = strong-weak` → `TG:DMPSY002`
- `activity = active-passive` → `TG:DMPSY003`

### BASIC OSGOOD SCORE RANGE
For each axis:
`score ∈ [-3, +3]`

### OSGOOD INPUT HANDLING
For each object:
- if object type is `TEXT`, score directly from textual content
- if object type is `IMAGE`, `FIGURE`, or `LOGO`, first build an automatic semantic description
- if user provides corrections, update the semantic description
- score the final semantic representation on the three Osgood axes

### OSGOOD COORDINATES
For each object:
`x = (e, p, a)`

where:
- `e` = evaluation score
- `p` = potency score
- `a` = activity score

### OSGOOD RAW DISTANCE
For a pair of objects:
`D_raw = sqrt((e1-e2)^2 + (p1-p2)^2 + (a1-a2)^2)`

### OSGOOD DISTANCE CORRIDOR
- `D_min = 0`
- `D_max = 6 * sqrt(3) ≈ 10.39`

Justification:
- each axis spans from `-3` to `+3`
- maximum per-axis gap = `6`
- three-axis Euclidean maximum = `sqrt(6^2 + 6^2 + 6^2) = 6*sqrt(3) ≈ 10.39`

### OSGOOD DISTANCE NORMALIZATION
`D_norm = D_raw / 10.3923048454`

Interpretation:
- `D_norm = 0` → identical Osgood profile
- `D_norm = 1` → maximum possible Osgood distance under the 3-axis basic scheme

## SECTION 14. ALPHA ESTIMATION AND CALIBRATION
Graph mapping:
- `TG:THMET084` — Osgood-Calibrated Alpha
- `TG:THMET082` — Alpha Estimation from Generalization and Distance

### OSGOOD_CALIBRATED
Use normalized Osgood distance as the direct alpha driver.

Alpha corridor:
- `alpha_min = 0.01`
- `alpha_max = 6.9078`
- `delta_alpha = 6.8978`

Calibration rule:
`alpha_raw_final = 0.01 + 6.8978 * D_norm`

Corridor justification at `d = 1`:
- if `alpha = 0.01` → `g(1) = exp(-0.01) ≈ 0.99`
- if `alpha = 6.9078` → `g(1) = exp(-6.9078) ≈ 0.001`

Interpretation:
- lower alpha bound preserves near-full transfer at maximal normalized distance
- upper alpha bound pushes transfer close to zero at maximal normalized distance

### SINGLE_PAIR
`alpha_raw_estimated = -ln(g) / d`

Constraints:
- `d > 0`
- `0 < g <= 1`

### MULTI_PAIR_FIT
Fit:
`ln(g_i) = -alpha_raw_estimated * d_i`

Then:
`alpha_raw_estimated = -slope`

### EXPLICIT_ONLY
Use user-provided alpha and skip estimation.

### BOUNDING
If `ALPHA_BOUND_MODE = HARD_BOUNDED`, estimate or calibrate `alpha_raw` only within `[0.01, 6.9078]`.

If `ALPHA_BOUND_MODE = ESTIMATE_THEN_CLIP`, apply:
`alpha_raw_final = max(0.01, min(alpha_raw_estimated, 6.9078))`

### NORMALIZATION
`alpha_norm = (alpha_raw_final - 0.01) / 6.8978`

### DENORMALIZATION
`alpha_raw_final = 0.01 + 6.8978 * alpha_norm`

### ALPHA STATUS
`alpha_status ∈ {in_range, clipped_low, clipped_high}`

## SECTION 15. SHEPARD STAGE
Graph mapping:
- `TG:THPSY200` — Exponential Decay of Generalization
- `TG:THMET081` — Shepard Generalization Function

Allowed `SHEPARD_DISTANCE_INPUT_MODE`:
- `SCALE_DISTANCE`
- `AGGREGATED_DISTANCE`
- `MDS_RECOVERED_DISTANCE`
- `OSGOOD_DISTANCE`

Shepard function:
`g(d) = exp(-alpha_raw_final * d_norm)`

Rule:
- Shepard stage consumes the chosen distance input mode
- Shepard stage must not modify recovered MDS distances
- psychological distance may be composite and may include skill-, task-, procedure-, goal-, context-, or condition-related scales
- when `OSGOOD_DISTANCE` is selected, `d_norm = D_norm`

## SECTION 16. OUTPUT TABLES

### COORDINATES_TABLE
`object_id | object_type | dim_1 | dim_2 | ... | dim_t`

### DISTANCE_TABLE
`object_a | object_b | pair_type | input_dissimilarity | recovered_distance_raw | recovered_distance_norm | abs_difference_raw | abs_difference_norm`

### DISTANCE_PROFILE_TABLE
`object_a | object_b | scale | distance_raw | distance_norm`

### AGGREGATED_DISTANCE_TABLE
`object_a | object_b | aggregation_mode | aggregated_distance_raw | aggregated_distance_norm`

### OSGOOD_DESCRIPTOR_TABLE
`object_id | object_type | auto_description | user_corrections_applied | final_semantic_descriptor`

### OSGOOD_PROFILE_TABLE
`object_id | evaluation_score | potency_score | activity_score`

### OSGOOD_DISTANCE_TABLE
`object_a | object_b | osgood_distance_raw | osgood_distance_norm`

### ALPHA_TABLE
`object_a | object_b | alpha_estimation_mode | alpha_bound_mode | alpha_raw_estimated | alpha_raw_final | alpha_norm | alpha_status`

### FIT_TABLE
- `raw_stress`
- `normalized_stress`
- `iterations`
- `selected_metric`
- `selected_dimensions`
- `initialization_mode`
- `missing_data_mode`
- `mds_mode`

### SHEPARD_TABLE
`object_a | object_b | distance_input_mode | scale_or_aggregate_label | distance | alpha_raw_final | alpha_norm | shepard_similarity`

## SECTION 17. VALIDITY RULES

### VALID
- pairwise dissimilarities exist
- matrix is symmetric
- cross-modal pairs have shared space
- all rules are explicit
- alpha remains within the defined raw range after bounding
- Osgood scores remain within the declared scale corridor
- Osgood distance remains within `[0, 10.39]`
- normalized Osgood distance remains within `[0,1]`

### INVALID
- no dissimilarity source
- asymmetric matrix
- cross-modal pair without shared space
- hidden imputation
- hidden metric switching
- hidden scale normalization
- hidden aggregation
- hidden alpha bounding
- hidden Osgood rescaling
- hidden descriptor overwrite

## SECTION 18. FINAL OUTPUT
- `COORDINATES_TABLE`
- `DISTANCE_TABLE`
- `DISTANCE_PROFILE_TABLE`
- `AGGREGATED_DISTANCE_TABLE`
- `OSGOOD_DESCRIPTOR_TABLE`
- `OSGOOD_PROFILE_TABLE`
- `OSGOOD_DISTANCE_TABLE`
- `ALPHA_TABLE`
- `FIT_TABLE`
- `SHEPARD_TABLE`
