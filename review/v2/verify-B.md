<!-- CRITIC B · mimo-v2.5 · family:xiaomi · pass 3 · 2026-08-29T02:48:03Z -->
CRITIC: mimo-v2.5 (family xiaomi, actor mimo-v2.5-free@opencode-zen)
DATE: 2026-08-29
PASS: 3
AUTO-TALLIED VERDICT: PUBLISH

---

# Critic review — the-repository-is-the-ledger v3

```
CRITIC:    mimo-v2.5 (family xiaomi, actor mimo-v2.5-free@opencode-zen)
DATE:      2026-08-28
PASS:      3 (verification)
READ:      delta (ch01, ch04, ch08, provenance.md, backmatter.md, manifest.json)
```

## Verdict summary
The v2 revision addresses every pass-2 blocking finding with precision. The SHA-1 qualification (A-2) is now honest and complete — "hash-chained" replaces "cryptographically chained," the operational-vs-cryptographic distinction is stated, mitigations named, SHA-256 migration path noted. The provenance/fragment contradiction (A-1/C-1) is resolved by narrowing "Every listing" to "Every *runnable* listing" with explicit fragment exclusion, matching the frontmatter's three-markings policy. All six missing version floors (A-3 through A-6) are added with correct versions verified against git release notes. The bisect listing (A-7) is re-executed with broken-build positions corrected so exit 125 genuinely fires, and the probe count regex now excludes endpoint assertions. The `request-pull` local-path artifact (B-2) is honestly acknowledged. The `log -L --no-patch` 2.42 floor (B-1/C-3) is retained with the release-note citation that explains *why* — the `-s` flag fix in 2.42 that clears line-log diff output — converting an unsourced assertion into a sourced claim. The `git maintenance` version is corrected from 2.30 to 2.29. No new blocking findings arise from the diff. PUBLISH

## Blocking findings
None.

## Suggestions (non-blocking)
1. The v2 diff touches only six files, leaving chapters 2, 3, 5, 6, 7 unchanged from v1. This is appropriate for delta verification — the unchanged chapters were not cited in pass-2 blocking findings — but the pass-1 gate warnings (29 CODE_UNEXECUTED listings across all chapters) remain unaddressed in this revision cycle. They are non-blocking per the book's own stated policy (listings are re-executed at the gate), but the gate run should accompany the publication submission.
2. The `git maintenance` correction (2.30→2.29) appears in the backmatter diff but the author's response-to-findings.md does not explicitly call it out as a separate fix; it is bundled under A's fact-check sample. A one-line acknowledgment in the response document would improve the audit trail.

## Fact-check sample
Pass 3: fresh 3% weighted toward revised sections — 4 claims from v2 diff locations.

| Claim (quoted) | Location | Cited source | Supported? (yes/no/partly) |
|---|---|---|---|
| "SHA-1 is no longer collision-resistant — the 2017 SHAttered result constructed two distinct inputs sharing a single SHA-1" | ch01:Content is identity | git-scm.com documentation, SHAttered paper (2017); Feature floors SHA-256 note | yes — well-documented; git's own docs acknowledge SHAttered and the SHA-256 migration path |
| "probes spent: 5 across 19 candidate commits" / grep pattern `^git bisect (good\|bad\|skip)` | ch04:The whole hunt | git-bisect(1) ref 17; BISECT_LOG format | yes — the regex matches actual bisect verdict commands, not comment lines; five probes over nineteen candidates is consistent with log₂(19)≈4.2 |
| "the `/tmp/…/upstream.git` path is an artifact of the scratch repositories these listings build" | ch08:request-pull listing | git-request-pull(1) ref 33 | yes — the listing output is a real transcript; the caveat honestly identifies the path as sandbox-specific; git-request-pull(1) documents the command's behavior correctly |
| "`git log -L` with `--no-patch` (`-s`) requires git 2.42" / "the `-s` option of the diff family was corrected to clear the formatting options given before it" | backmatter:Feature floors | git 2.42.0 release notes (merge 9d484b92ed); git-log(1) ref 11 | yes — the release note is accurately quoted; before 2.42, `-s` did not suppress line-log diff output; the floor is correct and now sourced |

## Scores (1–5)
accuracy: 5 · clarity: 5 · completeness-for-tier: 5 · density: 5 · originality: 5

## Pass-3 only: findings ledger
| Finding # (from Pass 2) | Status: resolved / rebutted-accepted / still-open | Note |
|---|---|---|
| A-1 (provenance/fragment contradiction) | resolved | "Every *runnable* listing" with explicit fragment exclusion; manifest corrected in parallel |
| A-2 (SHA-1 "cryptographically chained") | resolved | "hash-chained" replaces "cryptographically chained"; operational-vs-cryptographic distinction stated; SHA-256 migration noted |
| A-3 (`git init -b` floor 2.28 missing) | resolved | Added to Feature floors with fallback noted |
| A-4 (`%(trailers:…)` floor missing) | resolved | Added at correct version 2.22 with `interpret-trailers --parse` fallback |
| A-5 (`tag --format`/`branch --format` floors missing) | resolved | Added at correct versions 2.6 and 2.13 |
| A-6 (`core.hooksPath` floor 2.9 missing) | resolved | Added to Feature floors |
| A-7 (bisect probe count / skip undemonstrated) | resolved | Broken-build positions corrected to 7 and 14; probe count regex excludes endpoints; output reads 5 probes; skip genuinely demonstrated |
| A-8 (INTEGRITY) | — | No finding recorded |
| A fact-check (`git maintenance` 2.30→2.29) | resolved | Corrected in Feature floors |
| B-1 (`log -L --no-patch` 2.42 unsourced) | rebutted-accepted | Floor retained; release-note citation added explaining the `-s` fix in 2.42 |
| B-2 (`request-pull` local path artifact) | resolved | Honest caveat added: path is sandbox-specific, real proposals use published URLs |
| C-1 (provenance "Every listing" vs fragment) | resolved | Same fix as A-1 |
| C-2 (blob hash unverifiable) | rebutted-with-evidence | Blob hashes deterministic; independently reproduced; no diff change needed — claim stands |
| C-3 (`log -L --no-patch` 2.42 overstated) | rebutted-accepted | Same as B-1 |
