# T-2 — Core Cutover and Hardening: Closure Account

> **Public documentation — owner-authorized release (2026-11-01). Evidence limitations disclosed.**
>
> This account rests on:
> - the frozen specifications;
> - SQUNO's decision records;
> - production outputs that were examined;
> - sealed evidence packs identified by hash (*project records*);
> - the reviewed closure kit.
>
> **1 November 2026 evidence.** The Stage G and Stage F results come from the **operator transcripts**: the complete terminal output of both stages, shared by the operator and examined here. The final closure record and the Stage G draft were **reconstructed** from those transcripts. Each reconstruction hashes to the identity that Stage F and Stage G printed on the production host (`d93f78cc…` and `c482b4d5…`).
>
> **Not examined here:**
> - the original report files (the Stage G report and the Block D, Block E and §9.5 reports), whose results appear here as Stage G checked and printed them;
> - the DEP-11a amendment record (`272ed35b…`), which the closure record cites only by hash;
> - the individual Phase 2–4 packs.

---

## 1. Objective

T-2 runs Phases 2–4 of the Witness Core Hardening Spec rev47 "exactly as frozen". For the downstream finality requirement E2, it must close P1, P2, P3, P9, P12, P13 and P15, plus the TC-1 intake-ordering control at the Phase 4 gate. It must create no Season object.

The governing document is *T-2 — Core Cutover Execution Spec (Phases 2–4), v1.2*. SQUNO froze its lifecycle on 2026-10-06.

**How the stages work:**
- T-2 runs through stages S0 to S4, with stops STOP-2, STOP-3 and STOP-4.
- Every stop needs a sealed PASS pack and the next written authorization.
- A HOLD or FAIL ends T-2 at that point until SQUNO decides otherwise.

## 2. Baseline

A read-only review of production on 25–26 September 2026 (examined production outputs) found these gaps:

| Gap | Detail |
| --- | --- |
| Lost updates | 248 `update_id` values missing in 203 gaps: 247 updates the service received but never stored (225 callback queries, 22 messages), plus 1 that was never received |
| Reply detection | Topic-root messages were counted as replies |
| Edits | Edits were not linked to their messages |
| First-seen times | "First seen" times clustered around a restart |
| Mutable evidence | The application role could change evidence tables, and the foreign key from derived to raw rows was `ON DELETE CASCADE` |
| Version control | The deployed code was not under version control |

The Phase 0 isolation proof showed an isolated test environment byte-identical to the production code (`PHASE0_FREEZE_GATE=PASS`). rev47 was then frozen.

## 3. Design freezes

| Freeze | Date | What it fixes | Basis |
| --- | --- | --- | --- |
| Hardening Spec rev47 "Phase 0 Freeze" | 2026-09-26 | Properties P1–P15 and the phase plan | The freeze is stated in the revision. rev42 "awaits SQUNO's acceptance", so SQUNO's acceptance of rev47 is inferred. |
| T-2 Execution Spec v1.2 | 2026-10-06 | Stages, gates, closure matrix, DEP decisions | "FROZEN, SQUNO 2026-10-06" |
| S0 `DESIGN_FREEZE_T2` | 2026-10-07 | The exact build and test inputs (code, migrations 011a–e, tests, schema, fixtures, toolchain); closes DEP-4, 5 and 6 through gate results | `S0_RESULT=FROZEN` (project record), run under SQUNO's authorization |

rev47's "Phase 0 Freeze" and S0 are separate steps. S0 comes later and freezes the implementation inputs.

## 4. Phase 1

Phase 1 had to prove that each gap is real and detectable before anything was fixed. It ran 65 tests against the frozen, unchanged code, in an isolated workspace with its own database on the production host.

**Result (examined gate pack, 2026-09-26T22:52:43Z; counters only):**

```
TESTS=65  RED_EXPECTED=59/59  GUARD_GREEN=5/5  NOT_RUNNABLE_BY_DESIGN=1/1
RED_OTHER=0  GREEN_UNEXPECTED=0  GUARD_RED=0  SELFTEST 50/50
PHASE1_GATE=PASS_WITH_DEFERRED_P8
```

**What the result means.** The tests behaved exactly as predicted against the old code: 59 failed where gaps were expected, and the 5 guards passed. Production state was identical before and after the run.

