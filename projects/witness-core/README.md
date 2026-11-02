# Witness Core V1 — SBA | D_LAB

> **Public documentation — owner-authorized release (2026-11-01). Evidence limitations disclosed.**
> SQUNO authorized this release. It is not an external audit or a certification, and it does not carry a KORKI PASS (see "Who did what").
>
> **What these statements rest on:**
> - examined production outputs, specifications and decision records;
> - project records: sealed packs identified by hash in the reviewed closure draft, not opened here;
> - for 1 November 2026, the operator's Stage G and Stage F terminal transcripts. The final closure record was reconstructed from the Stage F transcript, and its SHA256 matches the value Stage F printed for it.
>
> **Not examined:** the original Stage G, Block D, Block E and §9.5 report files; the DEP-11a amendment record; the individual Phase 2–4 packs.
>
> Statements that still depend only on the owner's word are marked *(owner-reported)*.

**SQUNO — Founder, Lead Architect & Project Owner | SBA / D_LAB**
**Engineering decisions and release authority:** SQUNO

**Development method:** founder-led, AI-assisted engineering.
- **SQUNO** set the scope, decided the recorded design options, authorized the T-2 production steps in writing and accepted the results.
- **An AI builder** (Claude, named ASTER from October 2026) wrote the hardened application changes, the migrations, the tests and the operational kits.
- **A separate AI reviewer** (KORKI) reviewed the T-2 kits before they were used.

Most steps are recorded in hash-bound evidence packs. The known exceptions are listed in [EVIDENCE_INDEX.md](EVIDENCE_INDEX.md).

---

## What Witness Core is

Witness Core is the evidence layer of SBA | D_LAB. It records what a Telegram bot observes in the SBA community, derives typed events from those observations, and rebuilds participation state from those events.

The frozen hardening specification sets this as its entry gate for any later Proof Season use (translated from Polish):

> the same raw input + the same `processor_version` → the same semantic evidence → the same reconstructed state.

Witness Core stores **facts and provenance**. It does not decide eligibility, rewards or season outcomes; those belong to downstream layers. Dependencies run one way: Season logic depends on Witness, never the reverse.

## The problem it addresses

A read-only baseline review of the production system (25 September 2026) found the usual weaknesses of a first-generation bot:
- **Lost updates.** 248 `update_id` values are missing in 203 gaps. Of these, 247 were received but never stored.
- **Wrong reply detection.** Replies at forum-topic roots were counted as replies.
- **Unlinked edits.** Edits were not linked to their messages.
- **Unstable timestamps.** "First seen" times moved on restart.
- **Mutable evidence.** Evidence rows could be changed by the application role.
- **No version control** on the deployed code.

The hardening specification turned this into fifteen required properties (P1–P15):

| Area | Properties |
| --- | --- |
| Evidence cannot be changed once written | P1 append-only, enforced by the database |
| Time is recorded honestly | P2 `source_event_at`, `observed_at` and `persisted_at` kept separate |
| Each observation counts once | P3 identity and idempotency, P4 edit linkage, P11 derivation independent of other rows |
| Telegram structure is read correctly | P5 forum-topic replies, P6 membership changes |
| State can be rebuilt | P7 pure reducer over the event lineage, P8 replayable identity binding |
| The schema is what the migrations say | P9 schema parity and migration ledger |
| Logs are not evidence | P10 |
| Nothing is acknowledged before it is stored | P12 intake before side effects, P13 one transaction per observation and ack after commit, P15 no reads before persistence |
| Every derived row records how it was made | P14 `processor_version` and lineage |

## Status

| Item | Status | Basis |
| --- | --- | --- |
| Witness Core V1 (hardened build) | In production since the cutover of 8 October 2026 | Project records; production diagnostic of 13 October 2026. On 1 November 2026 the continuation's step 4.8a recorded that the service-unit configuration and dependency identity matched the frozen configuration (`MATCH=YES`). Stage G found this result in the sealed continuation pack (operator transcript). The check does not identify the running build or `processor_version`. Any later state is *(owner-reported)*. |
| Migration 011e (immutable evidence, role separation) | Applied in production on 12 October 2026 | Production diagnostic transcript |
| T-2 (cutover and hardening programme) | Formally closed by SQUNO on 1 November 2026 at 14:50:50 UTC | Stage F operator transcript. The closure record was reconstructed from it and matches its production hash `d93f78cc…`. |
| Proof Season / Season Engine | **Not implemented**; planned downstream work | Specifications; boundary audit; status records (8 Oct 2026) |

The per-mechanism picture is in [IMPLEMENTATION_STATUS.md](IMPLEMENTATION_STATUS.md).

## Witness Core is not Proof Season

Proof Season is a planned system. It would apply season rules (Ruleset 8, R01–R56, frozen as a specification on 6 October 2026) to the evidence Witness Core records.

- **No Season implementation is recorded.** The last examined status records (8 October 2026) list Season Genesis (T-4) and the Season Engine (T-5) as not started.
- **T-2 was barred from Season work.** It had to leave no Season object in the system and could not start T-4 or T-5.
- **At closure, SQUNO attested** that no Season object was created and that T-4/T-5 were not started under T-2. These are owner attestations, recorded in the closure record.

## Timeline

