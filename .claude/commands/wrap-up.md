Wrap up the day's work: commit all changes with a message derived from the journal, then push to GitHub.

---

## Procedure

### 1 — Check state

Run in parallel:
- `git status` — see what's changed
- `git log --oneline -5` — see recent commit style
- Read `.agent/journal.md` — extract context for the commit message

If the working tree is clean (nothing to commit), report that and stop.

### 2 — Derive the commit message

From the journal, find the most recent date section. Extract:
- The date
- All `[x]` completed items
- Key outputs or findings recorded beneath each item (the `###` subsections)

Write a commit message:
- **Subject** (≤72 chars): `<date>: <one-line summary of today's main output>`
- **Body**: 2–4 bullet lines summarizing what was produced — files written, decisions made, open questions flagged. Draw from the journal's `###` subsections, not from memory.
- **Trailer**: `Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>`

### 3 — Stage and commit

Stage all modified and new tracked files. Do not stage `.env`, credentials, or any file whose name suggests secrets.

Commit using a HEREDOC to preserve formatting:
```bash
git commit -m "$(cat <<'EOF'
<subject>

<body bullets>

Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>
EOF
)"
```

### 4 — Push

Push to the remote:
```bash
git push
```

If no upstream is set, run `git push -u origin <current-branch>`.

### 5 — Report

Output: commit hash, branch, and one-sentence description of what was pushed.
