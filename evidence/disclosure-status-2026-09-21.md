# Disclosure status — 2026-09-21

Investigation: **pyjama-investigation / Contagious Interview**

This file records the external disclosure state at the investigation freeze point. It is an operational-status record, not a claim that all infrastructure or repositories remain live.

## GitHub

Status:

```text
DISCLOSED
```

On **2026-09-21**, GitHub Trust & Safety was sent a consolidated disclosure package covering the bounded investigation corpus and the reverse-hunt methodology.

The disclosure explicitly distinguished:

- deeply reconstructed / source-proven chains;
- proven static malicious lineage;
- historical / repository-gone cases;
- unresolved, incomplete, or stale cases.

The package described the bounded corpus as:

```text
68 repositories — original core tracker
90 repositories — bounded reverse-hunt capture
0 overlap between those captured sets
158 distinct captured repositories total
```

The disclosure did **not** assert that all 158 repositories were independently proven active compromise chains.

The supporting package documented:

- proven chain reconstruction methodology;
- `TRIGGER -> LOCAL LOADER -> MID-TIER -> FINAL C2` taxonomy;
- fake-font A / BC / D / NEB / VMOBI lineage;
- source-level hunting fingerprints;
- evidence boundaries and confidence states;
- current / historical / unresolved distinctions.

### Confirmed GitHub enforcement evidence

A separate GitHub Trust & Safety response for `ritualaipro/MetaPlay` stated on **2026-09-21 08:43 UTC** that one or more Terms of Service violations had occurred and that appropriate action had been taken.

The repository subsequently returned `404 Not Found` through the GitHub repository API.

See:

```text
evidence/enforcement/github-2026-09-21.md
```

The exact enforcement mechanism was not disclosed and is not inferred.

## ThreatFox

Status:

```text
SUBMITTED
```

On **2026-09-21**, the current ThreatFox queue was rebuilt from the later evidence state rather than the older 2026-09-17 snapshot.

Important correction before submission:

```text
historical realfraction:
  /3aeb34a30 -> 172.86.126.227:8085/8086/8087

current realfraction:
  /3aeb34a35 -> 172.86.86.84:8085/8086/8087
```

The final preflight contained:

```text
45 local queue entries
38 READY
7 REVIEW
17 exact duplicates already present in ThreatFox
28 new IOC submissions
```

All **28 new submissions returned OK**.

Submitted groups included current/direct infrastructure from:

- AgentMesh / C2-1224;
- current realfraction / C2-808X;
- BrickFi / C2-808X;
- rsaw409/DeFi / error-body delivery / C2-808X;
- current Ethereum-resolved NullReceiver backend;
- Nava primary/secondary backend indicators.

Three REVIEW items were intentionally promoted for the final submission because the project had direct malware-chain evidence for their role:

```text
5.231.107.180:3000
138.226.247.152:443
http://166.88.134.75:80/$/boot
```

The local submission tool wrote its execution results to:

```text
threatfox_submission_results_20260921-180939.json
```

That local result filename is recorded here for traceability; the file is not asserted to be stored in this repository unless separately added.

See also:

```text
iocs/threatfox-submission-2026-09-21.md
```

## Provider-specific abuse reporting

Status:

```text
NOT EXHAUSTIVELY PERFORMED
```

The investigation did **not** attempt exhaustive provider-by-provider abuse reporting across every infrastructure provider observed in the campaign.

Examples of infrastructure providers that may appear in the evidence include Vercel-hosted endpoints and VPS/cloud-hosted backend services. Their presence in evidence does not imply that every provider received a separate abuse report.

This is intentional and should be described in publication/reporting language as:

> GitHub disclosed; ThreatFox submitted; provider-specific abuse reporting was not exhaustively performed.

Provider-side neutralization observed independently during the investigation, such as Vercel `451 / DEPLOYMENT_DISABLED`, remains recorded as an observation even when no separate project-originated provider report was filed.

## Reporting boundary

External disclosure status does not alter the evidence model:

- **PROVEN** remains source/capture/transaction-backed;
- **INFERRED-CANDIDATE** remains unpromoted without direct linkage;
- **HISTORICAL** remains separate from current campaign role;
- repository removal does not imply a specific enforcement mechanism unless the platform confirms it;
- provider/ASN/template reuse remains correlation evidence, not operator attribution.

## Investigation posture after disclosure

At this point the two main disclosure tracks are complete:

```text
GitHub    -> disclosed
ThreatFox -> submitted
```

The next project phase is publication preparation / evidence pinning rather than broad operational reporting.
