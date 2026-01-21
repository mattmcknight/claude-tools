---
name: bitbucket-pr
description: Check and address PR review comments from Bitbucket Data Center
---

# Bitbucket PR Comments

**IMPORTANT: Always use the scripts in `bin/` for Bitbucket operations. Never construct curl commands directly - the scripts handle authentication, escaping, and error handling correctly.**

Scripts are in the skill's `bin/` directory. Call them by full path or add to PATH.

## Setup

Required environment variables:
```bash
export BITBUCKET_TOKEN='your-http-access-token'
export BITBUCKET_URL='https://bitbucket.example.com'  # no trailing slash
export BITBUCKET_USER='your-email'  # optional, defaults to git config user.email
```

Add to `~/.claude/settings.json`:
```json
{
  "env": {
    "BITBUCKET_TOKEN": "your-token",
    "BITBUCKET_URL": "https://bitbucket.example.com"
  }
}
```

## Scripts

### bb-pr-comments
Get unresolved comments for a PR. Outputs JSON array.

```bash
# For specific PR
bb-pr-comments 42

# Auto-detect PR from current branch
bb-pr-comments
```

Output format:
```json
[
  {
    "id": 12345,
    "author": "Jane Doe",
    "text": "Should this be nullable?",
    "path": "src/main/java/Foo.java",
    "line": 42,
    "state": "OPEN",
    "severity": "NORMAL"
  }
]
```

### bb-pr-reply
Reply to a comment after addressing it.

```bash
bb-pr-reply 42 12345 "Fixed in latest commit"
```

### bb-pr-resolve
Mark a comment as resolved.

```bash
bb-pr-resolve 42 12345
```

### bb-pr-merge
Merge a PR (fetches version automatically).

```bash
bb-pr-merge 42
```

### bb-pr-info
Get PR details (state, reviewers, branches).

```bash
bb-pr-info 42
bb-pr-info  # auto-detect from current branch
```

### bb-pr-list
List open PRs in the repo.

```bash
bb-pr-list        # all open PRs
bb-pr-list --mine # only your PRs
```

### bb-pr-create
Create a new PR from current branch.

```bash
bb-pr-create "Fix login bug"
bb-pr-create "Add feature" "Detailed description here"
bb-pr-create "Hotfix" --to develop
```

## Workflow

When asked to address PR feedback:

1. Run `bb-pr-comments PR_ID` to get unresolved comments
2. For each comment with a `path` and `line`, read that file location
3. Make the requested change
4. Commit and push: `git add -A && git commit -m "Address PR feedback: ..." && git push`
5. Reply or resolve: `bb-pr-reply PR_ID COMMENT_ID "Done"` or `bb-pr-resolve PR_ID COMMENT_ID`

## Reducing Approvals

To avoid approving each script call, add to `~/.claude/settings.json`:

```json
{
  "permissions": {
    "allow": [
      "Bash(bb-pr-*)",
      "Bash(git add*)",
      "Bash(git commit*)",
      "Bash(git push*)"
    ]
  }
}
```

Or for trusted repos, use `claude --dangerously-skip-permissions`.

## Rules

1. **Never use curl directly for Bitbucket API calls** - always use bb-pr-* scripts
2. Scripts handle authentication, JSON escaping, and version management
3. If an operation isn't covered by a script, ask the user to extend the skill
