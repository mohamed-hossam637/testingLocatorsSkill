# Current-task scope

Start with the request and this session's changed elements. Inventory file/element, task ownership evidence, existing selectors, proposed add/reuse/omit decision. Surrounding unchanged markup is context, not edit scope.

Read-only discovery:

```sh
git status --short
git branch --show-current
git diff --name-status
git diff --cached --name-status
git ls-files --others --exclude-standard
git log --oneline -12
```

Read actual unstaged `git diff -- <paths>` and staged `git diff --cached -- <paths>` hunks. Untracked files need explicit reading; normal diffs omit them. HEAD alone misses committed task work; an existing branch/dirty tree does not prove ownership.

Use the user-specified base first. Otherwise inspect local refs, log/task commits and upstream (`git rev-parse --abbrev-ref --symbolic-full-name '@{upstream}'`). Upstream may be the same feature branch, not integration base. Do not blindly assume main, fetch, switch branches or alter Git config. With a justified base:

```sh
git merge-base HEAD <base>
git diff <merge-base> HEAD -- <paths>
git log --oneline <merge-base>..HEAD
git show <task-commit> -- <paths>
```

Replace placeholders and quote PowerShell upstream syntax. Combine committed, staged, unstaged and new-file evidence without assuming the union belongs to this task. For multiple/non-contiguous commits inspect individually, then current markup to avoid reintroducing obsolete changes.

- Known session changes: proceed within them even if base is unknown; disclose evidence and base uncertainty.
- Mixed ownership inside a file: edit only attributable elements; preserve unrelated staged/unstaged changes. Never stage to isolate them.
- Ambiguous scope: audit known portions and leave uncertain elements unchanged. Ask one focused question only when needed for dependent edits.
- Shared component: inspect callers for compatibility; do not treat inspected callers as migration scope.
- Audit-only/dry-run: propose changes without UI edits.

Report exact base/ref/commit and reason, or session-derived scope with base NOT VERIFIED. Branch-wide file discovery is never blanket authorization.
