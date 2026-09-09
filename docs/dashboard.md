# Dashboard & Reporting Semantics

## Read-only boundary

The dashboard is deliberately separated from source ingestion. It reads only a verified, active canonical generation and does not open Excel workbooks or mutate SharePoint data.

Before dashboard preparation, the runtime verifies:

- the active generation pointer;
- the `COMMITTED` marker;
- the exact approved source boundary;
- the expected 4,513 operational records and 32 exclusions;
- canonical data byte length and SHA-256 against the generation manifest.

A failed validation blocks preparation rather than publishing a partial snapshot.

## Metric semantics

The dashboard separates:

- **Registered Asset Records** — canonical operational records;
- **Known Asset Units** — only records with a valid high-confidence positive quantity.

In the validated generation:

- registered records: **4,513**;
- known units: **4,188**;
- valid quantity records: **4,188**;
- quantity not applicable: **309**;
- quantity missing/unparsed: **16**.

This prevents a record count from being mislabeled as a trustworthy physical-unit count.

## Condition handling

Validated condition counts were:

| Condition | Records |
| --- | ---: |
| GOOD | 1,503 |
| FAULTY | 638 |
| FOR_REPAIR | 14 |
| STOLEN | 4 |
| UNKNOWN | 2,354 |

UNKNOWN remains an explicit category and is never converted into GOOD.

## Search and traceability

Asset search indexes business-relevant canonical fields such as official asset number, description, serial number, asset type, category, designation, original/current location, condition and source workbook.

Internal snapshot identity is excluded from general search and shown only in trace detail, where it is labelled as snapshot-only evidence.

## Unsupported metrics are intentionally absent

The dashboard does not invent:

- currency conversion;
- replacement value;
- condition scores;
- durable lifecycle identity;
- unsupported data-quality scoring.

## Engineering lesson

A dashboard should communicate the limits of its source data. Separating record counts from known physical quantities and preserving UNKNOWN states makes the reporting more trustworthy, even when the headline numbers look less tidy.
