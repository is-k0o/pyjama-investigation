# Prior-art / novelty audit — updated 2026-09-21

Purpose: separate public prior art from defensible project contributions. **IOC freshness is not methodological novelty.**

Labels: **KNOWN**, **EXTENDED**, **CORRELATED/LINKED BY US**, **APPARENTLY NEW** (provisional; absence from search is not proof of priority).

| Claim | Classification | Prior art / source | Project delta / caveat |
|---|---|---|---|
| Fake coding interviews/repositories | KNOWN | Microsoft, 2026-03-11: https://www.microsoft.com/en-us/security/blog/2026/03/11/contagious-interview-malware-delivered-through-fake-developer-job-interviews/ | Campaign model is not new. |
| VS Code `runOn: folderOpen` | KNOWN | Microsoft 2026-03-11; OpenSourceMalware Fake Font, indexed 2026-01-28: https://lazarus.day/reports/post/new-dprk-contagious-interview-campaign-fake-font-uses-malicious-vscode-fonts-K31ls | Repeated across our corpus; Workspace Trust conditions matter. |
| JavaScript disguised as `.woff2` | KNOWN | OpenSourceMalware 2026-01-28; Socket/PolinRider 2026-09-17: https://socket.dev/blog/polinrider-github-packagist | A/BC/D/NEB/VMOBI lineage mapping may extend prior art; disguise itself does not. |
| VS Code terminal-profile trigger | KNOWN / EXTENDED instance | A public developer report in September 2026 independently described a malicious VS Code terminal profile used during a recruitment scam to download and execute a remote script when opening a terminal: https://www.linkedin.com/posts/mariuszrolinski_two-recruitment-scams-in-the-last-couple-activity-7495839900647481344-B_5D | The realfraction implementation remains project evidence, but the trigger pattern is not claimed as novel. |
| npm lifecycle/module-load execution | KNOWN | Microsoft 2026-03-11 and public campaign research | Repository-specific mappings are extensions, not discovery of the technique. |
| JSONKeeper dead-drop delivery | KNOWN | Ossprey campaign research: https://www.ossprey.com/blog/how-ossprey-uncovered-a-large-scale-dprk-contagious-interview-campaign | Exact SURUJ/Nava chains can extend prior art; JSONKeeper use generally is not new. |
| HTTP error-body execution | KNOWN mechanism / EXTENDED instance | NTT Security documents HTTP 500 -> catch -> execute response JavaScript: https://jp.security.ntt/insights_resources/tech_blog/en-contagious-interview-ottercookie/ | rsaw/DeFi exact sample/hash and downstream are project evidence. |
| gamboracle / 51.210.52.212:1224 | KNOWN / EXTENDED | Public ThreatFox-derived feed, 2026-09-18: https://radar.offseq.com/threat/threatfox-iocs-for-2026-09-18-1fe1ffc52ef30447 | Cross-trigger AgentMesh/MetaPlay linkage is more defensible than IOC novelty. |
| 8085/8086/8087 backend tuple | KNOWN in part / CORRELATED BY US | SigIntZero Aug 2026 documents a related sample using ports 8085/8086/8087: https://sigintzero.com/blog/fake-recruiter-github-poisoning-tailwind-config-malware | BrickFi/realfraction/rsaw linkage and endpoint-role mapping may be contribution; port tuple itself is not. |
| Ethereum NullReceiver recipient-address C2 encoding | KNOWN | Public Aug 2026 reporting; Socket 2026-09-17; https://www.scworld.com/brief/north-korea-linked-to-new-nullreceiver-c2-technique | Fresh pointer observation is IOC freshness, not novelty. |
| NullReceiver sender 0xa322... / helloipbot!! | KNOWN | Public NullReceiver reporting / Socket 2026-09-17 | Our later tx observation is temporal evidence only. |
| A/BC/D/NEB/VMOBI wrappers converge to same 16,674-char NullReceiver inner | CORRELATED/LINKED BY US | No matching five-generation normalization result found in targeted search. | Strong candidate contribution; code lineage != operator identity or per-repo liveness. |
| Bounded 90-repository reverse-hunt corpus | EXTENDED / CORRELATED BY US | Earlier public fake-font research reported smaller repository/variant sets. | 90 is a bounded captured dataset, not an exhaustive census. |
| Nava JSONKeeper -> TRON -> BSC calldata -> decode -> session backend | KNOWN mechanism / EXTENDED instance | Ransom-ISAC, Oct. 2025, Cross-Chain TxDataHiding: https://ransom-isac.org/blog/cross-chain-txdatahiding-crypto-heist/ | The technique is prior art. The project contribution is the independently reconstructed current Nava implementation from repository loader through the final HTTP-delivered agent. |
| Multiple trigger forms converge on reusable mid-tier/backend families | CORRELATED/LINKED BY US | Vendors already document multiple variants/infrastructure rotation. | Our contribution is the evidence-backed corpus architecture, not a universal taxonomy. |

## Corrections from first-pass intuition

- HTTP error-body execution is prior art, not apparently new.
- The 8085/8086/8087 tuple is prior art; cross-repository linkage/role mapping is the potentially useful contribution.
- JSONKeeper use at campaign scale is prior art.
- NullReceiver, recipient encoding, sender wallet and `helloipbot!!` are prior art.
- Cross-chain TxDataHiding is prior art; the Nava result is an extended/current implementation reconstruction, not discovery of the technique.

## Strongest provisional contributions

1. A/BC/D/NEB/VMOBI normalization to one NullReceiver inner.
2. Cross-trigger/cross-loader architecture mapping with explicit evidence boundaries.
3. Nava repository-to-final-agent reconstruction as a current instance of known Cross-Chain TxDataHiding tradecraft.
4. C2-808X cross-repository linkage/role mapping, not the port tuple itself.
5. realfraction terminal-profile implementation as an independently reconstructed instance of a publicly documented trigger pattern.

`APPARENTLY NEW` means only that no matching publication was found in the bounded audit for the specific claim carrying that label.
