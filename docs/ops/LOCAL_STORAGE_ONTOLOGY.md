# Local Storage Ontology (Draft)

This document defines a **human-readable, agent-friendly** directory layout for storing Long Horizon Agent artifacts and outputs on a local filesystem.

The goal is to make it easy to answer questions like:

- "Show me the approved extractor for Amex from JobSpec `job_123`."
- "Give me the Chase outputs for May 2024."
- "Find the run summary for `run_2024_05_12_001`."

The structure mirrors the system's artifact lifecycle (draft → pending → approved → executed) and preserves auditability.

---

## Guiding Principles

1. **Human-readable paths**: A non-technical user should be able to infer a path from a sentence.
2. **Stable identifiers**: `job_id`, `source_id`, and `run_id` are first-class keys.
3. **Separation of concerns**: Review artifacts stay separate from runtime outputs.
4. **Agent-friendly traversal**: Bucketing by source and time makes queries simple.

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
  - `long-horizon/exports/normalized/plaid/2024/04/` through `2024/06/`

---

## Modal Alignment

If Modal volumes are used for `pending/`, `approved/`, `exports/`, and `runs/`, the same directory structure can be mirrored locally for simplicity and auditability.

---

## Open Questions

- Should we include an optional `org_id` or `principal_id` layer above `long-horizon/` if multi-user support is ever added?
- Should `exports/` contain both per-run outputs and consolidated outputs, or remain consolidated only?

