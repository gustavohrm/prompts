---
name: writing-plans
description: Use when you have a spec or requirements for a multi-step task, before touching code.
metadata:
  short-description: Build complete implementation plans
---

# Writing Plans

## Overview

Write comprehensive implementation plans assuming the engineer has zero context for the codebase and questionable taste. Document everything they need to know: which files to touch for each task, code, testing, docs they might need to check, and how to test it. Give them the whole plan as bite-sized tasks. Prefer plans that assume delegated execution via subagents when practical. DRY. YAGNI. TDD. Frequent, atomic commits.

Assume they are a skilled developer, but know almost nothing about the toolset or problem domain. Assume they do not know good test design very well.

**Announce at start:** "I'm using the writing-plans skill to create the implementation plan."

**Context:** Default to implementation on a new branch or isolated worktree, with atomic commits and a PR at the end. If repo conventions or explicit user instructions call for another approach, follow that instead.

## Tool Compatibility

- Keep instructions tool-agnostic and avoid provider-specific wording.
- When behavior differs across tools, resolve conflicts in this order: OpenCode > Claude Code > Codex CLI > Gemini CLI.

## Save Plans To

- Prefer `.docs/plans/YYYY-MM-DD-.md`.
- Before choosing the path, inspect the project for an existing structure and respect it if one is already in use, for example `docs/superpowers/plans/`, `.docs/plans/`, or another documented path.
- User preferences for plan location override this default.

## Status

- Every plan must include `**Status:** Draft` near the top.
- Valid statuses for spec and plan docs are `Draft`, `Approved`, `In Progress`, and `Implemented`.

## Approval Gate

- Only write a plan from a spec documented as `Approved`, unless the user explicitly asks you to proceed without documented approval.
- Never mark a spec or plan `Approved` on your own. The user must update it, or you may update it only if they explicitly ask you to.

## Scope Check

If the spec covers multiple independent subsystems, it should have been broken into sub-project specs during brainstorming. If it was not, suggest breaking this into separate plans, one per subsystem. Each plan should produce working, testable software on its own.

## File Structure

Before defining tasks, map out which files will be created or modified and what each one is responsible for. This is where decomposition decisions get locked in.

- Design units with clear boundaries and well-defined interfaces. Each file should have one clear responsibility.
- You reason best about code you can hold in context at once, and your edits are more reliable when files are focused. Prefer smaller, focused files over large ones that do too much.
- Files that change together should live together. Split by responsibility, not by technical layer.
- In existing codebases, follow established patterns. If the codebase uses large files, do not unilaterally restructure, but if a file you are modifying has grown unwieldy, including a split in the plan is reasonable.

This structure informs the task decomposition. Each task should produce self-contained changes that make sense independently.

## Bite-Sized Task Granularity

**Each step is one action, 2-5 minutes:**

- "Write the failing test" - step
- "Run it to make sure it fails" - step
- "Implement the minimal code to make the test pass" - step
- "Run the tests and make sure they pass" - step
- "Commit" - step

## Plan Document Header

**Every plan MUST start with this header:**

````markdown
# [Feature Name] Implementation Plan

**Status:** Draft

> **For agentic workers:** REQUIRED SUB-SKILL: Use `executing-plans` to implement this plan task-by-task.
> Default implementation mode is delegated execution via subagents when available. Default git flow is a new branch, atomic commits, and a PR at the end unless repo conventions or explicit user instructions say otherwise.

**Goal:** [One sentence describing what this builds]
**Architecture:** [2-3 sentences about approach]
**Tech Stack:** [Key technologies/libraries]

---
````

Steps use checkbox (`- [ ]`) syntax for tracking.

## Task Structure

````markdown
### Task N: [Component Name]

**Files:**
- Create: `exact/path/to/file.py`
- Modify: `exact/path/to/existing.py:123-145`
- Test: `tests/exact/path/to/test.py`

- [ ] **Step 1: Write the failing test**

```python
def test_specific_behavior():
    result = function(input)
    assert result == expected
```

- [ ] **Step 2: Run test to verify it fails**

Run: `pytest tests/path/test.py::test_name -v`
Expected: FAIL with "function not defined"

- [ ] **Step 3: Write minimal implementation**

```python
def function(input):
    return expected
```

- [ ] **Step 4: Run test to verify it passes**

Run: `pytest tests/path/test.py::test_name -v`
Expected: PASS

- [ ] **Step 5: Commit**

```bash
git add tests/path/test.py src/path/file.py
git commit -m "feat: add specific feature"
```
````

## No Placeholders

Every step must contain the actual content an engineer needs.

These are **plan failures** and must never appear:

- "TBD", "TODO", "implement later", "fill in details"
- "Add appropriate error handling" / "add validation" / "handle edge cases"
- "Write tests for the above" without actual test code
- "Similar to Task N" - repeat the code, the engineer may be reading tasks out of order
- Steps that describe what to do without showing how, code blocks required for code steps
- References to types, functions, or methods not defined in any task

## Remember

- Exact file paths always
- Complete code in every step, if a step changes code, show the code
- Exact commands with expected output
- DRY, YAGNI, TDD, frequent atomic commits

## Self-Review

After writing the complete plan, look at the spec with fresh eyes and check the plan against it.

This is a checklist you run yourself, not a subagent dispatch.

1. **Spec coverage:** Skim each section and requirement in the spec. Can you point to a task that implements it? List any gaps.
2. **Placeholder scan:** Search your plan for red flags, any of the patterns from the "No Placeholders" section above. Fix them.
3. **Type consistency:** Do the types, method signatures, and property names you used in later tasks match what you defined in earlier tasks? A function called `clearLayers()` in Task 3 but `clearFullLayers()` in Task 7 is a bug.

If you find issues, fix them inline. No need to re-review, just fix and move on. If you find a spec requirement with no task, add the task.

## User Review Gate

After saving the plan and completing self-review, ask the user to review it before execution.

If they approve in chat but the document is still `Draft`, ask whether they want to update it themselves or want you to update it.

Only hand off to `executing-plans` once the plan document says `Approved`, unless the user explicitly instructs you to execute without documented approval.

## Execution Handoff

After the plan is documented as `Approved`, offer execution choice:

**"Plan complete and saved to `[PLAN_FILE_PATH]` with status `Approved`.

Two execution options:

1. Delegated execution
   - Default when subagents are available and the tasks can be split cleanly
   - Use `executing-plans` with fresh workers per task or independent workstream
2. Inline execution
   - Use when the work is tightly coupled, subagents are unavailable, or you explicitly prefer inline
   - Follow the same plan and verification steps in this session

Default implementation workflow is a new branch, atomic commits, and a PR at the end unless repo conventions or your instructions say otherwise.

Which approach?"**
