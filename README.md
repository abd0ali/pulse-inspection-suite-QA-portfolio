# ROV Inspection Software QA Portfolio — Pulse Inspection Suite

Real production QA work by **Abdelrahman Ali**, SQA Engineer — redacted for public sharing. Company and client names below have been changed; the system design, workflows, test artifacts, and defect data are drawn directly from work I actually did.

> **A note on anonymization:** I test proprietary offshore inspection software under an internal-use classification. The product names, company name, and client names in this repository are fictionalized. The testing approach, documentation structure, test coverage, and defect-tracing work are real.

---

## The system under test

**Pulse Inspection Suite** is a three-application Windows package used during live ROV (Remotely Operated Vehicle) subsea pipeline and structure inspections:

| App | Role |
|---|---|
| **DataLink** | Captures live readings from every connected sensor (navigation, depth, cathodic protection, DMS, etc. — each sensor on its own dedicated port/socket) and streams them to the orchestrator and a SQL Server database in real time. |
| **MissionHub** | The control/orchestration layer. Connects to DataLink over IP, handles project/pipe (or structure) setup, remotely initializes one or more video-capture machines, designs and pushes fixed/dynamic video overlays, drives recording start/stop/split, and logs inspection events — including matching new events against a historical baseline. |
| **StreamCap** | Runs on each video-capture machine (one per camera). Captures, displays, and records video from industrial capture cards or IP cameras, burns in the overlay MissionHub configures, and saves recordings locally — entirely under remote command from MissionHub. |

**Typical test environment:** 5 Windows 10 machines (1 MissionHub, 1 DataLink, 3 StreamCap — one per camera), SQL Server backend. No physical sensors on-site for testing — sensor strings are simulated with a lightweight IP/port sender tool as part of test setup.

## My role

I own QA for this system end to end:

- Design and execute **manual, smoke, regression, negative/edge, combination, and stability** test suites against every major release
- Log, triage, and track defects back to the test case that should have caught them
- Built the **formal QA documentation process** this repository showcases — it didn't exist before I introduced it, and it's designed to keep extending as new features and releases come in, not to be a one-time artifact

## What's in this repository

| File | What it is |
|---|---|
| [`docs/Master-Test-Plan.docx`](docs/Master-Test-Plan.docx) | Full test plan — scope, test levels, approach per component, environment, entry/exit criteria, risk register, release schedule |
| [`docs/Test-Procedures.docx`](docs/Test-Procedures.docx) | Nine step-by-step SOPs, one per stage of the workflow — from sensor simulation setup through end-to-end smoke check |
| [`docs/Test-Strategy.docx`](docs/Test-Strategy.docx) | Per-application strategy, a feature coverage map, test data management, and traceability approach |
| [`test-cases/Test-Case-Repository.xlsx`](test-cases/Test-Case-Repository.xlsx) | ~195 test cases across smoke, regression, DB validation, negative/edge, combination, stress/stability, and structure-mode suites |
| [`test-cases/Bug-Retest-Sequence.xlsx`](test-cases/Bug-Retest-Sequence.xlsx) | ~97 defects re-sequenced into the order a tester actually walks through the application, so retest and regression happen in one pass instead of jumping between unrelated screens |

## A few things I'm proud of here

- Took a team with **no formal test process** to a documented, repeatable one that holds up across every major release — built to extend, not a one-off document.
- Traced the large majority of a 97-defect backlog back to the exact test case that should have caught it, turning a flat bug list into real, closeable coverage gaps.
- Designed a reusable classification rule (feature/module → workflow stage) so sequencing retest and regression work isn't a one-off manual sort — it's a rule anyone on the team can keep applying as the system grows.
- All of it built and run manually, release after release, with no dedicated test-automation budget — the discipline is in the process, not the tooling.
