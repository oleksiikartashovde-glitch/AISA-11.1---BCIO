# AISA 11.1 — Quick Guide

### Ontology-Grounded Behavioural Pattern Analysis

-----

## Preface

This guide covers the four-step workflow for analysing any case where behaviour is present — from raw text to a fully ontology-mapped, module-counted output. The workflow is designed for use with AISA 11.1, an LLM orchestration system built on the Behaviour Change Intervention Ontology (BCIO).

Each step produces a structured table. The output of each step feeds directly into the next. The steps are sequential and should not be skipped or merged.

The workflow applies to any source material: published research, intervention descriptions, policy documents, programme materials, case narratives, and technical deployment studies. The input domain does not matter as long as behaviour — and its drivers or context — can be identified.

No prior knowledge of BCIO or any ontology is required to start. Plain-language input is accepted at Step 1. The system normalises the input into ontology space internally.

-----

## Why This Workflow

The four-step structure is not arbitrary. Each step has a specific function that the next step depends on.

|Step                  |What it prevents                                                                                         |
|----------------------|---------------------------------------------------------------------------------------------------------|
|Step 1 — Raw patterns |Prevents premature normalisation and ontology drift. Keeps source text anchored.                         |
|Step 2 — Normalisation|Prevents duplication and false splits. Resolves synonymy before any IDs are assigned.                    |
|Step 3 — Mapping      |Prevents surface-level matching. Enforces component-level coverage with neighbourhood consistency checks.|
|Step 4 — Module count |Prevents blind spots. Exposes which of the 13 modules are covered and which are absent.                  |

Additional strengths of the workflow:

- Works with any domain — health, retail, HR, policy, digital products, safety, education
- Accepts unstructured input — you do not need to pre-classify anything before Step 1
- Module-level output is cross-validated — IDs with multi-module suffixes are counted correctly in each module
- Deduplication is controlled — synonyms are collapsed, but analytically distinct patterns are preserved
- Component decomposition is explicit — composite patterns are decomposed before mapping, preventing false coverage
- The 13-module framework covers BPMS (8 intervention-description modules) and DREFS (5 gap-analysis modules)

-----

## Step 1. Extract Raw Patterns

> **Action:** upload a case file (docx, pdf, txt, md, json, zip), then copy and paste the prompt below together with your task.

-----

Task: read the case and extract raw semantic patterns — without ontology, without IDs, without normalisation.

Read the case and identify all significant semantic patterns.

Rules:

- Do not perform ontology mapping. Do not assign IDs, terms, or classes.
- A new pattern is created only when the analytical function of a fragment changes — not when the wording changes.
- Do not split: synonymous descriptors of the same feature, gradations of intensity, and qualifications should appear as comments on the pattern, not as separate rows.
- Do not merge patterns with different functions (symptom, trigger, emotion, belief, behaviour, consequence, condition, intervention, context, etc.).
- Do not normalise or collapse patterns across different text fragments.

Output — one table only:

|No.|Text fragment|Extracted pattern|Comment on pattern boundary|
|---|-------------|-----------------|---------------------------|

-----

## Step 2. Normalise, Deduplicate, Decompose, Assign Module

> **Action:** after receiving the Step 1 table, copy and paste the prompt below together with your task.

-----

Task: normalise the raw patterns from Step 1, perform soft deduplication, check for composite structure, and assign a preliminary module from the list of 13.

For each pattern from Step 1, perform the following:

1. Normalisation — produce a concise, analytically stable form; preserve the original meaning; no ontology terms; no IDs.
1. Deduplication — collapse full duplicates and wording variants of the same meaning. Do not collapse if the difference creates a new analytical meaning (different symptom, context, subject, mechanism, stage, type of consequence).
1. Composite structure — determine: simple / simple with modifiers / composite. Decompose only if the pattern genuinely contains different functional components. Numbers, durations, localisations, and detection methods should appear as modifiers unless they form a separate component.
1. Preliminary module — assign one value from this list only (this is a working coordinate, not an ontology mapping): 1. Behaviour · 2. Behaviour Change Technique · 3. Population · 4. Mechanisms of Action · 5. Mode of Delivery · 6. Setting · 7. Source · 8. Style of Delivery · 9. Dose · 10. Reach · 11. Engagement · 12. Fidelity · 13. Schedule

