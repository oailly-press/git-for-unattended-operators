<!-- CRITIC C · hy3 · family:tencent · pass 2 · 2026-08-29T02:14:22Z -->
CRITIC: hy3 (family tencent, actor hy3@opencode-zen)
DATE: 2026-08-29
PASS: 2
AUTO-TALLIED VERDICT: SALVAGEABLE

---

# Critic review — the-repository-is-the-ledger v1

```
CRITIC:    hy3@opencode-zen
DATE:      2026-08-28
PASS:      2 (panel)
READ:      full manuscript
```

## Verdict summary
This is a technically rigorous, well-structured pocket-tier manual that successfully reframes git as a ledger for unattended operators. The draft is internally coherent, the listings are plausible and the version-floor claims are almost entirely accurate, with one genuine self-contradiction (provenance vs. fragment policy) and two claims the critic cannot independently resolve without re-execution. The prose occasionally overstates editorial gloss as fact but never undermines the craft. **SALVAGEABLE — findings below.**

## Blocking findings

| # | Location (file:section) | Claim / problem | Evidence | Severity (high/med) |
|---|---|---|---|---|
| 1 | provenance.md ("WRITTEN BY") vs frontmatter.md (Introduction, "no-run / fragment" definitions) and ch08 (```bash fragment``` block) | Provenance asserts "Every listing was composed, executed, and its real output captured by the author," but the frontmatter defines fragments as listings "never executed on your behalf," and ch08 contains a ```bash fragment``` block (the `gh pr …` commands) that was not executed. The blanket provenance claim contradicts the book's own stated execution policy. | provenance.md line "Every listing was composed, executed…"; frontmatter "fragments are never executed on your behalf"; ch08 fragment block has no transcript. | med |
| 2 | ch01 "Content is identity" transcript | Asserts the exact SHA-1 `53d37c741becf6b5212e1c56ea94b5a38d1145fe` for blob content `retries = 5\n`. The critic cannot independently confirm the literal hash value without re-running; the publisher gate re-executes runnable listings, so this is resolvable at the gate, but it is unverified at review. | ch01 transcript; backmatter "A note on measured outputs" already concedes commit hashes differ on re-run (blob hashes are deterministic but unconfirmed here). | med |
| 3 | backmatter "Feature floors" | States `git log -L` with `--no-patch` requires git 2.42. `--no-patch` (`-s`) has been available in `git log` since ~2.10 and works with `-L` well before 2.42; the stated floor is likely overstated and should be confirmed against release notes. | git-log(1) knowledge; backmatter feature-floors row. | med |

## Suggestions (non-blocking)
1. Demonstrate or explicitly note that no listing in this volume needed the `no-run` marking, so the "three markings" claim reads as series policy rather than a promise that all three appear here.
2. The "porcelain name is a historical joke … named after the human layer" gloss in ch01 is editorial; a one-line citation to the plumbing/porcelain etymology would keep the tone consistent with the rest of the book's cited discipline.
3. Consider a short "command index" appendix mapping each chapter's listings to the man pages in the References section; pocket-tier readers benefit from a lookup surface.
4. ch07 discusses server-side `pre-receive`/`update` hooks only briefly; a one-paragraph contrast with forge protected-branch rules (already referenced) would help the "gates are authority" point land for forge-only readers.
5. The `git range-diff` tool (natural companion to ch05 stacked proposals / ch06 reshaping) is absent; a pointer in ch05 or ch08 would close a small gap.

## Fact-check sample

| Claim (quoted) | Location | Cited source | Supported? (yes/no/partly) |
|---|---|---|---|
| "git does not record renames" | ch02 "Renames are inferred, not recorded" | gitglossary(7), git-mv(1) (refs 1, 38) | yes |
| "`--force-with-lease` … compares against the local remote-tracking ref — your last fetch's knowledge" | ch06 "The lease, precisely" | git-push(1) (ref 29) | yes |
| "SSH keys since git 2.34 … the practical arrival" | ch01 "Identity, cryptographically" / backmatter feature floors | git docs / feature floors | yes |
| "exit 0 declares this commit good, exit 1 through 127 declares it bad — except 125 … skip" | ch04 "The whole hunt, unattended" / glossary | git-bisect(1) (ref 17) | yes |
| blob hash `53d37c741becf6b5212e1c56ea94b5a38d1145fe` for `retries = 5` | ch01 "Content is identity" | manuscript's own measured run / git-scm | limitation — critic cannot re-execute; resolve at publisher gate |
| "`rebase --update-refs` 2.38" | backmatter feature floors | git-rebase(1) (ref 23) | yes |
| "reflog entries expire (order of ninety days for reachable history, thirty for the unreachable)" | ch06 "The horizon" | git-reflog(1) / gc config (ref 26) | yes |
| "`git log -L` with `--no-patch` 2.42" | backmatter feature floors | git-log(1) (ref 11) | partly — limitation: `--no-patch` predates 2.42; floor likely wrong |

## Scores (1–5)
accuracy: 4 · clarity: 5 · completeness-for-tier: 4 · density: 5 · originality: 4

## Pass-3 only: findings ledger
| Finding # (from Pass 2) | Status: resolved / rebutted-accepted / still-open | Note |
|---|---|---|
*(Pass 3 ledger not applicable at Pass 2)* |
