# Terrastep

An Android walking game where verified outdoor movement earns hexagonal territory
in your own neighbourhood. Final year project, three students, 16 weeks.

Source of truth for scope: **Terrastep Product and Technical Plan v1.0**, 3 Oct 2026.
Where this README and the plan disagree, the plan wins.

## Layout

```
contracts/          Shared JSON Schema — the interface between modules. READ-ONLY for students.
packages/tracker/   Native Kotlin tracker (Student A)
packages/fitness/   Fitness maths, no platform imports (Student A)
apps/mobile/        Flutter app: Start / League / Me (Student B)
server/migrations/  Postgres schema + row-level security (Student C)
server/engine/      Deterministic scoring engine (Student C)
.github/            CI: contract validation + module path ownership
AGENTS.md           Rules for the AI agents working in this repo — read it first
```

## Start here

1. Read [`AGENTS.md`](AGENTS.md). It says who may write where, and what Done means.
2. Read [`contracts/README.md`](contracts/README.md). It says what the interfaces are
   and why each constraint exists.
3. Run the contract check: `python3 contracts/validate_contracts.py`

## The one rule that matters most

The server is authoritative. The client can never write ownership or score directly,
and client-supplied points are never accepted as truth. Everything else follows from
that.

## Status

Scaffold and shared contracts only. No module code yet.
