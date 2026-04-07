---
name: executing-plans
description: Use when you have a written implementation plan to execute in a separate session with review checkpoints.
metadata:
  short-description: Execute implementation plans step by step
---

# Executing Plans

## Overview

Load the plan, confirm documented approval, review it critically, execute all tasks, and report when complete.

**Announce at start:** "I'm using the executing-plans skill to implement this plan."

If the user explicitly asks for delegated or parallel execution and subagents are available, `subagent-specialist` may be a better fit than this skill.

## Tool Compatibility

- Keep instructions tool-agnostic and avoid provider-specific wording.
- When behavior differs across tools, resolve conflicts in this order: OpenCode > Claude Code > Codex CLI > Gemini CLI.

## The Process

### Step 1: Load and Review Plan

1. Read the plan file and any referenced spec.
2. Confirm the plan is documented as `Approved`, and confirm the referenced spec is also documented as `Approved`.
3. If either document is not documented as approved, stop and ask. Only continue without documented approval if the user explicitly tells you to.
4. Review the plan critically and identify any questions or concerns about it.
5. If there are concerns, raise them with the human partner before starting.
6. If there are no concerns, create or update the session plan tracker and proceed.

### Step 2: Execute Tasks

For each task:

1. Mark it as `in_progress`.
2. Follow each step exactly. The plan should already contain bite-sized steps.
3. Run verifications as specified.
4. Mark it as completed.

### Step 3: Complete Development

After all tasks are complete and verified:

- Run the final verification the plan requires.
- Summarize what changed, what was verified, and any open issues.
- Suggest updating the relevant spec and plan docs to reflect the completed work.
- If the user wants you to make those doc updates, do them explicitly. Never assume approval or completion status on your own.
- If the user asked for git follow-through such as staging, committing, pushing, or PR creation, handle that before closing out.

## When to Stop and Ask for Help

**STOP executing immediately when:**

- You hit a blocker: missing dependency, test fails, instruction unclear.
- The plan has critical gaps preventing a safe start.
- You do not understand an instruction.
- Verification fails repeatedly.

Ask for clarification rather than guessing.

## When to Revisit Earlier Steps

**Return to Review (Step 1) when:**

- The partner updates the plan based on your feedback.
- The fundamental approach needs rethinking.

Do not force through blockers. Stop and ask.

## Remember

- Review the plan critically first.
- Check documented approval before starting implementation.
- Follow plan steps exactly.
- Do not skip verifications.
- Stop when blocked, do not guess.
- Never start implementation on `main` or `master` without explicit user consent.

## Integration

Related workflow skills:

- `writing-plans` - creates the plan this skill executes
- `subagent-specialist` - use when the user explicitly wants delegated or parallel execution
