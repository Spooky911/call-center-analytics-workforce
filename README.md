# Call Center Analytics and Workforce Planning

An end-to-end call center project covering operational performance, monthly production targets and workforce planning. The analysis uses 500,000 calls recorded from January to March 2025.

## Get the complete project

Download [Call_Center_Complete_Project.zip](https://github.com/Spooky911/call-center-analytics-workforce/releases/latest/download/Call_Center_Complete_Project.zip) from the latest release. The archive contains every file from the original Report folder, including both Power BI reports, all six Excel workbooks, the complete call dataset and the existing inventory.

The Excel workbooks and PDF are also available directly in the repository. The large PBIX and CSV files are included in full in the release archive. FILE_MANIFEST.csv lists the original files, sizes and SHA-256 checksums.

## Files

| File | Purpose |
| --- | --- |
| Call Center Analysis.pbix | Operational analysis and detailed call-center findings. |
| Call Center Analysis.pdf | Complete 26-page written analysis with every table row, including all 250 employees. |
| Production Target Dashboard.pbix | Monthly weighted KPI scorecard, management filters and performance trends. |
| Daily_Half_Hour_Calls.xlsx | Daily calls by half-hour interval. |
| Daily_Half_Hour_Calls_With_Day.xlsx | Interval call counts with dates and weekday names. |
| Interval_Staffing_Plan.xlsx | Required staffing and interval planning assumptions. |
| Weekly_Employee_Schedules_Colored.xlsx | Weekly schedules for the 250 employees, with simple status colors. |
| Scheduled_Live_Agents.xlsx | Planned live agents in each interval. |
| Real_Time_Staffing_Tool.xlsx | Date/interval selection, live-name entry and comparison with required and scheduled staffing. |
| Data/Call_Center_Fact.csv | Complete prepared call dataset: 500,000 records. |
| Data/File_Inventory.csv | Inventory supplied with the original Report folder. |

## Open and use

1. Download and extract the complete project archive.
2. Open either PBIX in Power BI Desktop. Imported data is already included in the files.
3. Before refreshing, update local data-source paths in Power Query to the extracted files. Files on another computer will not share the original E: drive paths.
4. Open the Excel tools in desktop Excel. Repoint workbook links if Excel prompts for their location. The live staffing tool compares names entered by the user; it is not an automatic contact-center system feed.
5. Read the PDF for the findings, definitions and complete paginated tables.

## Scope and assumptions

The source includes template, simulated and synthetic records. The analysis describes this supplied dataset. A repeat-contact flag is a seven-day proxy and should not be treated as a verified same-issue first-contact-resolution measure.

The monthly KPI weights are AHT 20%, Sales 25%, ACR 10%, Answer rate 10%, Surveys 25%, and Issue resolved 10%. The latest sales target is 55 units per employee per month, with 50 points per unit. Other definitions and target settings are documented in the Power BI reports.

Workforce planning assumes an eight-hour day, including 90 minutes of breaks, and two days off per week. Planning assumptions include 30% additional shrinkage, 80% of calls answered within 20 seconds, ASA 20 seconds, maximum occupancy 85%, and adherence 95%. These are assumptions rather than observed results.

## Power BI Service

- [Call Center Analysis](https://app.powerbi.com/groups/me/reports/48fb7172-667d-4a14-9926-a75813a0aad6?experience=power-bi)
- [Production Target Dashboard](https://app.powerbi.com/groups/me/reports/808891c1-63b9-461c-91b1-0b723a01eb41?experience=power-bi)

Both reports are published in the owner's My workspace. These links require the appropriate Power BI account and access. Scheduled refresh is not configured by this upload; local sources require a supported gateway or a cloud source before automated refresh can run.

All 11 original Report files are preserved. The archive checksum manifest makes it possible to verify that no source file was omitted or changed during packaging.
