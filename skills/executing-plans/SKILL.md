---
name: executing-plans
description: Use when you have a written implementation plan to execute in a separate session with review checkpoints
---

# Executing Plans

## Mode Detection

**At the start of this skill, run:** `printenv SUPERPOWERS_AUTONOMOUS`

- If the output is `true`: You are in **autonomous mode**. Announce: "Running executing-plans in autonomous mode." Follow the `[AUTONOMOUS]` annotations throughout this skill.
- Otherwise: You are in **interactive mode** (default). Ignore all `[AUTONOMOUS]` annotations and follow the skill exactly as written.

**In autonomous mode, log every decision** you make that would normally require human input. Write your reasoning inline so the human can review it later.

## Overview

Load plan, review critically, execute all tasks, report when complete.

**Announce at start:** "I'm using the executing-plans skill to implement this plan."

**Note:** Tell your human partner that Superpowers works much better with access to subagents. The quality of its work will be significantly higher if run on a platform with subagent support (such as Claude Code or Codex). If subagents are available, use superpowers:subagent-driven-development instead of this skill.

## The Process

### Step 1: Load and Review Plan
1. Read plan file
2. Review critically - identify any questions or concerns about the plan
3. If concerns: Raise them with your human partner before starting
   - [AUTONOMOUS] Do not raise concerns with the human. Instead, log your concerns in the conversation (e.g., "Autonomous note: concern about X — proceeding because Y"). Self-resolve if possible. Only stop for truly unresolvable blockers (missing credentials, fundamentally ambiguous requirements that cannot be inferred from any available context).
4. If no concerns: Create TodoWrite and proceed

### Step 2: Execute Tasks

For each task:
1. Mark as in_progress
2. Follow each step exactly (plan has bite-sized steps)
3. Run verifications as specified
4. Mark as completed

### Step 3: Complete Development

After all tasks complete and verified:
- Announce: "I'm using the finishing-a-development-branch skill to complete this work."
- **REQUIRED SUB-SKILL:** Use superpowers:finishing-a-development-branch
- Follow that skill to verify tests, present options, execute choice

## When to Stop and Ask for Help

**STOP executing immediately when:**
- Hit a blocker (missing dependency, test fails, instruction unclear)
- Plan has critical gaps preventing starting
- You don't understand an instruction
- Verification fails repeatedly

**Ask for clarification rather than guessing.**

[AUTONOMOUS] Before stopping, make a genuine attempt to self-resolve:
- Missing dependency: check if it can be installed or if an alternative exists
- Test fails: debug the failure, check if the plan has a bug vs. the implementation
- Instruction unclear: infer from surrounding context and acceptance criteria
- Verification fails: try alternative verification approaches
Only stop for: missing credentials/secrets, requirements that are genuinely ambiguous with no reasonable default, external service access that cannot be obtained. Log what you tried before stopping.

## When to Revisit Earlier Steps

**Return to Review (Step 1) when:**
- Partner updates the plan based on your feedback
- Fundamental approach needs rethinking

**Don't force through blockers** - stop and ask.

## Remember
- Review plan critically first
- Follow plan steps exactly
- Don't skip verifications
- Reference skills when plan says to
- Stop when blocked, don't guess
- Never start implementation on main/master branch without explicit user consent

## Integration

**Required workflow skills:**
- **superpowers:using-git-worktrees** - REQUIRED: Set up isolated workspace before starting
- **superpowers:writing-plans** - Creates the plan this skill executes
- **superpowers:finishing-a-development-branch** - Complete development after all tasks