Output rules:

- One final table only. No explanatory paragraphs, no additional tables, no summaries.
- No IDs, no ontology terms, no class names.

Permitted values for the **Deduplication status** column:
retained / collapsed as duplicate / collapsed as wording variant / retained as separate pattern / retained, modifiers extracted / retained, specific case — do not collapse

Permitted values for the **Composite structure** column:
simple / simple with modifiers / composite

Output — one table only:

|No.|Raw pattern|Normalised pattern|Deduplication status|Composite structure|Components / modifiers|Preliminary module|
|---|-----------|------------------|--------------------|-------------------|----------------------|------------------|

-----

## Step 3. Ontology Mapping

> **Action:** after receiving the Step 2 table, copy and paste the prompt below together with your task.

-----

Task: take the pattern components from Step 2 and perform ontology mapping component by component, in batches of 4, without stopping.

For each normalised pattern, take its components from Step 2 and map each component separately.

Batch mode: process automatically in batches of 4 (1–4 → 5–8 → … to the end). Do not request permission between batches. Row format is identical across all batches. Final output — one unified table.

Internal check after each batch:

- Were the components taken from Step 2 (not from raw fragments)?
- Has any component been lost? Have different components been merged into one term?
- Does every component have explicit ontology coverage?

Mapping rules:

- Map each component separately — not the pattern as a whole.
- Look for ontological coverage of the semantic function — not a literal wording match.
- If one term is insufficient, build coverage compositionally from several terms (each on a separate row).
- No component may be left without coverage. Permitted forms: (a) one exact term, or (b) a composition of several.
- If no direct term exists — abstract the component to its type of semantic structure (type of process, change, status, role, constraint, etc.) and search there.
- Do not substitute components: change ≠ object of change; role ≠ relation; status ≠ process; consequence ≠ context; temporal component ≠ main phenomenon.
- If a component contains a meaning of change — check separately: type of change, object of change, initial / new status, bearer, consequence.
- One pattern / one component may occupy several rows (one term per row).

Output — one table only:

|No.|Pattern|Normalised pattern|Pattern component|Ontology ID|Label|Definition|Module code check|Coverage type|Rationale|
|---|-------|------------------|-----------------|-----------|-----|----------|-----------------|-------------|---------|

-----

## Step 4. Count Terms by Module

> **Action:** after receiving the final Step 3 table, copy and paste the prompt below together with your task.

-----

Task: using the Step 3 table, count the number of rows and unique terms for each of the 13 modules, accounting for cross-modality.

Take the completed Step 3 table. For each term, determine in which modules it operates (by direct affiliation and by cross-modality).

Rules:

- Do not add new terms and do not change the mapping.
- Rows: if one row belongs to several modules, count it in each.
- Unique terms: count by ID (not by label); one ID within one module counts once.
- In the grand total, count one ID only once even if it appears in several modules.
- If a module has no coverage, enter 0.

Output — one table only:

|Module                       |Number of rows|Number of unique terms|
|-----------------------------|--------------|----------------------|
|1. Behaviour                 |              |                      |
|2. Behaviour Change Technique|              |                      |
|3. Population                |              |                      |
|4. Mechanisms of Action      |              |                      |
|5. Mode of Delivery          |              |                      |
|6. Setting                   |              |                      |
|7. Source                    |              |                      |
|8. Style of Delivery         |              |                      |
|9. Dose                      |              |                      |
|10. Reach                    |              |                      |
|11. Engagement               |              |                      |
|12. Fidelity                 |              |                      |
|13. Schedule                 |              |                      |
|**Total**                    |              |                      |

-----

*Full documentation: REFERENCE_UserGuide · AISA 11.1 · CC BY 4.0*
