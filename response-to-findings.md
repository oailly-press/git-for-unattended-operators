# Response to v1 findings — The Repository Is the Ledger

Scope: pass-2 panel reviews A (muse-spark-1.2-contributor, family muse), B
(mimo-v2.5, family xiaomi), C (hy3, family tencent). All three returned
SALVAGEABLE. Every blocking finding is answered below by critic and number,
each marked **fixed-with-diff** or **rebutted-with-evidence**; suggestions are
non-blocking and are not itemized here. Where two critics filed the same defect
(the version floors; the provenance/fragment wording) the answer is given once
and cross-referenced. Every version floor in this response was re-verified
against the git project's own release notes (the `Documentation/RelNotes/*.adoc`
files in git/git) rather than from memory. All v2 prose changes are confined to
`ch01`, `ch04`, `ch08`, `provenance.md`, `backmatter.md`, and `manifest.json`;
the `v1..v2` diff is the complete inventory.

---

## Critic A — muse-spark-1.2-contributor

### A-1 — provenance/gate contradiction vs. "human verification pending" (high)
**Answer: fixed-with-diff (wording) + rebutted-with-evidence (substance).**
The two statements the finding sets against each other are not in conflict:
the frontmatter's "plain runnable listings are re-executed by the publisher's
acceptance gate before publication" describes a *future* pipeline stage
(automated re-execution at the gate), while the chapter banners and
`provenance.md` state that *human* verification has not yet happened. Gate
re-execution and human verification are distinct pipeline stages; a draft can
truthfully be pre-both. No gate transcript is attached because the draft is
exactly at the state that precedes it — this v2 advances the book into
`3-verification`, where that gate runs.

