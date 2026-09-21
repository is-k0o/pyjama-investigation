---
layout: default
title: "Pyjama Investigation: Hunting Contagious Interview Malware Across 158 GitHub Repos"
description: "A defensive threat-hunting investigation into Contagious Interview-style malicious GitHub repositories, rotating infrastructure and blockchain-based resolution."
---

# Pyjama Investigation: Hunting Contagious Interview Malware Across 158 GitHub Repos


**Investigation snapshot:** 21 September 2026  
**Operational liveness snapshot:** 20 September 2026 UTC  
**Evidence repository:** [is-k0o/pyjama-investigation](https://github.com/is-k0o/pyjama-investigation)

This article reflects the investigation state at the snapshot date. Liveness-sensitive statements use the project state consolidated on 20 September 2026 UTC; when a specific service was last directly validated earlier, that date is stated explicitly. Infrastructure and IOC liveness may change after publication.

---

# 1. I only wanted to inspect one repo

On September 11, I came across a post by **Amine Ben Asker** describing a suspicious technical interview involving a GitHub repository called `Jackpot`. The candidate was expected to clone the repository and run the project locally.

I checked the repository.

Some of the code immediately looked wrong.

The loaders were not especially sophisticated. One recurring pattern was essentially:

```text
remote request
    ↓
error.response.data
    ↓
Function.constructor("require", ...)
    ↓
execute returned JavaScript
```

Other variants hid the remote endpoint and request headers in environment variables, sometimes Base64-encoded:

```text
AUTH_AIP_KEY
AUTH_ACCESS_KEY
AUTH_ACCESS_VALUE
```

Across several repositories, those values eventually resolved to the same kind of behavior:

```text
remote endpoint
x-secret-header: secret
dynamic execution of the response
```

None of that was particularly exotic.

What was interesting was that the same unusual combination of primitives kept appearing in unrelated-looking coding-assessment repositories.

At that point, the code stopped being just suspicious and started becoming useful as a search fingerprint.

I extracted a few distinctive pieces:

```text
AUTH_AIP_KEY
error.response.data
Function.constructor
x-secret-header
/api/ipcheck-encrypted/
```

Then I searched GitHub for them.

That was where the investigation actually started.

---

# 2. Fine. Let’s grep GitHub.

The first searches were simple. I took the code fragments that looked specific enough to be reused across repositories and searched for them directly:

```text
AUTH_AIP_KEY
x-secret-header
error.response.data
Function.constructor
/api/ipcheck-encrypted/
```

That returned more repositories. Some were coding assessments, others were DeFi projects, games, React applications or backend APIs. The surrounding projects changed, but the malicious code stayed the same.

One recurring cluster used:

```text
AUTH_AIP_KEY
AUTH_ACCESS_KEY
AUTH_ACCESS_VALUE
```

Those values decoded into a remote endpoint, the header `x-secret-header`, and the value `secret`.

The hosts changed across repositories:

```text
gd.tracelic.com
walter-server.vercel.app
y-lilac-sigma.vercel.app
server-azure-tau.vercel.app
api-server-mocha.vercel.app
```

The loader logic stayed very similar.

I started using the repeated code as a fingerprint. I also split complete IOCs into smaller pieces and searched those independently.

For example:

```text
https://checking-ip-eight.vercel.app/api/ip-check-encrypted/3aeb34a37
```

became:

```text
3aeb34a37
/api/ip-check-encrypted/
x-secret-header
error.response.data
Function.constructor
```

I did the same with Base64-encoded endpoints and fragments of encoded values.

By September 14, the hunting process looked roughly like this:

```text
IOC
 ↓
host
path
token
Base64
behavioral primitives
 ↓
search separately
 ↓
compare results
```

This made it easier to follow the same loader family when the hostname changed.

Those searches also returned older repositories.

The first repositories I had been looking at were recent:

```text
AgentMesh  → September 2026
Jackpot    → September 2026
ZetaPlay   → September 2026
```

The same fingerprints then led to:

```text
coin-dex   → June 2025
ELO        → malicious history visible from May 2025
CoreX      → late 2025, still updated in 2026
```

So the search was already covering several generations of repositories.

The implementations varied, but the same primitives kept appearing:

```text
axios
process.env
Base64
error.response.data
eval(...)
Function.constructor(...)
```

At that point, my working model was still simple:

```text
repo
 ↓
loader
 ↓
remote server
```

Then several branches started pointing to infrastructure on port `1224`.

I checked those servers and found multiple IPs exposing the same application fingerprint.

That became the next thing to investigate.

---

# 3. Apparently, servers are disposable too

Several of the repositories eventually pointed to a backend on port `1224`.

The clearest example was:

```text
http://51.210.52.212:1224/api/checkStatus
```

The stage using it built system information such as:

```text
sysInfo
processInfo
tid
sysId
```

and sent it to `/api/checkStatus`.

On 2026-09-17, a minimal GET to the route returned:

```json
{"status":"ok","message":"server connected"}
```

I then checked nearby infrastructure using the same application pattern.

On 2026-09-17, I found the same `/api/checkStatus` behavior on:

```text
51.210.52.212:1224
51.210.78.63:1224
51.178.11.187:1224
```

The root responses also shared the same Express fingerprint:

```text
X-Powered-By: Express
Last-Modified: Mon, 13 Oct 2025 06:03:09 GMT
ETag: W/"0-199dc2a634d"
Content-Length: 0
```

`51.210.52.212` was directly linked to the malware chain I had reconstructed.

The other hosts were not enough to prove the same actor by themselves, but they were clearly running something very similar.

At that point I started treating the application fingerprint as an infrastructure fingerprint too.

The repeated `Last-Modified`, ETag, response shape and route behavior suggested that the same deployment artifact or template was being reused across different servers.

Operationally, the pattern looked simple:

```text
deploy server
    ↓
same Express application
    ↓
same routes
    ↓
same response behavior
```

If one IP disappeared, there was little reason to assume the application itself was gone.

The server looked disposable.

The deployment pattern was reusable.

That also explained why infrastructure indicators aged badly. A hostname or IP could be blocked, disabled or replaced while the application logic remained almost unchanged.

At this point my model was still:

```text
repo
 ↓
loader
 ↓
remote stage
 ↓
:1224 backend
```

and `:1224` looked like the main backend architecture I was dealing with.

---

# 4. Opening the workspace could be enough

`AgentMesh` added another useful piece to the model.

The repository contained a VS Code task configured with:

```text
runOn: folderOpen
```

In a trusted workspace where automatic tasks were allowed, opening the project was sufficient to trigger the task. VS Code documents both Workspace Trust and `task.allowAutomaticTasks` as controls for automatic `folderOpen` tasks.

The task selected a different remote stager depending on the operating system:

```text
/api/settings/mac
/api/settings/linux
/api/settings/windows
```

On 2026-09-17, all three routes returned `HTTP 200`.

The first stage then pulled additional components:

```text
/api/settings/bootstraplinux
/api/settings/bootstrap
/api/settings/env
/api/settings/package
```

The `env` stage contained the backend I had already seen:

```text
http://51.210.52.212:1224/api/checkStatus
```

So the chain looked like this:

```text
GitHub repository
        ↓
VS Code folderOpen task
        ↓
OS-specific stager
        ↓
bootstrap / env stage
        ↓
:1224 backend
```

This was useful because the execution point in the repository was now clear.

The malicious code did not need the application itself to start. Under those workspace conditions, opening the workspace could begin the chain.

I started separating the pieces by role:

```text
TRIGGER
   ↓
LOCAL LOADER
   ↓
REMOTE STAGE
   ↓
C2
```

For `AgentMesh`, that gave:

```text
TRIGGER
VS Code folderOpen

        ↓

LOCAL LOADER
task / OS-specific command

        ↓

REMOTE STAGE
ip-vm settings / bootstrap / env

        ↓

C2
51.210.52.212:1224
```

That made later repositories easier to compare.

Instead of asking only whether two repositories used the same URL or the same backend, I could compare where execution started, what local code handed execution off, what remote stage was loaded, and where that stage finally connected.

At this point, the model had four useful parts:

```text
TRIGGER
   ↓
LOCAL LOADER
   ↓
REMOTE STAGE
   ↓
C2
```

---

# 5. The remote stage had its own backend

`chainbits13/realfraction` used another variant of the remote-code pattern.

The repository contacted:

```text
https://www.ipregionchecker.org/api/ip-check-encrypted/3aeb34a30
```

with:

```text
POST
x-secret-header: secret
```

On September 17, the endpoint returned about 4 MB of JavaScript:

```text
4,090,518 bytes
SHA256:
c00faf9429a20cbdf1b8244b4dc408b109ce3f27d559f0e81cc434456d850d7b
```

Static analysis of that stage exposed three downstream services:

```text
172.86.126.227:8085
172.86.126.227:8086
172.86.126.227:8087
```

with roles visible in the code:

```text
http://172.86.126.227:8085/upload
http://172.86.126.227:8086/upload
ws://172.86.126.227:8087
```

I use `C2-808X` as the project label for this downstream backend cluster. In the observed stage, `8085` and `8086` were upload/exfiltration services and `8087` was the WebSocket realtime/control tier.

The three ports were reachable during the 2026-09-17 validation. I did not exercise the upload routes with real data.

The chain now looked like this:

```text
TRIGGER
   ↓
LOCAL LOADER
   ↓
ipregionchecker
4 MB JavaScript stage
   ↓
8085 / 8086 / 8087
```

Calling `ipregionchecker` the C2 was no longer very useful. It was delivering another stage which contained the actual downstream infrastructure.

I changed the model again:

```text
TRIGGER
   ↓
LOCAL LOADER
   ↓
MID-TIER
   ↓
FINAL BACKEND
```

For `realfraction`:

```text
TRIGGER / LOCAL LOADER
repository code

        ↓

MID-TIER
ipregionchecker
remote JavaScript stage

        ↓

FINAL BACKEND
172.86.126.227
├── 8085
├── 8086
└── 8087
```

The three exact `IP:port` indicators also appeared in the public `stamparm/trails` Lazarus trail:

```text
172.86.126.227:8085
172.86.126.227:8086
172.86.126.227:8087
```

I had already seen the same kind of public TI overlap with the `:1224` infrastructure. The `808X` branch gave me another independently reconstructed chain landing on infrastructure already present in the Lazarus-oriented trail.

I kept the attribution boundary simple: the exact indicators matched the public trail, while the repository-to-stage-to-backend chain came from my own analysis.

The working model was now:

```text
TRIGGER
   ↓
LOCAL LOADER
   ↓
MID-TIER
   ↓
FINAL BACKEND
```

And `:1224` was no longer the only backend pattern in the dataset.

---

# 6. Nava: JSONKeeper → TRON → BSC → HTTP

`Sabbirnde/Nava` used a completely different remote-resolution chain.

The repository referenced two JSONKeeper documents:

```text
https://www.jsonkeeper.com/b/7EBZP
https://www.jsonkeeper.com/b/PAB1R
```

Both returned the same obfuscated JavaScript payload:

```text
SHA256:
195362265d3dfce1b669bbbb40f9ab385ccbde57012ec91871dd2e59d1b3a1e4
```

The JavaScript queried a TRON address:

```text
TBMcSRezuxdoNEC8gD3uS4GQVGvybYU4UU
```

and retrieved its latest outgoing transaction.

The transaction I recovered was:

```text
a323fb6e0b9ff85d63f3fa5a16f2c369dd48acbb085460e4b7bb7c54ae1437fe
```

with a timestamp of:

```text
2026-09-15 06:27:48 UTC
```

The loader transformed `raw_data.data` from that transaction into a BSC transaction hash:

```text
0xbf3fa7a35ab2ee7e243134f39e367b8774d32810fd9b66355c8eba135b2fca9f
```

I then retrieved that transaction from BSC and decoded its calldata using the same transforms implemented by the loader:

```text
hex decode
→ reverse
→ XOR
→ BCGZ1|
→ Base64
→ gunzip
```

The decoded JavaScript produced:

```text
http://138.226.247.152:80/session/0f2a3fba69d29493
```

On 2026-09-17, that endpoint returned a 236 KB document:

```text
236,314 bytes
SHA256:
e60070ad02ab8f21e6f018c836a55e37901cf14b8ea8be1d2f8d117ce803235f
```

The response presented itself as a package-like manifest:

```text
name: vscode-python-envs
displayName: Python Environments
publisher: ms-python
```

Its `sessions` field contained about 206 KB of executable JavaScript:

```text
SHA256:
e22605a2363c05991146e2b7761be9077586ff5b61a7248c950b6e0e751cc958
```

Static analysis of that stage showed code for browser credential and cookie collection, wallet collection, `.ssh`, `.aws`, `.azure` and `gcloud` collection, file search and upload, Docker-related handling, database connection parsing, persistence and operator commands.

The complete chain was:

```text
GitHub repository
        ↓
JSONKeeper
        ↓
TRON transaction
        ↓
BSC transaction
        ↓
calldata decode
        ↓
HTTP /session/<token>
        ↓
final JavaScript agent
```

The cross-chain resolution technique itself was already documented. Ransom-ISAC had described it in October 2025 as [Cross-Chain TxDataHiding](https://ransom-isac.org/blog/cross-chain-txdatahiding-crypto-heist/). What I reconstructed here was a current Nava implementation of that pattern, from the repository to the final HTTP-delivered agent.

The four-layer model still worked, but `MID-TIER` now covered much more than a remote JavaScript endpoint:

```text
TRIGGER
   ↓
LOCAL LOADER
   ↓
MID-TIER
├── JSONKeeper
├── TRON
├── BSC
└── decoded session locator
   ↓
FINAL BACKEND
```

For `Nava`, the blockchain transactions were part of the resolution mechanism used to recover the downstream location.

The final stage also referenced:

```text
138.226.247.152:80
138.226.247.152:443
5.231.107.180:3000
```

On 2026-09-17, I verified `5.231.107.180:3000` only at the TCP level. I did not exercise its application endpoints.

At this point, `MID-TIER` was a useful generic category for everything between the local loader and the final backend, including HTTP stages, dead-drops and blockchain-based resolvers.

---

# 7. NullReceiver: the C2 address was in an Ethereum transaction

`VPRoyal/bloxhq` used another VS Code `folderOpen` trigger.

The hidden task executed:

```text
node ./frontend/src/components/skeletons/public/fonts/fa-solid-400.woff2
```

The `.woff2` file was obfuscated JavaScript, not a font.

It started with the marker:

```text
global['!']='9-0002-4'
```

which became the runtime version/header:

```text
A9-0002-4
```

The loader queried recent transactions from this Ethereum sender:

```text
0xa322E5f3D311D3080e6f0121063e9aDC2490Ef1a
```

It then read the recipient address of the latest transaction and interpreted its first eight bytes as two IPv4 addresses.

In the 2026-09-20 recheck, the transaction recipient I observed was:

```text
0xa658864ba658864b68656c6c6f6970626f742121
```

Decoded:

```text
a6 58 86 4b → 166.88.134.75
a6 58 86 4b → 166.88.134.75
remaining bytes → "helloipbot!!"
```

Public samples had previously pointed to `166.88.134.62`. The transaction I recovered pointed to `166.88.134.75`, showing that the backend pointer had been rotated through Ethereum state.

The loader then constructed its backend URLs from the decoded addresses:

```text
http://166.88.134.75:443
http://166.88.134.75:80
```

Despite the use of TCP port `443`, the protocol was plain HTTP.

Using the expected header:

```text
Sec-V: A9-0002-4
```

During the 2026-09-20 recheck, both known stage routes were still available:

```text
/0x/cls
/0x/ls
```

Both returned `HTTP 200`.

The `/0x/cls` branch eventually led to `/0x/clb`.

The raw response was:

```text
1,800,943 bytes
SHA256:
0d9aeb88d5c4f141f980f880ebc1bdaf9408f11200a88de1f8d82d704665d298
```

After decoding, the JavaScript stage was about 123 KB:

```text
SHA256:
41cb56f3b8e2df7c61b671813317aa277f59bbfb374cfddd483d22eec44723ff
```

Static analysis showed an interactive cross-platform backdoor with machine profiling, clipboard collection, file and directory upload, shell execution, JavaScript evaluation, persistence logic and Socket.IO-based command handling.

The stage contained commands such as:

```text
ss_info
ss_ip
ss_cb
ss_upf
ss_upd
ss_dir
ss_fcd
ss_connect:
ss_eval:
ss_eval64:
ss_exit
```

The `/0x/ls` branch downloaded another bootstrap from:

```text
http://166.88.134.75:80/$/boot
```

That stage prepared a Python environment and built a dynamic `/$/<id>` route used to retrieve additional Python code.

I stopped there and did not request the dynamic route.

The chain was:

```text
TRIGGER
VS Code folderOpen

        ↓

LOCAL LOADER
fake .woff2 executed with Node

        ↓

MID-TIER
Ethereum transaction
→ recipient-address IPv4 decode

        ↓

FINAL BACKEND
166.88.134.75
├── /0x/cls → Node / Socket.IO branch
└── /0x/ls  → Python bootstrap branch
```

This behavior matched the publicly documented **[NullReceiver](https://github.com/OpenSourceMalware/NullReceiver)** technique: the malware looked up the attacker-controlled Ethereum wallet, read the recipient address of its latest outbound transaction, decoded the backend IP from that address, and then connected to the resulting infrastructure.

The fake-font loader also provided a useful new group of GitHub search fingerprints:

```text
eslint-check
runOn: folderOpen
reveal: never
fa-solid-400.woff2
global['!']='9-...'
```

---

# 8. One fake font turned into 90 repositories

The fake `.woff2` loader gave me a much stronger GitHub fingerprint than the earlier URL-based searches.

The useful pieces were:

```text
eslint-check
runOn: folderOpen
reveal: never
fa-solid-400.woff2
global['!']='9-...'
```

I searched for combinations of those markers and captured a bounded set of **90 repositories**.

None of them overlapped with the 68 repositories already present in the main investigation corpus.

The 90 results did not all contain exactly the same outer JavaScript.

The first pass produced several recurring variants:

```text
A      → 35 repos
BC     → 45 repos
D      → 4 repos
OTHER  → 3 repos
STALE  → 3 repos
```

A representative from group A was `VPRoyal/bloxhq`:

```text
marker: 9-0002-4
blob:   c09f8f470293a37ca0594abdf80f32af239cc286
```

A BC representative was `Isaac-1-lang/Real_time_chatting`:

```text
marker: 9-9102-2
blob:   346cc3bb3d0d13c9f62639c274d0a8e0643a2f95
```

A D representative was `wpeventmanager/wp-event-manager`:

```text
marker: 9-6922-3
blob:   8d1d7f68a90141cbc2acd3072ffffbb8ea087669
```

The outer wrappers and markers were different.

After deobfuscation, the A, BC and D representatives converged on the same 16,674-character NullReceiver inner loader.

Two of the initially separate `OTHER` variants did the same.

`NEBOLISA/copilot-task` used:

```text
marker: 9-184-2
blob:   9fa6afd488a7c2880b35d1bdae4fe663f22724d2
```

and `lanmower/coder` used:

```text
marker: 9-8926-1
blob:   738972ec3ae1699256948b203f5099ad2e223b64
```

Both resolved to the same NullReceiver inner code.

The third `OTHER` result, `SURUJ404/NFT-GAMEFY`, was different. Its fake-font loader fetched JSON from:

```text
https://www.jsonkeeper.com/b/3KIH4
```

and did not use the Ethereum NullReceiver resolver.

That made the bounded 90-repository set:

```text
86 → NullReceiver-lineage
 1 → distinct SURUJ / JSONKeeper branch
 3 → STALE folderOpen triggers with the payload asset absent
```

The structure looked roughly like this:

```text
90 fake-font reverse-hunt repositories
├── 86
│   └── A / BC / D / NEB / VMOBI
│       ↓
│       same NullReceiver inner loader
│       ↓
│       Ethereum resolver
├── 1
│   └── SURUJ
│       ↓
│       JSONKeeper /b/3KIH4
└── 3
    └── STALE trigger, payload absent
```

That gave me another useful hunting rule:

```text
outer obfuscation may change
inner behavior may stay stable
```

The three `STALE` repositories kept the VS Code `folderOpen` trigger at `HEAD`, but the referenced fake `.woff2` payload was gone.

The 90 repositories therefore formed a bounded fake-font reverse-hunt corpus. Eighty-six converged on the NullReceiver lineage, one followed the separate SURUJ dead-drop branch, and three retained broken historical triggers.

At this point the repository count had grown significantly, but the more useful result was the lineage: several visibly different fake-font loaders could be normalized back to the same NullReceiver core.

---

# 9. The fake font was only one delivery surface

The NullReceiver hunt initially revolved around fake `.woff2` files executed through VS Code `folderOpen` tasks.

Then I found the same lineage somewhere else.

`VPRoyal/athena` contained malicious JavaScript appended directly to:

```text
postcss.config.mjs
```

The loader used the same marker I had already seen in `VPRoyal/bloxhq`:

```text
9-0002-4
```

but the outer wrapper was different:

```text
blob:
36b894564d217b79f9d954f4e872a384232af5cf

wrapper signature:
_0x37076c=_0xfb8b
```

So the fake font was not part of the malware architecture itself.

It was one place to hide the local loader.

The same idea could be embedded in files that developers or build tools would load naturally:

```text
postcss.config.mjs
eslint config
astro config
next config
src/index.ts
App.js
```

A broader GitHub search returned 87 indexed files matching this configuration-injection style.

I did not treat all 87 as proven instances of the same full downstream chain. The search result only established that the delivery pattern was being reused across multiple file types.

This changed how I looked at the first two layers of the model:

```text
TRIGGER
   ↓
LOCAL LOADER
```

The trigger and the loader were separate problems.

For example:

```text
VS Code folderOpen
        ↓
fake .woff2 loader
```

or:

```text
build / application config load
        ↓
JavaScript appended to config file
```

The downstream lineage could stay similar while the execution surface changed.

That gave me a cleaner model for the repository side of the chain:

```text
TRIGGER
├── folderOpen
├── application import
├── autoloaded config
└── lifecycle / tooling execution

        ↓

LOCAL LOADER
├── fake asset
├── JavaScript loader
└── shell / network fetcher
```

The repository was no longer one thing in the model.

It had an execution mechanism and a payload handoff, and those two pieces could change independently.

---

# 10. The 808X backend kept moving

The `realfraction` chain was not the only one ending on the `8085 / 8086 / 8087` layout.

A later hunt through `RealJDEX/BrickFi` recovered another remote stage.

Its first remote source was a Google Docs document:

```text
https://docs.google.com/document/d/1ADFFFjhrUgMOuKyFM8XqXh6rrM0hF2EzjYMXr4OBFXI/export?format=txt
```

After decoding the document, the loader contacted:

```text
https://ip-checkout.vercel.app/api/ip-check-encrypted/3aeb34a39
```

The stage behind that endpoint pointed to:

```text
144.172.110.154:8085
144.172.110.154:8086
144.172.110.154:8087
```

Another repository, `rsaw409/DeFi`, used:

```text
https://server-azure-tau.vercel.app/api/ipcheck-encrypted/606
```

with:

```text
x-secret-header: secret
```

The endpoint returned a roughly 4 MB JavaScript stage. Static analysis recovered:

```text
144.172.107.50:8085
144.172.107.50:8086
144.172.107.50:8087
```

The layout was now recurring across different chains:

```text
realfraction
    ↓
172.86.126.227
├── 8085
├── 8086
└── 8087

BrickFi
    ↓
144.172.110.154
├── 8085
├── 8086
└── 8087

DeFi
    ↓
144.172.107.50
├── 8085
├── 8086
└── 8087
```

`realfraction` itself later rotated.

The older branch used:

```text
/api/ip-check-encrypted/3aeb34a30
→ 172.86.126.227:8085/8086/8087
```

The branch active in the 2026-09-20 snapshot used:

```text
/api/ip-check-encrypted/3aeb34a35
→ 172.86.86.84:8085/8086/8087
```

That made `808X` useful as an architectural family in the model:

```text
MID-TIER
remote JavaScript stage

        ↓

C2-808X
host
├── 8085
├── 8086
└── 8087
```

I kept the individual chains and their infrastructure separate in the evidence. The repeated three-port layout and similar stage behavior were enough to group them technically, but not enough by themselves to identify an operator.

The rotation also forced me to track infrastructure state explicitly.

For the same chain, I could now have:

```text
CURRENT
172.86.86.84:8085/8086/8087

HISTORICAL
172.86.126.227:8085/8086/8087
```

That distinction became important later when preparing IOC submissions and checking which evidence still represented the active chain.

---

# 11. The final model

By the end of the investigation, the chains fitted into four layers:

```text
TRIGGER
│
├─ NPM-LIFECYCLE
├─ APP-START-IMPORT
├─ AUTOLOADED-CONFIG
├─ VSCODE-PROFILE
└─ VSCODE-FOLDEROPEN

        ↓

LOCAL LOADER
│
├─ FAKE-ASSET-NODE
├─ SHELL-FETCHER
└─ JS-NETWORK-LOADER

        ↓

MID-TIER
│
├─ HTTP-STAGE
├─ REMOTE-CHAIN
├─ DEADDROP-JSON
├─ ETH-RESOLVER
└─ BLOCKCHAIN-MULTIHOP

        ↓

FINAL BACKEND
│
├─ C2-1224
├─ C2-808X
├─ C2-NULLRECEIVER
└─ C2-NAVA
```

The trigger described how execution started inside the repository.

Examples included:

```text
npm lifecycle script
application import
autoloaded config
VS Code terminal profile
VS Code folderOpen task
```

The local loader described the code already present in the repository that handed execution to the next stage.

That could be:

```text
JavaScript disguised as an asset
shell bootstrap
network loader
```

The mid-tier covered everything used to resolve or deliver the next stage:

```text
HTTP-delivered JavaScript
remote bootstrap chains
JSON dead-drops
Ethereum-based resolvers
cross-chain transaction resolvers
```

The final backend was the infrastructure reached after those intermediate layers.

The main backend families I reconstructed were:

```text
C2-1224
C2-808X
C2-NULLRECEIVER
C2-NAVA
```

`C2-*` is the project naming convention for final backend families, not a claim that every service in a family is an interactive command channel. For `C2-808X`, the observed roles included upload/exfiltration on `8085/8086` and a WebSocket realtime/control tier on `8087`. For `C2-NAVA`, some service roles came from static analysis of the final agent, and `5.231.107.180:3000` was only TCP-validated.

The same final backend family could be reached through different triggers or local loaders, and similar repository-side techniques could lead to completely different backend architectures.

A fake `.woff2`, a malicious config file and a VS Code task therefore did not define the malware family by themselves. They described different parts of the delivery chain.

This four-layer model ended up being the simplest way to compare repositories without grouping them only by filename, hostname or individual IOC.

---

# 12. Where this ended

By the end of the hunt, the working corpus contained **158 distinct repositories**:

```text
68 repositories from the main investigation
90 repositories from the bounded fake-font reverse-hunt
```

The two bounded source lists used for this count are published in the investigation repository under `datasets/`, so the 68 + 90 total can be audited.

The investigation had also produced several independently reconstructed chains:

```text
C2-1224
C2-808X
NullReceiver
Nava
```

along with multiple trigger and loader variants across VS Code tasks, application imports, configuration files, lifecycle execution, fake assets and remote bootstrap chains.

The infrastructure changed repeatedly during the investigation.

By the 2026-09-20 operational snapshot, some Vercel deployments were disabled, some repositories had disappeared, and `realfraction` had rotated from:

```text
172.86.126.227:8085/8086/8087
```

to:

```text
172.86.86.84:8085/8086/8087
```

and the NullReceiver Ethereum pointer resolved to:

```text
166.88.134.75
```

in the 2026-09-20 recheck.

The evidence repository was therefore split between current, historical and candidate indicators instead of treating every IOC as permanently valid.

The reporting side ended with:

```text
GitHub
→ consolidated disclosure sent

ThreatFox
→ 61 distinct IOCs submitted and accepted during the investigation

Provider-specific abuse reporting
→ performed selectively
→ not exhaustive
```

GitHub Trust & Safety also confirmed enforcement action on several repositories reported during the investigation.

The representative repository mappings, artifact hashes, IOC states, enforcement records and supporting evidence are kept in the investigation repository rather than duplicated here.

This started with one suspicious coding-assessment repository.

It ended with a repository corpus, several malware delivery architectures, rotating backend infrastructure, two different blockchain-resolution mechanisms, and far too many Markdown files.

---

**Research by is-k0o — heavily assisted by OpenAI’s GPT-5.6 Sol, because apparently giving a curious guy in pajamas a reasoning model and GitHub access was a perfectly sensible idea.**

**AI assistance disclosure:** GPT-5.6 Sol was used extensively for static malware analysis, deobfuscation, code-lineage comparison, IOC extraction, infrastructure correlation, evidence organization and drafting. Active validation, operational boundaries, classifications and disclosure decisions were reviewed and authorized by the human investigator.

---

## References

- Amine Ben Asker — *SCAM ALERT - The “technical interview” can be the attack*, LinkedIn post, September 2026.
- Microsoft Defender Experts and Microsoft Defender Security Research Team — [Contagious Interview: Malware delivered through fake developer job interviews](https://www.microsoft.com/en-us/security/blog/2026/03/11/contagious-interview-malware-delivered-through-fake-developer-job-interviews/)
- Socket — [PolinRider Spreads Through Compromised GitHub Accounts and Packagist](https://www.socket.dev/blog/polinrider-github-packagist)
- Ransom-ISAC — [Cross-Chain TxDataHiding](https://ransom-isac.org/blog/cross-chain-txdatahiding-crypto-heist/)
- OpenSourceMalware — [NullReceiver](https://github.com/OpenSourceMalware/NullReceiver)
- Visual Studio Code — [Integrate with External Tools via Tasks](https://code.visualstudio.com/docs/debugtest/tasks)
- stamparm/trails — [apt_lazarus.txt](https://github.com/stamparm/trails/blob/main/malware/apt_lazarus.txt)
