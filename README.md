# CryptoFlow-Sym v0.1

CryptoFlow-Sym is an ARIS-inspired, symmetric-cryptography-first AI research workflow.

It is designed for block ciphers, tweakable block ciphers, stream ciphers, hash functions, compression functions, permutations, sponge/duplex constructions, AEAD/MAC/modes, PRF/PRP objects, S-boxes, Boolean/vectorial Boolean functions, sequences, and keystream generators.

## Core Files

| File | Role |
|---|---|
| `configs/config.sym.example.json` | Template/default policy. Do not edit for each project. |
| `configs/workflow.sym.yaml` | Machine-readable Stage 0–7 workflow. |
| `skills/cryptoflow-sym-pipeline/SKILL.md` | OpenClaw/agent-readable skill instructions. |
| `templates/output_templates.md` | Concrete output templates for reports, logs, matrices, reviews, and experiments. |

## How to Start a Real Project

1. Copy `configs/config.sym.example.json` to `configs/config.sym.json`.
2. Fill `configs/config.sym.json` with the real target primitive/component and task.
3. Load or invoke the skill `cryptoflow-sym-pipeline`.
4. Start at `stage_0_initialize_project`.

Example OpenClaw prompt:

```text
Use CryptoFlow-Sym to create a SIT integral-attack project.
Mode: full_auto_draft.
Sandbox: normal.
Create configs/config.sym.json from the example template and start at Stage 0.
Do not final-approve any serious cryptographic claim without gates and review.
```

## Automation Modes

- `human_in_the_loop`: safest; pauses at key checkpoints.
- `supervised_auto`: semi-auto with checkpoints.
- `overnight_auto`: ARIS-like overnight work; drafts and small experiments allowed, no automatic final serious claims.
- `full_auto_draft`: maximum automation; outputs are unverified drafts unless reviewed.

## Important Rule

CryptoFlow-Sym explores aggressively but claims conservatively.

Gate failures block finalization, not draft generation. The workflow may still produce research logs, unverified drafts, blocked reports, and negative-result reports.

## Formal Models and Source Code

For MILP/SAT/SMT/CP/monomial/division-property/source-code model results, create or update:

```text
outputs/modeling_assumptions.md
```

before using the result as evidence for any cryptographic claim.
