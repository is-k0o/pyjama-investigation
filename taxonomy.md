# Campaign taxonomy

Status: **working taxonomy frozen on 2026-09-20**.

This model is intentionally small. It exists to classify evidence consistently, not to reproduce every internal function of every sample.

## Four layers

### 1. TRIGGER

**Definition:** the local mechanism that causes malicious code contained in the repository to begin executing.

Canonical classes:
- TRIG-NPM-LIFECYCLE - npm lifecycle or package script such as prepare, preinstall, postinstall, start.
- TRIG-APP-START-IMPORT - normal application startup/import path reaches the malicious module.
- TRIG-AUTOLOADED-CONFIG - tooling automatically loads an infected config/build file such as postcss.config.mjs.
- TRIG-VSCODE-PROFILE - VS Code terminal/profile initialization is hijacked through workspace settings.
- TRIG-VSCODE-FOLDEROPEN - a VS Code task with runOn: folderOpen starts the chain.

The trigger answers only: **why did malicious local code start executing?**

### 2. LOCAL LOADER

**Definition:** local code executed after the trigger that initiates the transition to remote infrastructure.

Canonical classes:
- LOCAL-JS-NETWORK-LOADER - JavaScript module/config performs the network request or initiates the remote chain.
- LOCAL-SHELL-FETCHER - local shell/cmd/PowerShell command fetches or launches a remote stage.
- LOCAL-FAKE-ASSET-NODE - JavaScript disguised as a local asset, notably fake .woff2, is executed with Node.

Functions such as getCookie(), verifyToken(), initRuntimeConfig(), wrappers and dynamic-execution plumbing are implementation details of the local loader. They are not separate taxonomy layers.

### 3. MID-TIER / LINK-TO-C2

**Definition:** the remote/intermediate **method** used after the local loader to reach, discover or resolve the final C2.

A C1 endpoint can be one component of a mid-tier method. Mid-tier is not synonymous with a single C1.

Canonical classes:
- MID-HTTP-STAGE - one HTTP(S) endpoint returns a stage that directly reveals or reaches the final C2.
- MID-REMOTE-CHAIN - multiple remote stagers/config endpoints are required before the final C2 is recovered.
- MID-DEADDROP-JSON - a mutable JSON/dead-drop service returns code/config used to continue the chain.
- MID-ETH-RESOLVER - Ethereum state is used as a mutable resolver for the final C2.
- MID-BLOCKCHAIN-MULTIHOP - several remote/blockchain hops are required to reconstruct the final C2.

Providers are **not** classes. Vercel, JSONKeeper, Mocki, Mockoon, etc. are implementations/infrastructure used by one of the methods above.

### 4. FINAL C2 / BACKEND

**Definition:** the remote backend reached at the end of the mid-tier chain.

Confirmed backend classes:
- C2-1224
- C2-808X
- C2-NULLRECEIVER
- C2-NAVA

C2-UNRESOLVED is **not a backend architecture**. It is a classification state used when the final backend was not recovered or characterized.

Current IP addresses, domains and URLs are IOCs associated with a class; they do not define the class.

## Observed mappings

~~~text
TRIG-VSCODE-FOLDEROPEN
  -> LOCAL-FAKE-ASSET-NODE
  -> LOCAL-SHELL-FETCHER

TRIG-VSCODE-PROFILE
  -> LOCAL-SHELL-FETCHER

TRIG-AUTOLOADED-CONFIG
  -> LOCAL-JS-NETWORK-LOADER

TRIG-APP-START-IMPORT
  -> LOCAL-JS-NETWORK-LOADER

TRIG-NPM-LIFECYCLE
  -> LOCAL-JS-NETWORK-LOADER
  -> LOCAL-SHELL-FETCHER   [only where directly observed]

LOCAL-FAKE-ASSET-NODE
  -> MID-ETH-RESOLVER
  -> MID-DEADDROP-JSON

LOCAL-SHELL-FETCHER
  -> MID-REMOTE-CHAIN

LOCAL-JS-NETWORK-LOADER
  -> MID-HTTP-STAGE
  -> MID-DEADDROP-JSON
  -> MID-BLOCKCHAIN-MULTIHOP

MID-HTTP-STAGE
  -> C2-1224
  -> C2-808X

MID-REMOTE-CHAIN
  -> C2-1224
  -> C2-808X

MID-ETH-RESOLVER
  -> C2-NULLRECEIVER

MID-BLOCKCHAIN-MULTIHOP
  -> C2-NAVA

MID-DEADDROP-JSON
  -> C2-UNRESOLVED
~~~

Only observed relationships belong in the campaign graph. Do not add theoretically possible edges without evidence.

## Evidence language

- **PROVEN** - directly supported by source, captured response, hash, blockchain transaction, or equivalent primary evidence.
- **INFERRED-CANDIDATE** - strong correlation that is not directly attributable to the specific repository/sample.
- **UNKNOWN** - insufficient evidence.
- **HISTORICAL** - previously observed but not asserted current.

Static maliciousness and current infrastructure liveness are separate judgments.
