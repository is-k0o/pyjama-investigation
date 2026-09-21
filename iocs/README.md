# IOCs

Private working IOC set for the **pyjama-investigation** project.

Investigation start: **2026-09-12**

This directory is the staging area for IOC review before external reporting or publication.

## Canonical files — recalculated 2026-09-20

The canonical network IOC sets were rebuilt from the three conversation exports plus the bounded 2026-09-20 recheck.

- [current.csv](current.csv) — **110** direct/proven campaign-linked network indicators. This includes currently reachable, gated, provider-disabled, route-absent-by-method, and policy-not-contacted indicators. "Current" means the IOC is still part of the canonical direct campaign set; it does **not** mean every endpoint is live.
- [historical.csv](historical.csv) — **36** historically linked, rotated, neutralized, or superseded network indicators.
- [candidates.csv](candidates.csv) — **56** passive, same-template, context-only, or otherwise unlinked candidates that are deliberately **not promoted** to direct campaign IOCs.
- [recheck-2026-09-20.md](recheck-2026-09-20.md) — interpretation of the bounded network recheck.
- [threatfox-submission-2026-09-21.md](threatfox-submission-2026-09-21.md) — final ThreatFox submission summary, deduplication outcome, current-vs-historical correction and exclusions.
- [blockchain.csv](blockchain.csv) — Ethereum/TRON/BSC resolver indicators and transactions.
- [hashes.csv](hashes.csv) — SHA-256 hashes of recovered stages/artifacts.
- [repositories.csv](repositories.csv) — repository URLs, confidence/state and taxonomy path.

The canonical sets intentionally contain multiple representations of the same infrastructure when they represent materially different observables, e.g. IP, IP:port, and exact malicious route.

## Temporal merge rule

Conversation exports are snapshots, not competing truth sources.

For the same IOC:

```text
14/09  UNKNOWN / passive
16/09  PROVEN / repo-linked
20/09  reachable / disabled / historical / candidate
```

Earlier observations are preserved as evidence. The canonical files represent the best current classification after later investigation and recheck.

A later timeout, DNS failure, HEAD 404, or provider error does not silently erase stronger historical evidence. Conversely, a currently reachable old host is not automatically restored as the campaign's current backend if source evidence shows the branch rotated away.

## IOC model

The canonical chain is defined in [../taxonomy.md](../taxonomy.md):

```text
TRIGGER
-> LOCAL LOADER
-> MID-TIER / LINK-TO-C2
-> FINAL C2 / BACKEND
```

A provider name is not a class. An IP/domain/URL is an IOC associated with a class, not the class definition itself.

## Evidence and liveness semantics

- **PROVEN** — direct source/capture/transaction evidence supports the IOC or its service/template role.
- **INFERRED-CANDIDATE** — correlation exists but direct repository/stage linkage is absent.
- **HISTORICAL** — prior role is supported but is not asserted as the current campaign role.
- **REACHABLE** — the bounded check reached the host/service/route used for that observation.
- **HOST_REACHABLE / ROUTE_NOT_CONTACTED** — only transport/root reachability was checked; the sensitive exact route was not exercised.
- **ROUTE_ABSENT (METHOD-SPECIFIC)** — the tested method/path returned absent; this does not override an earlier stronger observation using another method.
- **DISABLED BY PROVIDER** — provider-side disabling was explicitly observed.
- **INDETERMINATE_FROM_EGRESS** — timeout/DNS/unreachability from the test egress; not universal proof of death.
- **NOT_CONTACTED_POLICY** — active recontact was intentionally suppressed, notably Nava-sensitive infrastructure.

## Reporting boundary

Before submitting an IOC externally:

1. verify that it is actually an IOC, not only a code-search fingerprint;
2. distinguish direct/current, historical, and candidate infrastructure;
3. preserve evidence linking the IOC to malicious behavior;
4. avoid promoting sibling/template correlations to direct attribution;
5. do not describe an exact dead-drop returning 404 as an active payload source;
6. deduplicate against the target TI platform before submission.

The publication/reporting set should be derived from these files, not reconstructed from memory.

## External reporting status

As of **2026-09-21**:

- **ThreatFox:** submitted; 28 new IOC submissions returned OK after exact deduplication, with 17 queue entries already present.
- **GitHub:** consolidated repository/method disclosure sent separately; see [../evidence/disclosure-status-2026-09-21.md](../evidence/disclosure-status-2026-09-21.md).
- **Provider-specific abuse reporting:** not exhaustively performed.

The reporting phase does not change evidence states or promote historical/candidate infrastructure.
