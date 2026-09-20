# IOCs

Private working IOC set for the **pyjama-investigation** project.

Investigation start: **2026-09-12**

This directory is the staging area for IOC review before external reporting or publication.

## Current files

- [current.csv](current.csv) — current/recent confirmed or bounded-observation infrastructure.
- [historical.csv](historical.csv) — historical or neutralized infrastructure kept separate from current claims.
- [blockchain.csv](blockchain.csv) — Ethereum/TRON/BSC resolver indicators and transactions.
- [hashes.csv](hashes.csv) — SHA-256 hashes of recovered stages/artifacts.
- [repositories.csv](repositories.csv) — repository URLs, confidence/state and taxonomy path.

## IOC model

The canonical chain is defined in [../taxonomy.md](../taxonomy.md):

```text
TRIGGER
-> LOCAL LOADER
-> MID-TIER / LINK-TO-C2
-> FINAL C2 / BACKEND
```

A provider name is not a class. An IP/domain/URL is an IOC associated with a class, not the class definition itself.

## Minimum fields

For every IOC, capture when available:

```text
ioc
ioc_type
layer
role
first_seen / last_checked
state
confidence
related_repo
evidence_ref
notes
```

Suggested state values:

- `ACTIVE`
- `REACHABLE/GATED`
- `DISABLED BY PROVIDER`
- `NEUTRALIZED`
- `INACTIVE — WATCH`
- `HISTORICAL`
- `UNKNOWN`

## Reporting boundary

Before submitting an IOC externally:

1. verify that it is actually an IOC, not only a code-search fingerprint;
2. distinguish current from historical infrastructure;
3. preserve evidence linking the IOC to malicious behavior;
4. avoid promoting sibling/template correlations to direct attribution;
5. do not describe an exact dead-drop returning 404 as an active payload source;
6. deduplicate against the target TI platform before submission.

The publication/reporting set should be derived from these files, not reconstructed from memory.
