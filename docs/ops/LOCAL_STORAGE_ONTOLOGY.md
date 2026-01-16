# Local Storage Ontology

This document defines a **human-readable, agent-friendly** directory layout for storing Long Horizon Agent artifacts and outputs on a local filesystem.

The goal is to make it easy to answer questions like:

- "Show me the approved extractor for Amex from JobSpec `job_123`."
- "Give me the Chase outputs for May 2024."
- "Find the run summary for `run_chase_20240512_001`."

The structure mirrors the system's artifact lifecycle (draft → pending → approved → executed) and preserves auditability.

---

## Guiding Principles

1. **Human-readable paths**: A non-technical user should be able to infer a path from a sentence.
2. **Stable identifiers**: `job_id`, `source_id`, and `run_id` are first-class keys.
3. **Separation of concerns**: Review artifacts stay separate from runtime outputs.
4. **Agent-friendly traversal**: Bucketing by source and time makes queries simple.
5. **Immutable artifacts**: Approved artifacts never change; new runs create new outputs.

---

## Ontology Overview

- **Specs**: Step 1 outputs tied to a JobSpec.
- **Artifacts**: Step 2 reviewable outputs (pending/approved).
- **Runs**: Per-execution outputs and metadata.
- **Exports**: Consolidated, analysis-ready outputs by source + time.

---

## Proposed Directory Layout

```
<root>/
  long-horizon/
    specs/
      <job_id>/
        job_spec.json
        DecisionBrief.md
        SourcePlan.md
        RiskAssessment.md

    artifacts/
      pending/
        <job_id>/
          <source_id>/
            extractor/
            TEST_REPORT.md
            RUNBOOK.md
            RISKS.md
            sample_output.jsonl
            SCHEDULE.json
      approved/
        <job_id>/
          <source_id>/
            extractor/
            TEST_REPORT.md
            RUNBOOK.md
            RISKS.md
            sample_output.jsonl
            SCHEDULE.json

    runs/
      <source_id>/
        <YYYY>/
          <MM>/
            <run_id>/
              run_summary.json
              output/
                normalized/
                  tx_v1.jsonl
                raw/
                  <raw_artifact_files>

    exports/
      normalized/
        tx_v1/
          <source_id>/
            <YYYY>/
              <MM>/
                tx_v1.jsonl

      raw/
        <source_id>/
          <YYYY>/
            <MM>/
              <raw_artifact_files>
```

---

## Path Examples

- **Approved extractor** for `job_123` + `amex`:
  - `long-horizon/artifacts/approved/job_123/amex/`
- **Run outputs** for `chase` in May 2024:
  - `long-horizon/runs/chase/2024/05/`
- **Normalized exports** for `plaid` in Q2 2024:
  - `long-horizon/exports/normalized/tx_v1/plaid/2024/04/` through `2024/06/`

---

## Consolidation Strategy

- **Runs/** contain per-execution outputs and are immutable.
- **Exports/** contain consolidated, analysis-ready outputs by source and month.
- Consolidation can be implemented as a deterministic merge of run outputs for the given bucket.

---

## Optional Extensions (If Needed Later)

- **Multi-user support**: add `<principal_id>/` above `long-horizon/`.
- **Account-level bucketing**: add `<account_id>/` under `runs/<source_id>/` and `exports/normalized/`.
- **Alternate schema versions**: add `tx_v2/` beside `tx_v1/` if/when introduced.

---

## Modal Alignment

If Modal volumes are used for `pending/`, `approved/`, `exports/`, and `runs/`, the same directory structure can be mirrored locally for simplicity and auditability.