| Date (UTC) | Milestone | Basis |
| --- | --- | --- |
| 2026-07-05 | Earliest recorded raw event. The first 340 records came from an earlier database, migrated on 2026-07-06. | Phase 0 supplement (production output) |
| 2026-07-14 | Start of the current legacy intake segment (Segment C) | D3 decision |
| 2026-09-25 | Read-only baseline and gap review of production | Production outputs |
| 2026-09-26 | Isolation proof PASS; Hardening Spec rev47 frozen at the end of Phase 0 | Production output; spec |
| 2026-09-26 | Phase 1 red-test gate: `PASS_WITH_DEFERRED_P8`. 65 tests ran on the frozen code in an isolated workspace and database on the production host; production state was unchanged. | Production gate pack |
| 2026-10-06 | T-2 Execution Spec v1.2 frozen; D3 decided (`CLOSED_WITH_CUTOVER_DELTA`); Ruleset 8 frozen; M3 ordering read | SQUNO decision records; production output |
| 2026-10-07 | S0 design freeze (`S0_RESULT=FROZEN`) | Project record |
| 2026-10-07 to 2026-10-20 | Phase 2 offline builds, reviews and gate packs. Six HOLDs are preserved. The RC10.3 run of 20 October passed and is the Phase 2 pack used by the closure. | Closure draft; production pack listing |
| 2026-10-07/08 | Phase 3. Two attempts on 7 October ended HOLD with no cutover write. The production cutover of 8 October sealed INCOMPLETE and was then completed with a PASS. | Project records |
| 2026-10-11 | Phase 4 first attempt: TC-1 INCONCLUSIVE (HOLD); DEP-11a floors set by SQUNO | Project records |
| 2026-10-12 | Phase 4 base run: TC-1 PASS under DEP-11a; 011e applied; HOLD at steps 4.6 and 4.7 | Production diagnostic transcript |
| 2026-11-01 | Phase 4 continuation; verification blocks; Stage G (`STAGE_G_EXIT=0`); Stage F closure at 14:50:50 | Stage G and Stage F operator transcripts; original report files not examined |

## Who did what

| Role | Party | Basis |
| --- | --- | --- |
| Founder, Lead Architect, Project Owner | **SQUNO** | Owner statement; consistent with every decision record |
| Recorded design decisions | **SQUNO**: D3; D4–D6 (as DEP-1 to DEP-3); DEP-7; DEP-8; the DEP-11 values; the DEP-11a amendment; T-2 v1.2 freeze; Ruleset 8 freeze. Several were AI-builder proposals that SQUNO approved. | Decision records |
| Other closures | DEP-4, DEP-5 and DEP-6 closed through S0 gate results. KORKI chose the variant of the final read-only database check. | T-2 spec; review records |
| Specifications (published versions) | Bylined to **SQUNO**. The records do not establish who drafted the text, whether by hand or with AI assistance. The text is written in the first person and refers to SQUNO as the decider. | Spec title pages; text |
| Hardened application code, migrations 011a–011e, tests, kits, verifiers, operator packages | **AI builder** (Claude; named "Aster"/ASTER from 13 October 2026) | Builder notes |
| Pre-hardening application code (before September 2026) | Not established | No authorship record |
| Production execution | The project operator, after SQUNO's written authorization of each step. The builder notes name SQUNO as the operator. The builder had no production access. | Authorization records; builder notes |
| Review of the T-2 kits before use | **KORKI** (AI reviewer, separate from the builder) from 12 October; unnamed separate reviewer agents before that. Reviews were offline, on packages and fixtures. | Review records |
| Acceptance of results and T-2 closure | **SQUNO** | Decision records; Stage F transcript and reconstructed closure record |
| Post-execution check of closure | **KORKI** (AI) prepared the November 1 handoff without host access, reconstructing the closure record and the Stage G draft from the operator-supplied terminal logs. KORKI reports that it checked the Stage G and Stage F transcripts and recomputed the 73-line record's SHA256 and its 12-line change. ASTER recomputed both separately. This is a second-path AI review of operator-supplied output, partly of KORKI's own reconstruction. It had no host access and is not an audit. | KORKI RC3 review; handoff provenance note |
| This documentation | Drafted by ASTER (AI). KORKI reviewed RC3 and RC4, both receiving HOLD. This subsequent owner-authorized public release has not yet undergone independent review. SQUNO authorized publication with disclosed evidence limitations. Publication does not imply KORKI PASS or external certification. | KORKI RC3 review (EV-28). The RC4 verdict is *(owner-reported)*: the RC4 review record is not in the evidence index. |

No other contributors are credited, and no corporate titles are claimed. The same AI identity built the kits and drafted this documentation; see [LIMITATIONS.md](LIMITATIONS.md) §11.

**Role glossary.**

| Term | Meaning |
| --- | --- |
| *Designed* | Decided what should exist |
| *Implemented* | Wrote the code or scripts |
| *Executed* | Ran it on the production system |
| *Verified* | Produced a check result |
| *Reviewed* | Examined someone else's work |
| *Approved* | Accepted a result or authorized a step |
| *Published* | Released it publicly |

## Documents

| File | Contents |
| --- | --- |
| [ARCHITECTURE.md](ARCHITECTURE.md) | Components, data flow, invariants, design accountability |
| [IMPLEMENTATION_STATUS.md](IMPLEMENTATION_STATUS.md) | What exists, what is verified, what is only specified or planned |
| [T2_CLOSURE.md](T2_CLOSURE.md) | Engineering account of T-2 |
| [EVIDENCE_INDEX.md](EVIDENCE_INDEX.md) | Evidence registry and its limits |
| [LIMITATIONS.md](LIMITATIONS.md) | Limitations, open findings, assurance levels |
