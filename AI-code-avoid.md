# SKILL.md — Write Idiomatic, Maintainer-Quality Code

## Purpose

Produce code that reads like it was designed and maintained by someone who understands the system, rather than code that merely satisfies the immediate prompt.

The goal is **good engineering**, not evading authorship or AI-detection systems. Do not manipulate code solely to fool detectors. Instead, avoid the recurring quality problems associated with rushed, generated, or “vibe-coded” software: unnecessary abstraction, duplication, shallow tests, generic error handling, arbitrary defaults, unexplained dependencies, inconsistent conventions, and weak understanding of invariants.

## Core rule

Before writing code, understand:

1. What the code is responsible for.
2. What assumptions and invariants are guaranteed.
3. Which existing abstractions, utilities, types, and conventions should be reused.
4. What failure modes matter.
5. What the smallest correct change is.

Prefer a small, coherent solution over a broad, generic one.

---

## 1. Match the existing codebase

Treat the repository as the primary source of truth.

Before editing:
- Inspect nearby files and call sites.
- Follow existing naming, formatting, error-handling, logging, testing, and dependency conventions.
- Reuse established helpers and abstractions when they are actually appropriate.
- Do not introduce a second pattern when the project already has a good one.
- Do not refactor unrelated code merely because it could be cleaner.

Do not make one new file look like it belongs to a different architecture or programming style.

### Avoid

- Introducing a new state-management library when one already exists.
- Creating a new `Service`, `Manager`, `Helper`, `Factory`, or `Utils` layer for one small operation.
- Switching between callback, Promise, async/await, futures, channels, or reactive patterns without a reason.
- Mixing naming conventions in the same subsystem.

---

## 2. Implement the smallest coherent change

Optimize for the smallest change that correctly satisfies the requirement.

Before coding, identify:
- the entry point;
- the existing implementation path;
- the minimal files that genuinely need modification;
- the tests that define the behavior.

Prefer:
- one focused function over three wrappers;
- one shared implementation over copy/paste variants;
- direct calls over needless indirection;
- localized changes over repository-wide churn.

Avoid large rewrites for small requirements. Do not regenerate an entire file when a small edit is sufficient.

---

## 3. Use domain language, not generic placeholder language

Names should communicate the domain and the invariant.

Prefer:
- `invoiceTotal` over `processedData`;
- `customerId` over `id2`;
- `expiresAt` over `dateValue`;
- `retryAfter` over `timeoutValue`.

Avoid chains of generic names such as:

```text
processData
handleData
result
responseData
updatedData
helper
manager
utils
context
value
item
```

Generic names are acceptable when the scope is genuinely generic. Do not use them because the underlying concept has not been understood.

---

## 4. Establish invariants instead of coding defensively everywhere

Do not add null checks, type checks, casts, and fallback values merely because they might make the compiler or runtime stop complaining.

First determine the actual contract.

Prefer:
- strong types;
- validated boundaries;
- explicit preconditions;
- meaningful return types;
- constructors/functions that establish valid state.

Then keep internal code simple because those invariants are known.

Avoid patterns such as:

```python
try:
    ...
except Exception:
    return None
```

or:

```ts
return data?.user?.profile?.account?.name ?? "Unknown";
```

when the program contract guarantees those values.

Do not hide uncertainty with `Any`, `any`, `object`, unchecked casts, `unwrap()`, `.clone()`, `shared_ptr`, `dynamic`, or `!!` merely to make a local problem disappear.

---

## 5. Prefer explicit ownership and lifecycle semantics

For systems languages, make ownership, borrowing, mutation, allocation, and cleanup understandable from the design.

### C
- Establish buffer sizes from actual requirements.
- Check allocation and API return values when they matter.
- Make ownership and cleanup paths obvious.
- Avoid unsafe legacy string operations when safer alternatives are practical.

### C++
- Prefer RAII.
- Express ownership with `unique_ptr` or values when appropriate.
- Use `shared_ptr` only for genuine shared ownership.
- Avoid unnecessary copies and `std::move`.
- Make invalidation and lifetime rules clear.

### Rust
- Prefer borrowing when ownership is not required.
- Do not sprinkle `.clone()` to satisfy the borrow checker without understanding why.
- Use `Arc`, `Mutex`, `Rc`, and `RefCell` only when the concurrency/ownership model needs them.
- Propagate typed errors instead of hiding them behind `unwrap_or_default()` or broad boxed errors.

---

## 6. Avoid abstraction for abstraction's sake

Create an abstraction when there is a real reason:
- multiple implementations;
- a stable domain boundary;
- a meaningful contract;
- a testing seam that is genuinely useful;
- repeated behavior with important invariants.

Do not create an abstraction because it sounds architecturally sophisticated.

Be suspicious of:

```text
Controller -> Service -> Manager -> Helper -> Utility
```

for a simple operation.

Likewise, avoid one-method interfaces, factories with one trivial branch, adapters that only rename a function, and repository wrappers that simply forward every call.

