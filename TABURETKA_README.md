# Taburetka Fork of Superpowers

Fork of [obra/superpowers](https://github.com/obra/superpowers) with autonomous mode support.

## What changed

Added `SUPERPOWERS_AUTONOMOUS=true` environment variable support across 5 skills. When set, the agent runs the full brainstorming -> planning -> implementation -> PR chain without human approval gates.

### Modified skills

| Skill | Autonomous behavior |
|-------|-------------------|
| brainstorming | Self-answers clarifying questions from Jira context, self-selects simplest approach, self-validates design against acceptance criteria, skips visual companion and spec review gate |
| writing-plans | Auto-selects subagent-driven execution |
| executing-plans | Self-resolves plan concerns and blockers before escalating |
| subagent-driven-development | Self-finds context for NEEDS_CONTEXT, retries alternative approaches for BLOCKED |
| finishing-a-development-branch | Auto-creates draft PR with no reviewers (removes auto-assigned reviewers), preserves worktree, doesn't transition Jira |

### Unchanged skills

- verification-before-completion (backpressure, not a human gate)
- requesting-code-review (already automated via subagent)
- All subagent prompt templates

## How it works

Each modified skill checks `printenv SUPERPOWERS_AUTONOMOUS` at entry. If `true`, it follows `[AUTONOMOUS]` annotations that bypass human gates. Without the env var, everything works exactly as upstream.

In autonomous mode, every decision that would normally require human input is logged with reasoning in the conversation, so the human can review the agent's choices in the draft PR or design doc.

## Usage

Set the env var before launching Claude:

```bash
SUPERPOWERS_AUTONOMOUS=true claude "/ticket RTM-1234"
```

## Releasing a new version

After making changes, bump the version in all three files and create a git tag:

```bash
# Update version in:
#   package.json
#   .claude-plugin/plugin.json
#   .claude-plugin/marketplace.json

git add -A && git commit -m "Bump version to X.Y.Z"
git tag vX.Y.Z
git push origin main --tags
```

Users then run:

```bash
claude plugin marketplace update taburetka-marketplace
claude plugin update superpowers@taburetka-marketplace
```

## Syncing with upstream

```bash
git fetch upstream main
git merge upstream/main
```

Resolve any conflicts in the `[AUTONOMOUS]` annotation blocks.