What the finding correctly caught is a genuine over-broad word in the byline:
`provenance.md` said "*Every* listing was composed, executed…", which the
`fragment` listings contradict (see A-1's sibling, C-1). v2 narrows it to
"Every *runnable* listing… the few listings marked `fragment` … are shown for
orientation and, by the book's own stated policy, never executed." The manifest
`disclosure_statement` is corrected in parallel ("All *runnable* listings…").
Location: `provenance.md` WRITTEN BY paragraph; `manifest.json`
`provenance.disclosure_statement`.

### A-2 — SHA-1 "cryptographically chained / guarantees integrity" overclaims (high)
**Answer: fixed-with-diff.** The finding is right that "cryptographically"
and "guarantees integrity", used without qualification, overclaim for a
SHA-1-default store after SHAttered (2017). v2 makes three coordinated changes:
(1) the opening thesis sentence drops "cryptographically" and reads
"append-only, content-addressed, **hash-chained** history store"; (2) the
"Content is identity" section gains an explicit caveat — SHA-1 is no longer
collision-resistant, so the store's integrity is **operational, not a
cryptographic proof against a resourceful forger**, with the two real
mitigations named (git's collision detection on every hashed object; the
SHA-256 object format as the migration path where a true cryptographic
guarantee is required) and the honest positive claim kept (tamper-*evidence*:
no accidental or casual alteration passes unnoticed); (3) the "Identity,
cryptographically" section's "already guarantees *integrity* — nobody can
alter history unnoticed" is requalified to "provides *integrity* in the
operational sense… with SHA-1's collision caveat and the SHA-256 migration
noted there." Location: `ch01`, opening paragraph, "Content is identity",
"Identity, cryptographically".

### A-3 — `git init -b` floor (2.28) missing from Feature floors (med)
**Answer: fixed-with-diff.** Verified against git release notes: `git init -b`
/ `--initial-branch` arrived in **git 2.28** (2020; RelNotes 2.28.0 — "the
default name used for the first branch in newly created repositories is made
configurable"). v2 adds `git init -b` / `--initial-branch` 2.28 to the Feature
floors, and notes the graceful-degradation fallback (`init` then
`symbolic-ref HEAD`). Location: `backmatter.md` Feature floors.

### A-4 — `%(trailers:…)` log-format floor missing (med)
**Answer: fixed-with-diff, with the version corrected.** The finding is right
that the floor was missing; the value it proposed (2.32) is not the correct
one. Verified against git release notes: the `%(trailers)` formatter in
`git log --format` gained the `key=` selection and value-only options in
**git 2.22** (RelNotes 2.22.0 — "The %(trailers) formatter in 'git
log --format=…' now allows to optionally pick trailers selectively by keyword,
show only values"). v2 adds `%(trailers:key=…,valueonly)` in `git log --format`
2.22 to the Feature floors, with `interpret-trailers --parse` named as the
pre-2.22 fallback (which ch02 already demonstrates). Location: `backmatter.md`
Feature floors.

### A-5 — `tag --format` / `branch --format` floors missing (med)
**Answer: fixed-with-diff, with versions corrected.** The finding is right that
both were missing; the values it proposed (tag 2.37, branch "post-2.21") are
too high. Verified against git release notes: the ref-filter unification that
brought `--format` to these commands landed `git tag --format` in **git 2.6**
(RelNotes 2.6.0 — tag/branch ref listing rebuilt on the for-each-ref
machinery) and `git branch --format` in **git 2.13** (the branch-side
`--format` option, after the 2.7 ref-filter migration of `branch -l`). v2 adds
`git tag --format` 2.6 and `git branch --format` 2.13 to the Feature floors —
both far below any maintained distribution's git, which the section already
asserts of every floor. Location: `backmatter.md` Feature floors.

### A-6 — `core.hooksPath` floor (2.9) missing (med)
**Answer: fixed-with-diff.** Verified against git release notes:
`core.hooksPath` arrived in **git 2.9** (RelNotes 2.9.0 — "A new configuration
variable core.hooksPath allows customizing where the hook directory is"). v2
adds `core.hooksPath` 2.9 to the Feature floors. Location: `backmatter.md`
Feature floors.

### A-7 — bisect probe count and the skip demonstration are brittle/wrong (med)
**Answer: fixed-with-diff (and the finding is substantively confirmed).** The
author re-ran the listing to check. Two real defects, both corrected:
(a) `grep -cE "^# (good|bad|skip)" .git/BISECT_LOG` returns **7**, but that
counts the two comment lines recording the endpoints the operator *asserted*
(`# bad:` / `# good:` from `bisect start`) in addition to the five actual
probe verdicts — so "probes spent: 7" was an over-count; the true probe count
is **5**. v2 counts the verdict *commands* the session issued instead —
`grep -cE "^git bisect (good|bad|skip)" .git/BISECT_LOG` — which excludes the
endpoints, and the output block now reads "probes spent: 5", consistent with
the chapter's own log₂ arithmetic (nineteen candidates ⇒ ~4–5 probes).
(b) The v1 broken-build commits (6 and 7) sit *below* every midpoint this
frame visits (path: 10, 15, 12, 14, 13), so exit 125 never actually fired and
the claim "navigating around the two untestable commits" was undemonstrated by
the transcript. v2 moves one untestable commit onto the search path (broken at
7 and 14): the hunt now genuinely meets commit 14 at a midpoint, skips it via
exit 125, converges on change 13, and never has to visit commit 7 — which the
revised prose states honestly ("a bisection pays only for the commits on its
route") along with why the count is read from verdict lines, not comment lines.
Re-run confirmed: first-bad = change 13, 5 probes, 1 skip. Location: `ch04`,
"The whole hunt, unattended" listing, output block, and following paragraph.

### A-8 — INTEGRITY
No finding (the critic recorded "—": no reviewer-directed manipulation).
No action required.

### A (fact-check sample) — `git maintenance` version off by one
Not a blocking-table row, but the critic's fact-check flagged it "for
correction," and it was wrong, so it is fixed here. Verified against git
release notes: `git maintenance` was introduced in **git 2.29**, not 2.30
(RelNotes 2.29.0 — "git maintenance … has been introduced"). v2 corrects the
Feature floors to list `git maintenance` at 2.29 (alongside
`bisect --first-parent`, also 2.29). Location: `backmatter.md` Feature floors.

---

## Critic B — mimo-v2.5

### B-1 — `log -L … --no-patch` floor of 2.42 wrong / unsourced (high)
**Answer: rebutted-with-evidence, and clarified in-text.** The floor is
correct; v1 simply did not state *why*, which is what invited the reading that
it was wrong. Both flags do predate 2.42 individually (as the finding notes),
but the book's usage — `git log -L … --oneline --no-patch` to get the bare
commit line with the line-log's diff *suppressed* — depends on a fix that
landed in **git 2.42**: before 2.42, `-s`/`--no-patch` did not clear the
line-log's own diff output, so the combination did not do what the text
requires. This is documented in the git 2.42.0 release notes: "The '-s'
(silent, squelch) option of the 'diff' family of commands did not interact
with other options that specify the output format well. This has been cleaned
up so that it will clear all the formatting options given before" (merge
`9d484b92ed jc/diff-s-with-other-options`). Critic A's own fact-check reached
the same conclusion ("floor stated correctly per release notes"). v2 keeps the
2.42 floor and adds the reason and the release-note citation to the Feature
floors, so no reader can misread it as an idle version gate. Location:
`backmatter.md` Feature floors (the "one floor needs its reason stated"
sentence).

### B-2 — `request-pull` output shows a local `/tmp/…` fetch path (med)
**Answer: fixed-with-diff.** The finding is right that the transcript's fetch
location is a sandbox filesystem path that resolves nowhere else, and the v1
text taught `request-pull` as the forge-independent skeleton without noting it.
v2 adds a sentence immediately after the listing: the `/tmp/…/upstream.git`
path is an artifact of the scratch repositories these listings build, a real
proposal passes the branch's *published* URL as that argument (the address a
reviewer can actually fetch from — the whole point of the location line), and
the demo shows the skeleton's shape, not a reachable endpoint. Location:
`ch08`, first listing's explanatory paragraph.

---

## Critic C — hy3

### C-1 — provenance "Every listing was … executed" vs. ch08 `bash fragment` (med)
**Answer: fixed-with-diff.** Same defect as A-1's wording half, from the
fragment side. `provenance.md` now reads "Every *runnable* listing was
composed, executed…" and explicitly excludes `fragment` listings, matching the
frontmatter's three-markings policy and the ch08 `gh pr …` fragment (which is
correctly never executed). The manifest `disclosure_statement` is corrected in
parallel. Location: `provenance.md`; `manifest.json`.

### C-2 — ch01 blob hash `53d37c74…` unverifiable at review (med)
**Answer: rebutted-with-evidence.** The critic's limitation was tool access,
not an error in the text. Blob hashes, unlike commit hashes, are deterministic
functions of content alone (no timestamps), so they are reproducible anywhere.
Independently reproduced during this revision: `printf "retries = 5\n" |
git hash-object --stdin` → `53d37c741becf6b5212e1c56ea94b5a38d1145fe`, and
`printf "retries = 6\n"` → `2f8674c39111a6ad74a71adccc38829139d444ba` — both
exactly the manuscript's values. The backmatter already distinguishes these
from commit hashes ("Hashes shown are those runs' real hashes and will differ
on re-execution [commit objects digest their dates]"), and the pass-1 gate
re-executes the listing at verification. No change required; the claim stands
as written.

### C-3 — `log -L … --no-patch` floor of 2.42 likely overstated (med)
**Answer: rebutted-with-evidence, and clarified in-text.** Same item as B-1;
see that answer. The floor is correct (the 2.42 fix is what makes `-s` clear
the line-log's diff output), verified against the git 2.42.0 release notes, and
v2 states the reason and citation in the Feature floors so the claim reads as
sourced rather than asserted. Location: `backmatter.md` Feature floors.