**What it does not mean.** It does **not** mean P1–P15 were fixed. P8 historical parity was deferred; it was later checked offline at S0.

## 5. Phase 2

Phase 2 runs offline. It does five things:
- builds the frozen V1 build twice, to prove the build is reproducible;
- runs the full test suite;
- checks schema parity;
- rehearses the production dump in isolation (PR-1);
- emits the kits for Phase 3 and Phase 4.

Each pack must carry a review record from a reviewer who is not the builder.

**History (closure draft; production pack listing):**
- **Runs.** From 7 to 20 October 2026, Phase 2 ran repeatedly. Six packs ended HOLD, and all are preserved as superseded.
- **Passes.** Five packs passed, all carrying the same frozen build: closure/processor `88eebb28…`, dist `1bfa6db6…`, suite total 96, P9 empty, no production write. Later passes re-emitted corrected kits; they did not change the build.
- **RC10.3 runs.** Run 1 (17 October) ended HOLD. Run 2 (20 October) passed, and it is the executing Phase 2 pack for the Phase 4 continuation.
- **Kit revisions.**
  - RC9 followed SQUNO's DEP-11a decisions.
  - RC10 followed the Phase 4 base-run HOLD and SQUNO's decisions of 12 October.
  - RC9.1–RC9.3 and RC10.1–RC10.3 each followed a KORKI review that had ended HOLD with concrete defects.
  - Each defect was fixed in the next revision (§15).

## 6. Phase 3: cutover

| Date | Run | Result |
| --- | --- | --- |
| 2026-10-07 | Two attempts | Ended HOLD with no cutover-class production write. Each restarted the frozen build before step 3.7, which was allowed (§12). Preserved as superseded. |
| 2026-10-08 | Production cutover | Interrupted by a signal after step 3.14. The pack was sealed `INCOMPLETE`. The V1 service it had started kept running untouched. |
| 2026-10-08 | Completion kit | Carried the INCOMPLETE evidence forward and produced the Phase 3 PASS pack, which closes P2, P12 and P15 (§9). |

## 7. Phase 4

| Run | Date | Result |
| --- | --- | --- |
| First attempt | 2026-10-11 | `TC1=INCONCLUSIVE`, gate HOLD, no production write (§10) |
| Base run | 2026-10-12 | `TC1=PASS` (under DEP-11a); 011e committed at step 4.3; gate **HOLD** with two reasons (13 October production diagnostic, examined) |
| Continuation | 2026-11-01 | Steps 4.6, 4.7 and 4.8 only, on the unchanged HOLD base. Gate PASS with no REASON, as checked by Stage G (operator transcript). The pack itself was not opened. |

**Base-run HOLD reasons.** The REASON lines come from the 13 October production diagnostic. The causes are the builder's diagnosis, from the builder notes.
- **4.6:** the log-control step did not verify. Builder's diagnosis: a log-file permission precondition in the kit.
- **4.7:** the PR-5 full database dump (input to the full-history replay) failed. It was taken as the service role. Builder's diagnosis: after 011e that role no longer has the privileges a full dump needs.

The builder diagnosed both as defects in the kit, not in Witness Core, and recorded that its own local validation had missed them.

**The continuation:**
- **What the kit specifies.** By the design of the reviewed continuation kit (operator package):
  - it first re-checks the base predicates live and read-only;
  - its only production write is step 4.6, a corrected log-rotation configuration and log-file permissions;
  - it takes the PR-5 dump in a read-only session with elevated database rights.
- **What Stage G checked.**
  - The continuation's authorization covers PR-5 and the P10 step, and does not allow service stop or start.
  - Block D passed. It reports NOT PASS on any extra production write.
  - Stage G's own re-run of the continuation verifier passed.
  - The continuation pack itself was not opened.
- **Who authorized it.** SQUNO authorized it in writing.

**Recorded result.** This line is in the final closure record, reconstructed from the Stage F transcript. Stage G also confirmed that the base REASONs were exactly 4.6 and 4.7 and that the continuation passed:

```
PHASE4_RESULT=CLOSED_BY_VERIFIED_CONTINUATION
```

