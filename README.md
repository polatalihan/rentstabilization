# Rent Stabilization NYC — Vector Tile Edition

This GitHub Pages package uses MapLibre GL JS and pre-generated Mapbox Vector Tiles instead of loading the 42 MB source GeoJSON in the browser.

## Publish

Upload the complete contents of this folder to the root of the GitHub repository and enable GitHub Pages. Keep the `tiles` folder structure unchanged.

No file in the package exceeds GitHub's 25 MB browser-upload limit. The largest file is `lots.json`, a geometry-free index used for totals, filters, charts, and BBL lookup.

## Data architecture

- `tiles/{z}/{x}/{y}.pbf`: vector tiles, zooms 11–15; MapLibre overzooms them at closer scales.
- `lots.json`: tax-lot attributes and representative coordinates, without polygon geometry.
- `RentStabilized2025.geojson`: not included and not modified.

## Geocoding

`config.js` contains the NYC Geoclient subscription key. A static GitHub Pages site exposes client-side keys, so apply any available origin/domain restrictions or use a serverless proxy before a high-profile public launch.

## Local testing

Do not open `index.html` directly. From this folder run a local HTTP server, for example:

```bash
python -m http.server 8000
```

Then open `http://localhost:8000`.

## Important fixes in this build

- Vector tiles are requested with repository-relative URLs (`./tiles/...`) so the site works both at a GitHub project URL and at a root custom domain.
- Geoclient authentication is sent as the `subscription-key` query parameter to avoid browser CORS preflight problems.
- A 401 still means the key is not currently authorized for the **Geoclient V2 User** product; sign in to the NYC API portal, subscribe that product, and use the primary key from that subscription.
- GitHub must contain the complete nested `tiles` directory. A typical deployed tile should be reachable at a URL such as `tiles/11/603/769.pbf` relative to the site root.
