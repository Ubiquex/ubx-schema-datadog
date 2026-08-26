# ubx-schema-datadog

A real, frozen, versioned Datadog provider schema snapshot -- the pinnable
distribution artifact `ubx-provider-dynamic` and `ubiquex` resolve a
`[providers.datadog]`/`[providers.datadog_v2]`/`[providers.datadog_ds]`/
`[providers.datadog_v2_ds]` entry against, with zero network calls at
schema resolution time (see `provider/acquireschema.go` in `ubiquex`, and
`internal/snapshot`'s own doc comment in `ubx-provider-dynamic`).

## What's here

Datadog's own real published identity (`repo_name = "datadog"`) is a
GROUP of four members, all fetched from Datadog's own real, published
OpenAPI specs (v1 and v2, same repo/branch) but built through genuinely
different pipelines:

- `datadog` -- v1 resource mode (26 real resource types).
- `datadog_v2` -- v2 resource mode (152 real resource types -- v1 alone
  misses most of Datadog's own real surface; v2 is not a v1 replacement,
  most of v1's own resource types have no v2 equivalent at all, so both
  stay configured, neither retires the other).
- `datadog_ds` -- v1 data-source mode (79 real, unclaimed read-only
  operations).
- `datadog_v2_ds` -- v2 data-source mode (450 real, unclaimed read-only
  operations).

- `manifest.json` -- the group's own real identity: `schema_format`,
  `provider`, one `version` for the WHOLE group, and which member names
  it bundles.
- `members/<name>.json` -- one real, complete, independently-diffable
  file per member. Committed as separate files, not one combined blob,
  so a real version bump's own git diff shows exactly which members
  changed.
- `.github/workflows/hash-watch.yml` -- runs weekly (and on manual
  dispatch), regenerates every member from the live specs and opens a PR
  only when the group's own mechanically-derived version (the highest
  real change level found across every member -- `internal/snapshot`'s
  `AssembleGroup`) actually moves. Never auto-merges.
- `.github/workflows/publish.yml` -- manual-dispatch-only. Packs
  `manifest.json` and every `members/*.json` into one compressed archive
  (`snapshot.tar.gz`) and cuts a real GitHub Release tagged `v<version>`
  carrying exactly two assets: `snapshot.tar.gz` and `SHA256SUMS`.

## Consuming a real, published version

In `ubiquex`, pin each real member you need -- all four point at the SAME
repo/version, `provider.AcquireSchema`'s own cache-by-source+version
collapses this into ONE real download and ONE extracted cache directory
regardless of how many members reference it:

```toml
[providers.datadog]
source  = "ubiquex/datadog"
version = "1.0.0"

[providers.datadog_v2]
source  = "ubiquex/datadog"
version = "1.0.0"

[providers.datadog_ds]
source  = "ubiquex/datadog"
version = "1.0.0"

[providers.datadog_v2_ds]
source  = "ubiquex/datadog"
version = "1.0.0"
```

Each launched process picks its own member back out of the shared group
by the `UBX_DYNAMIC_PROVIDER_NAME` it already receives.

Real v1/v2 collisions (two resource names, one data-source name -- v1's
own richer version always wins) are resolved at `ubx sdk gen` codegen
time in `ubiquex` (`[dynamic_provider_groups.datadog_all].exclude`), a
separate, pre-existing concern from this repo's own snapshot -- a
pinned member's own real content is exactly what was fetched, unmerged;
the exclude/merge step runs downstream of pinning, identical to how it
already runs downstream of a live fetch.

## Versioning

One real, mechanically-derived semver number for the WHOLE group, not
one per member: the highest real change level found across every
member (a brand new resource type or a field that gained write access
bumps MINOR; a resource type or field that disappeared, or a field that
lost write access, bumps MAJOR; a pure description-text change bumps
PATCH), plus an unconditional MAJOR if a member the group used to
bundle is gone entirely. See `internal/snapshot/diff.go` and
`AssembleGroup` in `ubx-provider-dynamic` for the real rule.
