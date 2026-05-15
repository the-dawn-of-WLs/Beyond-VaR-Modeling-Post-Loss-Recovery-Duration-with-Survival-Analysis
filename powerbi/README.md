# Power BI Risk Status Dashboard

This folder contains the exported Power BI Service dashboard artifacts for the Beyond VaR project.

## Files

- `beyond_var_risk_status_dashboard_page1.pdf` - exported Page 1 report from Power BI Service.
- `beyond_var_risk_status_dashboard_page1.png` - preview image used in the main README.
- `../data/powerbi/vw_powerbi_quickcreate_baseline_95_250_clean.csv` - cleaned single-table CSV used for the quick-create Power BI report.

## Dashboard Scope

The Power BI page is an executive summary for the 95% confidence, 250-day rolling-window VaR baseline. It is intentionally built from one flat table so it can be imported directly into Power BI Service without rebuilding the full star schema.

## Field Mapping

| Dashboard element | Power BI field | Aggregation or setting |
| --- | --- | --- |
| Ticker slicer | `Ticker` | Dropdown |
| Method slicer | `MethodName` | Vertical list |
| Year slicer | `Year` | Tile slicer |
| Total breaches KPI | `BreachCount` | Sum |
| Average VaR KPI | `VaR_pct` | Average |
| Average severity KPI | `SeverityForVisual` | Average |
| Average daily return KPI | `DailyReturn` | Average |
| BreachCount by Ticker | `Ticker`, `BreachCount` | Axis = Ticker, Values = Sum of BreachCount |
| VaR Breach Analysis table | `MethodName`, `VaR_pct`, `SeverityForVisual`, `BreachCount` | Average VaR, average severity, sum breaches |
| BreachCount by Year and Quarter | `Date` hierarchy or `Year`/`Quarter`, `BreachCount` | Values = Sum of BreachCount |
| Daily Return and VaR Forecast | `Date`, `DailyReturn`, `VaR_pct` | X-axis = Date, Values = Average DailyReturn and Average VaR_pct |
| Breach Flow | `Ticker`, `MethodName`, `Year`, `BreachCount` | Decomposition tree: Analyze = Sum of BreachCount, Explain by = Ticker, MethodName, Year |
| Breach Share by Method | `MethodName`, `BreachCount` | Legend = MethodName, Values = Sum of BreachCount |

## Aggregation Notes

- Use `Sum` for `BreachCount`; it is a row-level 0/1 event counter.
- Use `Average` for `VaR_pct` and `DailyReturn`; summing returns or VaR values would be misleading.
- Use `Average` for `SeverityForVisual`; it is a display-safe severity value.
- Use `Average` for `BreachFlag` only when calculating an exceedance rate.
- Use `RecoveryDaysForVisual` with `Median` when adding a recovery-duration KPI.

## Current Visual Design

- Canvas background: dark gray risk-monitoring surface.
- Card and chart panels: light gray or white with rounded corners.
- Primary color: navy for bars and the daily-return line.
- Secondary colors: green for VaR, teal and purple for method categories.
- Font: Segoe UI or Power BI default, with bold visual titles.
