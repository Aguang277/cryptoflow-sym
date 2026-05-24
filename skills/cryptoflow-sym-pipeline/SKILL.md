---
name: cryptoflow-sym-pipeline
description: Symmetric-cryptography-first ARIS-like research workflow for literature review, primitive/component profiling, cryptanalysis planning, experiments, review, and draft/final output generation.
---

# CryptoFlow-Sym Pipeline Skill

CryptoFlow-Sym is a symmetric-cryptography-first AI research workflow for block ciphers, tweakable block ciphers, stream ciphers, hash functions, compression functions, permutations, sponge/duplex constructions, XOFs, AEAD/MAC/modes, PRF/PRP objects, S-boxes, Boolean/vectorial Boolean functions, sequences, and keystream generators.

Public-key cryptography, PQC, ZK, MPC, FHE, and protocol-only topics are out of scope by default. They may be included only when the symmetric primitive/component is the core object under study.

---

## 0. Startup Behavior

When starting a real CryptoFlow-Sym project:

1. Treat `configs/config.sym.example.json` as a template and demonstration file, not as the normal working config.
2. If `configs/config.sym.json` does not exist, create it by copying `configs/config.sym.example.json`.
3. Fill `configs/config.sym.json` from the user's request, including project name, topic, target primitive, research task, automation mode, and sandbox profile.
4. Preserve `configs/config.sym.example.json` unless the user explicitly asks to update the template.
5. Load `configs/workflow.sym.yaml` when available.
6. Start execution at `stage_0_initialize_project`.
7. Record whether the project is real or example/demo in `pipeline/status.json`.
8. If the user asks for ARIS-like full automation, prefer `full_auto_draft` unless the user explicitly asks for final-gated operation.
9. Label draft, blocked, negative-result, and research-log outputs clearly; never present them as final-approved claims.

Source-of-truth order:

1. User's current explicit instruction.
2. Real project config: `configs/config.sym.json`.
3. Machine-readable workflow: `configs/workflow.sym.yaml`.
4. This skill file.
5. Example template: `configs/config.sym.example.json`.

If these conflict, prefer the user's explicit instruction unless it violates safety, sandbox, or claim-finalization rules. Never silently relax gates, sandbox restrictions, or claim-status requirements.

---

## 1. When to Use This Skill

Use this skill when the user asks to:

- Survey or analyze a symmetric primitive, component, construction, or attack family.
- Reproduce or validate a block cipher, stream cipher, hash, permutation, AEAD, MAC, mode, S-box, Boolean/vectorial Boolean function, or sequence result.
- Search for distinguishers, key-recovery attacks, integral trails, differential/linear trails, impossible/zero-correlation/invariant properties, algebraic properties, or component-level structures.
- Build or review MILP, SAT, SMT, CP, monomial prediction, division property, integral, differential, linear, or algebraic models.
- Generate a research plan, experiment plan, negative-result report, blocked report, claim evidence matrix, or paper draft in symmetric cryptography.
- Run an ARIS-like automated workflow for symmetric-cryptography research.

Do not use this as the primary workflow for generic software engineering, generic mathematics, or public-key/PQC protocol design unless a symmetric core object is central.

---

## 2. Required Inputs

Identify or create:

1. Project root directory.
2. Config file: `configs/config.sym.json` or fallback `configs/config.sym.example.json`.
3. Target primitive/component, if known.
4. Research task type, such as `cryptanalysis`, `security_evaluation`, `component_theory`, `automated_search`, `artifact_reproduction`, or `survey`.
5. Automation mode: `human_in_the_loop`, `supervised_auto`, `overnight_auto`, or `full_auto_draft`.
6. Sandbox profile: `safe`, `normal`, or `power`.

If the user has not specified a primitive, use the config topic as the default. If the config is an example file, label outputs as example/demo until a real target is chosen.

---

## 3. Directory Contract

Write only inside the project sandbox:

- `configs/`, `papers/`, `knowledge/`, `outputs/`, `project/`, `experiments/`, `iterations/`, `paper/`, `deliverables/`, `pipeline/`, `logs/`, `tmp/`.

Never read secrets or write to sensitive paths such as `~/.ssh`, `~/.aws`, `~/.gnupg`, `~/.kube`, `/etc`, `/var`, or `/root` unless the user explicitly overrides the sandbox for a narrow justified reason. Even then, never access private keys or credentials.

---

