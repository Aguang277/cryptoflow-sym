# CryptoFlow-Sym Output Templates

Reusable Markdown/JSON templates for CryptoFlow-Sym workflow artifacts.

These templates are structured working files, not final reports by themselves.

---

## 0. Template Usage and Trigger Rules

| Situation | Required / Recommended Artifacts |
|---|---|
| New project starts | `pipeline/status.json`, `outputs/research_log.md` |
| Literature search or survey | `outputs/source_registry.md`, `outputs/literature_search_matrix.md`, `outputs/candidate_paper_queue.md` |
| Cross-scope or unclear topic | `outputs/scope_classification_log.md` |
| Algorithm/component-specific work | `outputs/primitive_profile.md`, optionally `outputs/component_property_profile.md` |
| Prior attacks or security claims | `outputs/attack_boundary_table.md`, `outputs/claim_evidence_matrix.md` |
| MILP/SAT/SMT/CP/monomial/division-property model or source-code explanation | `outputs/modeling_assumptions.md`, `experiments/experiment_plan.md`, `experiments/experiment_log.md` |
| Experiment planned or executed | `experiments/experiment_plan.md`, `experiments/experiment_log.md`, `outputs/research_log.md` |
| Missing test vectors or weak validation | `outputs/validation_gap_report.md` |
| Gate blocks a serious claim | `outputs/blocked_report.md`, `outputs/claim_evidence_matrix.md` |
| Failed or unsupported route | `outputs/negative_results.md`, `outputs/research_log.md` |
| Early profile or attack-boundary sanity check | `iterations/lightweight_check_N.md` |
| Serious claim or final deliverable review | `iterations/review_round_N.md` |
| Human decision required | `pipeline/human_checkpoints.md` |
| Sandbox profile or permission changed | `pipeline/sandbox_override_log.md` |
| Draft paper generated before final approval | `paper/unverified_draft.md` or `paper/manuscript.md` with claim-status mapping |
| Final approved report | `deliverables/final_report.md` |

`outputs/modeling_assumptions.md` is required whenever code implements a formal model whose result may support a cryptographic claim. Stage 5 must create or update it before using any MILP/SAT/SMT/CP/monomial/division-property/source-code model result as claim evidence. If missing, the result may be logged as an experiment, but dependent claims must remain `preliminary`, `pilot_supported`, `blocked`, or `unverified_draft`.

---

## 1. `pipeline/status.json`

```json
{
  "project_name": "",
  "topic": "",
  "is_example_or_demo": false,
  "target_primitives": [],
  "research_task": "",
  "primitive_family": "",
  "automation_mode": "human_in_the_loop",
  "sandbox_profile": "normal",
  "current_stage": "stage_0_initialize_project",
  "current_output_mode": "draft",
  "completed_outputs": [],
  "open_gates": [],
  "blocked_claims": [],
  "claim_status_summary": {
    "idea": 0,
    "preliminary": 0,
    "pilot_supported": 0,
    "literature_supported": 0,
    "validated": 0,
    "review_passed": 0,
    "blocked": 0,
    "negative_result": 0,
    "unverified_draft": 0,
    "final_approved": 0
  },
  "last_updated": "",
  "next_actions": []
}
```

---

## 2. `outputs/source_registry.md`

```markdown
# Source Registry

| ID | Title | Authors | Year | Venue/Status | Source Type | URL/DOI/ePrint | Local Path | Read Status | Relevance | Notes |
|---|---|---|---:|---|---|---|---|---|---|---|
| S001 |  |  |  | peer_reviewed / eprint / standard / artifact / unknown | paper/spec/code/slides/book |  |  | unread / skimmed / deep_read / checked | design / attack / survey / component / artifact / background |  |

## Supersession / Version Notes

| Source ID | Relation | Newer/Older Source | Note |
|---|---|---|---|
|  | superseded_by / extended_by / corrected_by / journal_version_of |  |  |
```

---

## 3. `outputs/scope_classification_log.md`

```markdown
# Scope Classification Log

- Status: in_scope / cross_scope_with_symmetric_core / out_of_scope
- Symmetric core object:
- Cross-scope material, if any:

## Kept Items

| Item | Why Relevant to Symmetric Cryptography |
|---|---|
|  |  |

## Excluded Items

| Item | Reason for Exclusion |
|---|---|
|  |  |
```

---

## 4. `outputs/literature_search_matrix.md`

