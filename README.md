# Manifest

## Scope

- Apply this manifest to every language and project.
- Use the latest stable version of every tool.
- When rules conflict, follow this order: hard limits, linter, conventions of the language, simpler solution.

## Hard limits

- Never commit or push. Suggest a commit message instead: one tag and a few words, e.g. `[FIX] Status of old orders`.
- Keep credentials only in the designated secrets file, which version control ignores. Never put them in code, flags, logs, error messages, chat or anything shipped to a client.
- Never work against production data. Locally, use only test data and test directories.
- Stop every server and process you started once the test is over.
- A change is done only when formatter, linter, static analysis and all tests pass without findings.

## Before writing code

- Check whether the standard library, the platform or an existing dependency already solves the problem. Add a dependency or a build step only when they provably do not.
- Build only what the current task needs. Add options, extension points, interfaces or generics only when a second real case exists.
- Extract a building block into its own module as soon as it is needed twice. Never copy it.
- Delete unused functions, exports and commented-out code. Why: version control keeps the history.
- Leave touched code slightly cleaner, e.g. fix an unclear name or split a long function. Stay within the area you are working on.

## Names and values

- Name types after what they are and functions after what they do.
- Scale name length with scope. Single letters only in loops of a few lines.
- Follow the naming conventions of the language as its linter enforces them.
- Use the same term for the same concept in every layer (database, API, UI) and different terms for different concepts.
- Declare every fixed value (number, string, path, timeout, limit) as a named constant in the current scope, even if it is used only once, as close to its use as possible. Why: one searchable place to change it.

## Functions

- Give every function and every module exactly one purpose. If describing it needs "and", split it.
- Keep functions at most 30 lines long and nested at most 3 levels deep.
- Keep one level of abstraction per function: it either orchestrates steps or handles details.
- Return early with guard clauses. Check errors directly after the call. Never write `else` after `return`.
- Use at most 3 parameters and at most 2 return values, e.g. value and error.
- Never use boolean flag parameters. Split into two functions instead.
- Always use field names in struct and object literals.
- A function either changes state or returns data, never both.
- Do nothing the function name does not say. No code runs on import or load, and there is no mutable global state.
- Never put blank lines inside functions or templates, only between functions. Why: a section inside a function is a function waiting to be extracted.

## Explicit code

- Never use ternaries, optional chaining, nullish coalescing, destructuring, naked returns, inline closures as helpers or implicit type conversions.
- Never use multiple assignment. Except: where the language requires it, e.g. value and error.
- Make bindings immutable unless they are reassigned.
- Use strict comparisons only.
- Write plain procedural code. Use concurrency, reflection, generics and callbacks only when nothing else works.
- Give every background task a defined end and wait for it on stop.
- Make every handler a named function.
- Write asynchronous code that reads top to bottom, e.g. `async`/`await` instead of chained callbacks.

## Errors

- Handle every error or pass it on with context. Never ignore an error and never write an empty catch.
- When passing an error on, name the action and the affected object and keep the original error attached.
- Crash only on programming errors.
- Never return null as "nothing". Return a usable value or an error, and return empty lists as empty lists (`[]`, not `null`).

## Structure

- Cut modules by domain (`order`, `invoice`), never by technique (`models`, `utils`).
- Let every module own its lifecycle (start, stop), its logging and its errors. The entry point only lists the steps.
- Access only direct neighbours. No chains like `order.customer.address.city`.
- Connect every external package or service in exactly one thin module.
- Enforce business rules and validation on the server. Client-side checks only guide the user. Why: any request can bypass the client.
- Order every file top-down: constants, types, central functions, then the helpers they call.

## Comments and language

- Comment only why: workarounds, business rules, warnings. Never restate the code. Except: doc comments on public interfaces.
- Write everything developers read in English: identifiers, comments, file names, flags, URIs, logs, CLI help, documentation.
- Write everything users read in their language: templates, UI texts.
- Keep READMEs brief: how to build, test, run and use. No marketing, nothing tailored to a single customer.

## Tests

- Apply the same rules to tests as to production code.
- Use the built-in test tooling of the language without additional assertion libraries.
- Test one behaviour per test. Write variants as table-driven tests with one named case per row.
- Keep tests fast, independent of each other and deterministic.
- Keep test data in its own directory.
- Run concurrent code with a race detector where one exists.

## User interface

- Offer a selection instead of free text wherever fixed or default values exist. Take the options from the same constants the server uses.
- Keep recurring elements in the same place and order in every view, e.g. search, then filter, then sort. Build them once as a shared component.
- Never hide an unavailable control. Disable it and give the reason in a tooltip, not in explanatory text.
- Define statuses as typed constants and all allowed transitions in one table on the server. Let transitions move forward wherever possible.
- Confirm every irreversible action and name exactly what will happen. Confirm nothing else.

## Versions

- Pin all versions in the project and lock files, and install exactly from the lock file.
- Update only as a deliberate change.
