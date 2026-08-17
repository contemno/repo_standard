# Cross-pollination routine

This file is the versioned orchestrator prompt for the weekly cloud routine
**Repo standard cross-pollination**
(<https://claude.ai/code/routines/trig_01YF972aeWxAW3NETJkdMbER>). The routine
clones this repo plus the review set, reads this file, and follows it. The
per-stage subagent prompts live in `cross_pollination/`. Edit these files to
change the routine's behavior; changes take effect on the next run.

> **Maintenance:** the routine's clone sources must match the review set below.
> Editing the list here does not clone a new repo; the routine's
> `job_config.ccr.session_context.sources` must be updated to match.

These files are repo metadata, like `README.md` and `STANDARD.md`. Do not copy
them into projects that adopt the standard.

---

## Orchestrator instructions

You are the weekly cross-pollination orchestrator for contemno's repos
(all cloned into this environment). Run the three-stage pipeline below. Do not
do the stage work yourself: spawn subagents so each stage gets a clean context,
and pass each one the full contents of its stage prompt file.

### Context (include in every subagent prompt)

`contemno/repo_standard` is a template repo ("the standard") of
project-agnostic GitHub conventions: diff-classifying CI that skips (never
path-ignores) required checks, aggregator gate jobs, one reusable
build-release workflow, autotag with a VERSION floor, SHA-pinned actions with
grouped Dependabot, structured issue forms with invariant checkboxes, and a PR
template enforcing re-sync and docs-moved rules. `STANDARD.md` holds the
rationale; `ADOPTING.md` the checklist. Its purpose: capture lessons learned
across projects and spread them.

### Review set

All under `contemno/`:

- `freshroot`
- `luks-enroll`
- `scx_cohort`
- `thermal-hub`
- `packet-orchestrator`
- `dduper` (fork: only consider files contemno authored)

### Stage 1: harvest (one subagent per repo, in parallel)

First determine the harvest window. Find the most recent `Harvest:` issue on
`contemno/repo_standard` (open or closed:
`gh issue list --state all --search "Harvest: in:title"`) and use its date as
the window start; commit history before it was covered by previous runs. If no
such issue exists, this is the first run: use 3 months ago. A quiet stretch
with no harvest issues just widens the window back to the last one; dedupe
absorbs any re-reads.

For each review-set repo, spawn a subagent whose prompt is the contents of
`cross_pollination/harvest.md`, plus the context above, the repo's name, any
parenthetical note from the review set, and the window-start date. Collect
every subagent's findings. A subagent returning zero findings is a normal
outcome.

Then file the durable record yourself (harvest subagents never touch the
tracker). If any findings came back, open ONE issue on `contemno/repo_standard`
titled `Harvest: YYYY-MM-DD`: one section per repo with that repo's findings
verbatim, plus a closing line naming the repos with no findings. If every repo
returned nothing, open no issue and skip stage 2.

### Stage 2: incorporate (one subagent, after all harvests finish)

Spawn one subagent whose prompt is the contents of
`cross_pollination/incorporate.md`, plus the context above, ALL stage 1
findings, and the harvest issue URL. Wait for it to finish before stage 3, and
record the PR URL it reports, if any.

### Stage 3: propagate (one subagent per repo, in parallel)

For each review-set repo, spawn a subagent whose prompt is the contents of
`cross_pollination/propagate.md`, plus the context above, the repo's name and
note, and the stage 2 PR URL if one was opened. Propagate compares against the
LOCAL `repo_standard` checkout, which now includes anything stage 2 committed.

### Final summary

End with: harvest counts (found, accepted, rejected), the harvest issue URL or
"no findings", the incorporate PR URL or "no changes", issues opened or
updated per repo, repos with no findings, and any repo that could not be
accessed.

### Shared rules (include in every subagent prompt)

- Only act on items with concrete evidence and clear cross-project benefit;
  fewer, stronger suggestions beat a laundry list. Zero findings is a valid
  outcome; do not pad.
- If `gh` cannot access a repo or issues are disabled, note it in your report
  instead of failing.
