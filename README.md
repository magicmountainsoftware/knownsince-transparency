# KnownSince transparency log

Daily, append-only, hash-chained log of everything stamped at
[knownsince.com](https://knownsince.com) — published here so our history cannot
be quietly rewritten, by us or anyone else.

## What gets published

One file per UTC day: `entries/<date>.json`, with canonical bytes:

```json
{"log":"knownsince-transparency","date":"YYYY-MM-DD","count":N,"merkle_root":"<hex>","prev":"<hex>"}
```

- **merkle_root** — folds the day's distinct SHA-256 fingerprints into one hash.
  Leaves are the digests as raw bytes, sorted byte-lexicographically; each level
  pairs left-to-right with `sha256(left || right)`, promoting an odd last node
  unchanged. Zero leaves → `null`; one leaf → the leaf itself.
- **prev** — the SHA-256 of the previous day's entry file bytes. Every entry
  therefore commits to the entire history before it: altering any past day
  breaks every entry after it.
- Each entry's own SHA-256 is additionally timestamped through KnownSince
  itself, anchoring the chain head in the Bitcoin blockchain (proof link in
  each commit message).

## How to verify

1. `sha256sum entries/<yesterday>.json` — compare with the `prev` field of
   today's entry. Walk backwards as far as you like.
2. Pick any commit message's proof link and verify the entry hash against
   Bitcoin with the [OpenTimestamps](https://opentimestamps.org) client —
   independently of both KnownSince and GitHub.
3. If you stamped something on a given day, recompute the merkle root from the
   published algorithm and check your fingerprint's day includes it.

Published automatically by the KnownSince transparency bot.
Repo: `magicmountainsoftware/knownsince-transparency` · Operated by Magic Mountain Software.
