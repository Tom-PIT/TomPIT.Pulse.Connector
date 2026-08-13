# Relationships

Pulse entities are connected through integer identifiers.

Create or retrieve referenced records before submitting dependent records.

## Core operational relationships

```mermaid
graph TD
  P[Plant] --> L[Production line]
  L --> B[Batch]
  B --> S[Stage]

  B --> BP[Batch plan]
  B --> BU[Batch usage]
  B --> BS[Batch shift]
  B --> PR[Produced]

  S --> SP[Stage plan]
  S --> SU[Stage usage]
  S --> SD[Stage delay]
  S --> DT[Downtime]

  S --> RP[Resource plans]
  S --> RU[Resource usage]
  S --> W[Waste]
```

## Shared identity

The following records share their parent `id`:

| Parent | Related records |
| --- | --- |
| [Batch](../manufacturing/batch.md) | [Batch plan](../manufacturing/batch-plan.md), [Batch usage](../manufacturing/batch-usage.md) |
| [Stage](../manufacturing/stage.md) | [Stage plan](../manufacturing/stage-plan.md), [Stage usage](../manufacturing/stage-usage.md) |
| [Downtime](../manufacturing/downtime.md) | [Downtime plan](../manufacturing/downtime-plan.md), [Downtime usage](../manufacturing/downtime-usage.md) |
| [Maintenance](../maintenance/maintenance.md) | [Maintenance plan](../maintenance/maintenance-plan.md), [Maintenance usage](../maintenance/maintenance-usage.md) |

For example:

```text
Batch.id = BatchPlan.id = BatchUsage.id
```

Create or retrieve the parent record before submitting the related plan or usage record.

## Resource records

Resource plan and usage records reference the main operational entity directly.

Manufacturing resource records reference [Stage](../manufacturing/stage.md):

```mermaid
graph LR
  S[Stage] --> MP[Material plan]
  S --> EP[Energy source plan]
  S --> EQP[Equipment plan]
  S --> LP[Labor plan]
  S --> XP[Expense plan]

  S --> MU[Material usage]
  S --> EU[Energy source usage]
  S --> EQU[Equipment usage]
  S --> LU[Labor usage]
  S --> XU[Expense usage]
```

Maintenance resource records reference [Maintenance](../maintenance/maintenance.md):

```mermaid
graph LR
  M[Maintenance] --> MP[Material plan]
  M --> EP[Energy source plan]
  M --> EQP[Equipment plan]
  M --> LP[Labor plan]
  M --> XP[Expense plan]

  M --> MU[Material usage]
  M --> EU[Energy source usage]
  M --> EQU[Equipment usage]
  M --> LU[Labor usage]
  M --> XU[Expense usage]
```

Resource records do not reference Stage plan, Stage usage, Maintenance plan, or Maintenance usage directly.

## Waste relationships

A [Waste](../manufacturing/waste.md) record identifies waste, scrap, loss, or another unusable quantity associated with a Stage.

Waste detail records reference the Waste record:

```mermaid
graph TD
  W[Waste] --> WM[Waste material usage]
  W --> WE[Waste energy source usage]
  W --> WX[Waste expense usage]
```

## Downtime and maintenance

[Downtime](../manufacturing/downtime.md) and [Maintenance](../maintenance/maintenance.md) are separate entities.

[Downtime maintenance](../manufacturing/downtime-maintenance.md) links them and defines the share of a maintenance activity attributed to a downtime event.

```mermaid
graph LR
  D[Downtime] --> DM[Downtime maintenance]
  M[Maintenance] --> DM
```

The percentage is submitted as a decimal fraction. For example, 25% is `0.25`.

## Dimension relationships

Records such as Ambient value can use `dimension` and `dimensionId` instead of a fixed reference field.

The selected [Dimension](dimension.md) determines what type of record `dimensionId` identifies.

For example:

```json
{
  "dimension": 3,
  "dimensionId": 208
}
```

This combination identifies Stage `208`.

## Submission order

A safe submission sequence is:

1. Master data.
2. Main operational entity.
3. Shared-identity plan and usage records.
4. Resource plans and usage.
5. Output, downtime, waste, and measurements.
6. Relationship records such as Downtime maintenance.
