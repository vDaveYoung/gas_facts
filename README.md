# gas_facts

Gas Price Facts

## Refinery Capacity & Output Tracker (April 2026)

A new **Refinery Capacity & Output** section tracks supply-chain context between crude oil production and pump prices.

### What's tracked
- **U.S. operable refinery capacity** — annual TBPD from EIA `petroleum/refcap` (crude distillation capacity)
- **U.S. gross refinery input** — weekly TBPD from EIA `petroleum/sum/snd` (series `WCRRIUS2`)
- **U.S. utilization rate** — weekly % from EIA `petroleum/sum/snd` (series `WPULEUS`); right axis scaled 60–100 %
- **World total distillation capacity** — annual TBPD from EIA International (`productId=104`, region `WORL`)

### Layout
Two side-by-side Chart.js panels (stacks to single column below 900 px):
- **Left** — U.S. operations: capacity + gross input on left axis (TBPD), utilization on right axis (%)
- **Right** — World capacity: single area-fill line

### Controls
Toggle in **Market Overlays → "U.S. & World refinery capacity / utilization tracker"**. Section is hidden until the toggle is enabled. The date-range window slider applies to both charts. The section supports the standard collapse button.

### Resilience
All four EIA series are fetched in parallel. Any individual fetch failure silently returns an empty array; the rest of the dashboard continues to load normally.

---

## Conflict Timeline & Political Context (April 2026 Update)

### Conflict Timeline Enhancements
- Conflict strip is horizontally aligned with the main chart plot area using proportional padding (not raw pixels), preventing responsive layout collapse.
- Governance overlays (presidential terms, congressional control) are restricted to the main chart only; the conflict strip now shows conflict bands exclusively.
- Conflict table includes 5 columns: **Conflict**, **Range**, **Participants/Relationship**, **Casualties (Est.)**, **Deaths (Est.)**.
- Toggle between **Summary** and **Detailed** views — detailed mode shows civilian/combatant/U.S. loss breakdowns.

### Political Context — Election Vote Summary
- A new **Election Results** panel under Political Context displays popular vote percentages and electoral vote totals per election.
- Panel is **window-aware**: only shows elections that fall within the currently selected date range.
- **Historical toggle** switches between *Modern (1980+)* and *Expanded History (1948+)* views.

### Window Controls Reorganization
Controls are now organized into **5 semantic grouped cards**:
1. **Range** (full-width) — presets, slider, date display
2. **Conflict** — war toggle, US/oil filters, duration, summary/detailed view
3. **Political Context** — presidential/congressional overlays + election vote table
4. **Market Overlays** — buying power, vehicles, DOW, WTI/Brent, production
5. **News & Axis** — headline toggle, news mode, date axis detail

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

### Governance rendering stability notes

- The main-page governance timeline is rendered as DOM/CSS lanes (not canvas), with percentage-based segment positioning.
- The governance panel uses fixed-height lane rows to avoid responsive canvas sizing drift and overflow artifacts.
- Segment hover updates style state in place (no full timeline re-render on pointer movement), reducing flicker and layout churn.

## Local Development Cache Behavior

- On localhost (127.0.0.1/localhost), the app now unregisters service workers and clears cache storage on load.
- In non-local environments, service worker registration remains enabled.
- Service worker fetch strategy uses network-first for HTML/document requests to prevent stale UI code from persisting after edits.

### Crude per-gallon methodology

- Source: FRED daily spot series (`DCOILWTICO` for WTI, `DCOILBRENTEU` for Brent)
- Aggregation: daily values are averaged to weekly buckets
- Conversion: weekly average barrel price is divided by `42` gallons per barrel

