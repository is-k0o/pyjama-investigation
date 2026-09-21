# Pyjama Investigation

Defensive research notes, evidence and indicators from an investigation into **Contagious Interview**-style malicious GitHub repositories and related delivery infrastructure.

The bounded corpus contains **158 captured repositories**:

- 68 repositories from the main investigation tracker;
- 90 repositories from a bounded fake-font reverse-hunt.

This is a research corpus, not a claim that all 158 repositories were independently proven active compromise chains.

## Safety

Some indicators in this repository may still resolve or remain reachable.

- Do **not** execute code from investigated repositories.
- Do **not** run `npm install`, lifecycle scripts, VS Code tasks, fake assets or recovered loaders.
- Do **not** interact with upload, tasking, Socket.IO/WebSocket, credential, shell or dynamic `/$/<id>` routes.
- Treat URLs, IPs, ports, hashes and blockchain locators as defensive research indicators.

The investigation relied on static analysis, source review, passive threat intelligence, blockchain reads and narrowly bounded liveness checks.

## Repository layout

- [`taxonomy.md`](taxonomy.md) — four-layer campaign model: trigger, local loader, mid-tier and final backend.
- [`evidence/`](evidence/) — chain reconstruction, backend-family notes, representative samples, lineage and disclosure records.
- [`iocs/`](iocs/) — current, historical and candidate indicators, hashes, blockchain observables and bounded recheck results.

## Evidence language

- **PROVEN** — supported directly by source, captured response, hash, transaction or equivalent primary evidence.
- **INFERRED-CANDIDATE** — correlated but not directly linked to the specific repository/sample.
- **UNKNOWN** — insufficient evidence.
- **HISTORICAL** — previously observed but not asserted current.

Current infrastructure liveness and static maliciousness are tracked separately.

## Disclosure

As of **2026-09-21**:

- GitHub received a consolidated disclosure covering the bounded investigation corpus and methodology.
- ThreatFox accepted **61 distinct IOCs cumulatively across the investigation**; the final submission run contributed 28 new accepted IOCs after exact deduplication.
- Provider-specific abuse reporting was performed selectively, not exhaustively.

See [`evidence/disclosure-status-2026-09-21.md`](evidence/disclosure-status-2026-09-21.md) for the frozen disclosure record.

## License

Except where otherwise noted, original research text, analysis, taxonomy, annotations and project-generated datasets are licensed under the [Creative Commons Attribution 4.0 International license (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/).

Third-party material remains subject to its original rights and terms. See [`LICENSE`](LICENSE).
