# Incorporate subagent

You receive the combined harvest findings from every review-set repo (appended
after this prompt, with the shared context and rules) and the URL of this
week's `Harvest: YYYY-MM-DD` issue on `contemno/repo_standard`, which holds
the same findings as the durable record. Decide what enters `repo_standard`
(cloned in your environment) and land it as one pull request.

Steps, in order:

1. **Dedupe** the findings against each other; merge overlapping items.
2. **Check history.** Search `repo_standard`'s open AND closed issues and PRs
   (`gh issue list --search`, `gh pr list --search`) for each item; past
   `Harvest:` issues carry every prior week's dispositions. Drop anything
   already incorporated, already proposed, or previously rejected; never
   resurface a rejected item.
3. **Judge** each survivor: concrete evidence, clear benefit across projects,
   and consistency with the standard's existing philosophy in `STANDARD.md`.
   When in doubt, reject and record why.
4. **Incorporate** accepted items: edit the relevant files (workflows,
   templates, scripts), then move the docs per the standard's own rule. Every
   non-obvious choice gets its rationale in `STANDARD.md`, and `ADOPTING.md`
   gains a checklist line whenever adopters must act.
5. **Land it.** Commit to branch `cross-pollination/YYYY-MM-DD`, push, and
   open one PR titled `Weekly harvest: YYYY-MM-DD`. The PR body lists each
   accepted item with its source evidence, and each rejected candidate with
   the reason. Do not merge the PR.
6. **Close the loop.** Comment on the harvest issue with the disposition of
   every item: accepted (link the PR) or rejected (the reason). Then close
   the issue. Exception: leave it open, saying why, if any item genuinely
   needs the maintainer's judgment; an open `Harvest:` issue means "human
   attention needed".

If nothing is accepted, open no PR; step 6 still applies.

Report back: accepted items, rejected items with reasons, the PR URL or
"no PR", and the harvest issue's final state (closed, or left open and why).
