# Jazan Smart Parcel Locker Suitability

Finding the best places for parcel lockers in Jazan City, Saudi Arabia, so more deliveries succeed the first time.

An interactive GIS dashboard that ranks candidate locations for smart parcel lockers using a transparent, 9-criteria weighted suitability model (multi-criteria decision analysis, MCDA).

**Live demo:** https://Abutaleb012.github.io/jazan-parcel-locker-suitability/

> Adjust the link above to match your repository name and GitHub username.

---

## Overview

Saudi Arabia is one of the fastest-growing last-mile delivery markets in the region, yet the Southern region, including Jazan, remains under-penetrated, which makes it a strong candidate for new pickup infrastructure. Smart parcel lockers reduce failed first-delivery attempts, lower last-mile cost, and give newly expanding districts a reliable, geocoded collection point. Since January 2026 the National Address is mandatory on all parcel shipments inside the Kingdom, so every locker also becomes a permanent, verified address node.

This project answers a simple question with spatial analysis: where should the first parcel lockers go in Jazan City to maximize population coverage, accessibility, and operational efficiency, while avoiding overlap with existing pickup points.

## What the dashboard does

- Interactive dark-basemap map of candidate locker sites, colored by suitability class.
- A live 9-criteria weighted score for every site, visible in each site's popup.
- Cross-filtering by neighborhood, suitability class, and host type. Map, KPIs, table, and charts all update together.
- Toggleable map layers: candidate sites, 10-minute service areas, and a proxy demand-heat surface.
- Population coverage indicators for 5, 10, and 15 minute catchments.
- A ranked candidate-site table, click a row to fly to the site.
- A distribution chart by suitability class and an incremental coverage curve that shows where added lockers stop adding meaningful coverage.

## The suitability model

Each candidate site receives a score from 0 to 100, computed as a weighted sum of nine normalized criteria:

| # | Criterion | Weight |
|---|---|---|
| 1 | Potential delivery demand (proxy) | 20% |
| 2 | Population / residential density | 15% |
| 3 | Proximity to major roads | 12% |
| 4 | Proximity to commercial and public activity centers | 12% |
| 5 | Coverage of underserved neighborhoods | 10% |
| 6 | Parking and easy vehicle access | 8% |
| 7 | Safety and visibility | 8% |
| 8 | Distance from existing parcel points | 8% |
| 9 | Suitability for 24/7 access | 7% |

```
SuitabilityScore = Σ ( weightᵢ × normalized_criterionᵢ )
```

Sites are classified into four tiers:

| Class | Score | Meaning |
|---|---|---|
| Excellent | 80 to 100 | Priority pilot sites |
| Good | 65 to 79 | Strong secondary sites |
| Moderate | 50 to 64 | Conditional |
| Low | Below 50 | Not a priority for this phase |

Full methodology, datasets, dashboard design, business value, limitations, and the Jazan MVP plan are in [`docs/concept.md`](docs/concept.md).

## Tech stack

- [Leaflet](https://leafletjs.com/) 1.9 for the map
- [Leaflet.heat](https://github.com/Leaflet/Leaflet.heat) for the demand surface
- [Chart.js](https://www.chartjs.org/) 4 for the charts
- CARTO dark basemap tiles
- Vanilla HTML, CSS, and JavaScript, no build step and no dependencies to install

## Data note

The candidate sites, scores, and coverage values in this demo are representative illustrative values, not measured parcel data. Because neighborhood-level parcel volumes are not publicly released, potential demand is modeled from proxy indicators such as building-footprint density, population estimates, commercial POI density, proximity to universities, and urban-expansion zones. For production use, replace the demonstration data with real candidate geometry and a proxy-derived demand surface.

## Run locally

No install required. Either open `index.html` directly in a browser, or serve the folder:

```bash
python -m http.server 8000
# then open http://localhost:8000
```

## Deploy on GitHub Pages

1. Create a new repository, for example `jazan-parcel-locker-suitability`.
2. Add `index.html`, `README.md`, and the `docs/` folder.
3. Commit and push:

```bash
git init
git add .
git commit -m "Jazan smart parcel locker suitability dashboard"
git branch -M main
git remote add origin https://github.com/Abutaleb012/jazan-parcel-locker-suitability.git
git push -u origin main
```

4. In the repository, open **Settings, Pages**, set the source to the `main` branch and the root folder, then save.
5. After a minute the dashboard is live at `https://Abutaleb012.github.io/jazan-parcel-locker-suitability/`.

## Roadmap

- An ArcGIS Online version published as hosted feature layers, reproduced in a Dashboard or Experience Builder app.
- A Python pipeline (OSMnx road network, Google Places POIs, building-footprint density, network service areas) that generates the real candidate dataset and demand surface.
- SPL National Address API integration, so each locker resolves to a verified National Address.
- Expansion of the workflow to other Jazan Region cities.

## Author

**Ali Abutaleb, PhD**
Remote Sensing and Geospatial Intelligence Consultant

## License

Released under the MIT License. See [`LICENSE`](LICENSE).