---

## 7. Reuse before duplicating

Before adding code, search for:
- an existing helper;
- a nearby implementation;
- an existing validator;
- a shared type;
- an existing API client;
- an existing error type;
- an existing test fixture.

If two pieces of logic should stay in sync, give them one implementation.

Avoid creating:

```text
validateUser
validateNewUser
validateUserInput
checkUserData
```

when they are materially the same operation.

Do not copy a function merely to change one line. Parameterize only when the resulting abstraction is clearer than duplication.

---

## 8. Keep comments useful

Comments should explain information that the code itself cannot communicate well:
- why a surprising decision exists;
- an external constraint;
- a subtle invariant;
- a performance tradeoff;
- a compatibility workaround;
- a safety condition.

Avoid comments that merely narrate syntax:

```js
// Increment the counter
counter++;
```

Avoid inflated prose such as:

```text
// This function robustly and seamlessly handles the comprehensive process...
```

Prefer concrete explanations:

```text
// Keep the transaction open through the outbox insert so the event
// cannot be published for a database write that later rolls back.
```

Do not claim a rationale that you have not verified.

---

## 9. Write errors that preserve useful context

Errors should answer:
- what failed;
- which operation or resource was involved;
- whether the caller should retry;
- whether the failure is expected or exceptional.

Avoid universal fallbacks such as:

```text
Something went wrong
Operation failed
Invalid request
```

Use the project's established error model. Do not mix exceptions, sentinel values, empty collections, nullable returns, and special strings arbitrarily.

Never swallow errors merely to keep the program running.

---

## 10. Treat security as part of correctness

Do not add security as vague TODOs.

Explicitly reason about:
- authentication;
- authorization;
- resource ownership;
- input validation;
- output encoding;
- secrets;
- path traversal;
- SQL injection;
- command injection;
- SSRF;
- CSRF where applicable;
- unsafe deserialization;
- file-upload limits;
- rate limits;
- replay/idempotency;
- logging of sensitive information.

Never hard-code real credentials.

Do not use permissive configuration such as `*`, `777`, or unrestricted origins merely because it makes a prototype work.

---

## 11. Make dependencies intentional

Before adding a dependency:
1. Check whether the language/runtime already provides the capability.
2. Check whether the repository already has a suitable dependency.
3. Confirm the package name and current API from authoritative documentation.
4. Use only the smallest dependency necessary.

Avoid packages added because their name sounds relevant.

After adding a dependency, verify:
- it is actually imported/used;
- the version is compatible;
- the feature is not already available locally;
- the dependency does not duplicate another library.

---

## 12. Test behavior, not the implementation

Tests should express observable behavior and important invariants.

For each behavior, consider:
- happy path;
- invalid input;
- boundary values;
- authorization/ownership;
- duplicate/replayed operations;
- partial failure;
- timeout/network failure;
- concurrency/race behavior where applicable;
- persistence consistency;
- backward compatibility.

Avoid tests that only prove a method returned a non-null value or that a particular private helper was called.

Do not manufacture a large test suite just to increase apparent coverage. A small set of meaningful tests is better than many shallow tests.

---

## 13. Avoid magic numbers and magic defaults

Every non-obvious number should have a reason.

Bad:

```text
retry = 3
sleep = 500
limit = 137
window = 86400
```

when nobody knows why.

Better:
- derive values from the domain;
- use named constants;
- document externally imposed limits;
- put operational settings in configuration when appropriate.

Do not invent “empirically optimal” numbers without evidence.

---

## 14. Keep control flow understandable

Prefer straightforward control flow over cleverness.

Avoid:
- deeply nested conditionals;
- promise/effect/coroutine chains that only synchronize simple operations;
- abstractions whose primary purpose is hiding control flow;
- recursion where iteration is clearer;
- metaprogramming for ordinary behavior;
- shell sleeps used as synchronization;
- giant LINQ/stream/iterator expressions that are harder to review than a loop.

A maintainer should be able to trace the main path without reconstructing a generated framework.

---

## 15. Language-specific guidance

### JavaScript / TypeScript
- Prefer the repository's established async and state-management model.
- Use types to establish real contracts; do not escape with `any` or `as any`.
- Avoid `useEffect` as a general-purpose synchronization mechanism in React.
- Avoid unnecessary `useMemo`, `useCallback`, context layers, and custom hooks.
- Keep API response types aligned with reality.
- Do not use `setTimeout` as a substitute for understanding event/order guarantees.

### Python
- Use concrete types where they improve the contract.
- Avoid `except Exception` unless there is a specific recovery strategy.
- Prefer clear data models over unstructured dictionaries when the shape is stable.
- Avoid wrapping every function in `try/except`.
- Do not turn every failure into `None`.

### C
- Make memory ownership explicit.
- Derive buffer sizes from actual constraints.
- Check important return values.
- Avoid undefined behavior and unsafe legacy string operations.
- Ensure every allocation path has a clear cleanup path.

