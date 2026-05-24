# CryptoFlow-Sym Agent Guide

This is the lightweight routing guide for agents entering this workspace.

## Source of Truth

1. User's current explicit instruction.
2. `configs/config.sym.json` if it exists.
3. `configs/workflow.sym.yaml`.
4. `skills/cryptoflow-sym-pipeline/SKILL.md`.
5. `configs/config.sym.example.json`.

## Startup

- Treat `configs/config.sym.example.json` as a template.
- For a real project, create `configs/config.sym.json` from the example and fill target-specific fields.
- Start at `stage_0_initialize_project`.
- Record current state in `pipeline/status.json`.

## Safety and Claims

- Do not final-approve serious cryptographic claims in `overnight_auto` or `full_auto_draft`.
- Gate failures may produce drafts, blocked reports, validation gaps, modeling assumptions, and negative results.
- Serious claims require evidence, relevant gates, and review.
- Lightweight checks do not count against formal review rounds.

## Formal Models

When a claim depends on a MILP/SAT/SMT/CP/monomial/division-property/source-code model, create or update `outputs/modeling_assumptions.md`.
