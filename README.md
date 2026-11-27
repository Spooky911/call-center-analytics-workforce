# Call Center Management & Workforce Forecasting

Call Center Management & Workforce Forecasting started with a **50,000-call dataset** and a workforce planning brief for **250 employees**. The dashboards help explain the operation, while the forecasts turn call patterns into staffing requirements and working schedules.

The questions were practical: When do calls arrive? Where are customers being lost? How many people should be available in each half-hour? Can the existing team cover that demand after breaks and days off? And during the day, who should be live but is missing from the actual live list?

The result is two Power BI reports, six Excel workbooks and a written analysis. Each part answers a different question, while the prepared call data provides a common starting point.

## Contents

- [Data and project scope](#from-the-starting-dataset-to-the-reporting-model)
- [How the pieces fit together](#how-the-pieces-fit-together)
- [Excel workbooks and their worksheets](#excel-workbooks-and-why-each-one-exists)
- [The forecast](#forecasting-the-next-quarter)
- [Operational analysis in Power BI](#power-bi-report-1-operational-analysis)
- [Production targets in Power BI](#power-bi-report-2-production-targets)
- [Power BI model features](#power-bi-model-and-calculation-features)
- [The written report](#the-written-report)
- [Files and downloads](#project-files-and-where-to-find-them)
- [Opening and updating the tools](#opening-refreshing-and-updating-the-tools)

## Explore the project

- [Operational analysis on Power BI Service](https://app.powerbi.com/groups/me/reports/48fb7172-667d-4a14-9926-a75813a0aad6?experience=power-bi)
- [Production target dashboard on Power BI Service](https://app.powerbi.com/groups/me/reports/808891c1-63b9-461c-91b1-0b723a01eb41?experience=power-bi)
- [Download the complete project](https://github.com/Spooky911/call-center-management-and-workforce-forecasting/releases/latest/download/Call_Center_Management_and_Workforce_Forecasting.zip)
- [Read the written analysis](Call%20Center%20Analysis.pdf)

Power BI links require the appropriate account and access. Download **Call_Center_Management_and_Workforce_Forecasting.zip** from the release assets to get the complete data and both PBIX files. GitHub's automatically generated **Source code** archives contain the repository files and do not include the large release attachment.

## From the starting dataset to the reporting model

The original ConnectTel CSV contains 50,000 calls. The prepared analytical dataset used by the delivered tools contains **500,000 records for January-March 2025**. Its origin labels explain the difference:

| Origin label in the prepared data | Records | Share |
| --- | ---: | ---: |
| ConnectTel template | 50,000 | 10.0% |
| Simulated source template | 51,708 | 10.3% |
| Synthetic extension | 398,292 | 79.7% |
| Total | 500,000 | 100.0% |

The extra records are simulated/template data and a synthetic extension used in the analytical exercise. The 500,000 records should not be described as 500,000 independently observed customer calls. The forecast for the next quarter is a separate planning output, not another set of historical calls.

**The employee scope also needs a clear distinction.** The original 50,000-call CSV contains 450 distinct raw agent IDs. The prepared reporting model and the schedules use a roster of 250 named employees. The 250-person team is the scope of this project's planning model, rather than a count of unique agent IDs in the untouched source CSV. Unassigned calls remain a separate category.

The release preserves the 11 files that were in the final Report folder. Its data file is the prepared `Data/Call_Center_Fact.csv`. The separate original 50,000-call CSV is not an additional file inside that release.

### What was prepared for analysis

Call timestamps, durations, statuses, agent details and survey responses were put into a consistent call-level structure. Date, month, weekday and half-hour fields were added so the same call could be grouped correctly in Excel and Power BI. Helper fields identify answered, abandoned, missed and resolved calls, valid surveys, positive surveys and handled seconds.

The preparation kept the distinction between an unanswered call, an unresolved answered call and a customer who did not complete a survey. Those categories have different meanings and should not share a denominator just because they appear on the same dashboard.

## How the pieces fit together

| Step | What it produces | Why we needed it |
| --- | --- | --- |
| Prepare the call data | One consistent call-level dataset | Make the reporting totals and definitions agree. |
| Review performance | Power BI analysis and the PDF | Understand demand, waiting, losses, resolution and customer experience. |
| Count half-hour arrivals | Daily interval matrices | Find when coverage is needed instead of staffing from a daily total. |
| Forecast the next quarter | Calls and AHT by weekday and half-hour | Give scheduling a forward-looking demand pattern. |
| Calculate required staffing | Net agents needed in every interval | Translate demand into a service-capacity requirement. |
| Build the roster | Named shifts, breaks and rotating days off | Turn the staffing requirement into a practical plan for the existing team. |
| Calculate scheduled live coverage | Headcount at interval start and average coverage | Check what the roster actually supplies after breaks. |
| Compare a live snapshot | Required, scheduled and actual live names | Make shortages and missing scheduled agents visible during the day. |
| Track monthly targets | Weighted production scorecard | Review the business, a manager, a team or an employee against the same KPI rules. |

## Excel workbooks and why each one exists

### 1. Daily half-hour call counts

**File:** [Daily_Half_Hour_Calls.xlsx](Daily_Half_Hour_Calls.xlsx)  
**Worksheet:** `Calls`

This is the simplest demand view in the project. Each row is a date, each column is a half-hour, and each cell is the number of calls that arrived in that interval. It gives a workforce manager an easy way to compare days and identify busy periods without navigating a dashboard.

It includes all offered calls: answered, abandoned and missed. Counting only answered calls would leave out customers who still created demand. The 90 daily rows reconcile to the prepared dataset's 500,000 records.

The columns run from **08:00-08:30 to 19:30-20:00**, matching the recorded arrival window. An interval includes its start and excludes its end. These are plain worksheet ranges, with no Excel Table or charts.

### 2. The same counts, with the weekday visible

**File:** [Daily_Half_Hour_Calls_With_Day.xlsx](Daily_Half_Hour_Calls_With_Day.xlsx)  
**Worksheet:** `Calls`

This version adds a weekday column beside the date. It makes Monday-to-Monday and weekend comparisons easier to read and provides a useful starting point for the weekly demand profile. The weekday is calculated from the date. It uses the same counts as the first workbook and does not add more calls to the project.

### 3. Required staffing and the weekly capacity gap

**File:** [Interval_Staffing_Plan.xlsx](Interval_Staffing_Plan.xlsx)

| Worksheet | What it does | Why it matters |
| --- | --- | --- |
| `Interval staffing` | Lists date, half-hour, demand type, calls, AHT, required net agents, planned shifts, breaks, effective coverage and the gap. It also provides fields for actual live FTE and actual adherence. | Shows exactly where demand and capacity do not match. |
| `Weekly staffing` | Summarizes required and planned net agent-hours, the gap and a headcount lower bound for each week. | Shows whether a week has enough total capacity, while keeping interval shortages visible in the detailed sheet. |
| `Settings` | Records the service targets, shrinkage, shift rules, forecast method and model limitations. | Makes the assumptions behind the saved planning run explicit. |

There are **4,344 half-hour planning rows**: 181 days from 1 January to 30 June 2025, with 24 intervals per day. January-March uses historical demand from the prepared dataset. April-June uses the forecast described below.

**How required agents were calculated.** For each half-hour, offered calls multiplied by AHT gives the workload. An Erlang C calculation finds the smallest net agent count meeting the SLA, ASA and occupancy assumptions. Additional shrinkage increases the amount of available queue coverage that must be scheduled. Explicitly scheduled breaks are already removed from roster coverage and are not deducted twice.

| Planning input | Assumption used |
| --- | --- |
| Existing employee pool | 250 employees |
| Service level | 80% answered within 20 seconds |
| ASA | At most 20 seconds |
| Maximum occupancy | 85% |
| Other shrinkage | 30%, in addition to the explicitly scheduled breaks |
| Schedule adherence target | 95%, tracked as a target rather than another shrinkage multiplier |
| Skill coverage | One pooled queue, with employees treated as interchangeable |
| Abandonment target | Not supplied |

Historical abandonment is retained for context. Erlang C assumes callers wait rather than abandon, so this model does not predict an abandonment rate. It also does not invent a patience distribution. If the available model agents cannot support a stable queue, finite SLA and ASA results are shown as unavailable.

The saved SLA, ASA, occupancy results, staffing requirements and roster are outputs of the planning run. Editing a Settings cell does not rerun the queueing calculation or rebuild the roster. The directly linked actual-gap and adherence comparison formulas respond to their inputs. A revised forecast or set of targets requires the planning run to be regenerated.

### 4. Weekly employee schedules

**File:** [Weekly_Employee_Schedules_Colored.xlsx](Weekly_Employee_Schedules_Colored.xlsx)

This workbook turns the capacity plan into a named roster. Each weekly tab lists all 250 employees and shows their shift, lunch, short breaks and days off for each date. We kept the existing 250-person pool and reported the remaining shortage instead of adding fictional employees to make the plan appear fully staffed.

The shift choices are **08:00-16:00, 09:00-17:00, 10:00-18:00, 11:00-19:00 and 12:00-20:00**. Simple colors distinguish the five shifts, and grey marks days off. The same legend is shown on each weekly tab.

The roster follows these rules:

- Five working days and two days off in every complete roster week.
- The pair of days off changes each week.
- No more than six consecutive working days across week boundaries.
- An eight-hour shift containing 6.5 hours of queue time and 90 minutes of breaks.
- One 60-minute lunch, one 15-minute break and a second 15-minute break. The 10-minute restroom allowance sits inside the second break and is deducted once.

The assignment method tries to reduce interval shortages while respecting these rules and balancing early and late shifts. It is a coverage-matching heuristic, not a claim that the roster is a mathematically optimal or fully sufficient staffing solution.

There are **26 weekly tabs**, starting on Wednesday 1 January. The requested planning period ends on 30 June. The final tab includes **1 July as a clearly marked week-completion day** so the last roster week still has five workdays and two days off. Staffing demand and the planning summary stop at 30 June.

| Week | Worksheet | Dates covered |
| --- | --- | --- |
| 01 | `W01 01-01` | 01 Jan - 07 Jan 2025 |
| 02 | `W02 01-08` | 08 Jan - 14 Jan 2025 |
| 03 | `W03 01-15` | 15 Jan - 21 Jan 2025 |
| 04 | `W04 01-22` | 22 Jan - 28 Jan 2025 |
| 05 | `W05 01-29` | 29 Jan - 04 Feb 2025 |
| 06 | `W06 02-05` | 05 Feb - 11 Feb 2025 |
| 07 | `W07 02-12` | 12 Feb - 18 Feb 2025 |
| 08 | `W08 02-19` | 19 Feb - 25 Feb 2025 |
| 09 | `W09 02-26` | 26 Feb - 04 Mar 2025 |
| 10 | `W10 03-05` | 05 Mar - 11 Mar 2025 |
| 11 | `W11 03-12` | 12 Mar - 18 Mar 2025 |
| 12 | `W12 03-19` | 19 Mar - 25 Mar 2025 |
| 13 | `W13 03-26` | 26 Mar - 01 Apr 2025 |
| 14 | `W14 04-02` | 02 Apr - 08 Apr 2025 |
| 15 | `W15 04-09` | 09 Apr - 15 Apr 2025 |
| 16 | `W16 04-16` | 16 Apr - 22 Apr 2025 |
| 17 | `W17 04-23` | 23 Apr - 29 Apr 2025 |
| 18 | `W18 04-30` | 30 Apr - 06 May 2025 |
| 19 | `W19 05-07` | 07 May - 13 May 2025 |
| 20 | `W20 05-14` | 14 May - 20 May 2025 |
| 21 | `W21 05-21` | 21 May - 27 May 2025 |
| 22 | `W22 05-28` | 28 May - 03 Jun 2025 |
| 23 | `W23 06-04` | 04 Jun - 10 Jun 2025 |
| 24 | `W24 06-11` | 11 Jun - 17 Jun 2025 |
| 25 | `W25 06-18` | 18 Jun - 24 Jun 2025 |
| 26 | `W26 06-25` | 25 Jun - 01 Jul 2025 |

The January-March schedule is a proposed retrospective plan against historical demand. It is not an attendance record or evidence that those employees actually worked those shifts.

### 5. How many agents the schedule puts live

**File:** [Scheduled_Live_Agents.xlsx](Scheduled_Live_Agents.xlsx)

| Worksheet | What it shows | Why there are two views |
| --- | --- | --- |
| `Live at interval start` | The number of people scheduled to be on queue at the beginning of each half-hour. | Useful for a point-in-time comparison with a live-name snapshot. |
| `Average live coverage` | On-queue agent-minutes during the full interval, divided by 30. | Accounts for someone taking a break or starting partway through the interval. |

For example, someone available for only 15 minutes supplies 0.5 of a full half-hour of coverage. That is different from an integer count of people available at the interval's start. Days off, lunch and short breaks are excluded. The additional 30% shrinkage is not applied in this workbook, so scheduled coverage stays distinct from expected effective capacity.

These are saved coverage outputs from the roster. Editing a separate weekly schedule file does not automatically rebuild them.

### 6. Comparing required, scheduled and actual live staff

**File:** [Real_Time_Staffing_Tool.xlsx](Real_Time_Staffing_Tool.xlsx)

This is the operational control tool. The user chooses a date and half-hour, then pastes the names currently live, one name per row. Excel compares that snapshot with the required staffing and the employees expected to be on queue at the start of the selected interval.

| Worksheet | Role in the tool | Why it is separate |
| --- | --- | --- |
| `Live control` | Contains the date and interval selectors, live-name entry area, headcount comparisons, pie chart and missing/extra agent lists. | Gives the person running the floor one place to work. |
| `Agent status` | Reads each employee's schedule, determines their state and matches it against the pasted names. | Shows the calculation behind Live, OFF, Not started, Finished, Break 1, Lunch and Break 2. It is not a second place to enter live names. |
| `Required staffing` | Holds the saved date/interval requirements copied from the staffing plan. | Provides the service-capacity benchmark. It uses required net agents, without adding the shrinkage uplift again. |
| `Schedule` | Holds the 250 named employees and their schedule across 182 roster dates. | Provides the expected attendance and break timings for the comparison. |

The main outputs are required live agents, scheduled live agents, actual unique known agents, actual as a percentage of scheduled and required, the net gap, scheduled agents confirmed live, scheduled agents missing, and live agents outside their scheduled live state. The pie chart shows how much of the scheduled live group is confirmed in the entered list.

Name matching ignores letter case, extra spaces and nonbreaking spaces. Duplicate names are counted once. Unknown names are flagged for correction and excluded from the actual known-agent count. The entry area supports 300 rows.

An empty input list means **no snapshot has been supplied**. A separate confirmation is needed to record a verified zero-live snapshot. After changing the date or interval, replace the pasted list with the snapshot for that selection.

The tool recalculates from its internal `Required staffing` and `Schedule` sheets. Those are source copies, not an automatic connection to the other workbooks. Changes inside the tool update the comparisons. Changes to a separate source workbook must be copied or imported into the tool first. The 1 July roster-completion day has no demand requirement and is treated as unavailable rather than zero.

This is a manually supplied live comparison. It does not connect to a telephony platform, monitor agents continuously or measure full-day schedule adherence.

## Forecasting the next quarter

The forecasting task was to extend the original three-month analysis into another three months of planning. We used **March's average arrivals for each weekday and half-hour** as the baseline for April-June. A future Monday at 10:00 is therefore based on the March Monday 10:00 pattern. Forecast calls are rounded to whole calls, and AHT uses answered-call-weighted talk plus after-call work for the same weekday/interval group.

No extra growth factor was applied. The saved forecast contains **545,194 offered calls for April-June 2025**. This is a seasonal baseline for planning. It is not a trained machine-learning model, and no out-of-sample forecast accuracy or prediction interval is claimed.

The forecast feeds the staffing calculation, which feeds the proposed roster and the coverage views. The Power BI six-month chart serves a different purpose: it displays KPI attainment history on a six-month axis. **It does not turn the April-June staffing forecast into actual employee KPI results.**

## Power BI report 1: operational analysis

**File:** `Call Center Analysis.pbix`  
**Service:** [Open Call Center Analysis](https://app.powerbi.com/groups/me/reports/48fb7172-667d-4a14-9926-a75813a0aad6?experience=power-bi)

This report answers what happened in the prepared call data and where to investigate. Its nine pages move from the overall result to interval, queue, issue and employee detail.

| Page | Main features | Why it was built |
| --- | --- | --- |
| `Overview` | Cards for offered calls, answer and abandonment rates, AHT, wait and CSAT. Monthly call outcomes, response-time trends by channel and a talk/after-call-work split. | Put demand, access, handling and customer experience together without confusing waiting time with handling time. |
| `Demand & intervals` | Active half-hour arrivals, weekday demand, daily-volume cards and the complete interval-count table. | Support decisions about shift starts and break coverage during the actual arrival window. |
| `Customer experience` | Survey and resolution measures, issue comparisons and the issue-level detail table. | Find the types of contact associated with a poorer resolution or rating result. |
| `Teams & agents` | Manager/channel comparisons and the named-employee results table. | Understand workload and performance in context, with access to the individual records behind the aggregates. |
| `Data quality` | Source origins, survey coverage, incomplete ACR windows and checks for duplicate IDs, durations and waiting-time consistency. | Explain which records and definitions support the results. |
| `Queue pressure` | Abandonment by queue size, survey results by wait group and all queue-band rows. | Show how much loss sits behind a short overall answered-wait average. |
| `Resolution & repeats` | Recorded resolution, repeat-contact comparisons and issue-level results. | Separate an issue marked resolved from a customer having no reason to call again. |
| `Sales & CX` | Sales conversion and units by channel, with comparisons of handling time and customer ratings. | Check whether apparent sales differences still hold when the resolution result is comparable. |
| `Loss hotspots` | Interval demand, abandonment rates and a risk-ranked interval table. | Distinguish the busiest half-hour from the half-hour with the highest loss rate. |

The presentation uses consistent units, quieter colors, lighter text and aligned cards. Half-hour charts start when calls start rather than filling the axis with overnight zeroes. The handling chart separates talk from after-call work, which explains how AHT is spent. Tables provide the underlying detail when a chart alone is not enough.

### Findings that shaped the next steps

- The prepared quarter contains 500,000 offered calls, an 84.2% answer rate and a 13.5% abandonment rate. AHT averages 8.49 minutes.
- March brought 15.0% more calls than January, while mean answered wait rose from 10.64 to 12.79 seconds.
- The busiest half-hour is 14:30-15:00. The highest abandonment rate is later, at 16:00-16:30.
- Calls arriving with at least 11 already in the queue account for 12.4% of arrivals and 65.4% of abandoned calls.
- Network issues have the largest unresolved-call count. Their resolution rate is 85.7%.
- The apparent positive-survey advantage for calls with a sale shrinks to 0.03 percentage points when both groups are limited to resolved calls. This does not establish a causal benefit from selling.

These observations explain why the project went beyond a monthly dashboard into interval planning, queue review and more careful comparisons.

## Power BI report 2: production targets

**File:** `Production Target Dashboard.pbix`  
**Service:** [Open Production Target Dashboard](https://app.powerbi.com/groups/me/reports/808891c1-63b9-461c-91b1-0b723a01eb41?experience=power-bi)

The operational report explains the call center. The production report answers how the business or a selected team is performing against agreed planning targets.

| Page | Features | Purpose |
| --- | --- | --- |
| `Production target dashboard` | Four top selectors, overall weighted score, sales points, scope headcount, six KPI cards, earned/remaining contribution columns, a six-month line chart and a KPI scorecard. | Review the selected month's result and see which KPIs account for the score or shortfall. |
| `Employee performance report` | Monthly table with manager, team leader, employee, score, AHT, sales and the four rate KPIs. | Follow a management result down to individual employee results using the same definitions. |
| `Targets and definitions` | Weights, current targets, scoring rules and metric definitions. | Make the score explainable and keep its assumptions visible. |

### Why the filters work this way

The four top selectors are **Month, Manager, Team leader and Agent**. Select one month to calculate the monthly score. With the people filters cleared, the scope is the whole prepared operation. Selecting a manager narrows it to that manager's employees. Selecting a team leader or employee narrows it further. Combined selections apply together.

Sales targets scale with the number of named employees in the selected scope: one employee has a target of 55 units, and a scope of 10 employees has a target of 550. At the full 250-person scope, the monthly target is 13,750 units. The model does not assume an additional adjustment for part-time employment or joiners/leavers.

The **Trend KPIs** selector is separate from those four filters. It can show one or several KPI lines. The trend retains the selected people scope while showing January-June, independently of the single month used by the scorecards. Months without actual KPI data remain blank. A common 100% target line makes metrics measured in minutes, units and percentages comparable as attainment.

### Current KPI rules

| KPI | Weight | Current target | Calculation and purpose |
| --- | ---: | --- | --- |
| AHT | 20% | About 7.89 minutes | Answered talk plus after-call work, divided by answered calls. The target is 7% below the fixed January-March production baseline of about 8.49 minutes. Lower is better. |
| Sales | 25% | 55 units per employee per month | Units sold compared with scope headcount times 55. Each unit is worth 50 sales points, so one employee's monthly target is 2,750 sales points. Sales points are distinct from the weighted score out of 100. |
| ACR / no-repeat rate | 10% | 75% | Answered calls with source flag 0 divided by answered calls with flag 0 or 1. Incomplete seven-day windows are excluded. |
| Answer rate | 10% | 87% | Answered calls divided by offered calls in the selected scope. |
| Positive surveys | 25% | 91% | Scores 4 and 5 divided by valid numeric survey replies. Score 3 is neutral and 0-2 are negative. Not surveyed is excluded. |
| Issue resolved | 10% | 94% | Resolved answered calls divided by eligible answered calls, excluding explicitly recorded escalations. No escalated status is present in the current source. |

The original brief used easier targets, including five sales units per employee. The final version uses 55 sales units and the revised rate targets above so the targets sit closer to, or stretch beyond, the prepared historical baseline. The weights stayed the same.

For AHT, attainment is **target / actual**. For the other KPIs, it is **actual / target**. Each weighted contribution is capped at 100% attainment before the six contributions are summed. A strong result in one KPI therefore cannot earn unlimited points and hide another KPI's shortfall. The attainment cards and trend can still show a result above 100%. Missing KPI data is not silently treated as a zero score.

The contribution visual is a **stacked column chart** with earned and remaining weighted points. The time-series visual is a **line chart**. Rates for managers and teams are recalculated from their call or survey totals rather than obtained by taking a simple average of employee percentages.

## Power BI model and calculation features

| Feature | Why it was used |
| --- | --- |
| Power Query preparation and typed call fields | Keep dates, times, durations, IDs and survey values consistent before aggregation. |
| Call fact with Calendar, Intervals and CSAT score dimensions | Group the same records by reporting date, half-hour and valid survey score without rebuilding the logic in every visual. |
| Agents dimension with Manager and Team Leader attributes | Support management-to-team-to-employee filtering and count the employees in scope. |
| DAX measures for counts, weighted AHT, rates, sales, workload and quality checks | Keep each calculation and denominator consistent across cards, tables and charts. |
| Settings and KPI weights | Separate the scoring assumptions from the displayed result. |
| Separate Month selector and Trend months tables | Require one month for the scorecard while allowing the trend to keep its six-month context. |
| KPI picker and Trend series | Allow selected metric lines to share a normalized attainment axis and a target line. |
| No-data handling | Distinguish a future month without actuals, an incomplete observation window and a real zero. |
| Cards, columns, lines, bars and detail tables | Use a clear summary for the headline, a time axis for trends, categories for comparisons and tables for full detail. |
| Power BI Service publication | Make both reports available online in the owner's workspace with the imported data. |

The source places all 67,325 abandoned calls in Unassigned. Named managers and employees therefore do not inherit those queue losses. Their scoped answer rates should not be read as evidence that they caused or avoided abandonment. Workload hours also represent handled work, not actual login hours, occupancy or adherence.

The source's seven-day ACR flag closely matches a later call by the same Customer ID, but many later calls have a different issue label. It is a repeat-contact proxy, not a verified same-issue first-contact-resolution measure.

## The written report

**File:** [Call Center Analysis.pdf](Call%20Center%20Analysis.pdf)

The 26-page PDF gives the reader a complete explanation outside Power BI. It covers demand, queues, waiting, issue resolution, repeat contact, sales, customer ratings and the recommended checks. It includes a clickable contents page, consistent definitions and the complete lists behind the report tables.

Every scrolling table is laid out as full rows across readable pages. The appendix includes all 24 active intervals, all 10 issue types, the channel and weekday categories, the survey/source categories, and all 250 named employees plus Unassigned. Column headings repeat when a table continues onto another page. It is a standalone written analysis rather than a set of screenshots of partially visible visuals.

## Project files and where to find them

| File in the complete archive | Role | Availability |
| --- | --- | --- |
| `Report/Call Center Analysis.pbix` | Nine-page operational analysis | Release archive |
| `Report/Production Target Dashboard.pbix` | Three-page monthly production scorecard | Release archive |
| `Report/Call Center Analysis.pdf` | Complete written analysis | Repository and release archive |
| `Report/Daily_Half_Hour_Calls.xlsx` | Daily interval counts | Repository and release archive |
| `Report/Daily_Half_Hour_Calls_With_Day.xlsx` | The same demand matrix with weekday names | Repository and release archive |
| `Report/Interval_Staffing_Plan.xlsx` | Demand, required staffing and capacity gaps | Repository and release archive |
| `Report/Weekly_Employee_Schedules_Colored.xlsx` | 26 weekly rosters | Repository and release archive |
| `Report/Scheduled_Live_Agents.xlsx` | Interval-start headcount and average live coverage | Repository and release archive |
| `Report/Real_Time_Staffing_Tool.xlsx` | Required/scheduled/actual live comparison | Repository and release archive |
| `Report/Data/Call_Center_Fact.csv` | Complete prepared 500,000-record call dataset | Release archive |
| `Report/Data/File_Inventory.csv` | Inventory supplied with the Report folder | Repository as `File_Inventory.csv`, and release archive |
| `FILE_MANIFEST.csv` | Original Report file sizes and SHA-256 checksums | Repository and release archive |

The Excel files and PDF are directly accessible at the repository root. The complete archive preserves the original Report/Data folder structure and contains all 11 original Report files unchanged. The large PBIX and CSV files are inside that archive rather than stored in Git LFS. The README on `main` is the current project guide. The README inside the v1.0.0 archive is the guide saved with that release.

## Opening, refreshing and updating the tools

1. Download and extract `Call_Center_Management_and_Workforce_Forecasting.zip`.
2. Open the PBIX files in Power BI Desktop. Their imported data is already included. Before refreshing, point Power Query to the extracted data files instead of the original computer's local paths.
3. Open the workbooks in desktop Excel. The interval matrices and roster outputs are saved snapshots. Rebuild the planning outputs when demand or planning assumptions change.
4. In the live tool, keep the internal `Required staffing` and `Schedule` copies current, select the date/interval and paste a matching live-name snapshot. Excel recalculates those comparisons.
5. Use the online Power BI reports for interactive review and the PDF for the full written explanation.

Both Power BI reports are published in **My workspace**. Publication does not grant other people access automatically. Scheduled refresh has not been configured. Refreshing local data through the Service needs a supported gateway or a suitable cloud source.

## Checks made before delivery

The daily interval counts were reconciled to the 500,000 prepared calls. The roster checks covered 250 named employees in every weekly tab, five workdays and two rotating days off, the cross-week workday limit and the daily break budget. Interval coverage was reconciled across the 4,344 requested half-hours. The live tool was checked with duplicate and unknown names, missing and extra employees, break boundaries, invalid selections and an explicit zero-live snapshot.

The Power BI reports were reopened after publication and the month selector was tested. The full project archive was checked against the original files, and its published SHA-256 digest matched the local archive. These checks support the delivered exercise. Real operating decisions still need verified call data, actual agent-state records, skill eligibility and a review of the planning assumptions.