The closure record says this "is not a literal GATE=PASS of the base pack". **The base HOLD stays a HOLD in the historical record.** Accepting this interpretation is SQUNO's Stage F decision.

## 8. Database migration 011e

011e was applied in production at step 4.3 on 2026-10-12, and the service was restarted on the new permissions (13 October production diagnostic, examined; the ledger lists 011e). Its contents are described in [ARCHITECTURE.md](ARCHITECTURE.md) §6.

**After 011e:**
- **Data rollback** is no longer possible, by design.
- **R1**, the rollback to the frozen pre-hardening build, is refused.
- **Code rollback** remains possible only to builds made after the cutover.

## 9. Closure matrix

**Where these outcomes come from.** These are the closure-matrix lines of the closure draft. Production Stage G wrote that draft on 2026-11-01, and it is byte-identical to the draft that was rendered and reviewed in advance:
- Stage G printed the draft in full and printed its SHA256 `c482b4d5…` (operator transcript).
- For each line, Stage G printed its own PASS check.

**What was opened.** Each line is bound by hash to the sealed pack behind it. Those packs were not opened in preparing this documentation (project records).

| Item | Closed at | Recorded outcome |
| --- | --- | --- |
| P1 | 4.3 / 4.4 | 011e applied; production catalog = isolated database; live re-check equal on 2026-11-01 (Stage G check) |
| P2 | Phase 3 gate | `bot_id` null 0, `observed_at` null 0; window W checks 0 |
| P3 | Phase 4 | Duplicate identities 0; gate restarts `ACK_CONFIRMED` 2/2 |
| P9 | 4.5 | Schema parity diff empty. Ledger: 011a–011e with SHA256; 005 and 008 have no file by design. |
| P12 | Phase 3 gate | 213 live accounting batches ≥ K = 30 |
| P13 | Phase 4 | Ack after commit (TC-1 L3 ordering violations 0); legacy fallback file archived |
| P15 | Phase 3 gate | p15 tests PASS on the deployed build; no live fault injection |
| TC-1 | Phase 4 gate | PASS under rule DEP-11a (§10) |

## 10. TC-1 and the DEP-11a amendment

**What TC-1 checks.** TC-1 verifies that intake commits updates strictly in order and acknowledges only committed ones. It has three layers:
- **L1:** static review (reviewer: KORKI);
- **L2:** tests;
- **L3:** a live read-only check across an observation window W.

**The original requirement was not met.** L3(e) required **M = 20** batches with two or more updates.
- Under this deployment's polling, almost every update arrived as a batch of its own.
- According to the builder notes, the formal INCONCLUSIVE pack of 11 October sealed 496 in-window accounting lines but **only one** multi-update batch (**m = 1**). The pack itself was not opened (project record).
- The specification classes this as INCONCLUSIVE, so the first Phase 4 attempt ended HOLD.

**How DEP-11a came about:**

| Date | Step | Who |
| --- | --- | --- |
| 2026-10-11 | Production W showed only one multi-update batch. SQUNO set the DEP-11a floors and required a formal INCONCLUSIVE pack as provenance. That pack was sealed the same day. | SQUNO |
| — | The mechanism (extra ordering checks, kit floors, the amendment format) | Designed and implemented by the AI builder (RC9); reviewed by KORKI (builder notes) |
| 2026-10-12 | The hash-bound amendment record was written from the pack's sealed numbers and independently reviewed | — |

**The floors DEP-11a sets:**
- at least 1 live multi-update batch;
- at least 21 ordered lines;
- at least 20 ordered pairs;
- zero ordering mismatches.

**Result.** In the base run of 12 October, W was measured again: m = 1, with **506 lines and 505 pairs, and 0 mismatches**. TC-1 passed under DEP-11a.

**Disclosure.**
- **The rule was changed after the observation.** The closure record states that DEP-11a was decided on the sealed INCONCLUSIVE pack.
- **What the kit requires of the amendment.** According to the builder notes, the reviewed kit accepts a DEP-11a amendment only if it declares `RULE_CHANGED_AFTER_OBSERVATION=YES`, and refuses `=NO`.
- **The amendment record itself** (`272ed35b…`) was not examined. The closure record cites it by hash as independently reviewed.
- The sealed M = 20 value was never rewritten.
- **The original M = 20 condition is not described anywhere in this documentation as satisfied.**

