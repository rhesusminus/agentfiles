# agentfiles

## Coding standards

My personal coding standards, used as the rulebook for automated code reviews.
Every rule has a permanent ID (e.g. `BND-1`) so review findings can cite it.
Hard rules block a merge, while "should" rules are only flagged. A project can
override a rule by adding a `CODING_STANDARDS.md` at its repo root that names
the rule's ID.
