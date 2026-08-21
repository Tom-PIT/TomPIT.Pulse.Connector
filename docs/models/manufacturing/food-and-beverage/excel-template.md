# Excel template

The Food & Beverage Excel template provides a structured way to prepare data for a Pulse integration.

[**Download the Food & Beverage Excel template**](assets/FoodBeverage.xlsx)

It is intended for organizations that provide data through spreadsheets or need a common template for collecting information from production, maintenance, and quality teams.

The workbook uses Food & Beverage terminology rather than Pulse API terminology.

## Workbook structure

The workbook groups sheets by when and by whom the data is maintained:

| Group | Purpose | Examples |
| --- | --- | --- |
| **Setup** | Relatively stable data configured when the integration is established. | Sites, production lines, machines, products, recipes, materials, suppliers, customers, shifts, measurement definitions |
| **Production** | Operational data recorded as production takes place. | Incoming lots, batches, runs, stages, consumption, output, readings, line time, events |
| **Maintenance and quality** | Maintenance and quality-related activity. | Work orders, parts and labour, complaints |

The workbook also contains a **Start here** sheet with instructions for completing the template.

## How the template works

- Required and optional columns are visually distinguished.
- Column headings contain comments explaining the expected value.
- Related codes are selected through dropdowns where possible.
- Dates and times are entered in the plant's local time.
- Each sheet represents one type of business record.
- `Delete?` can be used to explicitly retract an existing record.
- Blank optional values mean that no value is being provided.

## Codes and relationships

Records are connected through business codes.

For example, a production line references a Site code, a recipe references a Product code, and production records select codes defined in the setup sheets.

Add stable setup data first so that it becomes available for selection in the operational sheets.

## Using the template

1. Complete the **Start here** sheet.
2. Enter the required setup data.
3. Complete production sheets as operational data becomes available.
4. Add maintenance and quality records when applicable.
5. Provide the completed workbook through the agreed integration process.