### C++
- Prefer RAII and value semantics where practical.
- Use smart pointers to express ownership, not as default replacements for raw pointers.
- Preserve `const` correctness.
- Avoid needless copies, casts, and template machinery.
- Be explicit about iterator invalidation and object lifetimes.

### Java
- Avoid one-method interfaces and needless wrapper layers.
- Do not create factories/repositories/services unless they represent a real boundary.
- Keep exception semantics coherent.
- Avoid streams when a simple loop communicates intent better.
- Keep ORM/entity/DTO mappings proportional to the domain.

### Rust
- Design ownership before fighting the borrow checker.
- Treat repeated `.clone()` as a prompt to re-check ownership.
- Avoid `unwrap()`/`expect()` on normal production failure paths.
- Use typed `Result` errors and propagate context deliberately.
- Avoid `Arc<Mutex<T>>` as a universal state-management pattern.

### Go
- Return and handle errors meaningfully.
- Avoid `any`/`interface{}` when a concrete type is available.
- Do not introduce interfaces before a real abstraction need exists.
- Be deliberate about goroutine lifetime and cancellation.
- Avoid `time.Sleep` as synchronization.

### C#
- Avoid `dynamic`, broad `object`, and nullable suppression as escape hatches.
- Preserve async semantics; do not block on tasks.
- Use cancellation tokens meaningfully.
- Avoid DI, repository, and service layers that merely forward calls.
- Keep LINQ readable and avoid unnecessary materialization.

### Kotlin
- Avoid `!!` as a routine nullability strategy.
- Use structured concurrency rather than detached global scopes.
- Keep coroutine and Flow usage proportional to the problem.
- Prefer domain types over nullable/hash-map plumbing.

### Swift
- Avoid `!`, `try!`, and `as!` unless the invariant is truly guaranteed and documented when surprising.
- Prefer structured concurrency.
- Keep View/ViewModel responsibilities coherent.
- Avoid `Any` when a concrete Codable/domain type is available.

### PHP
- Prefer explicit domain types and validated request data.
- Avoid loose associative arrays as the universal data model.
- Do not suppress errors with `@`.
- Keep controllers thin without building an arbitrary service labyrinth.

### Ruby
- Keep service objects small and purposeful.
- Avoid broad `rescue StandardError` with silent recovery.
- Prefer ordinary Ruby constructs before metaprogramming.
- Keep callbacks and model responsibilities understandable.

### SQL
- Establish join cardinality before adding `DISTINCT`.
- Avoid `SELECT *` in stable application queries.
- Keep predicates index-aware.
- Reason about transaction boundaries and isolation.
- Do not hide N+1 queries behind ORM abstractions.

### Bash / Shell
- Quote variables.
- Use safe temporary-file handling.
- Do not use `eval` casually.
- Check failure semantics explicitly.
- Never use `sleep` where readiness polling or process synchronization is required.
- Avoid destructive commands whose safety depends on an unchecked variable.

---

## 16. Review the diff before finishing

Before considering a change complete, inspect the resulting diff.

Ask:
- Did I modify only what was necessary?
- Did I accidentally introduce a new architecture?
- Did I duplicate existing code?
- Did I add dependencies or abstractions without need?
- Are comments explaining non-obvious reasoning rather than narrating syntax?
- Are errors handled according to project conventions?
- Did I add arbitrary defaults?
- Did I create new TODOs that hide incomplete functionality?
- Does the code match neighboring code?
- Are the tests meaningful?
- Is any security-sensitive behavior still assumed rather than verified?

If the diff is disproportionately large for the requirement, stop and simplify it.

---

## 17. Do not optimize for “looking human” superficially

Never insert arbitrary quirks, inconsistent formatting, fake mistakes, fake comments, or intentionally awkward names to make code appear human-authored.

Do not:
- deliberately add typos;
- randomly vary style;
- add unnecessary comments;
- split simple functions solely to change statistics;
- sabotage consistency;
- mimic a particular developer's idiosyncrasies without evidence.

The reliable way to avoid “vibe-coded” quality is to produce **coherent engineering artifacts that reflect real understanding of the repository**.

---

## 18. Final quality gate

Before delivering code, verify all of the following:

- The implementation fits the existing architecture.
- The changed surface is appropriately small.
- Domain concepts have meaningful names.
- Invariants are explicit and trusted rather than defended against everywhere.
- Ownership/lifetime semantics are clear.
- Duplication is minimized.
- Abstractions have a concrete reason to exist.
- Errors preserve useful context.
- Security-sensitive behavior is deliberate.
- Dependencies are justified and verified.
- Tests cover behavior and important failure modes.
- Magic numbers and defaults have reasons.
- Comments explain non-obvious decisions.
- No fake TODOs hide incomplete requirements.
- The diff is internally consistent and easy for a maintainer to review.

If a simpler implementation satisfies the same requirements, prefer the simpler implementation.
