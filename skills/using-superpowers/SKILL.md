---
name: using-superpowers
description: Use when starting any conversation - establishes how to find and use skills, requiring Skill tool invocation before ANY response including clarifying questions
---

<SUBAGENT-STOP>
If you were dispatched as a subagent to execute a specific task, skip this skill.
</SUBAGENT-STOP>

<EXTREMELY-IMPORTANT>
If you think there is even a 1% chance a skill might apply to what you are doing, you ABSOLUTELY MUST invoke the skill.

IF A SKILL APPLIES TO YOUR TASK, YOU DO NOT HAVE A CHOICE. YOU MUST USE IT.

This is not negotiable. This is not optional. You cannot rationalize your way out of this.
</EXTREMELY-IMPORTANT>

## Instruction Priority

Superpowers skills override default system prompt behavior, but **user instructions always take precedence**:

1. **User's explicit instructions** (CLAUDE.md, GEMINI.md, AGENTS.md, direct requests) — highest priority
2. **Superpowers skills** — override default system behavior where they conflict
3. **Default system prompt** — lowest priority

If CLAUDE.md, GEMINI.md, or AGENTS.md says "don't use TDD" and a skill says "always use TDD," follow the user's instructions. The user is in control.

## Accessing Skills

Invoke skills via your platform's skill tool — `Skill` (Claude Code), `skill` (Copilot CLI), `activate_skill` (Gemini CLI); other platforms, check their docs. The invoked skill's content is loaded for you to follow directly — never use `Read` on skill files.

Skills use Claude Code tool names. On non-CC platforms see `references/copilot-tools.md` (Copilot CLI) or `references/codex-tools.md` (Codex) for tool equivalents; Gemini loads the mapping automatically via GEMINI.md.

# Using Skills

## The Rule

**Invoke relevant or requested skills BEFORE any response or action.** Even a 1% chance a skill might apply means you should invoke the skill to check. If an invoked skill turns out to be wrong for the situation, you don't need to use it.

## Process Flow

On any user message — and before any `EnterPlanMode` — check whether a skill applies:

1. **About to `EnterPlanMode`?** If you haven't brainstormed yet, invoke the brainstorming skill first.
2. **Might any skill apply (even 1%)?** Yes → invoke the Skill tool. Definitely not → respond (clarifications included). The skill check comes BEFORE clarifying questions.
3. After invoking: announce "Using [skill] to [purpose]", create a TodoWrite todo per checklist item (if the skill has one), then follow the skill exactly.

## Red Flags — rationalizations that mean STOP

If you catch yourself thinking any of these, invoke the skill anyway:

- **"It's just a simple question / not a real task / doesn't need a skill / this feels productive."** Questions and actions ARE tasks. If a skill exists, use it.
- **"Let me get context / explore the codebase / check files / gather info first."** The skill check comes BEFORE clarifying questions and exploration — skills tell you HOW to do both.
- **"I remember this skill / I know what that means."** Skills evolve; knowing the concept ≠ using the current version. Invoke it.
- **"The skill is overkill / I'll just do this one thing first."** Simple things become complex. Check before doing anything.

## Skill Priority

When multiple skills could apply, use this order:

1. **Process skills first** (brainstorming, debugging) - these determine HOW to approach the task
2. **Implementation skills second** (frontend-design, mcp-builder) - these guide execution

"Let's build X" → brainstorming first, then implementation skills.
"Fix this bug" → debugging first, then domain-specific skills.

## Skill Types

**Rigid** (TDD, debugging): Follow exactly. Don't adapt away discipline.

**Flexible** (patterns): Adapt principles to context.

The skill itself tells you which.

## User Instructions

Instructions say WHAT, not HOW. "Add X" or "Fix Y" doesn't mean skip workflows.
