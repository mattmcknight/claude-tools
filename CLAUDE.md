# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This repository contains Claude Code skills - extensions that add capabilities to Claude Code for specific workflows. Skills are invoked via slash commands (e.g., `/bitbucket-pr`).

## Repository Structure

```
skills/
  <skill-name>/
    SKILL.md      # Skill definition and instructions (YAML frontmatter + markdown)
    bin/          # Executable scripts the skill uses
```

## Current Skills

### bitbucket-pr
Bitbucket Data Center PR management. Provides commands for listing, creating, commenting on, and merging PRs.

**Scripts** (in `skills/bitbucket-pr/bin/`):
- `bb-pr-comments [PR_ID]` - Get unresolved comments (auto-detects PR from branch if omitted)
- `bb-pr-reply PR_ID COMMENT_ID "text"` - Reply to a comment
- `bb-pr-resolve PR_ID COMMENT_ID` - Mark comment as resolved
- `bb-pr-info [PR_ID]` - Get PR details
- `bb-pr-list [--mine]` - List open PRs
- `bb-pr-create "title" ["desc"] [--to BRANCH]` - Create PR from current branch
- `bb-pr-merge PR_ID` - Merge a PR

**Required env vars**: `BITBUCKET_TOKEN`, `BITBUCKET_URL`

## Adding New Skills

1. Create `skills/<skill-name>/SKILL.md` with YAML frontmatter (`name`, `description`) and instructions
2. Add executable scripts to `skills/<skill-name>/bin/`
3. Scripts should handle auth via env vars and auto-detect repo context from git remote

## Shell Script Conventions

All scripts in this repo follow these patterns:
- Use `set -euo pipefail` for strict error handling
- Validate required env vars with `: "${VAR:?message}"`
- Auto-detect project/repo from git remote (supports both SSH and HTTPS URLs)
- Use `jq` for JSON construction (safe escaping) and parsing
- Output JSON to stdout, status messages to stderr
