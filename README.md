# Claude Code Skills

Custom skills for [Claude Code](https://claude.ai/code) that extend its capabilities for specific workflows. Skills are invoked via slash commands.

## Installation

Clone this repository and add the skills directory to your Claude Code configuration.

## Available Skills

### bitbucket-pr

Manage Bitbucket Data Center pull requests directly from Claude Code.

**Commands:**
- `bb-pr-comments [PR_ID]` - Get unresolved comments (auto-detects PR from branch)
- `bb-pr-reply PR_ID COMMENT_ID "text"` - Reply to a comment
- `bb-pr-resolve PR_ID COMMENT_ID` - Mark comment as resolved
- `bb-pr-info [PR_ID]` - Get PR details
- `bb-pr-list [--mine]` - List open PRs
- `bb-pr-create "title" ["desc"] [--to BRANCH]` - Create PR from current branch
- `bb-pr-merge PR_ID` - Merge a PR

**Setup:**
```bash
export BITBUCKET_TOKEN='your-http-access-token'
export BITBUCKET_URL='https://bitbucket.example.com'
```

See `skills/bitbucket-pr/SKILL.md` for full documentation.

## Adding Skills

1. Create `skills/<skill-name>/SKILL.md` with YAML frontmatter and instructions
2. Add executable scripts to `skills/<skill-name>/bin/`
3. Scripts should use env vars for auth and auto-detect repo context from git remote

## License

None
