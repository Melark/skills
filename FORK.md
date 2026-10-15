# This is a fork

Upstream: **[mattpocock/skills](https://github.com/mattpocock/skills)** (MIT).
This fork exists so local changes are tracked, survive reinstalls, and can be
merged against upstream instead of silently overwritten.

## Why a fork and not edits in `~/.claude/skills/`

These skills are a **system**, not independent files: at least eight call each
other by name through the Skill tool, and `ask-matt` documents the chain. A
rename or behaviour change in one therefore has to be followed through its
callers. Editing the installed copies gives no diff, no reason, and no merge
path — and the next install overwrites the lot.

## Staying current

```bash
git fetch upstream && git merge upstream/main
```

Conflicts land exactly where this fork diverged, which is the signal you want.

## Changes against upstream

Keep this list short, and record *why*, not just *what*.

### `code-review` → `code-review-two-axis`

Claude Code ships a **built-in `/code-review`** (correctness bugs, with a
verification pass that marks findings CONFIRMED or PLAUSIBLE, plus `--fix` and
`--comment`). Upstream's skill has the same name but a different job: Standards
(repo conventions + a Fowler smell baseline) and Spec (does the diff do what
the issue asked), run as parallel sub-agents and deliberately not merged.

Two skills, one name, so invocation was ambiguous. Renaming upstream's keeps
both available — but the rename alone silently rewires `implement`,
`implement-spec` and `tdd`, whose `code-review` calls would then resolve to the
built-in. Those three and `ask-matt` are updated to the new name, so the chain
still does what its author intended.

They are complementary. The built-in asks *is this code wrong?*; this one asks
*does it match our standards and what we actually asked for?*
