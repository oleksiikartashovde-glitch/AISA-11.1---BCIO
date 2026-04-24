# Shepard Prompts Pack — User Version

## Purpose
This file contains clean user-facing English prompts for:
- Osgood semantic differential scoring
- Osgood semantic distance
- alpha calculation
- Shepard transfer/generalization value `g(d)`

These prompts do not expose internal package structure, graph IDs, or runtime files.

Fixed assumptions used in this prompt pack:
- Osgood basic axes:
  - good-bad
  - strong-weak
  - active-passive
- Osgood score range:
  - [-3, +3]
- Osgood raw distance:
  - `D_raw = sqrt((e1-e2)^2 + (p1-p2)^2 + (a1-a2)^2)`
- Osgood normalized distance:
  - `D_norm = D_raw / 10.3923048454`
- Alpha corridor:
  - `[0.01, 6.9078]`
- Alpha rule:
  - `alpha = 0.01 + 6.8978 * D_norm`
- Shepard function:
  - `g(d) = exp(-alpha * d_norm)`

---

## Prompt 1. Full Osgood → Shepard Computation

```text
Run the full Osgood to Shepard computation for the given pair.

Task:
1. build semantic descriptors if needed
2. score each object on the three Osgood axes
3. compute raw Osgood distance
4. normalize Osgood distance
5. compute alpha
6. compute Shepard transfer/generalization value g(d)

Use the fixed Osgood axes:
- good-bad
- strong-weak
- active-passive

Use the fixed Osgood score range:
- [-3, +3]

Use:
- D_raw = sqrt((e1-e2)^2 + (p1-p2)^2 + (a1-a2)^2)
- D_norm = D_raw / 10.3923048454
- alpha = 0.01 + 6.8978 * D_norm
- g(d) = exp(-alpha * d_norm)

Required outputs:
1. OSGOOD_DESCRIPTOR_TABLE
2. OSGOOD_PROFILE_TABLE
3. OSGOOD_DISTANCE_TABLE
4. ALPHA_TABLE
5. SHEPARD_TABLE

OSGOOD_DESCRIPTOR_TABLE columns:
- object_id
- object_type
- auto_description
- user_corrections_applied
- final_semantic_descriptor

OSGOOD_PROFILE_TABLE columns:
- object_id
- evaluation_score
- potency_score
- activity_score

OSGOOD_DISTANCE_TABLE columns:
- object_a
- object_b
- osgood_distance_raw
- osgood_distance_norm

ALPHA_TABLE columns:
- object_a
- object_b
- alpha_raw_final
- alpha_norm

SHEPARD_TABLE columns:
- object_a
- object_b
- distance
- alpha_raw_final
- alpha_norm
- shepard_similarity

Do not invent extra scales.
Do not use a different alpha corridor.
```

---

## Prompt 2. Pair Prompt

```text
Run the Osgood to Shepard route for one pair of objects.

Input objects:
[PASTE OBJECT A]
[PASTE OBJECT B]

Required logic:
- if the input is text, score directly
- if the input is image, figure, or logo, first generate an automatic semantic description
- apply user corrections only if they are explicitly provided
- score on:
  - good-bad
  - strong-weak
  - active-passive
- compute D_raw
- compute D_norm
- compute alpha = 0.01 + 6.8978 * D_norm
- compute g(d) = exp(-alpha * d_norm)

Return:
1. OSGOOD_DESCRIPTOR_TABLE
2. OSGOOD_PROFILE_TABLE
3. OSGOOD_DISTANCE_TABLE
4. ALPHA_TABLE
5. SHEPARD_TABLE

Also give a brief interpretation of:
- semantic proximity
- semantic separation
- transfer/generalization strength
```

---

## Prompt 3. Image / Figure / Logo Prompt

```text
Run the Osgood suboperation first, then Shepard.

Important:
The object may be:
- image
- figure
- logo
- mixed visual object

Use this sequence:
1. build automatic semantic description
2. apply user correction only if explicitly provided
3. score the final semantic descriptor on:
   - good-bad
   - strong-weak
   - active-passive
4. compute Osgood profile
5. compute Osgood distance
6. normalize distance
7. compute alpha
8. compute Shepard similarity

Use:
- D_raw = sqrt((e1-e2)^2 + (p1-p2)^2 + (a1-a2)^2)
- D_norm = D_raw / 10.3923048454
- alpha = 0.01 + 6.8978 * D_norm
- g(d) = exp(-alpha * d_norm)

Required outputs:
- OSGOOD_DESCRIPTOR_TABLE
- OSGOOD_PROFILE_TABLE
- OSGOOD_DISTANCE_TABLE
- ALPHA_TABLE
- SHEPARD_TABLE

Do not skip the descriptor stage for visual inputs.
```

---

## Prompt 4. Alpha Only Prompt

```text
Compute Osgood-calibrated alpha only.

Task:
1. score both objects on the three Osgood axes
2. compute Osgood raw distance
3. compute Osgood normalized distance
4. compute alpha only

Use:
- good-bad
- strong-weak
- active-passive

Use:
- D_raw = sqrt((e1-e2)^2 + (p1-p2)^2 + (a1-a2)^2)
- D_norm = D_raw / 10.3923048454
- alpha = 0.01 + 6.8978 * D_norm

Return:
1. OSGOOD_PROFILE_TABLE
2. OSGOOD_DISTANCE_TABLE
3. ALPHA_TABLE

Do not compute alpha from observed g.
```

---

## Prompt 5. MDS + Osgood + Shepard Prompt

```text
Run the full MDS to Osgood to Shepard route.

Goal:
1. compute or recover distances through MDS where relevant
2. run the Osgood semantic differential suboperation
3. compute Osgood semantic distance
4. compute alpha
5. compute Shepard transfer/generalization value

Important:
Keep the MDS route and Osgood route distinct.
Do not collapse Osgood semantic distance into recovered MDS distance unless explicitly requested.

Required outputs:
1. optional COORDINATES_TABLE
2. optional DISTANCE_TABLE
3. optional FIT_TABLE
4. OSGOOD_DESCRIPTOR_TABLE
5. OSGOOD_PROFILE_TABLE
6. OSGOOD_DISTANCE_TABLE
7. ALPHA_TABLE
8. SHEPARD_TABLE

State clearly which distance source is finally used in Shepard:
- MDS recovered distance
- Osgood distance
- aggregated distance
- scale distance
```

---

## Prompt 6. Strict Computation Prompt

```text
Run strictly with the fixed Osgood and Shepard rules below.

Use:
- Osgood score range: [-3, +3]
- D_raw corridor: [0, 10.3923048454]
- D_norm = D_raw / 10.3923048454
- alpha corridor: [0.01, 6.9078]
- alpha = 0.01 + 6.8978 * D_norm
- g(d) = exp(-alpha * d_norm)

Return tables, not narrative only.

Do not invent a different alpha corridor.
Do not switch to a non-Osgood calibration.
Do not replace the three-axis Osgood base with a larger scale set unless explicitly requested.
```

---

## Prompt 7. Minimal Chat Prompt

```text
Compute Osgood semantic distance, alpha, and Shepard g(d) for this pair.

Use only:
- good-bad
- strong-weak
- active-passive

Use:
- D_raw = sqrt((e1-e2)^2 + (p1-p2)^2 + (a1-a2)^2)
- D_norm = D_raw / 10.3923048454
- alpha = 0.01 + 6.8978 * D_norm
- g(d) = exp(-alpha * d_norm)

Return:
- OSGOOD_PROFILE_TABLE
- OSGOOD_DISTANCE_TABLE
- ALPHA_TABLE
- SHEPARD_TABLE
```
