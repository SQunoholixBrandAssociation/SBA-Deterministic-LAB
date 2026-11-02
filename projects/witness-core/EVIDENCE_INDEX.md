# Witness Core V1 — Evidence Index

> **Public documentation — owner-authorized release (2026-11-01). Evidence limitations disclosed.**
> **Every artifact listed here is private.** None is publicly accessible. Each row says what the documentation actually examined (**Evidence basis**) and how much assurance the artifact carries (**Class**).

## How to read this index

**Evidence basis** — what this documentation actually had in hand:

| Evidence basis | Meaning |
| --- | --- |
| Original examined | The artifact itself was read, and its SHA256 recomputed, in preparing this documentation |
| Operator transcript examined | A copy of terminal output shared by the operator was read and hashed. It is not a file copied from the production host. |
| Reconstructed from transcript | Rebuilt byte for byte from an operator transcript. Its SHA256 matches the value the production procedure printed. It was not copied directly from the production host. |
| Checked by Stage G, not examined | Stage G checked the artifact on the production host, as the operator transcript shows. The artifact itself was not examined here. |
| Not examined | Known only by the identity printed in an operator transcript |
| Owner attestation | A recorded decision or attestation by SQUNO |
| Builder self-report | The AI builder's own account. This is the weakest basis. |

**Class** — the assurance class:

| Class | Meaning |
| --- | --- |
| `PUBLIC_VERIFIABLE` | Anyone can obtain and check the artifact (none at present) |
| `PRIVATE_VERIFIED` | A private artifact; this documentation examined it and recomputed its SHA256 |
| `OWNER_ATTESTATION` | A recorded decision or attestation by the project owner (examined) |
| `NOT_PUBLICLY_REPRODUCIBLE` | The result depends on the private production environment |
| `RECONSTRUCTED_FROM_OPERATOR_TERMINAL_LOG` | Used only for EV-25 (November 2026 evidence). Same meaning as the "Reconstructed from transcript" basis. |
| `STAGE_G_CHECKED_NOT_EXAMINED` | Used only for EV-20 to EV-23 (November 2026 evidence). Same meaning as the "Checked by Stage G, not examined" basis. |

The two November 2026 designations are deliberately weaker than `PRIVATE_VERIFIED`. They are not upgraded.

**A published hash is not access.** A SHA256 listed here lets someone who later receives the artifact confirm it is the same file. It gives the public no access to the private evidence, and it does not prove what the file says.

**Originals and derivatives.** Every hash below identifies an **original private artifact**, with these qualifications:
- **EV-24** identifies Stage G's own report. The original Stage G report was not directly examined. Stage G printed its identity, and the closure record records it.
- **EV-25** identifies a reconstruction. The final closure record was reconstructed from the operator transcript. Its SHA256 matches the value printed by the production procedure. It was not copied directly from the production host.
- **EV-26 and EV-27** identify operator-shared copies of terminal output. They have no host file.

No sanitized public derivative has been produced. If one is produced, it will get its own hash and a "derivative" label.

**Assurance level:**
- **KORKI** is an AI reviewer, used as a separate review path inside the project.
- **ASTER** is the AI that built the kits and drafted this documentation.
- Neither is an external auditor. Nothing here is a certification.

## Registry

