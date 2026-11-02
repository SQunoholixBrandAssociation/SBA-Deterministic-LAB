# Witness Core V1 — Implementation Status

> **Public documentation — owner-authorized release (2026-11-01). Evidence limitations disclosed.**

## Classification key

| Class | Meaning |
| --- | --- |
| `OPERATIONAL` | Running in production |
| `VERIFIED` | A specific recorded check passed (the basis column says how far it was examined) |
| `SPECIFIED` | Defined in a frozen specification; says nothing about deployment |
| `PLANNED` | Intended future work |
| `OPEN` | Known unresolved item |
| `NOT_VERIFIED` | Reported, but no recorded check was available to this documentation |

**Basis labels:**
- **examined** — the output itself was read in preparing this documentation;
- **project record** — a sealed pack identified by hash in the reviewed closure draft, not opened here;
- **operator transcript** — the Stage G or Stage F terminal output of 1 November 2026, shared by the operator and examined here. The original report files were not examined.
- **reconstructed** — rebuilt byte for byte from an operator transcript. Its SHA256 matches the value printed on the production host. It is not a file copied from the host.
- *owner-reported* — the owner's statement only.

A specification, an implementation, a verification result and a decision are different things.

## 1. System level

| Item | Class | Basis |
| --- | --- | --- |
| Witness Core V1 hardened build (dist `1bfa6db6…`, processor `88eebb28…`) | `OPERATIONAL` | Project records name it as deployed. The 13 October production diagnostic (examined) shows it ingesting after 011e. On 1 November the continuation's step 4.8a recorded `MATCH=YES`: the service-unit configuration and dependency identity matched the frozen configuration. Stage G found this result in the sealed continuation pack (operator transcript). The check does not compare the dist hash or `processor_version`. Any later state is *owner-reported*. |
| T-2 programme | Closure checks `VERIFIED` (Stage G, `STAGE_G_EXIT=0`). The closure itself is SQUNO's decision: `T2_CLOSURE=CLOSED`, 2026-11-01 14:50:50Z (Stage F, `STAGE_F_RC=0`). The decision and attestations are owner attestations. | Operator transcripts. The closure record was reconstructed from the Stage F output; its hash matches the value Stage F printed, `d93f78cc…`. |
| Phase 1 red-test gate | `VERIFIED` | Examined: `PHASE1_GATE=PASS_WITH_DEFERRED_P8` |
| Phase 2 gate | `VERIFIED` | Project record: RC10.3 run 2 (20 October) PASS; five PASS packs carry the same frozen build |
| Phase 3 gate (P2, P12, P15) | `VERIFIED` | Project record: completion pack, 8 October |
| Phase 4 base run | `VERIFIED` for TC-1. The gate itself stays HOLD (historical). | Examined: production diagnostic shows `TC1=PASS` and the gate HOLD |
| Phase 4 continuation (steps 4.6–4.8) | `VERIFIED` | Operator transcript: Stage G re-ran the continuation verifier (PASS, 140 OK lines) and confirmed the gate PASS with no REASON. The continuation pack itself was not opened. |

## 2. Core mechanisms

Raw capture, semantic processing, state reconstruction and replay are classified separately.

| Mechanism | Properties | Class | Basis |
| --- | --- | --- | --- |
| Raw-event capture with `(bot_id, update_id)` identity | P3, P12 | `OPERATIONAL`, `VERIFIED` | Project record: 0 duplicate identities; 213 accounting batches at the Phase 3 gate |
| Separate `source_event_at` / `observed_at` | P2 | `OPERATIONAL`, `VERIFIED` | Project record: no null `observed_at` or `bot_id` at the Phase 3 gate or across the observation window |
| Append-only evidence enforced by the database | P1 | `OPERATIONAL`, `VERIFIED` | 011e applied (examined transcript); catalog matched at step 4.4 (project record) |
| Schema parity and migration ledger | P9 | `VERIFIED` | Project record: empty parity diff. The ledger records 011a–011e with SHA256; earlier migrations are entered as applied before the ledger; 005 and 008 never had files. |
| One transaction per update; ack after commit | P13, TC-1 | `VERIFIED` | `TC1=PASS` (examined transcript). The PASS holds only under DEP-11a: m = 1 against the original M = 20 (project record). L3 ordering checks show 0 violations (project record). |
| No reads before persistence | P15 | `VERIFIED` (tests only) | Project record: p15 tests pass on the deployed build. There is no live fault injection, by design. |
| Semantic processing with `processor_version` lineage | P3, P14 | `OPERATIONAL` | Examined transcript: `processor_version` recorded at checkpoints |
| Restart-neutral projection | P7 | `VERIFIED` at gate restarts | Project record: P7 checkpoints at the Phase 3 gate and at step 4.3 |
| Full-history replay of the V1 build (step 4.7) | P7, P14 | `VERIFIED` (via continuation) | Not completed at the base run: step 4.7 went to HOLD when its input dump failed. The continuation completed 4.7, and Stage G confirmed its PASS (operator transcript). The replay output itself was not examined. |
| Legacy reproduction (frozen pre-hardening processor, Segment C) | — | `VERIFIED` | Examined D3 production pack: parity PASS |
| Log hygiene and rotation | P10 | `VERIFIED` (via continuation) | HOLD at base step 4.6. The continuation completed 4.6, and Stage G confirmed it (operator transcript). |
| Derivation fixes (edits, topic roots, membership) | P4, P5, P6 | `OPERATIONAL` | In the deployed build; the Phase 2 suite passed on it (project record). Not listed in the T-2 closure matrix. |
| Historical identity-binding parity | P8 | `VERIFIED` offline | Project record: S0 `p8_historical_parity` PASS, which closed DEP-4. The Phase 1 production variant was not runnable by design. |
| Silent suppression in legacy Segment C (3 events) | P11 | `OPEN` (legacy) | The D3 decision records it as a known issue. Historical rows are not rewritten. |

