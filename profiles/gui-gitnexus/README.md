# GUI local review graph

This Codex-only overlay keeps GitNexus and adds
[code-review-graph](https://github.com/tirth8205/code-review-graph) **2.3.8**
through `uvx`. It is already part of the GUI's
`nextjs+browser+frontend+tanstack+gui-gitnexus` selector.

Only six structural graph tools are exposed. Graph construction/querying is
local; the selected coding agent performs the review using its normal model.

## Automatic PR review

The user enabled automatic review for PRs successfully created or updated with
new commits **during an active Cue session using this overlay**. One independent
read-only reviewer uses the local graph and published diff before the existing
merge/cleanup workflow continues. Completed reviews are deduplicated by repo,
PR number, base SHA and head SHA in the lane's existing handoff/artifact.

Local edits, failed pushes, closed/unrelated PRs and metadata-only updates do
not trigger reviews. Failed/incomplete reviews are pending, not clean. A later
user skip/disable request overrides the standing approval.

This is agent workflow guidance, not an always-on webhook: PRs created elsewhere
while Cue is stopped are not watched. No global Stop-hook flag, upstream
installer, watcher, GitHub workflow, embeddings, automatic fixes, review-comment
posting, approval or merge is enabled by this setting.

## Local setup and checks

With `uvx` available, run from the repository you want to review:

```sh
uvx --from code-review-graph==2.3.8 code-review-graph build --repo "$PWD" --skip-flows
uvx --from code-review-graph==2.3.8 code-review-graph status --repo "$PWD"
cue launch codex --dry-run
```

Keep `.code-review-graph/` local (use the repository's local Git exclude file,
or its established ignore policy). Each worktree has its own graph. Always pass
the actual review worktree as `repo_root`; do not query the main checkout when
reviewing an isolated lane. Refresh the graph before reviewing, and choose the
review diff base explicitly instead of inheriting upstream's `HEAD~1` default.

A new Cue-launched Codex session loads the server and review routing. Verify
MCP initialization/tool listing and a bounded graph query; profile validation
alone does not prove the server works or that an agent has completed a PR review.

The upstream token-reduction illustration is not a measured result for GUI.
