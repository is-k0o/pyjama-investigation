# C2-NAVA evidence

Backend class: C2-NAVA

Representative repository: Sabbirnde/Nava.

## Chain

~~~text
TRIG-APP-START-IMPORT
-> LOCAL-JS-NETWORK-LOADER
-> MID-BLOCKCHAIN-MULTIHOP
-> C2-NAVA
~~~

Repository loader src/utils/errorHandler.js used:
- https://www.jsonkeeper.com/b/7EBZP
- https://www.jsonkeeper.com/b/PAB1R

Both returned the same 31,910-byte JSON on 2026-09-17.

SHA256:
195362265d3dfce1b669bbbb40f9ab385ccbde57012ec91871dd2e59d1b3a1e4

Static/offline analysis established:

~~~text
JSONKeeper
-> TRON account transaction
-> transform raw_data.data
-> BSC transaction hash
-> read BSC calldata
-> hex / reverse / XOR / Base64 / gzip
-> final backend URL
~~~

TRON address:
TBMcSRezuxdoNEC8gD3uS4GQVGvybYU4UU

TRON transaction:
a323fb6e0b9ff85d63f3fa5a16f2c369dd48acbb085460e4b7bb7c54ae1437fe

- block: 86260511
- timestamp: 2026-09-15 06:27:48 UTC
- result: SUCCESS

Recovered BSC transaction:
0xbf3fa7a35ab2ee7e243134f39e367b8774d32810fd9b66355c8eba135b2fca9f

Decoded final URL:
http://138.226.247.152:80/session/0f2a3fba69d29493

Exact final GET returned 236,314 bytes.

SHA256:
e60070ad02ab8f21e6f018c836a55e37901cf14b8ea8be1d2f8d117ce803235f

The JSON masquerades as Microsoft vscode-python-envs extension metadata. The sessions field is 206,004 characters.

sessions SHA256:
e22605a2363c05991146e2b7761be9077586ff5b61a7248c950b6e0e751cc958

A secondary backend 5.231.107.180:3000 was recovered from the final agent; TCP/3000 was observed open. Application routes on that service were not exercised.

Evidence state: **PROVEN / ACTIVE at observation**.
