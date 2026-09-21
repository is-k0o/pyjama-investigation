# Representative evidence index — 2026-09-20

Backfill for article-relevant representatives, not every repository in the bounded corpus.

Immutable repository-side anchors are listed in [`pins.csv`](pins.csv). A pin records the commit, path and Git blob ID where those values were recoverable. It does **not** imply that the repository or downstream infrastructure remains live.

| Representative | Trigger | Local loader | Mid-tier | Backend/state | Evidence |
|---|---|---|---|---|---|
| TarsAI-net/AgentMesh | TRIG-VSCODE-FOLDEROPEN | LOCAL-SHELL-FETCHER | MID-REMOTE-CHAIN | C2-1224 | PROVEN; active at 2026-09-17 observation |
| RealJDEX/BrickFi | TRIG-APP-START-IMPORT | LOCAL-JS-NETWORK-LOADER | MID-HTTP-STAGE | C2-808X | PROVEN |
| chainbits13/realfraction | TRIG-VSCODE-PROFILE | LOCAL-SHELL-FETCHER | MID-REMOTE-CHAIN | C2-808X | PROVEN chain; REPO GONE by public API check 2026-09-21; old backend HISTORICAL |
| VPRoyal/bloxhq (A) | TRIG-VSCODE-FOLDEROPEN | LOCAL-FAKE-ASSET-NODE | MID-ETH-RESOLVER | C2-NULLRECEIVER | PROVEN representative |
| Isaac-1-lang/Real_time_chatting (BC) | folderOpen | fake asset Node | ETH resolver | NullReceiver lineage | PROVEN static lineage; per-repo liveness UNKNOWN |
| wpeventmanager/wp-event-manager (D) | folderOpen | fake asset Node | ETH resolver | NullReceiver lineage | PROVEN static lineage; per-repo liveness UNKNOWN |
| NEBOLISA/copilot-task (NEB) | folderOpen | fake asset Node | ETH resolver | NullReceiver lineage | PROVEN static lineage; per-repo liveness UNKNOWN |
| lanmower/coder (VMOBI) | folderOpen | fake asset Node | ETH resolver | NullReceiver lineage | PROVEN static lineage; per-repo liveness UNKNOWN |
| Sabbirnde/Nava | TRIG-APP-START-IMPORT | LOCAL-JS-NETWORK-LOADER | MID-BLOCKCHAIN-MULTIHOP | C2-NAVA | PROVEN |
| SURUJ404/NFT-GAMEFY | TRIG-VSCODE-FOLDEROPEN | LOCAL-FAKE-ASSET-NODE | MID-DEADDROP-JSON | unresolved | PROVEN through dead-drop; final UNKNOWN |
| rony1235/Jp-Soccer | TRIG-VSCODE-FOLDEROPEN | LOCAL-JS-NETWORK-LOADER | unresolved | unresolved | top-level folderOpen trigger PROVEN; npm install/start/prepare is a subordinate handoff; local sink PROVEN; exact C1 UNKNOWN; sibling C1 INFERRED-CANDIDATE |
| upendra512/assessment1_solution | not established | suspicious JS module | unresolved | unresolved | UNKNOWN/incomplete |

## Pinning boundary

Most surviving representatives are pinned to a commit + path + Git blob. `Cryptense-E/Jackpot` retains a historical commit + path but no stored blob ID. `chainbits13/realfraction` is explicitly partial: the 2026-09-20 malicious branch and its recovered stage hashes are documented, but the exact repository commit SHA was not retained before the repository became unavailable on 2026-09-21. No commit is reconstructed by inference.

## Anchors

AgentMesh SHA-256: mac `67fa382beeeb4d7969c35d060240da9914662fc273edef670069a8451d53660a`; linux `0ce00ff2d161d42629294b225461efb3edacc36d24688394360e3699ff1c54a6`; windows `490bbef5361fa251606d0978f12ba54f1f829aa9279e72f1e352b515a0df7688`; env `6f22a6a54896ef8616db07b63e289845ce8f5169a6bb5d393373f94a1bf99a1f`.

BrickFi: raw `6779b1a7177192afc0fb317d959f3c7659cd36a77dca45e6228d921d6aa9ac78`; decoded `cbac948ca516ffb7fa027242325ed6c57bbea8e38ebe80faee80d46c4a83b555`; delivered `ca42dfca8d1d9deb17899e4bd3b3ff50c39565f42c0bdda38678fa2849c836fc`.

realfraction current: `6316b9c3...bc20`, `67710ff1...f274`, `20d51cbf...bc20`, delivered `46d6b13d...2013`.

Fake-font Git object IDs (not SHA-256): A `c09f8f470293a37ca0594abdf80f32af239cc286`; BC `346cc3bb3d0d13c9f62639c274d0a8e0643a2f95`; D `8d1d7f68a90141cbc2acd3072ffffbb8ea087669`; NEB `9fa6afd488a7c2880b35d1bdae4fe663f22724d2`; VMOBI `738972ec3ae1699256948b203f5099ad2e223b64`.

Nava: JSONKeeper SHA-256 `195362265d3dfce1b669bbbb40f9ab385ccbde57012ec91871dd2e59d1b3a1e4`; final agent `e60070ad02ab8f21e6f018c836a55e37901cf14b8ea8be1d2f8d117ce803235f`; sessions `e22605a2363c05991146e2b7761be9077586ff5b61a7248c950b6e0e751cc958`.

SURUJ fake-font Git object ID `0d1d6424451ac50db7efa8c7bdd804c9e96848a4`; current 404-body SHA-256 `67795bf3846287322b92aee164b5745a75b6835d1fbefdcb0361745ec47cdf26`.

## Boundaries

Reverse-hunt representatives prove static lineage, not independent current C2 liveness. C2-UNRESOLVED is a state, not an architecture. rony's sibling C1 remains INFERRED-CANDIDATE. upendra remains UNKNOWN/incomplete. realfraction historical/current backends stay separate.
