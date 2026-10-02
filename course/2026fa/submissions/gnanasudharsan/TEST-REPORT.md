# Test Report: gnanasudharsan-de-mlops-h1b

## 1. Toolchain Baseline (Before & After)
- **Before Run:** `npm run doctor` passed; `npm run verify` passed (with `.venv` PyYAML installed).
- **After Run:** `npm run doctor` passed; `npm run verify` passed cleanly.

## 2. Real Sample Run Output
- Evaluated 12 postings → Scored 6 (Apply: 2, Consider: 4, Skip: 0) · On hold: 6 · Not-apply rate: 50%.
- Output saved to `course/2026fa/submissions/gnanasudharsan/runs/2026-10-01/`.

## 3. Failure Cases Exercised
- **Mutation Test (Blank sponsorship rule):** Caught by offline tests (2 tests failed, successfully reverted).
- **Pre-OPT Status Trap Persona:** Correctly caught and halted by profile gate check.
- **Past Dates Persona:** Exited cleanly without writing a report.
- **Schema Drift (Renamed CSV column):** Failed safely with exit code 2.

## 4. Git Diff Stat
- Confined strictly to namespaced paths: `course/2026fa/submissions/gnanasudharsan/`, `recipes/cases/2026fa/`, and `scripts/contrib/2026fa/gnanasudharsan-de-mlops-h1b/`.