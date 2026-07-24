# Rent Stabilization NYC

A static, GitHub Pages-ready Manhattan rent-stabilization explorer using the supplied `RentStabilized2025.geojson` without modifying it.

## Included functionality

- Three filterable map categories that recalculate for the selected year:
  - **Currently Rent Stabilized**
  - **Previously Rent Stabilized**
  - **Other Residential Tax Lots**
- Community District and Neighborhood filters; selecting either resets the other.
- Summary statistics and a residential-unit composition chart for the active geography, category filters, and year.
- Clickable tax lots with a 2008–2025 line chart and annual values.
- Address lookup using NYC Geoclient v2, converting the entered Manhattan address to a BBL and selecting the matching lot.
- Time slider from 2008 through 2025.

## Run locally

Browsers will not reliably fetch GeoJSON when opening `index.html` directly. Start a local web server from this folder:

```bash
python -m http.server 8000
```

Then open `http://localhost:8000`.

## NYC Geoclient setup

1. Register at the NYC API Developers Portal.
2. Subscribe to **Geoclient v2**.
3. Paste the subscription key into `config.js`.

Because GitHub Pages is static, anything in `config.js` is visible to visitors. For a private key, route geocoding through a small serverless proxy and restrict its origin to `https://rentstabilization.nyc`.

## GitHub Pages and custom domain

1. Push all files in this folder to the repository root.
2. Enable GitHub Pages from the main branch/root.
3. Add `rentstabilization.nyc` under **Settings → Pages → Custom domain**.
4. Configure the domain DNS records for GitHub Pages and enable HTTPS after DNS resolves.

## Data logic

For selected year `Y`:

- **Currently Rent Stabilized:** `RS_Y > 0`
- **Previously Rent Stabilized:** `RS_Y = 0` and any `RS_2008 ... RS_(Y-1) > 0`
- **Other Residential Tax Lots:** residential by `LandUse` 1–4 or `UnitsRes > 0`, with no RS units from 2008 through `Y`

The source GeoJSON is copied unchanged and all categorization occurs client-side.