## 11. Cutover evidence and legacy provenance

**D3 (2026-10-06)** classified the provenance of all records from before the cutover. The production gate ended `HOLD`. Its post-run verifier reported `FAIL (1)`, which was a false positive in a secret scan. Both results are preserved unchanged. SQUNO's decision v2 resolved the HOLD reasons by explicit classification:

| Segment | Raw range | Classification |
| --- | --- | --- |
| A | 1–340 | Migrated from an earlier database; unattributed |
| B | 341–1992 | Legacy intake; unattributed |
| C | 1993–20120 | `LEGACY_VPS_INTAKE_ATTRIBUTED` / `LEGACY_ATTRIBUTED_POSTHOC` (the frozen processor reproduced it exactly) |
| Cutover delta | 20121 onward | Unresolved until replayed at cutover |

`D3 = CLOSED_WITH_CUTOVER_DELTA`. In the decision's own words, this "is not a retroactive PASS verdict".

**CUT-1 (step 3.4).** CUT-1 replayed the cutover delta (raw 20121–20364) under the frozen processor. Expected and actual rows matched (393).
- The range is recorded as a separate cutover-delta segment, classified `LEGACY_ATTRIBUTED_POSTHOC`.
- **Segment C stays 1993–20120.**

**CUT-2.** CUT-2 archived the legacy local fallback file (292 bytes, identified by hash). The D3 decision classed its three lines as test artifacts, not evidence. The file was archived, never deleted.

## 12. Rollback and restarts

**R1 (rollback) was never executed.** The closure checks show this by absence:
- there is no R1 record;
- no cutover or Phase 4 pack lists a restore, a rollback or a frozen-build start among its production writes;
- after the cutover stop, only the V1 build was started.

**Frozen-build restarts are not R1.** The closure checks allow a restart of the frozen (pre-hardening) build only in a **superseded attempt before cutover step 3.7**, and only with a clean H-F integrity record. From step 3.7 until the first confirmed graceful stop, the frozen code may never start; this is the DEP-7 boundary.

**Two such restarts took place**, one in each of the two superseded Phase 3 attempts, according to Stage G's §9.4 check (operator transcript). The attempts started on 7 October 2026 at 23:49:57Z and 23:59:02Z UTC; the times come from the attempt pack names. Stage G does not print the restart times.

| Superseded Phase 3 attempt | Event | H-F integrity check | Classification |
| --- | --- | --- | --- |
| `t2_phase3_20261007T234957Z` | Frozen build restarted before step 3.7 | 0/0 (clean) | Allowed under DEP-7 |
| `t2_phase3_20261007T235902Z` | Frozen build restarted before step 3.7 | 0/0 (clean) | Allowed under DEP-7 |

Neither attempt made a cutover-class production write, and neither is a rollback. Stage G reports these occurrences; the two attempt packs themselves were not opened in preparing this documentation.

**Gate restarts of the V1 build** are part of the procedure, not rollbacks. They were checked to be projection-neutral (P7) and acknowledgement-confirmed.

## 13. Verification on 1 November 2026

