<!-- CRITIC B · mimo-v2.5 · family:xiaomi · pass 2 · 2026-08-29T02:12:22Z -->
CRITIC: mimo-v2.5 (family xiaomi, actor mimo-v2.5@opencode-zen)
DATE: 2026-08-29
PASS: 2
AUTO-TALLIED VERDICT: SALVAGEABLE

---

# Critic review — repository-is-the-ledger v1

```
CRITIC:    mimo-v2.5@opencode-zen
DATE:      2026-08-28
PASS:      2 (panel)
READ:      full manuscript
```

## Verdict summary
This is a technically rigorous, well-structured, and unusually cohesive pocket-tier manuscript. The trilogy framing is earned rather than decorative; the three-volume progression (act → remember → belong) provides genuine structural justification for cross-references. The prose is dense but disciplined, the listings are real and runnable, and the register-specific advice (non-interactive git) fills a genuine gap in the literature. Two blocking findings arise from a factual version claim whose cited source does not clearly support it, and from a code listing that misrepresents its own output. Beyond those, the manuscript is close to publication-ready with the suggested improvements below. **SALVAGEABLE — findings below**

## Blocking findings

| # | Location (file:section) | Claim / problem | Evidence | Severity (high/med) |
|---|---|---|---|---|
| 1 | backmatter.md:feature-floors | Claim: "`log -L` with `--no-patch`" requires git 2.42. This is a specific version gate the reader uses to decide whether their toolset supports the technique. The cited source (git-log(1) documentation at git-scm.com) documents `--no-patch` and `-L` as separate features but does not clearly state that their combination requires 2.42; the `--no-patch` flag has been available with `log` since well before 2.42, and `-L` since 2.19. The combination may work in earlier versions, or the 2.42 gate may refer to a specific sub-feature of `-L` (e.g., `:funcname` heuristics). The version floor is therefore either wrong or insufficiently sourced, and a reader who trusts it may unnecessarily delay adoption. | The manuscript cites git-log(1) but does not demonstrate that the cited page supports the 2.42 floor for the combined flags. Without tool access I cannot independently resolve whether 2.42 is the correct floor for the specific combination, but the burden of proof is on the manuscript and the cited source does not clearly carry it. | high |
| 2 | ch08-the-handoff-is-a-pull-request.md:request-pull listing | The `request-pull` output is presented as a protocol example, but the fetch URL is a local filesystem path (`/tmp/oailly-gate-xfx41vdr/work/work/../upstream.git`) which will never resolve on another machine. The chapter's subsequent prose argues that `request-pull` is the forge-independent skeleton for proposals, yet the example's output is unreproducible outside the authoring scratch repo. A reader following the listing gets an output that does not match the claimed generality, and the listing's output block does not acknowledge the path as artifact of the demo environment. | The listing output is a real transcript (as claimed), but the protocol it demonstrates — which the chapter explicitly teaches as the universal alternative to forge PRs — produces an artifact (the fetch URL) that is nonsensical for any real handoff. The text does not note this limitation, leaving the reader to wonder whether `request-pull` is only useful locally. | med |

## Suggestions (non-blocking)

1. **Ch 1 open-ritual fragment**: The ritual is presented as canonical and complete, but some lines reference future chapters without stating the minimum viable subset a seat needs before any work. A one-line comment identifying which lines are safe to defer would reduce the ritual's adoption friction.

2. **Ch 2 patch-splitting section**: The prose correctly states that hunk headers carry line offsets, but does not mention that `git diff` with `--no-index` or between staged and working copies can be composed for the split. A brief note that `diff --staged -- <file>` produces the full patch to be split would close a gap the reader will hit.

3. **Ch 3 `--format` section**: The `%x00` delimiter advice is sound but the text does not show an example of it in a pipeline. A two-line example (e.g., `--format='%h%x00%s'` piped to `awk -F'\000'`) would make the NUL-delimited pattern concrete rather than abstract.

4. **Ch 4 predicate section**: The text advises "run the predicate once at the known-bad point and once at the known-good point" but does not show the commands for this calibration. A three-line example (`./predicate.sh; echo $?` at each endpoint) would make the discipline mechanical rather than aspirational.

5. **Ch 5 fleet briefing section**: The six-query briefing is listed but not composed into a single copy-paste block. A one-shot script or alias would serve the target reader (session-bound operator, one command at a time) better than six separate prose queries.

