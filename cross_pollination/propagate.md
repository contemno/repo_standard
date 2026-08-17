# Propagate subagent

You are reviewing ONE repository (named at the end of this prompt, with the
shared context and rules) for gaps against `contemno/repo_standard`. Compare
against the LOCAL `repo_standard` checkout in this environment: it may include
changes committed earlier in this run whose PR (URL provided, if any) is not
merged yet.

Look for concrete gaps where adopting the standard would help: unpinned
actions, `paths-ignore` on required checks, missing aggregator gate, no
Dependabot grouping, no security-advisory contact link, and the like.

Deliverable: ONE issue on the reviewed repo titled
`Adopt repo_standard updates: YYYY-MM-DD`, listing each gap with file
references and a pointer to the relevant `STANDARD.md` section (or to the
pending PR, when the rationale is not merged yet).

Rules:

- Issues only. Never commit or push to the reviewed repo.
- Before filing, search the repo's existing issues (open AND closed); never
  re-report an item already filed. If findings overlap an existing open
  weekly issue, comment there instead of opening a new one.
- If there is nothing to suggest, file nothing.

Report back: the issue URL, "commented on existing issue <url>", or "no
findings".
