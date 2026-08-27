# STATE.md — current state

> Rewritten, not appended, as the LAST act of every session. See `HISTORY.md`
> for the narrative.

## In flight

Nothing in flight as of 2026-08-27.

## Blocked

Nothing blocked. Zero open PRs.

## Current release

Latest published: `v1.0.0` (verify directly — `gh api
repos/Ubiquex/ubx-schema-datadog/releases/tags/v1.0.0`). 4 members
(`datadog`, `datadog_v2` resource; `datadog_ds`, `datadog_v2_ds`
data-source). Does NOT carry a real `min_binary_version` yet — predates that
field (UBI-194). A pinned `[providers.datadog]` resolution falls back to
`ubiquex`'s own bootstrap table (`schema_format 3 -> ubx-provider-dynamic
1.0.0`), confirmed working correctly and logging visibly. Deliberately not
forced to regenerate — recommendation on record (UBI-194) is to let this
happen naturally on this repo's own weekly `hash-watch.yml` cron whenever
Datadog's real API next moves, rather than a metadata-only republish now.

## Before touching anything

- Never self-merge here. See `CLAUDE.md`.
- `hash-watch.yml` now builds against a real, tagged `ubx-provider-dynamic`
  release (fixed UBI-194) — a regeneration today would carry a real
  `min_binary_version` if it runs.
