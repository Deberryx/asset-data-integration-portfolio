# Data Quality & Reconciliation

## Audited scope

The source audit inspected **4,545 non-empty mapped rows**, comprising:

- **4,513 operational records**;
- **32 AGA item-only placeholders** excluded from the operational canonical set.

Issue counts were rule instances rather than distinct records:

| Severity | Instances |
| --- | ---: |
| ERROR | 315 |
| WARNING | 3,261 |
| INFORMATIONAL | 2,351 |

## Identifier findings

The three source registers did not have equally reliable identifier semantics.

- AGAMal and CLM/CSS were strongly identifier-oriented.
- AGA's `ASSET NUMBER` column sometimes contained descriptions such as office furniture or equipment labels rather than trustworthy identifiers.

The audit therefore avoided treating every non-empty asset-number cell as an official ID.

Six formal-ID duplicate candidate groups were reported, along with repeated non-ID descriptive text and one duplicate serial-number group. **No duplicate was automatically deleted or merged.**

## Conservative normalization

The rules preserve source evidence and normalize only where justified.

Examples:

- leading/trailing whitespace can be trimmed for comparison while raw values remain preserved;
- case/punctuation variants can be grouped conservatively;
- ambiguous location families are reported for review rather than silently collapsed;
- positive numeric quantities are parsed, but missing values are never defaulted;
- explicit donation markers remain donation semantics rather than becoming zero-cost purchases.

## Condition semantics

The data supported the following initial condition vocabulary:

- GOOD
- FAULTY
- FOR REPAIR
- STOLEN
- UNKNOWN / blank

The project did not invent unsupported categories such as FAIR or POOR.

## Reporting confidence

The audit explicitly graded future dashboard metrics by confidence. Counts by source were high-confidence, while metrics such as current location, combined acquisition value, condition distribution, total physical units and data-quality scoring carried lower confidence until business rules were approved.

## Engineering lesson

A useful dashboard begins with a trustworthy data contract. The goal of this phase was not to make messy source data look clean; it was to make uncertainty visible and machine-readable without losing provenance.
