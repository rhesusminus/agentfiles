These rules apply to every project. A project can add its own in a `CODING_STANDARDS.md`
at its repo root; where the two disagree, the project's rule wins.

## Code style
- **STY-1**: A function does one thing. If you need "and" to describe it, split it.
  *Why: small single-purpose functions are easier to test and reuse.*
- **STY-2**: Functions over ~40 lines should be split unless there's a clear reason
  (e.g. a flat lookup table or switch).
- **STY-3**: Use `async`/`await`, not raw `.then()` chains.
- **STY-4**: Use named exports, not default exports.

## Single source of truth
- **SRC-1**: One concept has one implementation. Before writing a helper, validation, limit or
  message format, search for an existing one and reuse it.
  *Why: copies drift, and a fix lands in one copy only.*
- **SRC-2**: Every entry point that accepts the same input applies the same checks and limits,
  through shared code.
  *Why: a limit enforced on one path and not the other is not a limit.*

## Boundaries
- **BND-1**: Validate untrusted input where it enters the trusted side (the server, the
  library's public API). Client-side checks are a convenience, never the enforcement.
- **BND-2**: Check everything that can be checked up front before starting long or expensive
  work.
  *Why: failing after a long run loses the work; failing before it costs nothing.*

## Tests
- **TST-1**: A bug fix comes with a test that fails without the fix.
  *Why: it proves the fix addresses the real bug and keeps it from coming back.*
