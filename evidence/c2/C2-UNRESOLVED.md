# C2-UNRESOLVED cases

C2-UNRESOLVED is a **classification state**, not a backend architecture.

## SURUJ404/NFT-GAMEFY

~~~text
TRIG-VSCODE-FOLDEROPEN
-> LOCAL-FAKE-ASSET-NODE
-> MID-DEADDROP-JSON
-> C2-UNRESOLVED
~~~

Fake font:
- path: public/fontawesome/fa-solid-400.woff2
- Git blob/object ID: 0d1d6424451ac50db7efa8c7bdd804c9e96848a4

Static unpacking recovered:

~~~text
https://www.jsonkeeper.com/b/3KIH4
-> res.data.model
-> Function.constructor("require", ...)
~~~

The same 3KIH4 loader is independently present in server/controllers/product.js.

Exact dead-drop observation on 2026-09-20:

~~~json
{"status":404}
~~~

- body size: 14 bytes
- SHA256: 67795bf3846287322b92aee164b5745a75b6835d1fbefdcb0361745ec47cdf26

The known payload object was absent, so the final backend was not recoverable.

## rony1235/Jp-Soccer

The malicious loader/sink is proven, but the exact current AUTH_API is absent from the public snapshot.

https://ipcheck-six.vercel.app/api is a strong sibling candidate recovered from a byte-identical sibling repository, but it remains **INFERRED-CANDIDATE**, not directly attributable to rony1235/Jp-Soccer.

State: INDETERMINATE - C1 UNRESOLVED.

## STALE fake-font triggers

These repositories retain hidden folderOpen tasks, but the referenced fake .woff2 payload is absent at current HEAD and at the indexed task-introducing commit:
- JHK0723/MIC-JailbreakLLM
- PinsaraPerera/Student_Attendace_System
- Rakshi2609/BhagVirusHei

Classification: **STALE / BROKEN T2**.
