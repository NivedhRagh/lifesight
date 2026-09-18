# Northfield & Co. — Revenue & Marketing Dashboard (Power BI)

Analytics case study submission for the Lifesight second-round task.
File: `lifesight1.pbix`

## What this is

A Power BI dashboard built on Northfield & Co.'s daily US e-commerce revenue
and marketing spend data (852 days, 1 Apr 2024 – 31 Jul 2026), exploring what
drives day-to-day revenue and where the business should investigate further.

## How to open it

1. Requires **Power BI Desktop** (free, Windows only — [download here](https://www.microsoft.com/en-us/power-platform/products/power-bi/downloads)).
2. Double-click `lifesight1.pbix`, or open Power BI Desktop → File → Open →
   select the file.
3. All data is embedded in the file itself — no external data source or
   internet connection needed to view it.
4. Use the page tabs at the bottom of the window to move between the three
   report pages.

## Data model

- **Master data** — the source fact table (one row per day): revenue,
  new-customer revenue, five promo/calendar flags, and 13 individual paid
  media spend lines plus the Google/Meta platform roll-ups.
- **DateTable** — a dedicated calendar table (`Year`, `MonthName`, `DayName`,
  `DayNum`, `MonthSort`), related to Master data on Date (many-to-one,
  Master data → DateTable), marked as an official Date table so time
  intelligence works correctly.
- **EventLift** — a small manual lookup table (5 rows) holding the average
  revenue on vs. off for each of the five event flags, used to drive the bar
  chart on "What drives revenue".

## Key measures

| Measure | What it does |
|---|---|
| `Total Revenue` | `SUM('Master data'[Total_Revenue])` — the base revenue figure everything else builds on |
| `Avg Daily Revenue1` | Average (not sum) daily revenue by year — used instead of a yearly total specifically because 2024 and 2026 are partial years in this dataset (Apr–Dec and Jan–Jul respectively); summing would make them look artificially low |
| `Media % of Revenue` | Total paid media spend (all 13 channels, using the Google/Meta roll-ups rather than double-counting components) as a share of Total Revenue — currently ~8% |
| `New Customer %` | `Revenue_New_Customer` as a share of Total Revenue — currently ~32.5% |
| `Returning Customer Revenue` | `Total Revenue − Revenue_New_Customer`, used for the new-vs-returning donut |

> **Naming note:** `Avg Daily Revenue1` has a stray "1" left over from an
> earlier duplicate — cosmetic only, safe to rename to `Avg Daily Revenue` if
> you want to clean it up before presenting.

## Pages

**Page 1 (Overview)**
- Total Revenue and New Customer % cards
- Media % of Revenue card
- Revenue over time (daily line, by Date — can be drilled by year/quarter/month/day)
- Avg Daily Revenue by Year (uses the average, not the total, for the
  partial-year reason above)
- New vs. Returning Customer Revenue donut

**What drives revenue**
- Clustered bar chart: average revenue on vs. off for each of five event
  flags (site-wide promo, UWG mailing, in-store promo, BFCM window, US
  holiday), sourced from the `EventLift` table

**Media Mix**
- Donut chart: all-time spend share across the 13 individual paid media
  channels

## Data checks performed

- 852 consecutive calendar days, no gaps, no duplicate dates, no missing
  values in the source data.
- Google roll-up (`spend_Google`) matches the sum of its five components
  almost exactly; Meta roll-up (`spend_Meta`) matches its three components
  with a small rounding difference, consistent with the source glossary's
  own note. Total media spend uses the roll-ups directly, not re-summed
  components, to avoid double-counting.

## Key findings

- **UWG mailing days show the largest raw revenue lift (~+105%) of the five
  event flags shown**, but the large majority of mailing days also had a
  site-wide promo running the same day — the true standalone mailing effect
  is very likely smaller than this raw comparison suggests. All comparisons
  on the "What drives revenue" page are observed averages, not causal
  estimates; none of the event flags were randomly assigned, so none of
  these differences should be read as proof of impact.
- **BFCM shows the largest lift overall** (BFCM days average roughly 3x a
  typical day), consistent with it being the brand's largest revenue event
  each year.
- **Paid media is only ~8% of total revenue** — most of Northfield's revenue
  is driven by factors other than paid advertising (promotions, mailings,
  organic/CRM demand).
- **Media spend is concentrated in a handful of channels** — Google Brand
  Search, Google PMAX, and Meta ASC together account for the majority of the
  13-channel mix.


## Submission

Either present this `.pbix` file directly in Power BI Desktop, or publish it
to the Power BI Service (Home → Publish) for a hosted link, if a link
submission is preferred over the file itself.