## 3. Decisions, specifications and downstream programmes

| Item | Class | Basis |
| --- | --- | --- |
| D3 (legacy provenance) | `VERIFIED` (decision record): `CLOSED_WITH_CUTOVER_DELTA`, SQUNO, 2026-10-06. The gate result stays HOLD. | Examined decision v2 and production pack |
| M3 (Telegram ordering read) | `VERIFIED` (production output). Ran once on 2026-10-06 and found 8 source-time inversions, at most 1 s apart. The T-2 spec records M3 as FAIL for the ordering property only; it is not re-run. | Examined output; T-2 spec |
| Ruleset 8 (R01–R56) | `SPECIFIED`, frozen 2026-10-06 | T-2 spec; project status records |
| E2 (evidence-intake finality) | `OPEN`; stays CLOSEABLE until T-4 | T-2 spec; closure draft |
| T-4 SEASON_GENESIS | `PLANNED`. Not started as of 2026-10-08. At closure, SQUNO attested "not started under T-2" (`ATTESTED_BY_SQUNO`). | Status records (8 Oct); closure record (reconstructed) |
| T-5 Season Engine | `PLANNED`. Same basis as T-4. | Same |
| Proof Season | `PLANNED`. No Season implementation is recorded. | Proof Season Extension v1 (later marked superseded); boundary audit |

## 4. What T-2 closure does not cover

T-2 closure covers the T-2 programme only. It does not cover:
- Season Genesis, the Season Engine or Proof Season;
- future processor versions;
- the security items SEC-01 and SEC-1;
- the open items in [LIMITATIONS.md](LIMITATIONS.md).

## 5. How far verification was examined

**Examined directly:**
- Phase 1, D3, M3, the baseline and the 13 October diagnostic.
- The Stage G and Stage F operator transcripts of 1 November.

**Not opened here:**
- **The Phase 2–4 packs.** These are **project records**.
  - On the production host, Stage G checked every pack's external anchors, `SHA256SUMS` and result line.
  - It re-ran the reviewed verifiers for the S0 pack, the executing Phase 2 pack, the Phase 3 completion pack, the Phase 4 base pack and the continuation, and printed PASS (operator transcript).
  - Their individual contents were not opened in preparing this documentation.
- **The original Stage G, Block D, Block E and §9.5 report files.** These are known only from the operator transcripts: by the identities printed there, and by the checks shown there. Stage G checked Block D, Block E and §9.5; Stage F checked the Stage G report.
- **The DEP-11a amendment record** (`272ed35b…`). Its bytes were not available. Its content is known from the builder notes and from the closure record.

Reviewers should weigh these accordingly.

## 6. Engineering ownership and accountability

**Project-level responsibility.**
- Project owner & lead architect: **SQUNO**.
- Build / engineering: **SQUNO + ASTER**.
- Review / verification: **SQUNO + KORKI**.
- Release authority: **SQUNO**.

The rows below record who performed each activity.

| Responsibility | Party | Basis |
| --- | --- | --- |
| Founder, Lead Architect & Project Owner; scope; recorded design decisions; freezes; authorizations; acceptance; release authority | **SQUNO** | Decision and authorization records |
| Production operation | Project operator under SQUNO's written authorization. The builder notes name SQUNO as the operator. | Authorization records; builder notes |
| Hardened application code, migrations 011a–011e, tests, kits, verifiers, operator packages | ASTER (Builder Support), under SQUNO's architectural direction and authorization. Builder records use the name from 2026-10-13. Earlier ones name the same builder by its model name. | Builder notes |
| Pre-hardening application code | Not established | — |
| Review of T-2 kits before use | KORKI (Reviewer & Solution Support) from 2026-10-12; unattributed AI reviewers earlier | Review records |
| Future ownership of T-4, T-5 and Proof Season | SQUNO as project owner | Owner statement |

These documents make no claim that SQUNO wrote the code or repeated KORKI's checks. No external audit took place, and no corporate office is claimed.
