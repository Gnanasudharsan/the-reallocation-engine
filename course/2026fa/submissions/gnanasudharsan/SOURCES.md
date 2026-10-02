# SOURCES — de-mlops-h1b

## Executive summary

**What this is:** credits for everything this submission builds on: the repository and its
rules, the data files, the outside data source, the tools, and the AI assistant.

**Why read it:** it separates what came from others from what this submission added, and
what the AI did from what the student decided.

**What it says:** all data is from the course repository's shipped files plus a public job
board. All code ran locally. Most code and drafting was done by an AI coding agent, under
the student's direction and approval. Final judgment and sign-off belong to the student.

## Repository and governing documents

- *The Reallocation Engine* repository, Nik Bear Brown (`nikbearbrown/the-reallocation-engine`), forked as `Gnanasudharsan/the-reallocation-engine`.
- `SNICKERDOODLE.md` (constitution), `DOMAIN.md`, `AGENTS.md`, `CONTRIBUTING.md`, `scripts/contrib/README.md`, `recipes/cases/README.md`, `recipes/_shared.md` (run-log template).
- Style models: `recipes/local-wage-adjustment.md`, `recipes/local-wage-adjustment.card.md`.
- Course assignment: "The Reallocation Engine — Recipe Design Assignment", INFO 7375, Fall 2026.

## Code reused (unchanged)

- `scripts/score/role-scorer.mjs`: the Ch.11 combiner; spawned by the prototype, never copied.
- `scripts/ats/check-liveness.mjs` (`npm run ats:liveness`): posting liveness.
- Company-name normalization rule ported from `normalize_company_name()` in `scripts/sec/sec-all-quarters.py` (suffix list and stripping rule), so CSV and Form D names join by the same rule.

## Data

| Source | Path | Notes |
|---|---|---|
| 80 Days to Stay company table (H-1B approvals/denials, top sponsored titles, funding) | `data/80-days-to-stay/80-days-csv/mapped_student_employment_targets_v3.csv` | shipped with the repo; 13 rows copied into `fixtures/companies.sample.csv` (officer, director, phone columns dropped) |
| SEC Form D processed samples | `data/sec/form-d/processed/sample/*.sample.json` | 50 companies per quarter; 3 records copied into `fixtures/formd/` with related persons and addresses removed |
| BLS OEWS / O*NET compact | `data/bls/compact/soc_occupation_compact.csv` | context only |
| Greenhouse public job boards | `https://boards-api.greenhouse.io/v1/boards/<board>/jobs` (databricks, twilio, instacart, roku, robinhood, roblox, upstart) | read-only GET, 2026-10-01; titles, URLs, locations of 11 public postings in `sample/postings.json` |
| Persona | `fixtures/persona*.json` | **fictional** (`@example.com`); no real dates or personal data |

No file from `private/`, `data/ats/`, or `search/resume.json` was read or committed.

## Tools

Node.js v26.8.2 (built-in `node:test`), Python 3.14 (data inspection only), git, Claude Code
desktop app.

## AI contribution

**AI:** Claude Code (model Claude Opus 5.5, Anthropic), used as a coding agent in this repository on 2026-10-01.

- **The AI did:** profiled the data; drafted CHANGE-BRIEF §1–§4; wrote the recipe, card, README, prototype, tests, and fixtures; fetched the public postings; ran liveness, the scorer, tests, conformance, and the PII scan; ran the break attempts; drafted this file, the worked run, the run log, and the AI-attributed entries in FRICTIONAL.md.
- **The student decided:** the career situation and scope (rejecting the earlier Data Scientist scope); the CHANGE-BRIEF §5 prediction (written by the student); approval of the implementation plan; the file paths and commands to use.
- **Still the student's to do:** replace the placeholder fit ratings and start dates; accept or replace the tier→p values; verify the visa-timeline assumptions; re-run and sign the attestation; write the first-person sections of FRICTIONAL.md.

# Domain Justification: Data Engineering & MLOps H-1B Visa Triage

## 1. User & Situation
- **Target User:** International Master's student in Data Analytics Engineering graduating December 2026, targeting Data Engineer and MLOps / ML Platform Engineer roles.
- **Situation:** Facing an F-1 OPT unemployment window countdown and strict H-1B sponsorship requirements. The primary hurdle is information asymmetry—knowing which companies actually sponsor visas for data/ML roles versus those that only appear to hire.

## 2. Information Asymmetry Addressed
Without this engine, a student cannot easily see:
- Whether a company with recent funding (SEC Form D) has historical DOL H-1B approvals for data roles (as ~95% of companies in the file have blank approval records).
- Whether a posting is a "ghost job" or dead link before spending hours tailoring a résumé.
- Whether timeline constraints (EAD start date vs. role start date and lottery windows) are viable.

## 3. Engine Layer Connection & The 3-3-2 Split
- **Layers Used:** 80 Days to Stay (sponsorship & funding), Job-Ops (liveness), and Cognitive Pivot (BLS SOC code context).
- **3-3-2 Impact:** Takes over the heavy research phase of the "2" (researching and tailoring applications), saving an estimated 4–6 hours per week and cleanly routing dead/non-sponsoring roles into the networking queue ("Consider / Network").

## 4. Domain-Specific Failure Modes
- **Title Mismatch:** "MLOps" appears in zero sponsored job title strings, requiring model judgment mappings.
- **Legal vs. Brand Name Discrepancies:** Companies like Upstart appear only under legal entity names (`UPSTART NETWORK INC`), which would cause false skips if not normalized.