```markdown
# Literature Search Matrix

| Round | Query | Source | Results Count | Relevant Hits | Notes |
|---:|---|---|---:|---|---|
| 1 |  | IACR ePrint / DBLP / OpenAlex / local / other |  |  |  |

## Coverage Assessment

- Design/specification paper found: yes / no / unknown
- Best-known attack papers found: yes / no / unknown
- Standardization documents found: yes / no / unknown
- Artifact/code found: yes / no / unknown
```

---

## 5. `outputs/candidate_paper_queue.md`

```markdown
# Candidate Paper Queue

| Priority | Source ID | Title | Reason | Task | Status |
|---:|---|---|---|---|---|
| 1 | S001 |  | design/spec / best-known attack / survey / artifact / component theory | skim / deep read / reproduce / cite only | pending |
```

---

## 6. `outputs/primitive_profile.md`

```markdown
# Primitive Profile

## Identification
- Name:
- Version / parameter set:
- Primitive family:
- Primitive type:
- Source/specification:

## Parameters
- Block/state size:
- Key size:
- Tweak/nonce/IV size:
- Number of rounds:
- Word size:
- Field/ring, if applicable:

## State Layout
Describe bit/byte/nibble ordering, row/column convention, endian convention, and indexing.

## Round and Indexing Conventions
- Round numbering convention:
- Initial key addition counted as round? yes / no / not applicable
- Final key addition counted as round? yes / no / not applicable
- State index convention:
- Bit numbering convention:
- Nibble/byte ordering convention:
- Difference/mask/activity notation:

## Round Function

## Key Schedule

## Test Vectors and Reference Implementations

| Type | Available? | Source | Notes |
|---|---|---|---|
| Official test vector | yes/no/unknown |  |  |
| Reference implementation | yes/no/unknown |  |  |

## Ambiguities

| Issue | Impact | Current Handling |
|---|---|---|
|  |  |  |
```

---

## 7. `outputs/component_property_profile.md`

```markdown
# Component Property Profile

- Name:
- Type: S-box / Boolean function / vectorial Boolean function / linear layer / permutation / sequence / other
- Location in primitive:
- Definition source:

| Property | Value | Method | Status |
|---|---|---|---|
| Size / dimension |  |  |  |
| Algebraic degree |  |  |  |
| Differential uniformity |  |  |  |
| Linearity / nonlinearity |  |  |  |
| Branch number |  |  |  |
| Walsh spectrum |  |  |  |
| DDT/LAT property |  |  |  |
| BCT / boomerang connectivity |  |  |  |
| Algebraic immunity / resiliency |  |  |  |
| Propagation criterion / correlation immunity |  |  |  |
| Fixed points / linear structures |  |  |  |
```

---

## 8. `outputs/attack_boundary_table.md`

```markdown
# Attack Boundary Table

| ID | Source ID | Attack Type | Rounds | Round Convention | Model | Data | Time | Memory | Unit Normalization | Success Probability | Guessed/Recovered Key Bits | Reproduction Status | Notes |
|---|---|---|---:|---|---|---|---|---|---|---|---|---|---|
| A001 | S001 | distinguisher / key_recovery / differential / linear / integral / impossible / algebraic / other |  | paper / project / unclear | chosen plaintext / known plaintext / related-key / fixed-key / other |  |  |  | encryptions / S-box calls / solver seconds / unknown |  |  | not_attempted |  |

## Complexity Normalization Notes
Explain how data, time, memory, success probability, and key-guessing costs are normalized across papers.

## Boundary Summary
- Best known distinguisher:
- Best known key-recovery attack:
- Known security bounds:
- Open or unclear boundary:
```

---

## 9. `outputs/claim_evidence_matrix.md`

```markdown
# Claim Evidence Matrix

| Claim ID | Claim | Claim Type | Scope | Status | Depends On | Evidence | Validation Method | Reproduction Status | Required Gates | Open Limitations | Reviewer Status | Next Action |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| C001 |  | distinguisher / key_recovery / structural_property / component_property / complexity_claim / survey_conclusion / other | primitive / component / experiment / literature | idea / preliminary / pilot_supported / literature_supported / validated / review_passed / blocked / negative_result / unverified_draft / final_approved | source/attack/experiment IDs |  |  | not_attempted | literature / profile / complexity / validation / modeling / review |  | not_reviewed |  |

## Blocked Claims

| Claim ID | Blocking Gate | Missing Evidence | How to Unblock |
|---|---|---|---|
|  |  |  |  |
```

---

## 10. `outputs/modeling_assumptions.md`