## 4. Claim and Reproduction Status

Claim status answers: how strong is our claim?

Allowed claim statuses:

- `idea`
- `preliminary`
- `pilot_supported`
- `literature_supported`
- `validated`
- `review_passed`
- `blocked`
- `negative_result`
- `unverified_draft`
- `final_approved`

A claim may move upward only when evidence is added. Never silently upgrade a claim.

Reproduction status answers: what happened when checking a prior reported result?

Allowed reproduction statuses:

- `not_attempted`
- `paper_checked_only`
- `reproduced_as_reported`
- `reproduced_with_discrepancy`
- `not_reproduced`
- `contradicted_by_experiment`
- `superseded_or_corrected`

Reproduction status is separate from claim status. `reproduced_as_reported` does not automatically mean `final_approved`.

---

## 5. Gate Principle

Gates block finalization and strong claims, not exploration, draft generation, or negative-result reporting.

When a gate fails:

- Do not erase the work.
- Do not force a strong claim.
- Downgrade the claim status.
- Record the reason in `outputs/blocked_report.md`, `outputs/validation_gap_report.md`, `outputs/modeling_assumptions.md`, or `outputs/negative_results.md` as applicable.
- Continue in draft mode when the automation mode allows it.

Gate levels:

- `soft_gate`: continue in draft mode, record assumptions and missing evidence.
- `review_gate`: require independent reviewer/fresh-context checker before the claim can be marked supported.
- `hard_gate`: block final deliverables and paper-level claims until resolved; blocked reports and unverified drafts remain allowed.

---

## 6. Core Gates

Apply gates conditionally by claim type and output status.

### Literature Boundary Gate

Required for novelty, best-known-attack, standardization, comparison, and survey-conclusion claims. Requires source registry, literature search matrix, and candidate paper queue. Failure blocks novelty/comparison/final claims, but notes and drafts may continue.

### Primitive or Component Profile Gate

Required for algorithm/component-specific claims. Record state size, key/tweak/nonce parameters, round function, key schedule, constants, S-box, permutation, Boolean/vectorial Boolean function, or sequence definition as applicable. Failure blocks algorithm-specific final claims.

### Claim Evidence Matrix Gate

Required for every paper-level or serious claim. Each row must include claim, claim type, scope, evidence, validation method, reproduction status when relevant, required gates, limitations, reviewer status, and next action.

### Complexity Audit Gate

Required for attack/performance/benchmark/cost claims. Audit data, time, memory, precomputation, partial encryption/decryption, filtering, false positives, guessed key bits, success probability, and comparison with exhaustive search.

### Validation Gate

Required for implementation-dependent claims. Prefer official test vectors, reference implementation comparison, independent implementation comparison, inverse tests, randomized consistency tests, intermediate state checks, and re-derived vectors. If official test vectors are missing, use independent validation and record the gap.

### Modeling Assumptions Gate

Required before any formal model or source-code model result supports a cryptographic claim. Applies to MILP, SAT, SMT, CP, monomial prediction, division property, integral, differential, linear, algebraic, and automated-search models.

Before upgrading dependent claims beyond `preliminary`, `pilot_supported`, `blocked`, or `unverified_draft`, create or update `outputs/modeling_assumptions.md` with:

- model type and solver/tool version;
- variables and domains;
- constraint semantics;
- key modeling policy;
- input/output activity or mask policy;
- SAT/UNSAT/optimality meaning;
- soundness risks such as bit order mismatch, missing key schedule, fixed-key vs all-key confusion, and solver timeout.

### Independent Review Gate

Required for paper-level claims, attack/security claims, complexity claims, and final deliverables. The reviewer must search for mistakes, missing assumptions, literature gaps, model mismatch, complexity errors, validation gaps, and overclaiming.

---

## 7. Automation Modes

### `human_in_the_loop`

Default safest mode. Pause at human checkpoints. Do not auto-select final directions, run expensive experiments, or promote claims without confirmation.

### `supervised_auto`

Auto-proceed through low-risk stages and small experiments. Pause before paper-level claims, expensive experiments, and final deliverables.

### `overnight_auto`

ARIS-like overnight workflow. Allows literature collection, rough reading, candidate queues, profile drafts, idea generation, small experiments, blocked/negative reports, and unverified drafts. Does not automatically final-approve serious claims.

### `full_auto_draft`

