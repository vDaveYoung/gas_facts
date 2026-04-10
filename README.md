# gas_facts

Gas Price Facts

## Refinery Capacity & Output Tracker (April 2026)

The **Refinery Capacity & Output** section tracks supply-chain context between crude oil production and pump prices.

### What is tracked
- **U.S. operable refinery capacity** — weekly TBPD via EIA `petroleum/pnp/wiup` series `WOCLEUS2`
- **U.S. gross refinery input** — weekly TBPD via EIA `petroleum/pnp/wiup` series `WGIRIUS2`
- **U.S. utilization rate** — weekly % via EIA `petroleum/pnp/wiup` series `WPULEUS3`
- **World refined products output** — annual TBPD via EIA International (`productId=54`, `activityId=1`, `countryRegionId=WORL`, `unit=TBPD`)
- **World refinery capacity** — optional best-effort annual series via `productId=104` (often sparse)

### Layout
- Refinery context now renders as **one combined full-width chart** (same large-panel sizing as the other major context charts).
- Combined view includes:
	- **U.S. capacity + input** (TBPD)
	- **U.S. utilization** (%)
	- **World refinery series** (annual TBPD)
- Timeline window is explicitly pinned to the selected main date range (`min/max`) so refinery context moves in lockstep with the rest of the dashboard.

### Controls
Use **Market Overlays → "U.S. & World refinery capacity / utilization tracker"** to show/hide refinery context.

- Toggle now updates both:
	- the dedicated refinery context chart, and
	- refinery overlays on the main timeline chart.

### Interaction behavior
- Refinery chart now follows the same hover/legend highlight pattern used elsewhere:
	- active series is emphasized,
	- non-active series are dimmed,
	- leaving hover restores base styling.

### Axis and sizing notes
- Secondary right-side axes were hidden where appropriate to reduce right-margin expansion and keep chart plotting areas visually consistent.
- Main and refinery large charts were normalized to matching height profiles.

### Local development notes
- FRED CSV overlays (DJIA/WTI/Brent) are skipped on localhost to avoid browser CORS noise.
- This does not affect EIA refinery data loading.

### Current troubleshooting checkpoint
- API endpoints were corrected from invalid `refcap/sum/snd` routes to working `pnp/wiup` routes.
- Combined refinery status text now reports latest U.S. and world refinery values to verify load state quickly.

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

