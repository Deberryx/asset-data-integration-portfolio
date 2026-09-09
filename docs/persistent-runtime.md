# Persistent State, Integrity & Recovery

## Runtime goal

The persistent runtime turns a one-cycle reconciliation process into a recoverable local canonical state that survives process restarts without silently replacing a good generation with partial or malformed source data.

## State layout

```text
state/
  source-status.json
  generations/
    <generation-id>/
      canonical-generation.json
      canonical-assets.json
      data-quality-findings.json
      source-reconciliation.json
      parsed-source-slices.json
      COMMITTED
```

Each completed generation is immutable. `source-status.json` is the active-generation pointer and source-health record.

## Promotion model

Promotion follows a guarded sequence:

1. write a unique staging generation;
2. flush and validate all required files;
3. verify the full expected record/exclusion contract;
4. verify source metadata and source boundaries;
5. record byte length and SHA-256 for generation files;
6. rename into its immutable final generation path;
7. replace `source-status.json` last.

If any step fails before pointer replacement, the previous active generation remains in place.

## Startup recovery

On startup the runtime can:

- load and integrity-check the generation referenced by a valid pointer;
- recover from a missing/corrupt pointer by scanning only committed generations;
- ignore incomplete generation directories without a `COMMITTED` marker;
- fall back from a corrupt newest generation to the newest older generation whose manifest, hashes, allowlist and reconciliation still validate;
- return a clean `UNKNOWN` state when no valid generation exists instead of inventing data.

## SharePoint / Graph boundary

The source integration remains **GET-only** for approved SharePoint files. It uses exact-filename discovery, source item/eTag metadata and change detection to avoid unnecessary downloads and parsing.

A live restart validation demonstrated the intended behavior:

- process 1 downloaded and reconciled all three approved sources and promoted a 4,513-record generation;
- process 2 loaded the persisted generation, rechecked the source metadata and detected no changes;
- the second cycle required **zero content downloads** for unchanged source files;
- persisted source statuses remained healthy and generation hashes matched the manifest.

## Engineering lesson

Operational data integration needs a failure model, not just a happy path. Immutable generations, content hashes and atomic pointer replacement make it possible to reject bad cycles without sacrificing the last known-good canonical state.
