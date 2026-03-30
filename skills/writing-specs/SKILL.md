---
name: writing-specs
description: Use after brainstorming or other design work to write a validated feature spec or design document before implementation planning.
metadata:
  short-description: Write validated specs before planning
---

# Writing Specs

## Overview

Write the validated design as a complete spec, review it for planning readiness, and get user approval before moving on to implementation planning.

**Announce at start:** "I'm using the writing-specs skill to write the spec."

## Tool Compatibility

- Keep instructions tool-agnostic and avoid provider-specific wording.
- When behavior differs across tools, resolve conflicts in this order: OpenCode > Claude Code > Codex CLI > Gemini CLI.

## Save Specs To

- Prefer `.docs/specs/YYYY-MM-DD--design.md`.
- Before choosing the path, inspect the project for an existing structure and respect it if one is already in use, for example `docs/superpowers/specs/`, `.docs/specs/`, or another documented path.
- User preferences for spec location override this default.

## What the Spec Must Do

- Capture the validated design, not a fresh brainstorming pass.
- Cover the design clearly enough that `writing-plans` can turn it into an implementation plan without guessing.
- Cover architecture, components, data flow, error handling, and testing.
- If the project is too large for a single implementation plan, split it into sub-project specs instead of forcing everything into one document.

## The Process

1. Write the validated design to the spec file.
2. Commit the design document to git if the workflow calls for committing planning artifacts.
3. Run the spec self-review loop.
4. Ask the user to review the written spec before proceeding.
5. Only after approval, invoke `writing-plans`.

## Spec Self-Review

After writing the spec document, look at it with fresh eyes:

1. **Placeholder scan:** Any "TBD", "TODO", incomplete sections, or vague requirements? Fix them.
2. **Internal consistency:** Do any sections contradict each other? Does the architecture match the feature descriptions?
3. **Scope check:** Is this focused enough for a single implementation plan, or does it need decomposition?
4. **Ambiguity check:** Could any requirement be interpreted two different ways? If so, pick one and make it explicit.

Fix any issues inline. No need to re-review, just fix and move on.

## User Review Gate

After the spec review loop passes, ask the user to review the written spec before proceeding:

> "Spec written and saved to `[SPEC_FILE_PATH]`. Please review it and let me know if you want to make any changes before we start writing out the implementation plan."

Wait for the user's response. If they request changes, make them and re-run the spec review loop. Only proceed once the user approves.

## Implementation Planning

- Invoke `writing-plans` to create a detailed implementation plan.
- Do NOT invoke any implementation skill before the written spec is approved.