```markdown
# Modeling Assumptions

## Model Identification
- Model ID:
- Related experiment ID:
- Related claim ID(s):
- Primitive/component:
- Research task:
- Model type: MILP / SAT / SMT / CP / monomial_prediction / division_property / integral / differential / linear / algebraic / other
- Source code path:
- Solver/tool:
- Solver/tool version:

## Cryptographic Object Modeled
- Rounds modeled:
- Round convention:
- State variables:
- Key variables:
- Tweak/nonce variables:
- Constants:
- S-box / nonlinear layer:
- Linear layer / permutation:
- Key schedule:

## Variable Semantics

| Variable Pattern | Type | Meaning | Domain | Index Convention | Source Code Location |
|---|---|---|---|---|---|
| x_r_i | state | state bit/nibble/word at round r index i | binary/integer/bit-vector |  |  |
| k_r_i | key | round-key bit/nibble/word | binary/integer/bit-vector |  |  |

## Constraint Semantics

| Constraint Group | Code Location | Mathematical Meaning | Assumption | Checked? |
|---|---|---|---|---|
| S-box constraints |  | DDT/LAT/division transition/etc. |  | yes/no |
| Linear layer constraints |  | permutation/MDS/branch/activity propagation |  | yes/no |
| Key schedule constraints |  | round-key relation / independence / master-key expansion |  | yes/no |
| Objective constraints |  | minimize active S-boxes / find impossible trail / search zero-sum / other |  | yes/no |

## Key Modeling Policy
- Key modeled? yes / no / partial
- Master key modeled? yes / no / partial
- Round keys treated as independent? yes / no / partially
- Key schedule constraints included? yes / no / partially
- If key variables are constrained by `sum(key_vars) >= 1`, explain why:
- Does the model search properties for all keys or fixed-key/key-dependent behavior?
- Can the resulting distinguisher/claim be used for key recovery? Explain limitations.

## Input / Output Activity or Mask Policy
- Active input pattern:
- Active output pattern:
- Fixed variables:
- Balanced / zero-sum / impossible / integral / mask condition:
- Quantifier interpretation: exists / for all / fixed-key / random-key / unknown

## Objective and Search Meaning
- Objective function:
- Feasibility meaning:
- UNSAT meaning:
- SAT solution meaning:
- Optimality meaning:
- Search limits:
- Timeout behavior:

## Soundness Notes

| Potential Issue | Impact | Mitigation / Check |
|---|---|---|
| Bit order mismatch |  |  |
| Missing key schedule constraints |  |  |
| Treating dependent round keys as independent |  |  |
| Confusing fixed-key and all-key claims |  |  |
| Overinterpreting infeasibility |  |  |
| Solver timeout mistaken for proof |  |  |

## Source Code Explanation Notes

| File / Function | Role in Model | Important Assumptions | Notes |
|---|---|---|---|
|  |  |  |  |

## Current Status
- Model status: draft / sanity_checked / validated / blocked
- Claim status impact:
- Required next checks:
```

---

## 11. `outputs/validation_gap_report.md`

```markdown
# Validation Gap Report

| Item | Claim/Component Affected | Missing Evidence | Impact | Temporary Status |
|---|---|---|---|---|
|  |  | official test vector / independent implementation / intermediate states / other |  |  |
```

---

## 12. `outputs/blocked_report.md`

```markdown
# Blocked Report

- Claim / task / experiment:
- Related stage:
- Blocking gate:
- Reason:
- Evidence currently available:
- Missing evidence:
- Blocks final claim: yes / no
- Allows draft continuation: yes / no
- How to unblock:
- Current status: blocked / unverified_draft / preliminary / negative_result
```

---

## 13. `outputs/negative_results.md`

```markdown
# Negative Results

| ID | Date | Attempted Idea/Claim | Method | Result | Interpretation | Future Relevance |
|---|---|---|---|---|---|---|
| N001 |  |  |  | failed / inconclusive / contradicted / not useful |  |  |
```

---

## 14. `outputs/research_log.md`

```markdown
# Research Log

### YYYY-MM-DD HH:MM — Entry Title

- Stage:
- What changed:
- Reason:
- Evidence or observation:
- Impact on claims:
- Next action:
```

---

## 15. `outputs/idea_candidates.md`

