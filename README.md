# Sales Pipeline Dashboard (Power BI)

A two-page Power BI dashboard that analyses a B2B sales pipeline: how many deals are won, how much revenue they bring in, how long they take to close, and how sales teams compare.

> \*\*Practice project.\*\* Built on a public, fictitious dataset (Maven Analytics, \*CRM Sales Opportunities\*). It is not connected to any employer's data.

The below are the screenshots of the power bi window,it has two pages named as follows.
overview.png
team.png

## Business questions

1. How healthy is the pipeline? (win rate, revenue, open deals, sales cycle)
2. How does performance differ between sales managers, agents and regional offices?

## Data

* **Source:** Maven Analytics Data Playground, *CRM Sales Opportunities* (free download). The data is not included in this repository. See `data/README.md`.
* **Tables:** `sales\_pipeline` (8,800 deals), `accounts`, `products`, `sales\_teams`.
* **Period:** deals engaged between late 2016 and 2017.

## Data model

Star schema with `sales\_pipeline` as the fact table:

|From (many)|To (one)|Column|
|-|-|-|
|sales\_pipeline|accounts|account|
|sales\_pipeline|products|product|
|sales\_pipeline|sales\_teams|sales\_agent|

All relationships are many-to-one with single-direction filtering. All measures sit in a separate `\_Measures` table (see `measures.dax`).

## Key measures

|Measure|Definition|
|-|-|
|Win Rate|Won deals / (Won + Lost)|
|Revenue|Sum of `close\_value` for won deals|
|Avg Deal Size|Revenue / Won deals|
|Open Pipeline|Deals in Prospecting or Engaging|
|Avg Sales Cycle (days)|Average days from engage date to close date, won deals only|

## Key findings

* **4,238 of 6,711 closed deals were won (63.2% win rate)**, generating about **10.0M** in revenue, with an average deal size of about 2,361.
* A won deal takes about **52 days** from first engagement to close.
* **2,089 deals (about 24%) are still open** (1,589 Engaging, 500 Prospecting).
* **Melvin Marxen** leads on revenue (about 2.25M, roughly 22% of the total) and on won deals (882).
* Win rates are close across managers (**62.1% to 64.4%**), so revenue differences come mainly from deal volume and deal size.
* **Dustin Brinkmann** has the second-highest number of won deals (747) but a lower average won deal (about 1,465, against roughly 2,350 to 2,840 for the other managers). The data does not show why.

## Data cleaning log

* Promoted the first row to headers in `sales\_teams` (columns were loaded as Column1 to Column3).
* Found a product name mismatch: `GTX PRO` in `sales\_pipeline` vs `GTX Pro` in `products`, which left 1,480 deals without a product. Standardised the spelling in both tables so every deal links to the products table.



* Open deals (Prospecting, Engaging) have no `close\_date` or `close\_value`. This is expected and was left blank. Lost deals have a `close\_value` of 0.
* Checked totals: deal stages sum to 8,800, and revenue by manager sums to the Overview total.

## Skills shown

Power BI data modelling, Power Query cleaning, DAX measures, KPI design, data validation, and turning data into written findings.

## Repository structure

```
sales-pipeline-dashboard/
  README.md
  measures.dax
  powerbi/
    sales-pipeline-dashboard.pbix
  docs/
    overview.png
    team.png
  data/
    README.md        (download link only)
```

## How to open

1. Download the dataset from Maven Analytics (see `data/README.md`).
2. Open the `.pbix` file in Power BI Desktop (Windows).
3. If prompted, update the file paths under Transform data, Data source settings.

