# GUI local review graph

This Codex-only overlay keeps GitNexus and adds
[code-review-graph](https://github.com/tirth8205/code-review-graph) **2.3.8**
through `uvx`. It is already part of the GUI's
`nextjs+browser+frontend+tanstack+gui-gitnexus` selector.

Only six structural graph tools are exposed. No upstream installer, automatic
review, watcher, GitHub workflow, embedding provider, or global agent config is
enabled. Graph construction/querying is local; the selected coding agent is
still responsible for the review and may use its normal remote model.

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
alone does not prove the server works. This setup does not run a review.

The upstream token-reduction illustration is not a measured result for GUI.
