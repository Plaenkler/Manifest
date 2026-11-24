# Manifest

## 1. Principles

### Simple, not short-sighted
**The simplest solution wins as long as it duplicates nothing.**

Every piece of knowledge has exactly one place. A building block needed twice moves into its own module instead of being copied.

### Little magic, few dependencies
**Every function and every dependency must earn its place.**

Each dependency brings updates, vulnerabilities and foreign concepts. What the code does should be visible when reading it, without studying a framework first.

### Use what exists
**Before installing or building anything, check whether the standard library or the platform already does it.**

Standard libraries, operating systems and browsers cover far more than expected. A third-party package or a build step comes only when these are proven not to be enough.

### Build only what is needed
**Code for an imagined future is a burden in the present, and code for a past need is deleted.**

Options, extension points, interfaces and generics are added when a second real case arrives, not in advance. Unused functions, exports and commented-out code are removed, because version control already remembers them.

### Boy scout rule
**Leave code slightly cleaner than you found it.**

Whoever touches a file fixes an unclear name or splits a long function along the way. The improvement stays small and within the area already being worked on.

## 2. Names and values

### Names reveal intent
**A good name makes a comment unnecessary.**

Types are named for what they are, functions for what they do, and the reach of a name decides its length: a single letter inside a three-line loop, a full name across a module. Beyond that, the naming conventions of the language apply as its linter enforces them.

### One term per concept
**The same thing has the same name in every layer.**

If the database, the API and the user interface all call it `customerId`, nobody has to translate. Different things get different names.

### Values have names
**Every fixed value becomes a named constant in the current scope, even if it is used only once.**

Numbers, strings, paths, timeouts and limits never appear bare. The name explains the purpose, makes the value searchable and is the single place to change it, declared as close to its use as possible.

## 3. Functions

### One purpose
**A function or module does exactly one thing and changes for exactly one reason.**

Anything that can only be described with "and" is two things. Modules are cut by domain (`order`, `invoice`), never by technique (`models`, `utils`).

### One level of abstraction
**A function either describes the flow or handles the details, never both.**

A function that describes a process calls steps instead of building queries or markup in between. The details live one level down.

### Return early
**Edge cases are handled first, the normal path runs straight down.**

Guard clauses reject invalid input and errors at the top and leave immediately. Errors are checked right after the call, and there is no `else` after a `return`.

### Narrow interfaces
**Few parameters, at most two return values, no flag parameters.**

A pair such as value and error is the limit. A boolean that switches behaviour reveals two functions in one. Struct and object literals always use field names.

### No hidden effects
**A function either changes something or returns something, and does nothing its name does not say.**

Queries can be called any number of times, commands cannot, just like `GET` versus `POST`. No code runs on import or at load time, and there is no mutable global state.

### Explicit, not clever
**Code is written for the reader, not for the shortest form.**

There are no ternaries, optional chaining, nullish coalescing, destructuring, multiple assignments beyond what the language requires, naked returns, inline closures as helpers or implicit type conversions. Bindings are immutable unless reassignment is required, and comparisons are strict.

### Procedural first
**A straight flow from top to bottom is enough in most cases.**

Concurrency, reflection, generics and callbacks are used only when there is no other way. When they are needed, every background task has a defined end and is awaited on stop, every handler is a named function, and asynchronous code still reads from top to bottom.

## 4. Errors

### Errors are never swallowed
**Every error is handled or passed on with context.**

An ignored error is a bug, and there is no empty catch. An error passed upwards names the action and the affected object and keeps the original attached. Crashing is reserved for programming errors, and no message ever contains a secret.

### No null as an answer
**A function returns a usable value or an error, never a silent "nothing".**

Empty lists are returned as empty lists, so an API delivers `[]` instead of `null`. Callers stay free of special cases.

## 5. Structure

### Modules manage themselves
**A module owns its lifecycle, logging and errors, the entry point only lists the steps.**

Every module with a lifecycle offers start and stop. The entry point reads like a list of steps.

### Talk to neighbours only
**Code uses what it knows directly, not the inside of the inside.**

Chains like `order.customer.address.city` tie code to the structure of foreign types. Whoever needs something asks the direct neighbour for it.

### Foreign code at the border
**Every external package or service is connected in exactly one place.**

An external service gets its own module that bundles all calls to it. The layer stays thin enough to add no magic of its own.

### The server decides
**Business rules and validation are binding only on the server.**

The client may check input early to guide the user, but every request can bypass it. Each rule therefore has exactly one authoritative place.

### Top to bottom, without gaps
**A file reads like an article, and functions contain no blank lines.**

Constants, types and central functions come first, the helpers they call below. Blank lines separate functions, never sections inside a function or template, because such sections are functions waiting to be extracted.

## 6. Comments and language

### Comments explain why
**The code says what, the comment says why.**

Comments exist only where a decision is not obvious: a workaround, a business rule, a warning. Doc comments on public interfaces are the one exception.

### English for developers, the user's language for users
**Everything developers read is English, everything users read is in their language.**

This covers identifiers, comments, file names, flags, URIs, logs, CLI help and documentation on one side, and templates and UI texts on the other.

### The README is a tool
**The README states briefly how to build, test, run and use the project.**

No marketing and nothing tailored to a single customer.

## 7. Tests

### Tests are production code
**The same rules apply to tests as to all other code.**

Messy tests get switched off, and then the protection is gone. Tests use the language's own test tooling without additional assertion libraries.

### One case, fast and independent
**Each test checks one thing, runs fast, depends on no other test and gives the same result everywhere.**

Variants of one behaviour are table-driven tests with one named case per row. Test data lives in its own directory, and concurrent code runs with a race detector where one exists.

## 8. User interface

### Select, don't type
**Where fixed values exist, the user selects instead of typing.**

The options come from the same constants as on the server. Free text exists only where there is no standard.

### Same element, same place
**Whoever knows one view knows all of them.**

Recurring bars always keep the same order, for example search, then filter, then sort, and exist once as a shared component.

### Disable, don't hide
**What is not possible right now is disabled with a tooltip giving the reason.**

A vanishing control leaves the user guessing whether the feature exists at all. The tooltip replaces explanatory text on the page.

### Status moves forward
**Status transitions are defined in one place and move forward wherever possible.**

Statuses are typed constants, the allowed transitions live in a single table on the server, and the interface only shows which ones are currently possible.

### Confirm before destroying
**Every irreversible action is confirmed first, and nothing else is.**

The confirmation names exactly what will happen. Confirmations stay rare so they keep their weight.

## 9. Operations

### Secrets in one place
**Credentials live only in the designated secrets file, which version control ignores.**

They never appear in code, flags, logs, chat or anything shipped to a client, because everything sent to a client is public. A client that needs a protected service calls it through the server.

### Latest versions, pinned
**The language, runtime and all tools run on the latest stable version, pinned per project.**

Versions are recorded in the project and lock files, and installs reproduce exactly from the lock file. An update is a deliberate change, never an accident.

## 10. Workflow

### Humans commit
**The AI suggests a commit message but never commits or pushes.**

The message is a tag and a few words, for example `[FIX] Status of old orders`. Whoever has seen the change decides what enters the history.

### Done means clean
**A change is finished only when formatter, linter, static analysis and all tests pass without findings.**

When a tool and a style rule conflict, the tool wins and the rule is adjusted. Findings left behind pile up until nobody takes them seriously.

### Tests stay contained
**Local work uses only test data and test directories, and everything started is stopped again.**

A mistake during development must never reach real data, and a forgotten server blocks ports and skews the next test.