| ID | Date (UTC) | Phase | Artifact | SHA256 | Outcome | Supports | Evidence basis | Reproducibility | Class |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| EV-01 | 2026-09-25 | Baseline | Read-only baseline recon (production output) | `2f0037a745ca71429d6a7f8325690ea133128408202fca25ac4dabc9ee97634b` | — | Baseline gaps | Original examined | Not publicly reproducible | `PRIVATE_VERIFIED` |
| EV-02 | 2026-09-25 | Baseline | Gap closure queries (production output) | `8e6ae679ecf951ab8a3c6d9f81b9320568ea7538397caabf547d8086021e4b0f` | — | 248 missing ids; mutation counters | Original examined | Not publicly reproducible | `PRIVATE_VERIFIED` |
| EV-03 | 2026-09-26 | Phase 0 | Isolation proof (production output) | `224be09d8cbd77d935620b982ac8ac15df7d3c521db684f1ef20560243dd4219` | `PHASE0_FREEZE_GATE=PASS` | Test environment = production code | Original examined | Not publicly reproducible | `PRIVATE_VERIFIED` |
| EV-04 | 2026-09-26 | Phase 0 | Hardening Spec V1 rev47, Phase 0 Freeze (PDF) | `35f567e8e6dad4225a49ef5e3928824759ef6a5b46f1092817cf3507105aaa60` | Frozen | Architecture; P1–P15 | Original examined | Document | `PRIVATE_VERIFIED` |
| EV-05 | 2026-09-26 | Phase 1 | Red-Test Matrix P1–P15 (PDF) | `46bbda67d604b179bc23fd2c687f018501b7a89c46623d010032a48c7516c166` | Plan | Phase 1 method | Original examined | Document | `PRIVATE_VERIFIED` |
| EV-06 | 2026-09-26 | Phase 1 | Phase 1 gate pack (archive) | `cafb54f30c76db3c68930a9de0c8d1c774f05692086b5861a73f3fc90f055855` | `PASS_WITH_DEFERRED_P8` | Phase 1 result | Original examined | Not publicly reproducible | `PRIVATE_VERIFIED` |
| EV-07 | 2026-09-26 | Phase 1 | Phase 1 `GATE.txt` (inside EV-06) | `6f77dfaa27a3a02450da4f1465290b21c60b1da89ee00e7cebbb2e4bbb9b5e37` | Same | Matches the closure-draft binding | Original examined | Not publicly reproducible | `PRIVATE_VERIFIED` |
| EV-08 | 2026-10-06 | D3 | D3 production pack (archive) | `99c762e33442b1faeed9d443b590067fa4b5bdc2f21850293d9d6ec6d48b365b` | Gate HOLD | Segment C reproduction parity | Original examined | Not publicly reproducible | `PRIVATE_VERIFIED` |
| EV-09 | 2026-10-06 | D3 | D3 closure decision v2 ("Authority: SQUNO") | `5ad94e4ebb331a308469d2f898b76285d2b0acaa69c371ed073cf19f5c3a1d50` | `CLOSED_WITH_CUTOVER_DELTA` | Legacy provenance | Owner attestation (original examined) | Decision record | `OWNER_ATTESTATION` |
| EV-10 | 2026-10-06 | T-2 | T-2 Core Cutover Execution Spec v1.2 (PDF) | `e5eee498a62b193b87b750f2e0f88d292f911178f03bebabc6acdc6388cd45c7` | Frozen | Objective, matrix, TC-1, DEP decisions | Original examined | Document | `PRIVATE_VERIFIED` |
| EV-11 | 2026-10-06 | Boundary | Witness Core × Proof Season boundary audit (PDF) | `d3a4b6cc593d5fce7b31416c1878331a8ea21a5e4fbc5e408fc48cabdf77e36c` | PASS with explicit dependencies | Proof Season boundary | Original examined | Document | `PRIVATE_VERIFIED` |
| EV-12 | 2026-10-06 | M3 | M3 read output (production output) | `0caa5249ae39ff2b633b5203887ffe9b120cc1119f4658b0335f8f142ddeb067` | 8 inversions, maximum 1 s | M3 | Original examined | Not publicly reproducible | `PRIVATE_VERIFIED` |
| EV-13 | 2026-10-07 | S0 | S0 kit v2.1.0 | `073b97dfcf31934782cf55590277b60b06bf6d6e3effc226a94fcad4a303e07e` | Kit | S0 freeze scope | Original examined | Kit (needs private inputs) | `PRIVATE_VERIFIED` |
| EV-14 | ≤ 2026-10-12 | Phase 4 | Migration 011e source | `fc6802085f55ebe294b7a8aefa995c4c2298c5b78cd964c8899d5753bcd9bc05` | Applied 2026-10-12 | Database integrity controls; matches the closure-draft binding | Original examined | Source file | `PRIVATE_VERIFIED` |
| EV-15 | 2026-10-13 | Phase 4 | Pre-continuation diagnostic (production transcript) | `b46baea620f9936c23e22c7c2c21caea06068704293fedd0928e09775b1e2cf5` | Base `GATE=HOLD`, `TC1=PASS`, 011e in ledger | Phase 4 base; 011e applied | Original examined | Not publicly reproducible | `PRIVATE_VERIFIED` |
| EV-16 | 2026-10-12 | All | Builder notes RC10.3 (AI builder) | `4b98fa0a2d8b66a94b8fd480f4392cd9f3822841e71dc84b7383689c4f681271` | — | Build history; DEP-11a decision; defect record | Builder self-report (original examined) | Builder's own account | `PRIVATE_VERIFIED` (as builder self-report) |
| EV-17 | 2026-11-01 | Closure | Closure kit r2 (archive) | `c342c54a18d47fc32b0fbc1676246fe3ea4f60d2f294b93f3205a24b70df9215` | Kit | Stage G / Stage F logic | Original examined | Kit (needs private packs) | `PRIVATE_VERIFIED` |
| EV-18 | 2026-11-01 | Closure | KORKI review of closure kit r2 | `159f8e69703360630c8a1b8759598fba4351468f373918920082ca0579717e23` | `PASS_FOR_STAGE_G_WITH_PRODUCTION_EVIDENCE_PENDING` | Review scope and limits | Original examined | Review record | `PRIVATE_VERIFIED` |
| EV-19 | 2026-11-01 | Stage G | Closure record draft. It was pre-rendered and reviewed, and production Stage G wrote the same bytes (hash printed in EV-26). | `c482b4d503edc1fbaf42d7c8147d17350e87313e2a71a1680d44d6cad7532ad7` | Draft; 12 lines `PENDING` | Closure-matrix content | Original examined (pre-rendered draft). The production copy is shown equal only by the transcript (EV-26); it was not examined. | Document | `PRIVATE_VERIFIED`; production equality shown by EV-26 |
| EV-20 | 2026-11-01 | Phase 4 | Block D post-run report | `35bce49595c569360943a3f49ed27a7d95e2ba458ed84aac946724145fbb96ad` | PASS (Stage G check). Stage G's own re-run of the continuation verifier gave 140 OK lines. | Continuation verification | Checked by Stage G, not examined. The original report file was not independently examined. | Not publicly reproducible | `STAGE_G_CHECKED_NOT_EXAMINED` |
| EV-21 | 2026-11-01 | Phase 4 | Block D independent verification transcript | `146808d4c36c0602e9673c1c5e8db902081cab06dff0beb71036a0c528b8e192` | PASS (Stage G check) | Block D | Checked by Stage G, not examined. The original file was not independently examined. | Not publicly reproducible | `STAGE_G_CHECKED_NOT_EXAMINED` |
| EV-22 | 2026-11-01 | §9.5 | SPEC §9.5 fresh read report | `0bb72207b27f28b7a66806d69b6f17f61e815d7cbfd1ae0efb8625ce3c3aba02` | 0/0/0/0, PASS (Stage G check) | §9.5 | Checked by Stage G, not examined. The original report file was not independently examined. | Not publicly reproducible | `STAGE_G_CHECKED_NOT_EXAMINED` |
| EV-23 | 2026-11-01 | Block E | Closure evidence collection report | `b330f1da440095392728f3f7342dc61dde9c2a415f17e55038854498db7e7e2b` | PASS (Stage G check) | Evidence set | Checked by Stage G, not examined. The original report file was not independently examined. | Not publicly reproducible | `STAGE_G_CHECKED_NOT_EXAMINED` |
| EV-24 | 2026-11-01 14:44 | Stage G | Stage G report file | `418cea5670d1fce72d702668e6d886e800a84d2ac768f43733d15821f21b42c2` | `STAGE_G_EXIT=0` | Final verification | Not examined. The original Stage G report was not directly examined. Its identity is printed in EV-26 and recorded in EV-25. | Not publicly reproducible | No assurance class assigned, because the file was not examined |
| EV-25 | 2026-11-01 14:50:50 | Stage F | **T-2 final closure record** (73 lines) | `d93f78cc358c99ec167bdc055d6b59fbd77a8da7c12e01797da5b37c01cece34` | `T2_CLOSURE=CLOSED`; 8 attestations `ATTESTED_BY_SQUNO` | Formal closure | Reconstructed from transcript (EV-27). Its SHA256 matches the value printed by the production procedure. It was not copied directly from the production host. | Decision record | `RECONSTRUCTED_FROM_OPERATOR_TERMINAL_LOG`. The 12 lines changed at Stage F (eight §9.7 attestations, `SQUNO_DECISION`, `T2_CLOSURE`, `DATE` and the Stage G report hash) are an `OWNER_ATTESTATION`. The other 61 lines are the Stage G draft (EV-19), unchanged. |
| EV-28 | 2026-11-01 | Documentation | KORKI independent review of documentation RC3. Includes KORKI's post-execution check of the Stage G and Stage F transcripts (record hash and 12-line change recomputed). | `0cd509a875dbdcf2fec4732f78dacdf1bc84175c0474680a3ca0887f5544961e` | `RC3_REVIEW=HOLD` | KORKI's post-execution transcript check (EV-25 roles) | Original examined | Review record | `PRIVATE_VERIFIED` |
| EV-26 | 2026-11-01 | Stage G | Stage G operator terminal transcript (175 lines) | `4eb6f5195f79e9b4dd1f54e93347a40c4e68971771bd899e4e307f541a00d76d` | All checks PASS; two frozen-build restarts before 3.7 (allowed); `STAGE_G_EXIT=0` | Closure verification | Operator transcript examined | Not publicly reproducible | `PRIVATE_VERIFIED` (operator-shared copy of terminal output) |
| EV-27 | 2026-11-01 | Stage F | Stage F operator terminal transcript (82 lines) | `241f978f798b2bf4b7c38bda5fb759c6933eb35bf75e873c7fa52e3c678d69d8` | `STAGE_F_RC=0`; record hash printed | Formal closure | Operator transcript examined | Not publicly reproducible | `PRIVATE_VERIFIED` (operator-shared copy of terminal output) |

