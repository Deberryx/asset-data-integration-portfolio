# Canonical Data Contract & Identity

## Core rule

Raw source values remain authoritative evidence. Derived values support comparison and reporting but never overwrite the original spreadsheet value.

## Snapshot identity

The canonical contract distinguishes one immutable workbook snapshot from durable real-world asset identity.

Key concepts:

- `SourceSnapshotID` identifies an exact workbook version using `sha256:<workbook SHA-256>`.
- `SourceRecordKey` combines workbook, allowlisted sheet, snapshot identity and observed source row to locate one record inside that snapshot.
- `CanonicalSnapshotRecordKey` is a deterministic UUIDv5-based key unique within the canonical snapshot/generation.
- snapshot keys are never presented as official asset numbers.
- durable asset identity remains explicitly unavailable when source evidence cannot guarantee lifecycle continuity.

## Business identifiers

`AssetNumber` is populated only when the source value is trustworthy as an official identifier. Descriptive text found in an identifier column remains raw evidence rather than being promoted into the official-ID field.

A separate normalized comparison key can standardize case, Unicode dashes and whitespace for duplicate review without changing the original value.

## Location and quantity

Original and moved-to locations are retained separately. Normalized versions are used for comparison, and current location follows an approved structural rule rather than free-form inference.

Quantity is parsed only when a valid positive numeric value is present. Missing or ambiguous quantity remains missing, with an explicit confidence/state field.

## Cost semantics

Raw acquisition-cost values are preserved exactly, including formulas and text. Numeric canonical cost is populated only when the source cell is genuinely numeric.

Acquisition cost type distinguishes:

- `PURCHASED`
- `DONATED`
- `UNKNOWN`

An explicit donation marker is never converted to numeric zero.

## Why durable identity was deferred

Row positions, workbook UUIDs, content fingerprints and duplicated business IDs cannot reliably prove that a record in a future workbook version represents the same physical asset.

Rather than overstate certainty, the model marks identity scope as snapshot-only and defers lifecycle identity until an approved architecture can provide a durable source identifier or matching process.

## Engineering lesson

Internal identifiers are useful for traceability, but they should not be confused with business identity. Explicitly modelling that boundary prevents downstream dashboards and automation from making unsupported lifecycle claims.
