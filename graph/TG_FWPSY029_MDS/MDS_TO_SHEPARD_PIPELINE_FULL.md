# MDS_TO_SHEPARD_PIPELINE_FULL

## SECTION 1. PURPOSE
Convert pairwise dissimilarities between objects into:
1. Object coordinates
2. Recovered pairwise distances
3. Fit diagnostics
4. Shepard-stage similarity outputs

## SECTION 2. SCOPE
Applies only to:
- Multidimensional Scaling (MDS)
- Shepard-stage distance use

Does **not** define external theories of meaning, category, or action.

## SECTION 3. CORE ENTITIES

### OBJECT
Any analysis unit with pairwise comparisons.

### OBJECT_TYPE
`OBJECT_TYPE(oi) ∈ {TEXT, IMAGE, FIGURE, LABEL, MIXED}`

### PAIR_TYPE
`PAIR_TYPE(oi, oj) = OBJECT_TYPE(oi) + "-" + OBJECT_TYPE(oj)`

### CROSS_MODAL_PAIR
`{TEXT-IMAGE, IMAGE-TEXT, TEXT-FIGURE, FIGURE-TEXT, IMAGE-LABEL, LABEL-IMAGE, FIGURE-LABEL, LABEL-FIGURE, MIXED-anything, anything-MIXED}`

## SECTION 4. INPUTS

- `OBJECTS`
- `OBJECT_TYPE(oi)`
- `DISSIMILARITY_SOURCE`
- `DISSIMILARITY_SOURCE_TYPE ∈ {MATRIX, PAIR_TABLE, ORDINAL_PAIRS, FUNCTION}`
- `TARGET_DIMENSIONS = t`
- `SHARED_SPACE_FLAG`
- `COORDINATE_STRUCTURE ∈ {HOLISTIC_CONTINUOUS, ADDITIVE_FEATURES, UNCERTAIN}`
- `INITIALIZATION_MODE ∈ {RANDOM, PCA_INIT}`
- `MISSING_DATA_MODE ∈ {STOP, IGNORE_OBSERVED_ONLY}`
- `MDS_MODE ∈ {METRIC, NONMETRIC}`
- `STOPPING_RULES`
- `SHEPARD_RULE`

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
Order relations only, for example:
- `δab < δac`

Allowed only in `NONMETRIC` mode.

### FORMAT D. FUNCTION
Deterministic rule computing `δij` from `(oi, oj)`.

## SECTION 6. OBJECT REGISTRATION
- assign unique `object_id`
- assign explicit `object_type`
- no implicit type inference

## SECTION 7. BUILD DISSIMILARITY STRUCTURE
- obtain `δij`
- enforce `δii = 0`
- enforce `δij = δji`
- pair tables are mirrored only
- ordinal pairs preserve order only

## SECTION 8. INPUT VALIDATION
- matrix must be square, symmetric, zero diagonal
- if missing values and `MISSING_DATA_MODE = STOP` → stop
- if missing values and `MISSING_DATA_MODE = IGNORE_OBSERVED_ONLY` → restrict computations to observed pairs
- no automatic imputation

## SECTION 9. MODALITY CHECK
If a pair is cross-modal and `SHARED_SPACE_FLAG = FALSE` → stop.

## SECTION 10. CHOOSE DISTANCE METRIC

### Metric rule
- `HOLISTIC_CONTINUOUS` → `EUCLIDEAN`
- `ADDITIVE_FEATURES` → `CITY_BLOCK`
- `UNCERTAIN` → test `MINKOWSKI` family and choose by lowest normalized stress

### Distance definitions
- **EUCLIDEAN**: `dij = sqrt( Σl (xil - xjl)^2 )`
- **CITY_BLOCK**: `dij = Σl |xil - xjl|`
- **MINKOWSKI**: `dij = ( Σl |xil - xjl|^r )^(1/r)`

## SECTION 11. INITIALIZATION
- `RANDOM` → random coordinates in `t` dimensions
- `PCA_INIT` → allowed only if interval approximation support exists

## SECTION 12. ITERATIVE MDS LOOP
1. Compute coordinates `xi`
2. Compute distances `dij`
3. If `NONMETRIC`, fit monotone distances `d_hat_ij`
4. Compute raw stress `S*`
5. Compute normalized stress `S`
6. Update coordinates to minimize `S`
7. Repeat until stopping rules are met

## SECTION 13. FIT FUNCTIONS
- **Raw stress**: `S* = Σij (dij - d_hat_ij)^2`
- **Normalized stress**: `S = sqrt( Σij (dij - d_hat_ij)^2 / Σij dij^2 )`

If missing-data mode is `IGNORE_OBSERVED_ONLY`, all sums are restricted to observed pairs only.

## SECTION 14. STOPPING CRITERIA
Stop if any of the following is true:
- `iteration_count >= max_iterations`
- `normalized_stress <= stress_threshold`
- `relative_change_in_stress <= relative_stress_change_threshold`

## SECTION 15. DISTANCE NORMALIZATION
Raw and normalized distances are distinct outputs.

### Normalization rule
`d_norm(i,j) = d_ij / d_max`

where:
- `d_ij` = recovered raw distance
- `d_max` = maximum recovered distance in the current analysis

Interpretation:
- `0` = coincidence / zero distance
- `1` = maximum distance in current analysis

## SECTION 16. OUTPUT TABLES

### COORDINATES_TABLE
`object_id | object_type | dim_1 | dim_2 | ... | dim_t`

### DISTANCE_TABLE
`object_a | object_b | pair_type | input_dissimilarity | recovered_distance_raw | recovered_distance_norm | abs_difference_raw | abs_difference_norm`

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
`object_a | object_b | recovered_distance_raw | recovered_distance_norm | shepard_similarity`

## SECTION 17. SHEPARD STAGE
Use recovered distance as Shepard-stage input.

Rule:
- Shepard stage must not modify recovered MDS distances
- it only consumes them

## SECTION 18. VALIDITY RULES

### VALID
- pairwise dissimilarities exist
- matrix is symmetric
- cross-modal pairs have shared space
- all rules are explicit

### INVALID
- no dissimilarity source
- asymmetric matrix
- cross-modal pair without shared space
- hidden imputation
- hidden metric switching

## SECTION 19. FORBIDDEN ACTIONS
- inventing missing dissimilarities
- silently ignoring invalid cross-modal pairs
- mixing metric families without explicit rule
- treating coordinates as semantically interpreted by default

## SECTION 20. FINAL OUTPUT
- `COORDINATES_TABLE`
- `DISTANCE_TABLE`
- `FIT_TABLE`
- `SHEPARD_TABLE`
