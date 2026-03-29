# Rules & Guidelines

## Context & Discovery
- Scan project structure and folders before acting to understand the context.
- Read documentation files (`CONTRIBUTING.md`, `AGENTS.md`, etc.).

## Naming Conventions
- **Files/Folders**: `kebab-case`
- **Variables/Functions**: `camelCase` (Functions as verbs)
- **Classes/Types**: `PascalCase`
- **Constants/Enums**: `UPPER_SNAKE_CASE`
- **Booleans**: Prefix with `is/has/can/should`
- **Arrays**: Plural nouns

## TypeScript & JavaScript
- Enable strict mode; use `unknown` instead of `any`.
- Prefer interfaces for objects, type aliases for unions/intersections.
- Use `const` over `let`; never use `var`. Always use `===` and `!==`.
- Max 2 function parameters; use typed objects for 3+.
- Do not mutate function parameters. No magic strings or numbers.

## Architecture & Design
- **Principles**: Single Responsibility, High Cohesion, Loose Coupling.
- Prefer Dependency Injection and Factory patterns.
- **OOP**: Avoid God Objects, Feature Envy, and excessive method chaining.
- **Control Flow**: Avoid long `if/else` chains; prefer polymorphism.
- **Domain Logic**: "Tell, don't ask." Keep logic close to the data it operates on.
- **Organization**: Keep files under 200-300 lines. Extract reusable logic.

## Async & Performance
- Prefer `async/await` over raw promises. Never mix callbacks and promises.
- Use `Promise.all()` for parallel execution; avoid sequential awaits in loops.
- Prioritize readability over premature optimization.
- Use appropriate structures (`Map`/`Set` vs `Object`/`Array`). Watch for memory leaks.

## Error Handling & Security
- **Fail Fast**: Validate inputs early. Throw specific, meaningful errors.
- **Never** swallow errors silently. Use `try/catch` for recoverable errors.
- Validate and sanitize all user input. Prevent directory traversal.
- Use parameterized queries (no SQL concatenation). Avoid `eval()`.
- Keep secrets in environment variables; never expose them in logs/errors.

## Testing
- Test behavior, edge cases, and error paths (not just happy paths).
- One behavior per test. Use descriptive names (e.g., `shouldReturnEmptyWhenNull`).
- Mock external dependencies (APIs, DBs, File System).

## Documentation & Formatting
- **Comments**: Explain *WHY*, not *WHAT* or *HOW*.
- Remove redundant comments, commented-out code, and outdated docs.
- Rely on tooling (e.g., Prettier) for formatting. Max line length: 80-120 chars.