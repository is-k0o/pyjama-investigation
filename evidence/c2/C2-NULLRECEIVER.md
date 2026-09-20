# C2-NULLRECEIVER evidence

Backend class: C2-NULLRECEIVER

Canonical representative: VPRoyal/bloxhq.

## Representative chain

~~~text
TRIG-VSCODE-FOLDEROPEN
-> LOCAL-FAKE-ASSET-NODE
-> MID-ETH-RESOLVER
-> C2-NULLRECEIVER
~~~

The hidden VS Code task executes:

~~~text
node ./frontend/src/components/skeletons/public/fonts/fa-solid-400.woff2
~~~

The file is obfuscated JavaScript, not a font.

Git blob/object ID:
c09f8f470293a37ca0594abdf80f32af239cc286

This is a Git object ID, not SHA-256.

## Ethereum resolver

Sender:
0xa322E5f3D311D3080e6f0121063e9aDC2490Ef1a

Fresh 2026-09-20 observation:
- tx: 0x98b27caa023221ed5718501ed9cb452d634480c1a2ace2064dd4b3c65f40f58a
- block: 26016999
- nonce: 406
- recipient: 0xa658864ba658864b68656c6c6f6970626f742121
- decoded IP #1: 166.88.134.75
- decoded IP #2: 166.88.134.75
- tail: helloipbot!!

The blockchain value proves the current pointer at observation time; it does not by itself prove service liveness.

## Backend/stages observed

Known routes:
- http://166.88.134.75:443/0x/cls
- http://166.88.134.75:443/0x/ls
- http://166.88.134.75:443/0x/clb
- http://166.88.134.75:80/$/boot

HTTP is cleartext even on TCP/443.

Artifacts:
- raw /0x/cls SHA256: 6bc870593f447c7727b487c2d382636d4699afffe67fa6a2075f78117ad15f6b
- raw /0x/ls SHA256: affbbe7b899a60541365224a99ff0d51dff83bd2999a0041bc8d125d1742b0d2
- raw /0x/clb SHA256: 0d9aeb88d5c4f141f980f880ebc1bdaf9408f11200a88de1f8d82d704665d298
- decoded /0x/clb SHA256: 41cb56f3b8e2df7c61b671813317aa277f59bbfb374cfddd483d22eec44723ff
- raw /$/boot SHA256: 16cd50e50aa56797f356d60af7c7db3bf4ab923d0a7271c639796bb4add782d0
- XOR-decoded boot SHA256: ab8a54f50bbda7eb2156941ef99a801158794a787bd9d78bbe6cf6822e53fd1a

No dynamic per-host /$/<id>, Socket.IO tasking or operator commands were exercised.

## Reverse-hunt outer generations

Distinct outer wrappers statically closed to the same 16,674-character NullReceiver inner:
- A - _0x7058...
- BC - _0x514f9b=_0x5b5d
- D - _0x3509f4=_0x4b4a
- NEB - _0x5847cd=_0x343e
- VMOBI - _0x42d753=_0xdd5b

This is code/resolver lineage, not proof of common operator identity or per-repository current C2 liveness.
