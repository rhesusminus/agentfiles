# Coding standards

These rules apply to every project. A project can add its own in a `CODING_STANDARDS.md`
at its repo root. A project rule wins over a global one only when it names the ID it
overrides, for example:

```
STY-6: off — Next.js pages and routes need default exports.
```

**How to read the rules.** A rule that says "never", "no", or states what code does is a
hard rule: a violation blocks the merge. A rule that says "should" or "prefer" is a
judgment call: flag it, don't block on it.

**Lint first.** Rules marked *(lint)* are enforced by the linter. Reviewers don't comment
on them unless lint is missing or turned off.

**IDs are permanent.** When a rule is removed, its ID is marked retired and is never
reused, so old references don't point at the wrong rule.

## Code style
- **STY-1**: A function does one thing. If you need "and" to describe it, split it.
  A function that only coordinates other functions at one level of abstraction counts
  as one thing.
  *Why: small single-purpose functions are easier to test and reuse.*
- **STY-2**: Functions over ~40 lines (not counting blank lines and comments) should be
  split unless there's a clear reason, such as a flat lookup table or switch.
  *Why: a long function usually hides more than one job.*
- **STY-7**: No empty `catch`. Either handle the error, rethrow it with context, or log
  it and add a comment saying why it's safe to continue.
  *Why: a swallowed error turns a clear failure into a mystery later.*

## TypeScript / JavaScript
- **STY-3**: No `any`; use `unknown` and narrow. *(lint: `no-explicit-any`)* Don't
  bypass narrowing with `as` casts, `as unknown as T`, or `@ts-ignore`. If one of these
  is unavoidable (e.g. an untyped third-party API), it needs a one-line comment saying why.
  *Why: each one switches off the type checker exactly where it's needed.*
- **STY-4**: Use `async`/`await`, not raw `.then()` chains. `Promise.all` and
  `Promise.allSettled` are fine.
  *Why: `await` reads top to bottom, and errors go through normal `try`/`catch`.*
- **STY-5**: Use named exports, not default exports. Default exports are fine where a
  framework requires them.
  *Why: named exports keep one name across imports and make renames and searches reliable.*

## Single source of truth
- **SRC-1**: One concept has one implementation. Before writing a helper, validation,
  limit or message format, search for an existing one and reuse it. If the existing one
  doesn't quite fit, extend it rather than copy it.
  *Why: copies drift, and a fix lands in one copy only.*
- **SRC-2**: Every entry point that accepts the same input applies the same checks and
  limits, through shared code.
  *Why: a limit enforced on one path and not the other is not a limit.*

## Boundaries
- **BND-1**: Validate untrusted input where it enters the trusted side (the server, the
  library's public API). Client-side checks are a convenience, never the enforcement.
  Untrusted input is `unknown` until validated; parse it into a typed shape.
  *Why: anything the client checks, an attacker can skip.*
- **BND-2**: Check everything that can be cheaply checked up front before starting long
  or expensive work. Up-front checks don't remove the need to handle failures mid-run.
  *Why: failing after a long run loses the work; failing before it costs nothing.*
- **BND-3**: Check permissions on the trusted side, for every request, separately from
  input validation. Valid input from someone who isn't allowed to act is still refused.
  *Why: well-formed doesn't mean authorized.*

## Security
- **SEC-1**: No secrets in code, config committed to the repo, or logs. Read them from the
  environment or a secret store, and redact them in error messages.
  *Why: a committed secret stays in git history after it's deleted.*

## Tests
- **TST-1**: A bug fix comes with a test that fails without the fix. If a failing test
  isn't practical (race conditions, UI timing), say why in the PR.
  *Why: it proves the fix addresses the real bug and keeps it from coming back.*
