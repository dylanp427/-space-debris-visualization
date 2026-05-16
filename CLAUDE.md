# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a university assignment (FIT2179 Data Visualisation 2) about space debris. It is a static HTML project that renders five Vega/Vega-Lite interactive charts on a space-themed (black background) page. There is no build system, package manager, or test suite.

## Common Commands

- Open the main page: `start "data visualization 2.html"` (Windows)
- Validate Vega/Vega-Lite specs: paste JSON into the [Vega Editor](https://vega.github.io/editor/)
- Serve locally if needed: `python -m http.server 8000` (allows testing external data fetches that may fail with `file://` protocol)

## Architecture

### File Structure
- `data visualization 2.html` — Entry point. Loads Vega libraries from CDN, then `datavisualization2.js`
- `datavisualization2.js` — Embeds charts `Chart1.json` through `Chart5.json` into divs `#c` through `#c5`
- `Chart1.json` – `Chart5.json` — Vega/Vega-Lite specification files (see note below on schema versions)

### Vega vs Vega-Lite Mix
- **Charts 1 and 2** use **Vega** (`$schema: https://vega.github.io/schema/vega/v6.json`). Chart 1 is a line chart with an interactive interpolation dropdown. Chart 2 is a parallel coordinates/nomogram with manually positioned axes.
- **Charts 3, 4, and 5** use **Vega-Lite** (`$schema: https://vega.github.io/schema/vega-lite/v6.json`, except Chart 5 which uses v5). Chart 3 is a choropleth map using an external topojson URL. Chart 4 is a scatter plot using an external CSV from a GitHub raw URL. Chart 5 is an interactive pie chart with a filter dropdown.

### Data Sources
- Chart 1: embedded inline `values` (number of tracked objects by year)
- Chart 2: embedded inline `values` (mass, impact energy, impact angle)
- Chart 3: embedded country debris/population data + external topojson map from `raw.githubusercontent.com/FIT3179/Vega-Lite/main/...`
- Chart 4: external CSV `https://raw.githubusercontent.com/dylanp427/Space-junk/refs/heads/main/satcat.csv` (requires HTTP server or internet)
- Chart 5: embedded inline `values` (breakdown categories with a time filter)

### Styling Notes
- The HTML uses a black background (`#000000`) and non-standard tags `<cat>` (large white headings) and `<dog>` (smaller white subheadings) via inline `<style>`. All chart axes/text colors are explicitly set to white in Chart 2 to match the dark theme.
- Chart 3 (choropleth) uses an orange threshold color scale (`#fdbe85` to `#a63603`).

### Important Implementation Details
- Chart 4 (scatter plot) uses `vega-lite/v6.json` schema with `params` and `transform` for interactivity, filtering `OBJECT_TYPE` to `DEB`, `PAY`, or `R/B`.
- Chart 5 uses a data structure with duplicate categories across two time periods (`zeit`: "All" and "Past 10 years"), filtered by a bound `params` dropdown.
- Chart 2 has a non-standard axis configuration: the `Impact angle` scale uses a manually specified `range` array (not a standard linear range) to position axis values at specific pixel offsets.