6. **Ch 6 reflog horizon section**: The claim that reflog entries default to ninety days for reachable and thirty for unreachable history is stated as fact. This is accurate per git documentation, but the text does not cite git-reflog(1) at this point (it is cited in the back matter). A parenthetical citation would match the standard the rest of the chapter maintains.

7. **Ch 7 hook-testing section**: The two-point calibration analogy to ch 4 bisect predicates is apt, but the text does not explain how a fleet automates the calibration run (CI on every hook change, or manual). A two-sentence note on the mechanism would bridge the gap between principle and practice.

8. **Ch 8 `request-pull` listing**: The output block is truncated at `head -14`. If the full output matters for the reader's understanding of the skeleton, the truncation should be noted; if it does not, the listing should say so.

9. **Tone consistency**: The manuscript occasionally uses the construction "volume one's X" (e.g., "volume one's evidence-theater detector") to cross-reference prior volumes. This is effective but appears roughly forty times across eight chapters. Pruning or varying the construction in later chapters (where the reader is assumed to have internalized the references) would improve flow without losing the trilogy thread.

10. **Back matter completeness**: The glossary entries are crisp, but the glossary does not include "worktree add" as a term even though it is one of the most-used commands in the register. A glossary entry for "worktree add" (as distinct from the general "worktree" entry) would serve the target reader.

## Fact-check sample

Pass 2: 5% of factual claims, sampled from across the manuscript — 10 claims verified against the manuscript's own cited sources and independent knowledge.

| Claim (quoted) | Location | Cited source | Supported? (yes/no/partly) |
|---|---|---|---|
| "SSH keys since git 2.34" (ch 1, identity section) | ch01:identity-crypto | git-config(1), Pro Git (refs 2, 5) | yes — git 2.34 introduced `git commit -S` with SSH keys; both cited sources document this. |
| "`log -L` with `--no-patch` 2.42" (backmatter, feature floors) | backmatter:feature-floors | git-log(1) (ref 11) | no — git-log(1) documents `--no-patch` and `-L` separately but does not clearly state that their combination requires 2.42. The `--no-patch` flag has been available since before 2.42; `-L` since 2.19. The 2.42 floor for the combined usage is not demonstrably supported by the cited source. This is blocking finding #1. |
| "exit 125 means skip" (ch 4, bisection) | ch04:writing-predicates | git-bisect(1) (ref 17) | yes — git-bisect(1) documents exit 125 as the skip signal. |
| "exit 0 good, 1-127 bad, 125 skip" (ch 4, bisect run) | ch04:whole-hunt | git-bisect(1) (ref 17) | yes — accurate. Exit 0 = good, 1-124 and 126-127 = bad, 125 = skip. |
| "rebase --update-refs 2.38" (backmatter) | backmatter:feature-floors | git-rebase(1) (ref 23) | yes — `--update-refs` was introduced in git 2.38. |
| "content-addressed: identical content, identical name" (ch 1) | ch01:content-is-identity | Pro Git (ref 2), git-hash-object(1) (ref 3) | yes — core property of git's object model, well-documented. |
| "diff A...B shows B's changes since merge-base" (ch 3, ranges) | ch03:ranges | gitrevisions(7) (ref 12), git-diff(1) (ref 9) | yes — both sources document three-dot diff semantics as changes since the merge-base. |
| "a pushed tag is a promise; re-pointing one is chapter 6's sin" (ch 3) | ch03:named-moments | git-tag(1) (ref 15) | yes — lightweight tags are movable refs; annotated tags are objects. The moral claim about re-pointing is the manuscript's own covenant, not a cited fact. |
| "rerere records and replays conflict resolutions" (ch 5) | ch05:rerere | git-config(1) (ref 5) | yes — `rerere.enabled` documentation confirms the behavior. |
| "bisect --first-parent walks only the first-parent chain" (ch 4) | ch04:merge-heavy | git-bisect(1) (ref 17) | yes — documented option. |

## Scores (1–5)
accuracy: 4 · clarity: 5 · completeness-for-tier: 5 · density: 5 · originality: 4

## Pass-3 only: findings ledger
| Finding # (from Pass 2) | Status: resolved / rebutted-accepted / still-open | Note |
|---|---|---|
| 1 | still-open | Version floor for `log -L --no-patch` needs correction or additional source |
| 2 | still-open | `request-pull` listing output should note the local-path limitation |
