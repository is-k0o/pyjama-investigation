# Bounded repository datasets

These files expose the two source lists behind the **158 captured repositories** described in the article.

- [core-68.csv](core-68.csv) — the 68-repository main investigation tracker, reconciled through the 2026-09-20 operational snapshot.
- [reverse-hunt-90.csv](reverse-hunt-90.csv) — the bounded fake-font reverse-hunt capture made on 2026-09-18 and reconciled on 2026-09-20.

The captured sets had **zero overlap**, so the publication count is:

```text
68 core repositories
+ 90 reverse-hunt repositories
= 158 distinct captured repositories
```

## Boundaries

These datasets are publication/audit indexes, not malware bundles.

- The 68-row core contains different evidence and operational states; not every repository was active at the same time.
- The 90-row reverse-hunt is a bounded search capture, not an exhaustive census of GitHub.
- A reverse-hunt signature match is code-lineage evidence. It is not, by itself, operator attribution or proof of a live downstream C2.
- Repository URLs may now return 404 or contain later changes.
- `state_observed_at`, `captured_at` and `reconciled_at` are included so state labels are not read as timeless claims.

The richer chain reconstruction, hashes and evidence boundaries are kept under [../evidence/](../evidence/).
