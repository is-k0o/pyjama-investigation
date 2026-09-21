# Evidence

Evidence supporting the **pyjama-investigation** project.

Investigation start: **2026-09-12**

## Purpose

This directory stores evidence needed to reproduce and support findings without executing suspicious repository code.

The working campaign model is defined in [../taxonomy.md](../taxonomy.md).

## Current structure

- [chain-map.csv](chain-map.csv) — repository → trigger → local loader → mid-tier → final C2 mapping.
- [c2/C2-1224.md](c2/C2-1224.md) — direct `:1224/api/checkStatus` backend family.
- [c2/C2-808X.md](c2/C2-808X.md) — modular `:8085/:8086/:8087` backend family.
- [c2/C2-NULLRECEIVER.md](c2/C2-NULLRECEIVER.md) — Ethereum-resolved NullReceiver backend.
- [c2/C2-NAVA.md](c2/C2-NAVA.md) — Nava multihop-blockchain backend.
- [c2/C2-UNRESOLVED.md](c2/C2-UNRESOLVED.md) — unresolved final-backend cases; this is a state, not a C2 architecture.
- [reverse-hunt/fake-font-lineage.md](reverse-hunt/fake-font-lineage.md) — A / BC / D / NEB / VMOBI fake-font lineage and boundaries.
- [enforcement/github-2026-09-21.md](enforcement/github-2026-09-21.md) — GitHub Trust & Safety enforcement confirmation and API verification for `ritualaipro/MetaPlay`.
- [disclosure-status-2026-09-21.md](disclosure-status-2026-09-21.md) — external disclosure freeze: GitHub disclosed, ThreatFox submitted, provider-specific abuse reporting not exhaustive.

## Evidence states

- **PROVEN** — directly supported by source, captured response, hash, transaction, or equivalent primary evidence.
- **INFERRED-CANDIDATE** — strong correlation, but not directly attributable to the specific sample/repository.
- **UNKNOWN** — insufficient evidence.
- **HISTORICAL** — previously observed but not asserted current.

## Handling rules

- Do not execute suspicious repository code.
- Do not run `npm install` or lifecycle scripts from investigated repositories.
- Keep raw evidence separate from interpretation.
- Record timestamps in UTC whenever possible.
- A 40-hex Git object ID is not a SHA-256 hash.
- Provider / ASN / hosting reuse is correlation evidence, not actor attribution.
- Preserve historical and current infrastructure separately.
- Static maliciousness and current infrastructure liveness are separate judgments.

## Disclosure status

As of **2026-09-21**:

```text
GitHub    -> disclosed
ThreatFox -> submitted
Provider-specific abuse reporting -> not exhaustively performed
```

See [disclosure-status-2026-09-21.md](disclosure-status-2026-09-21.md).

## Publication state

The remaining publication work is evidence pinning and maintenance of exact repository file / commit / blob / date anchors without weakening the evidence boundary.
