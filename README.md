# your-fixed-task

# Dynamo - Fix the Broken Terminal-Bench Task

This repository contains a corrected Harbor Terminal-Bench 2 task for the **"Fix the Broken Terminal-Bench Task"** assessment.

## Objective

The original task intentionally contained multiple authoring defects. The goal was to repair the task so it conforms to the Harbor Terminal-Bench 2 specification while remaining reproducible and honestly verifiable.

The task requires generating a JSON report from an Apache-style access log.

---

## Fixes Performed

### task.toml

- Corrected `artifacts` to a TOML array
- Updated artifact path to `/app/report.json`
- Added missing required metadata fields
- Added valid `task`, `agent`, `verifier`, and `environment` sections
- Verified Harbor TB2 format compliance

### instruction.md

- Rewritten in Harbor prompt style
- Added explicit output location
- Added numbered success criteria
- Removed ambiguity
- Kept instructions under Harbor token limits

### Docker Environment

- Replaced floating Docker image tag with a pinned SHA256 digest
- Removed solution leakage from the runtime image
- Installed verifier dependencies during image build
- Eliminated runtime package installation

### Verifier

- Updated tests to validate JSON contents rather than simply checking file existence
- Added comments mapping tests to instruction success criteria
- Updated `test.sh` to:
  - execute pytest
  - generate CTRF output
  - write `/logs/verifier/reward.txt`

### Four-Way Consistency

Verified consistency across:

- instruction.md
- task.toml
- solution output
- verifier expectations

All components now reference:

```
/app/report.json
```

---

## Repository Structure

```
log-report/
├── task.toml
├── instruction.md
├── environment/
│   └── Dockerfile
├── solution/
│   └── solve.sh
└── tests/
    ├── test_outputs.py
    └── test.sh
```

---

## Running

Build and execute using Harbor:

```bash
harbor run -p log-report -a oracle
```

Expected result:

```
reward.txt = 1
```

Verify the task rejects an empty agent:

```bash
harbor run -p log-report --agent nop
```

Expected result:

```
reward.txt = 0
```

---

## Notes

This repository demonstrates proper Harbor TB2 task authoring practices including:

- reproducible environments
- pinned Docker images
- no solution leakage
- deterministic verification
- aligned instructions and tests
- correct verifier output

---

## License

Created for the Dynamo Terminal-Bench assessment.
