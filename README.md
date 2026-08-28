# ubx-schema-datadog

A real, frozen, versioned Datadog provider schema snapshot -- the pinnable
distribution artifact `ubx-provider-dynamic` and `ubiquex` resolve a single
`[providers.datadog]` entry against, with zero network calls at schema
resolution time (see `provider/acquireschema.go` in `ubiquex`, and
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

In `ubiquex`, one pin serves all four real members -- resources and data
sources together, `[providers.datadog_v2]`/`[providers.datadog_ds]`/
`[providers.datadog_v2_ds]` are not needed at the config level at all,
matching Kubernetes' own single-entry shape:

```toml
[providers.datadog]
source  = "ubiquex/datadog"
version = "1.0.0"
```

**Real collision, resolved on the artifact itself**: Datadog's own real
v1/v2 surface has a genuine collision -- two resource type names and one
data-source type name that both v1 and v2 independently produce. Unlike
`ubx-schema-kubernetes` (whose two real members never collide, so this
never came up there), `manifest.json` here carries a real `exclude`
table recording which member's own copy loses each contested name --
the same real "v1 wins on field richness" judgment `ubx sdk gen` already
makes today via `[dynamic_provider_groups.datadog_all].exclude` in
`ubiquex`, now recorded on the published snapshot itself so the pinned
path can read it too, rather than inventing a second, separate
judgment. `ubx-provider-dynamic`'s own real, generic group-merge
mechanism (`internal/snapshot.MergeOpenAPIGroup`) consults this table
during merge and still refuses loudly on any collision it does NOT
name -- confirmed live against this repo's own real, regenerated
snapshot: 176 real resource types and 528 real data-source types
survive the merge, with `datadog_application_key_response` correctly
carrying v1's own 5 real attributes rather than v2's 3.

## Versioning

One real, mechanically-derived semver number for the WHOLE group, not
one per member: the highest real change level found across every
member (a brand new resource type or a field that gained write access
bumps MINOR; a resource type or field that disappeared, or a field that
lost write access, bumps MAJOR; a pure description-text change bumps
PATCH), plus an unconditional MAJOR if a member the group used to
bundle is gone entirely. See `internal/snapshot/diff.go` and
`AssembleGroup` in `ubx-provider-dynamic` for the real rule.

<!-- README-GEN:BEGIN -->
**Real, current published version:** `v1.0.0`

## Links

- Docs: https://docs.ubiquex.io
- Internals (architecture and design): https://github.com/Ubiquex/ubiquex-internals
- Linear board: https://linear.app/ubiquex
<!-- README-GEN:END -->
