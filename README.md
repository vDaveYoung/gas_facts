# gas_facts
Gas Price Facts

## Locked Interaction Behavior (April 2026)

The Gas Price Timeline interaction model is now aligned with the Oil Production Context chart behavior.

- Hover on a timeline series line: active series is emphasized, non-active series are dimmed.
- Hover on timeline legend item: applies the same active/dim highlight behavior.
- Leave hover state: all series return to base styling.
- Highlight state persists correctly during chart updates (range changes, overlay toggles).

## Main Timeline Overlays

The main timeline supports these overlays:

- Inflation-adjusted gas price estimate
- Presidential term bands
- House/Senate control bands
- U.S. registered vehicles
- DOW annual average
- WTI crude per-gallon equivalent (`WTI / 42`)
- Brent crude per-gallon equivalent (`Brent / 42`)
- Oil production context overlays

## Governance Context Timeline

The governance section on the main page is now a single consolidated timeline instead of three separate charts.

- President, Senate, and House share one aligned time axis.
- The governance timeline follows the currently selected main-chart window.
- Hovering a governance segment highlights the matching office period on the main Gas Price Timeline.
- The standalone [politics.html](politics.html) page remains the full-history view beyond the oil-focused window.

### Crude per-gallon methodology

- Source: FRED daily spot series (`DCOILWTICO` for WTI, `DCOILBRENTEU` for Brent)
- Aggregation: daily values are averaged to weekly buckets
- Conversion: weekly average barrel price is divided by `42` gallons per barrel