| Block | What it checks | Kind | Result |
| --- | --- | --- | --- |
| Block D | The continuation pack, gate bindings and read-only host state, plus a separate clean re-run of the continuation verifier. No database query. | Verifier re-run over sealed evidence | Stage G checked the Block D report and transcript by hash and content: PASS. Stage G's own re-run of the continuation verifier: PASS, 140 OK lines. The Block D report itself was not examined. |
| SPEC §9.5 read | One read-only, repeatable-read snapshot. Four counters must be 0: violations, failed V1 derivations, rollback redelivery duplicates, rollback orphan derivations. Run under a written DEP-9 authorization (`6e4743ef…`, bound by hash in Stage G's check). By the reviewed read package, that authorization is SQUNO's act and permits one read (`RUNS=ONE`). The authorization file itself was not examined. | **Fresh live database read** | Stage G read the report (`0bb72207…`): 0/0/0/0, PASS. Also 0 at the Phase 3 gate, at re-derivation and in W. The report itself was not examined. |
| Block E | Collects the identities, gates and results of every pack. Reads files only. | Sealed historical evidence | Stage G check: PASS (report `b330f1da…`, read-only, names the base and continuation pairs). The report itself was not examined. |
| Stage G | Re-runs the five phase verifiers against the sealed packs (S0, Phase 2, Phase 3, Phase 4 base, continuation: all PASS). Checks Block D, §9.5 and Block E. Writes the closure draft. | Verifier re-runs over sealed evidence, plus read-only checks of host files; no database query | `STAGE_G_EXIT=0`. Draft `c482b4d5…`; report `418cea56…` (operator transcript; the report file was not examined). |

**Re-run versus sealed evidence.** A verifier re-run gives a new result over sealed evidence. It confirms that the evidence is intact and consistent. It does not recreate the event.

**Live checks.** Among the four blocks, only the §9.5 read queried the live database.

Earlier that day, the continuation itself read production read-only:
- **Live re-checks.** The closure record shows the live re-checks of P1/011e and P9 as equal. The kit also specifies live re-checks of the ledger and P7 (operator package).
- **The PR-5 dump.**

**What links the production result to the reviewed kit:**
- The production host ran the same closure kit that was reviewed: the archive is `c342c54a…`, and its integrity check passed on the host (operator transcript).
- The closure draft that Stage G had to write was rendered in advance and reviewed. Its SHA256 is `c482b4d5…`.
- Production Stage G printed that same hash for the draft it wrote.
- Stage F refuses any other draft.

## 14. Formal closure

Stage F is SQUNO's step. It changes only the twelve `PENDING` lines of the reviewed draft.

**Eight attestations** (scope items that T-2 did not change, open or start):

| Attestation | Meaning |
| --- | --- |
| `SCOPE_R01_R56_UNCHANGED` | Ruleset 8 rules R01–R56 unchanged |
| `SCOPE_G_UNCHANGED` | Parameter G unchanged |
| `SCOPE_E2_UNCHANGED` | E2 unchanged |
| `SCOPE_M3_UNCHANGED` | M3 unchanged |
| `SCOPE_D3_UNCHANGED` | D3 unchanged |
| `SCOPE_NO_SEASON_OBJECT` | No Season object created |
| `SCOPE_SEC1_NOT_HANDLED` | SEC-1 not handled under T-2 |
| `SCOPE_T4_T5_NOT_STARTED_UNDER_T2` | T-4 and T-5 not started under T-2 |

**The other four PENDING lines** are the Stage G report hash, the decision `T2_CLOSED by SQUNO`, `T2_CLOSURE=CLOSED` and the date.

**Recorded closure (Stage F operator transcript):**
- `T2_CLOSURE=CLOSED`, decided by SQUNO at 2026-11-01T14:50:50Z from the reviewed draft `c482b4d5…`.
- Stage F exited with return code 0.
- It printed the final record's SHA256 on the production host: `d93f78cc358c99ec167bdc055d6b59fbd77a8da7c12e01797da5b37c01cece34`.

**How the record was checked.** Stage F printed the 73-line record in full. A copy reconstructed from that output hashes to the same value. The reconstruction differs from the reviewed draft in exactly the twelve expected lines:
- eight `ATTESTED_BY_SQUNO`;
- the Stage G report hash;
- the decision line;
- `T2_CLOSURE`;
- `DATE`.

As far as the operator transcript can show it, the reconstruction is byte-identical to the record that Stage F wrote and hashed on the host (`RECONSTRUCTED_FROM_OPERATOR_TERMINAL_LOG`). It is a reconstruction from terminal output, **not** a file copied from the production host. The eight attestations remain SQUNO's own attestation.

## 15. Build and decision provenance

| Date (UTC) | Event | Act | Party | Evidence type |
| --- | --- | --- | --- | --- |
| 2026-09-25 | Baseline review and gap closure | Executed | Project operator | Production output (examined) |
| 2026-09-26 | Isolation PASS; rev47 frozen | Executed; designed | Operator; spec bylined to SQUNO (drafting not attributed); acceptance by SQUNO inferred | Production output; spec |
| 2026-09-26 | Phase 1 gate `PASS_WITH_DEFERRED_P8` | Executed, verified | Project operator | Production pack (examined) |
| 2026-10-06 | D3 gate HOLD, then decision `CLOSED_WITH_CUTOVER_DELTA` | Executed; approved | Operator; **SQUNO** | Production pack; decision record (examined) |
| 2026-10-06 | T-2 v1.2 frozen; DEP-1/2/3/7/8 decided; Ruleset 8 frozen; M3 read | Approved; executed | **SQUNO**; operator | Spec; production output |
| 2026-10-07 | Class A candidate (hardened application, migrations, tests); S0 `FROZEN` | Implemented; verified | AI builder; operator | Builder notes; project record |
| 2026-10-07 to 12 | Phase 2 runs (HOLDs and PASSes); kits rc1 to rc9.3; reviews. On 7 October, two superseded Phase 3 attempts each made an allowed frozen-build restart before step 3.7 (§12). | Implemented, reviewed, executed | AI builder; reviewer agents and KORKI; operator | Builder notes; production pack listing |
| 2026-10-07/08 | Phase 3: two HOLD attempts; cutover INCOMPLETE; completed PASS | Executed | Operator | Project records |
| 2026-10-11 | DEP-11a floors set; Phase 4 attempt INCONCLUSIVE | Approved; executed | **SQUNO**; operator | Builder notes; project record |
| 2026-10-12 | Phase 4 base: TC-1 PASS, 011e applied, HOLD at 4.6/4.7; SQUNO approves the fixes and the continuation path; DEP-11a record written and reviewed | Executed; approved; reviewed | Operator; **SQUNO**; reviewer | 13 October production diagnostic (examined, EV-15); decision record |
| 2026-10-12 | Kits RC10 (after the base HOLD) to RC10.3 (after KORKI findings) | Implemented; reviewed | AI builder; KORKI | Builder notes |
| 2026-10-13 | Read-only pre-continuation diagnostic | Executed | Operator | Production transcript (examined) |
| 2026-10-17 / 2026-10-20 | Phase 2 RC10.3 run 1 HOLD; run 2 PASS (executing pack for the continuation) | Executed | Operator | Closure draft |
| 2026-11-01 | Continuation, Block D, §9.5 read, Block E | Executed; verified | Operator | Checked by Stage G (operator transcript); original reports not examined |
| 2026-11-01 14:44 | Stage G: all checks PASS, draft written, `STAGE_G_EXIT=0` | Verified | Operator | Operator transcript (examined) |
| 2026-11-01 14:50:50 | Stage F: eight attestations, `T2_CLOSED by SQUNO`, record `d93f78cc…` | Approved | **SQUNO** | Operator transcript (examined); record reconstructed, hash matches |
| 2026-11-01 | KORKI checks the Stage G and Stage F transcripts after execution and recomputes the record hash and the 12-line change | Reviewed | KORKI (AI) | KORKI RC3 review (EV-28). This is a second-path AI review of operator-supplied output, not an audit. KORKI had no access to the production host or to the original Stage G, Block D, Block E and §9.5 report files. The reconstructed record it checked was built from the operator logs in KORKI's own handoff; ASTER recomputed its hash separately. |

"Operator" means the project operator, acting under SQUNO's written authorization. The builder notes name SQUNO as the operator.

**HOLDs and failures are preserved, not erased:**
- 6 Phase 2 HOLD packs;
- 2 Phase 3 HOLD packs and 1 INCOMPLETE pack;
- the INCONCLUSIVE Phase 4 attempt;
- the Phase 4 base HOLD;
- the D3 gate HOLD and its verifier FAIL.

**Defects found in the builder's kits during review:**
- verifier paths that would accept forged packs;
- an offset edge case;
- false TC-1 violations;
- unpinned authorizations;
- a verifier that could exit 0 on failure;
- informational checks that should have been gates.

Each was fixed in a new revision before production use.

## 16. Scope boundary

**What T-2 closure establishes.** This rests on the closure record as reconstructed from the Stage F transcript, with its hash matching:
- the hardened V1 build is in production;
- the T-2 closure matrix closed on recorded evidence;
- TC-1 passed under DEP-11a.

**What it does not establish:**
- Season Genesis (T-4), the Season Engine (T-5) or Proof Season;
- resolution of SEC-01 or SEC-1;
- general process supervision;
- behaviour at activity levels higher than those observed.

See [LIMITATIONS.md](LIMITATIONS.md).