Highest automation. Run the workflow as far as possible, auto-select candidates, generate experiment plans, run small experiments, and produce paper drafts. All outputs must be labeled `unverified_draft` unless reviewed. Serious claims cannot become `final_approved` automatically.

---

## 8. Lightweight Checks vs Formal Review

Lightweight reviewer checks are allowed after Stage 2 and Stage 3, especially for primitive/component profiles, bit ordering, attack-boundary tables, and early model assumptions.

Lightweight checks:

- Do not count against `max_review_rounds`.
- Are recorded as `iterations/lightweight_check_N.md`.
- May flag risks, downgrade claims, request formal review, or block unstable assumptions.
- Cannot mark serious claims as `final_approved`.

Formal reviews:

- Count against `max_review_rounds`.
- Are recorded as `iterations/review_round_N.md`.
- Are required for paper-level claims, attack/security claims, complexity claims, and final deliverables.

---

## 9. Experiment Policy

Small experiments may run automatically in `overnight_auto` and `full_auto_draft` when inside sandbox limits.

Allowed small experiments include reference tests, encrypt/decrypt inverse tests, randomized consistency tests, S-box DDT/LAT/Walsh calculations, Boolean/vectorial Boolean function checks, small-round distinguisher validation, small MILP/SAT/SMT/CP searches, limited random-key validation, and lightweight reproduction.

Default small-experiment limits:

- 30 minutes per job.
- 8 GB memory per job.
- 8 CPU threads.
- 1000 trials.

Require explicit confirmation for over-limit runtime/memory, full key-space search, large exhaustive search, untrusted external code, global dependency installation, network beyond declared sources, API cost over budget, and GPU/cluster jobs.

---

## 10. Stage Workflow

### Stage 0 — Initialize Project

Create controlled workspace, load config, create directories, initialize `pipeline/status.json`, record automation mode and sandbox profile, and mark example/demo vs real project.

### Stage 1 — Scope Classification and Literature Boundary

Classify scope, search local/configured sources, build source registry, search matrix, candidate paper queue, and identify design/specification and best-known attack sources.

### Stage 2 — Primitive or Component Profile

Record the target's state layout, bit/byte ordering, round convention, round function, key schedule, components, constants, test vectors, reference implementations, and ambiguities. Request lightweight check when profile risk is high.

### Stage 3 — Attack Boundary and Claim Matrix

Extract prior attacks/results, separate distinguisher vs key recovery and fixed-key vs arbitrary-key claims, record complexities and assumptions, assign reproduction status, record discrepancies, and build/update claim evidence matrix.

### Stage 4 — Idea Generation

Generate candidate ideas based on gaps, classify claim type, list required evidence and failure modes, estimate novelty risk and cost, rank candidates, and auto-select only when allowed.

### Stage 5 — Experiment Planning and Execution

Write experiment plan, define hypothesis and success/failure criteria, check sandbox/resource limits, define interpretation rules before execution, implement/run allowed experiments, log commands/parameters/runtime/environment/results, update claim status, and record blocked/negative results.

For formal-model or source-code-model experiments, create or update `outputs/modeling_assumptions.md` before using the result as claim evidence.

### Stage 6 — Independent Review

Prepare review packet, run independent/fresh-context review, record objections, revise claim status, and block finalization when disagreements remain unresolved.

### Stage 7 — Output Selection and Delivery

Produce the correct output type: `paper/unverified_draft.md`, `outputs/blocked_report.md`, `outputs/negative_results.md`, `outputs/research_log.md`, `deliverables/final_report.md`, or `paper/manuscript.md`. Final deliverables require all relevant gates.

---

## 11. Status File Minimal Schema

`pipeline/status.json` should include project name, topic, target primitives, research task, automation mode, sandbox profile, current stage, completed outputs, open gates, blocked claims, claim-status summary, current output mode, and next actions.

---

## 12. Final Response Rules

When reporting to the user:

1. State what stage was completed.
2. State what files were created or updated.
3. State any gate failures or downgraded claims.
4. State whether the output is final, draft, blocked, or negative.
5. Do not present unverified claims as final.
6. Keep the summary concise unless the user asks for full detail.

---

## 13. Default Philosophy

CryptoFlow-Sym should be aggressive in exploration and conservative in claims.

Explore automatically when safe; run small experiments automatically when allowed; preserve failed attempts as knowledge; use gates to prevent overclaiming; produce drafts even when claims are blocked; require review before finalizing serious cryptographic claims.
