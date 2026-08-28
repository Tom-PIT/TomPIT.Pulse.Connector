# Excel template

The Food & Beverage Excel template provides a structured way to prepare data for a Pulse integration. The completed workbook can be imported into Pulse, where its data is mapped to the corresponding Food & Beverage resources.

<div class="grid cards" markdown>

[**Download the Food & Beverage Excel template**](assets/FoodBeverage.xlsx)

</div>

It is intended for organizations that provide data through spreadsheets or need a common template for collecting information from production, maintenance, and quality teams.

The workbook uses Food & Beverage terminology and mirrors the main concepts exposed by the Food & Beverage API.

## Workbook structure

The workbook groups sheets by when and by whom the data is maintained:

| Group | Purpose | Examples |
| --- | --- | --- |
| **Setup** | Relatively stable data and definitions configured when the integration is established. | Sites, production lines, machines and sensors, products, recipes, materials, measurements, limits, targets, cleaning rules, reasons, types |
| **Production** | Production activities and operational data recorded as production takes place. | Lots, batches, runs, stages, cleans, holds, consumption, output, readings, settings, line time, events |
| **Maintenance and quality** | Maintenance and quality-related activity. | Work orders, parts and labor, complaints |

The workbook also contains a **Start here** sheet with instructions for completing the template and links to the relevant Pulse integration documentation.

## How the template works

- Required and optional columns are visually distinguished.
- Column headings contain comments explaining the expected value.
- Related records are referenced through their documented business identifiers.
- Dropdowns are provided where predefined codes are available.
- Dates and times are entered in the site's local time.
- Each sheet represents one type of business record.
- `Delete?` can be used to explicitly retract an existing record.
- Blank optional values mean that no value is being provided.

## Codes and relationships

Records are connected through documented business identifiers.

For example, a Production line references a Site code, a Recipe references a Product code, and operational records reference codes defined in the setup sheets.

Add stable setup data first so that referenced codes are available when completing production, maintenance, and quality records.

## Using the template

1. Open the **Start here** sheet and follow its instructions and documentation links.
2. Enter the required setup data and definitions.
3. Complete production and operational sheets as data becomes available.
4. Add maintenance and quality records when applicable.
5. Import the completed workbook into Pulse through the available integration process.