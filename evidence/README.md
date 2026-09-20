# Evidence

Private working evidence for the **pyjama-investigation** project.

Investigation start: **2026-09-12**

## Purpose

This directory stores evidence needed to reproduce and support findings without executing suspicious repository code.

Preferred evidence types:

- source snapshots or minimal excerpts establishing a trigger / loader / sink;
- exact repository paths, Git commit/blob identifiers, and observation timestamps;
- raw HTTP response metadata and small response bodies from bounded validation;
- SHA-256 hashes of locally saved artifacts;
- static deobfuscation outputs and derivation notes;
- passive infrastructure observations;
- provider / abuse-report references;
- blockchain transaction evidence used for dynamic infrastructure resolution.

## Evidence states

Use the following labels consistently:

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

## Suggested naming

```text
YYYY-MM-DD_<repo-or-family>_<artifact>.<ext>
```

Examples:

```text
2026-09-20_realfraction_env-setup.js.txt
2026-09-20_nullreceiver_blockscout_tx.json
2026-09-20_suruj_3KIH4.response.json
```