**Hash-bound identities.** The closure draft (EV-19) pins 59 distinct hashes, among them:
- the S0, Phase 1, Phase 2, Phase 3 and Phase 4 gate packs;
- the build, kit and freeze identities;
- the authorizations;
- the DEP-11a amendment (`272ed35b…`);
- the CUT-1 report (`7ea54ede…`);
- the D3 decision records and the 011e source;
- the Block D, Block E and §9.5 reports.

Apart from EV-07, EV-09 and EV-14, the artifacts behind these hashes were not opened in preparing this documentation.

**Steps without an evidence pack:**
- **Phase 0 P5 reconciliation.** Its result survives only in a terminal transcript. Only the script hash is recorded.
- **Phase 2 rehearsal input.** The read-only production dump used for the Phase 2 rehearsal was captured by a one-off script outside the reviewed kit.

## Roles per material artifact

This table shows only roles that an examined artifact states.

| ID | created_by | verified_by | reviewed_by | approved_by |
| --- | --- | --- | --- | --- |
| EV-06/07 | Run by the project operator. The harness author is not named in the pack. | Phase 1 runner and self-test | Not named in the pack (harness files matched a pinned "reviewed" checksum list) | — |
| EV-09 | — | — | — | SQUNO ("Authority: SQUNO") |
| EV-10 | Bylined to SQUNO. The drafter of the text is not established. | — | — | SQUNO (lifecycle "FROZEN, SQUNO 2026-10-06") |
| EV-14 | AI builder (Class A candidate; builder notes) | Phase 2 suite; P1/P9 checks (project records) | Reviewer agents and KORKI | SQUNO (Phase 4 authorization) |
| EV-17 | AI builder ("Builder: Aster, 2026-11-01") | Builder's local fixture tests (builder labels them "not evidence") | KORKI (EV-18) | — |
| EV-19 | Rendered in advance by the AI builder from a fixture; written in production by Stage G | Stage G (EV-26) | KORKI: expected draft, before execution; after execution, KORKI checked the Stage G transcript | SQUNO at Stage F (EV-27) |
| EV-25 | Stage F, run by the operator; reconstructed by KORKI from EV-27 | Hash recomputed by ASTER and by KORKI | KORKI (EV-28; after execution; second-path AI review of operator-supplied output, not an audit) | **SQUNO** (`T2_CLOSED by SQUNO`) |

## Not published

The following are never published:
- raw Telegram data;
- participant information and identifiers;
- hostnames, addresses and account names;
- configuration and credentials;
- database contents;
- full operational transcripts.

Where these support a claim, only a hash and a summary appear here. A few result-counter lines are quoted, such as the Phase 1 gate summary.

More evidence may be added in later revisions, where publishing it is appropriate. Nothing listed here as private is available publicly today.
