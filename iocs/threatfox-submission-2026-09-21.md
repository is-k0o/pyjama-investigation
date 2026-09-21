# ThreatFox submission — 2026-09-21

## Summary

Final ThreatFox submission run:

```text
45 local queue entries
38 READY
7 REVIEW
17 exact duplicates already present
28 new IOC submissions
28 submissions returned OK
```

Exact IOC deduplication was performed before submission.

ThreatFox accepted the malware labels used by the queue:

```text
js.beavertail
js.otter_cookie
unknown
```

and the IOC / threat-type combinations:

```text
url + payload_delivery
url + botnet_cc
ip:port + botnet_cc
```

`is_compromised` was omitted, leaving the documented ThreatFox default of `False`.

## Current-vs-historical correction

The older queue snapshot incorrectly treated the old realfraction branch as current:

```text
https://www.ipregionchecker.org/api/ip-check-encrypted/3aeb34a30
172.86.126.227:8085
172.86.126.227:8086
172.86.126.227:8087
```

These are retained as **historical**.

The submitted current realfraction branch was:

```text
https://www.configs.live/settings/linux?flag=5
https://www.configs.live/settings/mac?flag=5
https://www.configs.live/settings/windows?flag=5
https://configs.live/settings/bootstraplinux?flag=5
https://configs.live/settings/env?flag=5
https://configs.live/settings/package
https://www.ipregionchecker.org/api/ip-check-encrypted/3aeb34a35
172.86.86.84:8085
172.86.86.84:8086
172.86.86.84:8087
```

## New submissions — groups

### AgentMesh / C2-1224

New intermediate resources included:

```text
https://ip-vm.vercel.app/api/settings/bootstraplinux
https://ip-vm.vercel.app/api/settings/bootstrap
https://ip-vm.vercel.app/api/settings/package
```

Previously submitted AgentMesh/gamboracle indicators were skipped as exact duplicates.

### realfraction / C2-808X

The current configs.live / ipregionchecker / 172.86.86.84 chain was submitted.

### BrickFi / C2-808X

Submitted:

```text
https://ip-checkout.vercel.app/api/ip-check-encrypted/3aeb34a39
144.172.110.154:8085
144.172.110.154:8086
144.172.110.154:8087
```

### rsaw409/DeFi / C2-808X

The C1 was already present as a ThreatFox duplicate.

Submitted downstream:

```text
144.172.107.50:8085
144.172.107.50:8086
144.172.107.50:8087
```

### NullReceiver

Submitted current Ethereum-resolved backend and payload routes:

```text
166.88.134.75:80
166.88.134.75:443
http://166.88.134.75:443/0x/cls
http://166.88.134.75:443/0x/ls
http://166.88.134.75:443/0x/clb
http://166.88.134.75:80/$/boot
```

### Nava

Previously submitted primary indicators were skipped as exact duplicates.

The following directly recovered final-agent infrastructure was added:

```text
138.226.247.152:443
5.231.107.180:3000
```

These were initially held in REVIEW because their application behavior was not independently exercised in the final bulk recheck. They were submitted because their roles are directly recovered from the confirmed Nava final agent rather than inferred from template similarity.

## Deliberately excluded

The default current submission did not include:

- historical realfraction `172.86.126.227` branch;
- provider-disabled Vercel routes merely for historical completeness;
- same-template C2-808X candidates without direct repo/stage linkage;
- same-template :1224 candidates without direct linkage;
- shared public Ethereum RPC / Blockscout infrastructure;
- unresolved or stale dead-drops merely because they appeared in historical source.

This preserves the project boundary:

```text
direct/current evidence -> submit
historical -> retain separately
candidate/template-only -> do not promote
```

## Local execution artifact

The submitting host wrote:

```text
threatfox_submission_results_20260921-180939.json
```

This filename is preserved for local audit traceability.