```markdown
# Idea Candidates

| ID | Idea | Claim Type | Required Evidence | Failure Modes | Novelty Risk | Experiment Cost | Priority | Status |
|---|---|---|---|---|---|---|---:|---|
| I001 |  | distinguisher / key_recovery / component_property / modeling_result / reproduction_result / survey_conclusion |  |  | low/medium/high | low/medium/high | 1 | idea |
```

---

## 16. `experiments/experiment_plan.md`

```markdown
# Experiment Plan

## Related Claim or Idea

## Hypothesis

## Success Criterion

## Failure Criterion

## Resource Budget
- Expected runtime:
- Memory:
- CPU threads:
- Trials:
- Sandbox profile:
- Timeout:

## Tooling and Reproducibility
- Script path:
- Git commit / code snapshot:
- Python / solver / compiler version:
- Random seed(s):
- Exact command template:

## Interpretation Rule Before Execution
```

---

## 17. `experiments/experiment_log.md`

```markdown
# Experiment Log

| Run ID | Experiment ID | Date | Command | Parameters | Seed | Runtime | Environment / Versions | Code Snapshot | Result Label | Notes |
|---|---|---|---|---|---|---|---|---|---|---|
| RUN001 |  |  |  |  |  |  |  |  | supports / does_not_support / inconclusive / bug_detected / needs_rerun |  |
```

---

## 18. `iterations/lightweight_check_N.md`

```markdown
# Lightweight Check N

- Stage checked: Stage 2 / Stage 3 / other
- Checker:
- Date:

| Item | Status | Notes |
|---|---|---|
| State layout / bit order | pass / concern / fail / not checked |  |
| Primitive/component definition | pass / concern / fail / not checked |  |
| Attack boundary interpretation | pass / concern / fail / not checked |  |
| Distinguisher vs key recovery separation | pass / concern / fail / not checked |  |
| Fixed-key vs arbitrary-key assumption | pass / concern / fail / not checked |  |

Recommendation: continue / continue_in_draft_mode / request_formal_review / block_finalization
```

---

## 19. `iterations/review_round_N.md`

```markdown
# Formal Review Round N

| Claim ID | Reviewer Decision | Reason | Required Revision |
|---|---|---|---|
| C001 | pass / revise / downgrade / block / negative |  |  |

## Complexity Audit Notes
## Validation Notes
## Literature Boundary Notes
## Overclaiming Risks
## Final Recommendation
```

---

## 20. `pipeline/human_checkpoints.md`

```markdown
# Human Checkpoints

| ID | Date | Stage | Decision Needed | Recommendation | User Decision | Notes |
|---|---|---|---|---|---|---|
| H001 |  |  |  |  | pending / approved / rejected / modified |  |
```

---

## 21. `pipeline/sandbox_override_log.md`

```markdown
# Sandbox Override Log

| ID | Date | From Profile | To Profile | Requested Capability | Reason | Approved By User? | Notes |
|---|---|---|---|---|---|---|---|
| SO001 |  | normal | power | longer runtime / more memory / more threads / external artifact / other |  | yes/no |  |
```

---

## 22. `paper/unverified_draft.md`

```markdown
# Unverified Draft

> Status: unverified_draft. This draft may contain preliminary, blocked, or not-yet-reviewed claims. Do not treat it as final.

## Abstract Draft
## Background
## Target Primitive / Component
## Literature Boundary
## Method / Idea
## Experiments or Evidence
## Claims and Status

| Claim ID | Claim | Status | Missing Evidence |
|---|---|---|---|
|  |  |  |  |
```

---

## 23. `paper/manuscript.md`

```markdown
# Manuscript Draft

> Status: manuscript_draft. Each serious claim must have a claim ID and status in `outputs/claim_evidence_matrix.md`.

## Title
## Abstract
## 1. Introduction
## 2. Background and Target Description
## 3. Related Work and Attack Boundary
## 4. Method / Analysis
## 5. Experiments / Validation
## 6. Complexity Analysis
## 7. Limitations and Threats to Validity
## 8. Conclusion

## Claim Mapping

| Manuscript Claim | Claim ID | Status | Required Remaining Work |
|---|---|---|---|
|  |  |  |  |
```

---

## 24. `deliverables/final_report.md`

```markdown
# Final Report

- Output status: final_report
- Final gates passed: yes / no
- Reviewer approval: yes / no
- Human/final-checker approval: yes / no

## Executive Summary
## Target
## Literature Boundary
## Primitive or Component Profile Summary
## Main Findings
## Claim Evidence Summary
## Validation and Complexity Audit
## Limitations
## Reproducibility Notes
## Files Produced
```
