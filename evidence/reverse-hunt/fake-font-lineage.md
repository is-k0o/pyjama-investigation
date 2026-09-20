# Fake-font reverse-hunt lineage

Bounded reverse-hunt dataset: **90 captured repositories**, external to the original 68-row core tracker.

This is a bounded dataset, not an exhaustive census.

## A
Reference: VPRoyal/bloxhq
- marker: 9-0002-4
- Git blob/object ID: c09f8f470293a37ca0594abdf80f32af239cc286
- outer: _0x7058...

## BC
Representative: Isaac-1-lang/Real_time_chatting
- marker: 9-9102-2
- blob: 346cc3bb3d0d13c9f62639c274d0a8e0643a2f95
- outer: _0x514f9b=_0x5b5d

Normalization control: brainbrewlabs/brainbrew-devkit, marker 9-9958-1, blob 6554cbef05b3a136f6ce5e00aeb0d453368f0757.

After replacing only the global campaign marker, the normalized loaders are byte-for-byte identical.

## D
Representative: wpeventmanager/wp-event-manager
- marker: 9-6922-3
- blob: 8d1d7f68a90141cbc2acd3072ffffbb8ea087669
- outer: _0x3509f4=_0x4b4a

All four bounded D repositories carry the exact same 37,661-character Git blob:
- wpeventmanager/wp-event-manager
- wpeventmanager/wp-event-manager-migration
- wpeventmanager/wp-user-profile-avatar
- wpeventmanager/wpem-rest-api

## NEB
Representative: NEBOLISA/copilot-task
- marker: 9-184-2
- blob: 9fa6afd488a7c2880b35d1bdae4fe663f22724d2
- outer: _0x5847cd=_0x343e

## VMOBI
Fresh representative: lanmower/coder
- marker: 9-8926-1
- blob: 738972ec3ae1699256948b203f5099ad2e223b64
- outer: _0x42d753=_0xdd5b

A, BC, D, NEB and VMOBI representative static analysis converges on the same 16,674-character NullReceiver inner.

## SURUJ distinct branch
SURUJ404/NFT-GAMEFY uses a distinct self-checking eval/XOR packer and JSONKeeper /b/3KIH4. It does not use the demonstrated NullReceiver Ethereum resolver.

## Boundary
Code/loader identity and recovered inner logic are technical lineage evidence. They do not establish operator identity or per-repository current C2 liveness.
