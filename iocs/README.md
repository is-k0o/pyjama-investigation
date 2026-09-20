# IOCs

Private working IOC set for the **pyjama-investigation** project.

Investigation start: **2026-09-12**

This directory is the staging area for IOC review before external reporting or publication.

## Keep current and historical IOCs separate

Recommended categories:

- `urls` — C1, dead-drops, stagers, exact malicious routes;
- `domains` — infrastructure domains;
- `ips` — C2 / downstream IPs;
- `wallets` — blockchain resolver addresses;
- `hashes` — SHA-256 only unless the hash type is explicitly named;
- `github` — malicious/suspicious repository URLs and source paths;
- `provider-context` — ASN / hosting correlation kept separate from attribution.

## Minimum fields

For every IOC, capture when available:

```text
ioc
ioc_type
role
first_seen
last_checked
state
confidence
family_or_cluster
related_repo
evidence_ref
notes
```

Suggested state values:

- `ACTIVE`
- `REACHABLE/GATED`
- `DISABLED BY PROVIDER`
- `INACTIVE — WATCH`
- `HISTORICAL`
- `UNKNOWN`

## Reporting boundary

Before submitting an IOC to a third party such as ThreatFox:

1. verify that it is actually an IOC, not only a GitHub fingerprint;
2. distinguish current from historical infrastructure;
3. preserve the evidence linking the IOC to malicious behavior;
4. avoid promoting sibling/template correlations to direct attribution;
5. do not submit a 404 dead-drop as an active payload source.

The final publication/reporting set should be derived from this directory, not reconstructed from memory.
