# Rules

## Context & guidelines

- Always scan the project's structure to have a better understanding about the whole content
- You don't have to know the content of all files, but you should at the very list know all the existing folders, and from there scan relevant files
- Look for documentation & guidelines files, such as CONTRIBUTING.md, AGENTS.md and similar

## Naming Conventions

- Files & Folders: kebab-case (`user-service.ts`, `api-routes/`)
- Variables & Functions: camelCase (`getUserById`, `isValid`)
- Classes & Types: PascalCase (`UserService`, `ApiResponse`)
- Constants & Enums: UPPER_SNAKE_CASE (`MAX_RETRY_ATTEMPTS`)
- Booleans: prefix with is/has/can/should (`isLoading`, `hasPermission`)
- Arrays: plural nouns (`users`, `items`)
- Functions: verb phrases (`calculateTotal`, `fetchUser`)

## Architecture & Design

- Single Responsibility: each function/class does ONE thing well
- High Cohesion: logic that belongs together stays together
- Loose Coupling: minimize dependencies between modules
- Prefer dependency injection over hard-coding
- Use factory patterns when object creation is non-trivial
- Enable strict mode in TypeScript
- Avoid `any`; use `unknown` for truly unknown types
- Prefer interfaces for objects, type aliases for unions/intersections
- Prefer `const` over `let`; never use `var`
- Don't mutate function parameters

## Code Organization

- Keep files under 200-300 lines
- Functions should be small and clearly nameable
- Extract reusable logic into separate modules
- Avoid circular dependencies
- Never use magic numbers or strings; extract to named constants

## Good Practices

- Code should be self-explanatory; use descriptive names
- Design for predictable, deterministic behavior
- Pure functions wherever possible
- Keep logic close to the data it operates on

Function parameters:
- 0-1 parameters: excellent
- 2 parameters: acceptable  
- 3+ parameters: refactor into typed object

```javascript
// ❌ Bad
function createUser(name, email, age, role, department) { }

// ✅ Good
function createUser(userData: UserData) { }
```

Represent domain concepts with validated types:

```javascript
// ❌ Bad
const tax = order.getAmount() * 0.15;

// ✅ Good
const tax = order.calculateTax();
```

## Bad Practices to Avoid

- God Objects: classes/functions handling multiple unrelated responsibilities
- Excessive Conditionals: avoid long `if/else` chains; prefer polymorphism
- Feature Envy: don't pull data from other objects to perform calculations

```javascript
// ❌ Bad - feature envy
const total = cart.getItems().reduce((sum, item) => 
  sum + item.getPrice() * item.getQuantity(), 0);

// ✅ Good - tell, don't ask
const total = cart.calculateTotal();
```

- Method Chaining Abuse: avoid `object.getA().getB().doSomething()`
- Always use `===` and `!==` (not `==` and `!=`)
- Minimize global variables; make side effects explicit

## Asynchronous Code

- Prefer `async/await` over raw promises
- Always handle errors with try-catch
- Never use async functions without awaiting or returning the promise
- Avoid mixing callbacks and promises
- Use `Promise.all()` (fails fast) or `Promise.allSettled()` (all resolve) appropriately
- Don't create promises in loops:

```javascript
// ❌ Bad - sequential
for (const id of ids) {
  await fetchUser(id);
}

// ✅ Good - parallel
const users = await Promise.all(ids.map(id => fetchUser(id)));
```

## Error Handling

- Fail fast: validate inputs early and throw meaningful errors
- Never swallow errors silently
- Prefer specific error types over generic `Error`
- Always handle promise rejections
- Use try-catch for recoverable errors; let unrecoverable errors bubble up

```javascript
// ❌ Bad
try {
  await riskyOperation();
} catch (e) {} // silent failure

// ✅ Good
try {
  await riskyOperation();
} catch (error) {
  logger.error('Failed to process operation', { error, context });
  throw new OperationError('Operation failed', { cause: error });
}
```

## Security

- Never trust user input; always validate and sanitize
- Avoid `eval()` and `Function()` constructor
- Use parameterized queries; never concatenate SQL
- Don't expose sensitive data in errors or logs
- Use environment variables for secrets
- Validate file paths to prevent directory traversal

## Performance

- Don't prematurely optimize; prioritize readability first
- Profile before optimizing; measure, don't guess
- Watch for memory leaks (event listeners, closures, timers)
- Use appropriate data structures (`Map`/`Set` vs `Object`/`Array`)
- Consider lazy loading and code splitting for large apps

## Testing

- Write tests for business logic and complex algorithms
- Test edge cases and error paths, not just happy paths
- Each test verifies ONE behavior
- Test names describe behavior: `shouldReturnEmptyArrayWhenInputIsNull`
- Test behavior, not implementation details
- Mock external dependencies (APIs, databases, file system)

## Documentation & Comments

Comments explain WHY, not WHAT or HOW.

Good comments:
- Business logic rationale: "30-day window required by regulation X"
- Non-obvious algorithms: "Binary search for O(log n) performance"
- Workarounds: "Temporary fix for Chrome bug #12345"
- Complex regex: "Matches RFC 5322 email format"

Avoid:
- Redundant comments that repeat the code
- Commented-out code (use version control)
- Outdated comments
- Large and overly decorated comments

## Code Formatting

- Use consistent formatter (Prettier recommended)
- Commit formatter config
- Maximum line length: 80-120 characters
- One statement per line
- Let tooling handle formatting
