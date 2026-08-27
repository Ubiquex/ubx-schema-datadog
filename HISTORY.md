# HISTORY.md — narrative archive

> Consulted only when a session needs to know why a decision was made, not on
> every open. For what's current, read `STATE.md` instead.

This file is new as of UBI-183 (2026-08-27). Real history predating it lives
in `ubiquex`'s own `HISTORY.md` (search `UBI-182`, `UBI-185`, `UBI-194`) and
in this repo's own real `git log`/merged-PR history, which is authoritative
for what actually shipped and when.

## Real, known decisions worth carrying forward

**First real published version was `1.0.0`, not `0.1.0`.** Matching every
other schema repo in this org: `ubx-provider-dynamic`'s own generic
first-snapshot default originally produced `0.1.0`; the founder's own
correction (a schema snapshot's version tracks real API surface change, not
artifact maturity — no pre-1.0 phase for something complete on first publish)
means every one of these six repos published its real first version at
`1.0.0`, not `0.1.0`.

**Bootstrap fallback, not yet retired (2026-08-27).** This repo's own
`v1.0.0` predates `min_binary_version` (UBI-194). `ubiquex`'s own
`ResolveDynamicProviderBinaryVersion` falls back to an explicit, logged table
for it — real, working, not a bug. Deliberately not forced to regenerate; see
`STATE.md`.
