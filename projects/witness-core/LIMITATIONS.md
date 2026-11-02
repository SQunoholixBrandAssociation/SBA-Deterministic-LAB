# Witness Core V1 — Limitations

> **Public documentation — owner-authorized release (2026-11-01). Evidence limitations disclosed.**
> Items marked *(owner-reported)* rest on the owner's statement alone.

## 1. TC-1: the original requirement was not met

TC-1's live layer required **M = 20** multi-update batches within the observation window.

**What was observed:**
- The INCONCLUSIVE Phase 4 attempt of 11 October recorded **m = 1**, across 496 in-window accounting lines (builder notes; the pack was not opened).
- The base run of 12 October measured the window again: still m = 1, with 506 ordered lines, 505 pairs and 0 mismatches.

**How TC-1 passed.** TC-1 passed only under the DEP-11a amendment:
- SQUNO set its floors **after** the first observation.
- According to the builder notes, the amendment must record this (`RULE_CHANGED_AFTER_OBSERVATION=YES`). The amendment file itself was not examined (see §13).
- The floors were met.

**What this means.** Ordered commit across many updates in one batch is **less directly evidenced** than a full M = 20 run would have made it. The original M = 20 condition remains unmet.

## 2. Low-activity conditions

With this deployment's long-polling intake, almost every update arrived as a batch of its own. At the observed traffic, the original M = 20 floor was therefore out of reach. Activity in AGORA, the SBA community, was low, and this limited the TC-1 batch sample *(owner-reported)*.

All results describe the conditions that were observed. They do not show behaviour at higher activity levels. They say nothing about future participation.

## 3. Historical HOLDs are preserved

| Event | How it was closed | What stays on record |
| --- | --- | --- |
| Phase 4 base run | A separately authorized continuation (2026-11-01), recorded as `PHASE4_RESULT=CLOSED_BY_VERIFIED_CONTINUATION` in the closure record, which was reconstructed from the Stage F transcript | The base run ended HOLD, and it is not restated as a PASS |
| D3 gate | A provenance decision | The gate stays HOLD, and its verifier result stays FAIL (1) |

Also kept as evidence:
- six Phase 2 HOLDs;
- two Phase 3 HOLDs and one INCOMPLETE pack;
- one INCONCLUSIVE Phase 4 attempt.

## 4. Known data gaps from before hardening

- **Lost updates.** At the 25 September 2026 baseline, 248 `update_id` values were missing, in 203 gaps. Of these, 247 were updates the service received but never stored, and 1 was never received. These gaps are permanent.
- **Legacy rows with weaker provenance.**
  - Segments A and B (before 14 July 2026) remain **unattributed**.
  - In legacy Segment C, the old database constraints silently suppressed 3 derived events. The D3 decision records this as a known issue, and the rows were not rewritten.
- **Telegram retention.** Telegram keeps undelivered updates for about 24 hours. A database outage longer than that loses data. This is by design and is stated openly.

## 5. Verification boundary

Every result covers a specific check, at a specific time, on a specific system state. Passing checks show that those checks passed. They do not show that every possible failure is absent.

Specific limits:
- P15 has no live fault injection, by design.
- A replay comparison alone does not prove fold equality.
- Most closure checks re-verify sealed evidence. The original 1 November report files and the continuation pack's own outputs, including the replay output, were not themselves examined. Their results are known from Stage G's checks in the operator transcript.
- On 1 November 2026, only these looked at live state again:
  - the §9.5 read;
  - the continuation's read-only live re-checks, its runtime-environment check (step 4.8a) and its PR-5 dump;
  - Block D's read-only host-state checks (no database query);
  - the operator's host pre-checks.
- The continuation also made one production write: the corrected log-rotation configuration and log-file permissions (P10, step 4.6).
- Many Phase 2–4 results in this documentation are **project records**. They are bound by hash, but they were not opened here.

## 6. Assurance levels

| Level | What it is | Status |
| --- | --- | --- |
| First-party verification | The project's own gates, verifiers and evidence packs | Recorded for every phase |
| Second-path AI review | KORKI, an AI reviewer separate from the builder. Before 12 October, unnamed separate reviewer agents. | Reviewed the T-2 kits, operator packages and closure package before use, offline, on packages and fixtures. Its pre-execution closure review says production evidence was still pending, and that its test replay was partial (23/23 targeted; the full suite was not completed in one run). After execution, KORKI prepared the November 1 handoff by reconstructing the closure record from the operator logs. It reports that it checked the Stage G and Stage F transcripts and recomputed the closure-record hash and its 12-line change; ASTER recomputed both separately. KORKI had no access to the production host or to the original Stage G, Block D, Block E or §9.5 report files. |
| External third-party audit | An independent firm's audit | **None has taken place** |

