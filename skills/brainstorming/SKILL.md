---
name: brainstorming
description: "Use when the work needs collaborative design before implementation, especially for new features, behavior changes, or unclear requirements. Explores user intent, requirements, and design before implementation."
metadata:
  short-description: Brainstorm ideas into approved designs
---

# Brainstorming Ideas Into Designs

Help turn ideas into fully formed designs through natural collaborative dialogue. Start by understanding the current project context, then ask questions one at a time to refine the idea.

Once you understand what you're building, present the design and get user approval before any implementation action. Do NOT invoke any implementation skill, write any code, scaffold any project, or take any implementation action until you have presented a design and the user has approved it. Design approval does not approve the spec, the plan, or implementation. After approval, invoke `writing-specs` when the approved design should be documented or used to continue into planning or implementation. If the user explicitly wants brainstorming only, stop after the approved design.

## Anti-Pattern: "This Is Too Simple To Need A Design"

Do not skip design just because something looks simple if there is any meaningful choice about behavior, interface, structure, or constraints.

For trivial, low-risk work with one obvious shape, keep the design lightweight: confirm the goal, state the intended approach briefly, and get acknowledgement before moving on. If you are unsure whether the work is trivial, treat it as non-trivial.

## Checklist

You MUST cover these stages in order. Track them explicitly when the environment supports task tracking:

1. **Explore project context** - check files, docs, recent commits
2. **Ask clarifying questions** - one at a time, understand purpose, constraints, success criteria
3. **Propose options** - compare 2-3 approaches when there are meaningful choices; otherwise state the single obvious approach and why it is sufficient
4. **Present design** - in sections scaled to their complexity, get user approval after each section
5. **Transition or stop** - invoke `writing-specs` when the approved design should be documented or carried forward; if the user explicitly wants brainstorming only, stop after the approved design

## Tool Compatibility

- Keep instructions tool-agnostic and avoid provider-specific wording.
- When behavior differs across tools, resolve conflicts in this order: OpenCode > Claude Code > Codex CLI > Gemini CLI.

## Process Flow

```text
Explore project context
  -> Ask clarifying questions
  -> Propose options when there are real choices
  -> Present design sections
  -> User approves design?
     -> no: revise and continue
     -> yes + wants documentation, planning, implementation, or leaves the next step unspecified: invoke writing-specs
     -> yes + explicitly wants brainstorming only: stop after the approved design
```

Do NOT invoke `frontend-design` or any other implementation skill from brainstorming.

Invoke `writing-specs` only when the approved design should be documented or used to continue into planning or implementation. If the user explicitly wants to stop at brainstorming or defer documentation, do not invoke it. When the user's intent is unclear after design approval, prefer invoking `writing-specs`.

## The Process

**Understanding the idea:**

- Check out the current project state first: files, docs, recent commits.
- Decide whether the request needs the full brainstorming flow or a lightweight design confirmation. Use the full flow for new functionality, behavior changes, unclear requirements, multiple reasonable approaches, or work that spans multiple components. For trivial, low-risk, self-evident changes with one obvious path, keep the design brief but still confirm it before moving on. If unsure, use the full flow.
- Before asking detailed questions, assess scope. If the request describes multiple independent subsystems, for example "build a platform with chat, file storage, billing, and analytics", flag this immediately. Do not spend questions refining details of a project that needs to be decomposed first.
- If the project is too large for a single spec, help the user decompose it into sub-projects: what are the independent pieces, how do they relate, and what order should they be built in? Then brainstorm the first sub-project through the normal design flow. Each sub-project gets its own spec, plan, and implementation cycle.
- For appropriately scoped projects, ask questions one at a time to refine the idea.
- Prefer multiple choice questions when possible, but open-ended is fine too.
- Only one question per message. If a topic needs more exploration, break it into multiple questions.
- Focus on understanding: purpose, constraints, success criteria.

**Exploring approaches:**

- When there are materially different reasonable options, propose 2-3 different approaches with trade-offs.
- For trivial or single-path work, state the chosen approach and why it is sufficient.
- Present options conversationally with your recommendation and reasoning.
- Lead with your recommended option and explain why.

**Presenting the design:**

- Once you believe you understand what you're building, present the design.
- Scale each section to its complexity: a few sentences if straightforward, up to 200-300 words if nuanced.
- Ask after each section whether it looks right so far.
- Cover: architecture, components, data flow, error handling, testing.
- Be ready to go back and clarify if something does not make sense.

**Design for isolation and clarity:**

- Break the system into smaller units that each have one clear purpose, communicate through well-defined interfaces, and can be understood and tested independently.
- For each unit, you should be able to answer: what does it do, how do you use it, and what does it depend on?
- Can someone understand what a unit does without reading its internals? Can you change the internals without breaking consumers? If not, the boundaries need work.
- Smaller, well-bounded units are also easier for you to work with. You reason better about code you can hold in context at once, and your edits are more reliable when files are focused. When a file grows large, that is often a signal that it is doing too much.

**Working in existing codebases:**

- Explore the current structure before proposing changes. Follow existing patterns.
- Where existing code has problems that affect the work, for example a file that has grown too large, unclear boundaries, or tangled responsibilities, include targeted improvements as part of the design, the way a good developer improves code they are working in.
- Do not propose unrelated refactoring. Stay focused on what serves the current goal.

## After the Design

**Documentation:**

- Invoke `writing-specs` to write the validated design to a spec document when the approved design should be documented or carried forward.
- If the user explicitly wants brainstorming only or to defer documentation, stop after the approved design instead of forcing a handoff.
- Set the expectation that approved work normally continues with subagent-first planning and implementation on a new branch, with atomic commits and a PR at the end, unless the repo or user calls for another path.
- Do NOT invoke `writing-plans` directly from brainstorming.

## Key Principles

- **One question at a time** - Do not overwhelm with multiple questions.
- **Multiple choice preferred** - Easier to answer than open-ended when possible.
- **YAGNI ruthlessly** - Remove unnecessary features from all designs.
- **Explore alternatives** - Propose 2-3 approaches when there are real choices; otherwise state the single obvious approach.
- **Incremental validation** - Present design, get approval before moving on.
- **Be flexible** - Go back and clarify when something does not make sense.
