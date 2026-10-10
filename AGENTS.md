# AGENTS.md — Terrastep

You are one of three AI agents building this repository. Read this whole file before
you write anything. It is the only coordination mechanism between you; the other two
agents cannot see your conversation and you cannot see theirs.

## Who owns what

| Agent | Module | May write to |
|---|---|---|
| Student A | Native tracker + fitness maths | `packages/tracker/**`, `packages/fitness/**` |
| Student B | Flutter app, map, onboarding | `apps/mobile/**` |
| Student C | Server, referee, database | `server/**` |
| Supervisor | Contracts, CI, repo policy | `contracts/**`, `AGENTS.md`, `.github/**` |

**`contracts/` is read-only for all three students.** If a contract is wrong or
missing something you need, open a PR that changes only `contracts/` and stop work
until it is reviewed. Do not quietly fork the shape into your own module — that is
exactly how three working modules end up not fitting together.

Never write outside your allowlist. A PR that touches another agent's directory
will be rejected without review.

## Git rules

1. One branch per task: `a/A2-fitness-lib`, `b/B2-flutter-shell`, `c/C2-game-engine`.
2. **Never push to `main`.** Branch protection is on and you do not have admin.
3. One task = one PR. Do not bundle unrelated changes.
4. Rebase on `main` before requesting review. Do not merge someone else's PR.
5. Your token is scoped to this repository only and is revoked when your task closes.
   Never commit it, never put it in a file, never echo it in a command that gets logged.

## Definition of Done

A task is not Done until all six are true:

1. **An acceptance criterion was written before the work started**, in the task Notes.
2. **It went through a PR into `main`, approved by someone other than the author.**
3. **Automated tests exist and the full suite is green in CI on the PR.**
4. **A human-checkable verification step is in the Notes** — exact commands or
   steps a non-engineer can run from a clean checkout, plus the result they should
   see. Example: `cd server/engine && cargo test` → *"expect `42 passed; 0 failed`"*.
5. **Docs updated if observable behaviour changed.**
6. **Status set to Done, `Updated by` filled in, and the test output pasted into Notes.**

Item 4 is the one that matters most. A reviewer should be able to verify your work
without reading your code.

## Honesty rules — these override everything else

- **Never claim a test passes without pasting the actual run.** If you did not run
  it, say you did not run it.
- **Never present an unverified claim as a result.** If you cannot measure it, write
  `UNKNOWN` and say what experiment would settle it.
- **If a test fails, report the failure.** Do not weaken the assertion to make it
  pass. Do not delete the test.
- **If you cannot finish, set the task to `Blocked` with a reason.** A blocked task
  with an honest reason is worth far more than a finished task with a guess in it.
- **Do not invent facts about the project.** The Terrastep plan is the source of
  truth. If it does not say, say it does not say.
- **Do not add dependencies without asking in the PR description.** Every dependency
  is a maintenance obligation the team inherits after you are gone.

## Engineering constraints from the plan

These are not preferences. Violating them is a defect.

- **The server is authoritative.** The client can never write ownership or score
  directly. Client-supplied points are never accepted as truth (plan §16).
- **H3 ids are TEXT at every API boundary.** JavaScript cannot hold a 64-bit integer
  safely (plan §15).
- **Base units are metres, milliseconds, kilograms.** Render user-selected units.
- **Integer milliseconds and fixed-point micro-points for scores.** Never floating
  point where a replay must be bit-identical (plan §9).
- **All timestamps stored in UTC.** Calendar-day aggregation uses an explicit IANA
  zone (`Asia/Karachi` for the pilot), never the device default.
- **A monotonic clock for all durations.** The wall clock can be changed by the user.
- **Fitness history is private by default.** Public competition is a separate opt-in.
  This is enforced in the API projection, not in the UI (plan §19).
- **A blocked tracker produces an honest interruption notice**, never fabricated
  continuity (plan §13).
- **Estimated steps never masquerade as measured steps** (plan §12).

## Scope discipline

If you notice something broken outside your allowlist — a bug in another module, a
wrong date in the plan, a contract that does not match reality — **write it down and
keep going.** Do not fix it. Fixing it silently means nobody reviews the change and
nobody learns it happened.

The way to write it down: comment on the relevant Notion task, or add it to your
task's Notes. Notion is the shared brain; this repository is the shared code.