None of these levels is a certification. No human or external party has reviewed this documentation. KORKI, itself an AI system, reviewed RC3 and RC4, both receiving HOLD (§13). This subsequent owner-authorized public release has not yet undergone independent review. SQUNO authorized publication with disclosed evidence limitations. Publication does not imply KORKI PASS or external certification.

## 7. SEC-01: privileged access

An open finding about the service account's system permissions. It was recorded on 2026-11-01 as outside T-2's scope. Remediation is planned as a separate change after T-2 closure.

**Status: OPEN.** No configuration details are published.

## 8. SEC-1: database credential finding

An open finding about the production database credential. Remediation is a separate authorized security change, outside T-2. At Stage F, SQUNO attested that SEC-1 was not handled under T-2 (closure record).

**Status: OPEN.** No credential or configuration details are published.

## 9. Operational supervision

- **What is established.** The service runs as a system service unit. The T-2 operator checks treated its main process ID and restart count as stop conditions.
- **What is not established.** An earlier informal observation raised a question about process supervision. It has not been resolved or attributed, and no record of it is available. It is not assumed to have been resolved by the cutover or by T-2.
- **Status: OPEN.**

Restart behaviour and supervision are separate questions:
- **Restart behaviour** — does a restart preserve correct state? This was verified at gate restarts.
- **Supervision** — does a manager reliably own and restart the process? This is not verified.

## 10. VERIFY-1 and other open items

- **VERIFY-1.** The project records define VERIFY-1 as a verifier improvement: the secret-scan check should skip the reviewed public script copy and name the files it flags. It is outside T-2. KORKI's T-2 review does **not** complete it.
- **P8.** Full historical identity-binding parity was checked offline at S0. The production variant was not runnable by design.

## 11. AI involvement and independence

- **What the AI builder wrote:**
  - the hardened application changes;
  - migrations 011a–011e;
  - the tests, kits, verifiers and operator packages;
  - the closure draft, rendered in advance.
- **Its local tests are not evidence.** They ran on fixtures and stubs, and the builder labels them that way itself.
- **The same AI identity drafted this documentation.** KORKI reviewed RC3 and RC4, both receiving HOLD. This owner-authorized release has not yet undergone independent review (§13).
- **Production runs.** These were authorized by SQUNO through recorded authorizations. The builder notes name SQUNO as the operator; that SQUNO personally typed each run is *(owner-reported)*.
- **Decisions.** The recorded decisions are SQUNO's. Several began as proposals from the AI builder that SQUNO approved.

## 12. Proof Season boundary

Witness Core records evidence and reconstructs state.

**Not implemented:**
- Season Genesis (T-4);
- the Season Engine (T-5);
- Proof Season.

Ruleset 8 is a frozen specification. The boundary audit lists interface questions that must be settled before any Season implementation starts.

## 13. Documentation status

**Release status.** SQUNO authorized this release on 1 November 2026, with its evidence limitations disclosed. It does not change the status of T-2, which remains closed.

**Review findings still open:**
- KORKI reviewed RC3 and RC4, both receiving HOLD. This subsequent owner-authorized public release has not yet undergone independent review. SQUNO authorized publication with disclosed evidence limitations. Publication does not imply KORKI PASS or external certification.
- The RC4 HOLD concerned evidence-classification and assurance limitations *(owner-reported; the RC4 review record is not in the evidence index)*. This release does not answer those findings point by point. The limitations disclosed in this documentation are unchanged by publication.
- The two November 2026 designations (reconstructed from transcript; checked by Stage G, not examined) were already in RC4, marked draft-only, and KORKI reviewed RC4 (HOLD) *(owner-reported)*. This release removes the draft-only label without upgrading them, and adds the evidence-basis column to [EVIDENCE_INDEX.md](EVIDENCE_INDEX.md). KORKI has not reviewed this revised presentation.
- If KORKI or later evidence finds errors, they will be corrected in later revisions. The historical record will not be rewritten.

**What later revisions may add:** original evidence artifacts, where publishing them is appropriate; more detailed reproducibility instructions; further verification reports. None of these is available yet.

The 1 November closure facts in this release rest on two kinds of evidence:
- the Stage G and Stage F **operator transcripts** (terminal output shared by the operator);
- the closure record and the Stage G draft, **reconstructed** from those transcripts. Each reconstruction matches the hash printed on the production host.

**What was not available:**
- **Original files.** No 1 November file was copied directly from the production host. The original Stage G, Block D, Block E and §9.5 report files were not available.
- **The DEP-11a amendment record.** Its bytes were not available. Its content is known from builder notes and from the closure record.

Transcripts can be complete and accurate, but they are not the original files.
