# Skill Authoring Guidelines

Use these rules for every skill in this directory.

## Core Rules

1. Keep skill content model-agnostic and provider-agnostic.
2. Do not mention specific models or company names in skill instructions.
3. Preserve each skill's purpose and keep edits minimal, clear, and coherent.

## Tool Compatibility Priority

When tool behavior differs or instructions conflict, prefer this order:

1. OpenCode
2. Claude Code
3. Codex CLI
4. Gemini CLI

Write instructions so they remain usable across all of these tools.

## Writing Guidance

- Describe capabilities in generic terms instead of tool- or model-specific wording.
- Prefer neutral labels such as "agent", "worker", or "execution profile".
- Keep operational instructions explicit: scope, constraints, expected outputs, and verification steps.
- Avoid adding unrelated refactors when updating existing skills.

## Update Checklist

For any skill update:

1. Update `SKILL.md`.
2. Check related files under the same skill directory (for example `references/` and `agents/`).
3. Remove model/company-specific wording if present.
4. Confirm compatibility instructions follow the priority order above.
