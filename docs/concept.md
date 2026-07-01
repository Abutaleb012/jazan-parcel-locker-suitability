# Jazan Smart Parcel Locker Suitability Map
### A GIS-Based Site Suitability and Last-Mile Logistics Project Concept
**Prepared for:** Jazan City, Jazan Region, Saudi Arabia
**Discipline:** Geospatial Intelligence, Multi-Criteria Site Suitability, Urban Logistics
**Platform:** ArcGIS Pro and ArcGIS Online (Dashboards / Experience Builder)

---

## 1. Project Background

Saudi Arabia is one of the fastest-moving last-mile markets in the region. National e-commerce reached roughly 118 million orders in the first quarter of 2026, up about 49 percent year on year, and Vision 2030 has committed on the order of SAR 280 billion to transport and logistics corridors, digital customs, and special economic zones. Digital payments now dominate transactions, and quick-commerce expectations of same-day and even sub-hour delivery are spreading from Riyadh, Jeddah, and Dammam into secondary cities.

The critical detail for Jazan is that the Southern region remains one of the least penetrated parts of this market, accounting for less than 10 percent of national e-commerce volume. This is not a weakness, it is headroom. As delivery volumes migrate outward from tier-1 cities, Jazan sits directly in the path of that growth, and the infrastructure that gets built now will define service quality for the next decade.

Three structural pressures make smart parcel lockers particularly relevant to Jazan City today:

**Last-mile inefficiency and failed deliveries.** The single largest cost driver in last-mile logistics is the failed first delivery attempt, where the courier arrives and no one is available to receive the parcel. Each failed attempt forces a redelivery, a return to a branch, or a customer service call, multiplying cost and delay. Parcel lockers convert an uncertain doorstep handoff into a scheduled, self-service pickup available around the clock, removing the recipient-availability problem entirely.

**Urban expansion and addressing gaps.** Jazan City is expanding, and newly developed districts are exactly where descriptive addressing breaks down and where couriers waste the most time locating buildings. A well-placed locker network gives these growth areas a reliable, geocoded collection point even before doorstep addressing matures.

**The National Address mandate.** As of January 1, 2026, the National Address (the 4-letter, 4-number short address, for example EMDA5321) is mandatory on all parcel shipments inside the Kingdom under a General Transport Authority decision, and SPL exposes a free National Address API for developers. Every locker in this project becomes a permanent, verified National Address node, which directly supports compliance and gives operators a fixed, machine-readable destination that never suffers from an incomplete customer address.

There is also a clear incumbent to design around rather than ignore. SPL already operates a Parcel@t smart locker network, on the order of 62 stations across 146 cities nationwide, accessible 24/7 without staff. A Jazan-specific suitability model should therefore treat existing SPL, Naqel, and private courier pickup points as an input layer, so that new sites fill genuine coverage gaps instead of cannibalizing what already exists.

---

## 2. Main Objective

To identify and rank the most suitable locations for smart parcel lockers in Jazan City using a transparent, weighted, GIS-based multi-criteria suitability model, so that locker placement maximizes population coverage, accessibility, operational efficiency, and service convenience, while minimizing overlap with existing pickup infrastructure.

The deliverable is a defensible shortlist of pilot sites, each carrying a suitability score from 0 to 100, a service-area footprint, and the underlying evidence for why it scored the way it did.

---

## 3. Key Research Questions

1. Where are the most suitable locations for smart parcel lockers across Jazan City, ranked by a composite suitability score?
2. Which neighborhoods carry the highest **potential** delivery demand, estimated from proxy indicators where real parcel records are unavailable?
3. Which candidate sites deliver the best population coverage within 5, 10, and 15 minute service areas, for both customer walk or drive access and courier restocking?
4. Where can lockers most reduce failed delivery attempts and last-mile inefficiency, particularly in newly expanding or addressing-weak districts?
5. Which existing pickup points (SPL Parcel@t, courier branches) already cover demand, and where are the genuine gaps a new network should fill?
6. What are the best 10 to 20 candidate sites for a pilot implementation in the Jazan urban core, and what is the incremental coverage each one adds?

---

## 4. Required Datasets

The table below lists each dataset, a realistic source for the Saudi and Jazan context, and its role in the model. Where an authoritative source is unavailable, a practical open substitute is given.

| Dataset | Primary / realistic source | Role in model |
|---|---|---|
| Neighborhood (hayy) boundaries | Jazan Municipality (Amanah), GASTAT, OSM | Reporting units, demand aggregation |
| Road network | OSM via OSMnx, Esri StreetMap Premium (KSA), HERE | Network analysis, service areas, road proximity |
| Land use / zoning | Municipality master plan, OSM, manual classification from imagery | Constraint layer, eligible-site filter |
| Population / building density | GASTAT census, WorldPop, GHSL, Google/Microsoft building footprints | Demand proxy, walk-in catchment |
| Commercial centers and shopping malls | Google Places API, OSM | Footfall, pickup convenience |
| Universities and schools | Ministry of Education open data, Google Places, OSM | High e-commerce user proxy (students) |
| Hospitals and clinics | Ministry of Health, Google Places, OSM | Anchor footfall, 24/7 activity |
| Government service centers | OSM, Baladi, Absher service points | Anchor footfall, trusted siting |
| Fuel stations | Google Places API, OSM | Strong candidate hosts (parking, 24/7, visibility) |
| Public parking areas | Municipality, OSM | Vehicle access scoring |
| Existing parcel / logistics points | SPL Parcel@t "nearest branch" map, Naqel points, courier sites | Overlap / cannibalization penalty |
| Urban expansion areas | GEE multi-date imagery change detection, municipal plan | Addressing-gap and future-demand weighting |
| National Address readiness | SPL National Address API (free) | Address-density / readiness indicator |
| Official delivery statistics | GASTAT, CST/TGA e-commerce and courier reports, SPL data | Regional calibration, sense-checking |

**Proxy indicators for potential demand (when real delivery records are unavailable).** Because neighborhood-level parcel volumes are not publicly released, potential demand is modeled as a composite index built from measurable proxies:

- Residential building footprint count and density (multi-family / apartment blocks weighted higher, since home delivery is harder there and locker value is greater)
- Estimated population (WorldPop / GHSL / GASTAT)
- Commercial and retail POI density (Google Places, OSM)
- Proximity to universities and large employers (student and young-professional e-commerce intensity)
- Newly urbanized / expansion zones (higher failed-delivery risk from immature addressing)
- Nighttime-lights or built-up intensity as a general economic-activity proxy

This index is explicitly an **estimate of potential**, not a measurement of actual parcel flow, and the report states this limitation plainly (see Section 11).

---

## 5. Suitability Criteria and Weights

A multi-criteria decision analysis (MCDA) drives the model. Criteria weights below were set using an AHP-style pairwise logic, prioritizing the factors that most directly govern where parcels want to be delivered and how efficiently a locker can be served. Weights sum to 100 percent.

| # | Criterion | Weight | Reasoning |
|---|---|---|---|
| 1 | Potential delivery demand (composite proxy) | 20% | The core purpose is to place lockers where parcel demand concentrates. Highest single weight. |
| 2 | Population / residential density | 15% | Direct walk-in catchment and a strong secondary demand signal. |
| 3 | Proximity to major roads | 12% | Governs courier restocking efficiency and customer drive access. |
| 4 | Proximity to commercial / public activity centers | 12% | Footfall means people can collect parcels while already out, raising utilization. |
| 5 | Coverage of underserved neighborhoods | 10% | Rewards sites that extend the network into gaps rather than piling onto served areas. |
| 6 | Parking / easy vehicle access | 8% | Needed for both courier vans and drive-up customers; fuel stations and malls score high. |
| 7 | Safety and visibility | 8% | Well-lit, visible, monitored locations reduce vandalism and increase user trust. |
| 8 | Distance from existing parcel / logistics points | 8% | Penalizes cannibalization of SPL and courier pickup points; rewards true gap-filling. |
| 9 | Suitability for 24/7 access | 7% | Zoning, lighting, and security that allow round-the-clock use, the main locker value proposition. |

**Modeling note on double counting.** Criteria 1 and 2 are correlated, since density feeds the demand index. To avoid inflating the density signal, the demand index (criterion 1) is built primarily from commercial, POI, expansion, and building-typology inputs, while criterion 2 isolates the residential catchment. Analysts should test weight sensitivity (for example, plus or minus 5 points on criteria 1 and 2) and report how site rankings shift, which is standard practice for a defensible MCDA.

Criteria 5 and 8 are penalty-style: a site near an existing locker or already-served neighborhood scores lower on those axes, which is intentional.

---

## 6. GIS Methodology (ArcGIS Pro and ArcGIS Online)

A clear, reproducible workflow. Each stage produces an inspectable intermediate output.

**Step 1. Data collection.** Assemble all layers from Section 4. Pull POIs through the Google Places API and save every API response to JSON immediately, before any processing, so the raw data is preserved.

**Step 2. Data cleaning and geocoding.** Deduplicate POIs, standardize attributes, and geocode any address-based records. Validate against the National Address API where possible. Remove points that fall outside the built area (for example, snap or clip using the road-network convex hull to eliminate stray sea or desert points).

**Step 3. Projection and coordinate system.** Standardize all layers to a single projected CRS for accurate distance and area computation. For the Jazan area, UTM Zone 38N (EPSG:32638) is the appropriate projected system; keep a WGS84 (EPSG:4326) copy for web publishing.

**Step 4. Spatial joins.** Aggregate POIs, buildings, and population to neighborhood units and to the analysis grid, producing per-unit counts and densities that feed the criteria layers.

**Step 5. Candidate site generation.** Create the pool of candidate locations two ways and merge: (a) anchor-based candidates snapped to eligible hosts (fuel stations, malls, parking lots, government centers, transit nodes), and (b) a hexagonal or fishnet grid over the demand surface, keeping the top-scoring cells. Target roughly 50 candidates for the pilot city.

**Step 6. Buffer analysis.** Generate straight-line buffers for quick proximity screening (for example, road proximity, distance to existing lockers, distance to activity centers).

**Step 7. Network analysis.** Using ArcGIS Network Analyst on the road network, build realistic travel-based catchments rather than simple circles.

**Step 8. Service area analysis (5, 10, 15 minutes).** Produce two families of isochrones per candidate: walk-time areas for customer convenience and drive-time areas for courier restocking and drive-up access. These quantify the population reachable at each threshold.

**Step 9. Weighted overlay / MCDA.** Normalize each criterion to a common 0 to 1 scale (higher is better, with penalty criteria inverted), then combine using the Section 5 weights. Apply hard constraint layers first as a binary mask (exclude water, protected areas, restricted zones, road medians, and other ineligible parcels).

**Step 10. Suitability score (0 to 100).** Compute the final score per candidate:

```
SuitabilityScore = 100 × Σ ( wᵢ × normalizedᵢ )   , applied only to eligible (unmasked) sites
```

**Step 11. Ranking.** Sort candidates by score. Then run an incremental-coverage pass so the pilot shortlist maximizes *added* population coverage rather than clustering several high-scoring sites in the same catchment.

**Step 12. Field or visual validation.** Verify the top sites against high-resolution imagery and Street View equivalents, confirming parking, visibility, access, and 24/7 feasibility. Where possible, a short field visit confirms the final pilot set.

**Step 13. Publishing to ArcGIS Online.** Publish candidate sites, service areas, demand surface, and neighborhood coverage as hosted feature layers, with clean metadata for each.

**Step 14. Interactive dashboard.** Build the operator- and stakeholder-facing dashboard (Section 8).

---

## 7. Scoring Model and Classification

Each candidate receives a final suitability score from 0 to 100, classified into four tiers:

| Class | Score range | Interpretation |
|---|---|---|
| Excellent | 80 to 100 | Priority pilot sites; deploy first |
| Good | 65 to 79 | Strong secondary sites; phase-two rollout |
| Moderate | 50 to 64 | Conditional; revisit after pilot performance data |
| Low | Below 50 | Not recommended without new evidence |

The pilot shortlist is drawn from the Excellent and Good tiers, filtered by the incremental-coverage pass so the selected lockers collectively cover the most population and the most underserved districts.

---

## 8. Expected Outputs (Deliverables)

1. **Interactive suitability map** of Jazan City with all candidate sites and scores.
2. **Dashboard** presenting key indicators, filters, and rankings.
3. **Estimated demand heatmap** built from the proxy index (clearly labeled as potential demand).
4. **Top 10 / Top 20 recommended locker locations**, each with its score breakdown.
5. **Service-area map** (5, 10, 15 minute) for each selected locker.
6. **Neighborhood coverage analysis** showing population served and gaps remaining.
7. **Short technical report** documenting data, methods, weights, and assumptions.
8. **Executive summary** for decision-makers.
9. **Commercial brochure** aimed at delivery companies, e-commerce platforms, and site hosts.

---

## 9. Dashboard Design (ArcGIS Dashboards / Experience Builder)

A recommended layout, optimized for both operators and non-technical stakeholders:

- **Main map (center):** candidate sites symbolized by suitability class, with the demand heatmap and service areas as toggleable layers.
- **Filters (left panel):** by neighborhood, by suitability class, and by candidate host type (fuel station, mall, government center, parking, other).
- **Suitability score cards (top):** count of Excellent / Good / Moderate / Low sites, plus city-wide average score.
- **Top candidate sites table:** rank, site name, score, host type, population reached in 10 minutes.
- **Coverage indicators:** percent of city population within 5, 10, and 15 minutes of a recommended locker, and percent of underserved neighborhoods newly covered.
- **Accessibility indicators:** average drive-time and walk-time catchment across the pilot set.
- **Charts:** bar chart of sites by suitability class, and a chart of population coverage gained per additional locker (the incremental-coverage curve, which visually justifies the pilot count).

For richer interactivity (side-by-side scenarios, a "build your own pilot set" selector), Experience Builder is the better choice; for a fast, clean stakeholder view, a single ArcGIS Dashboard is sufficient.

---

## 10. Business Value

The project produces measurable value for several stakeholder groups:

- **Parcel delivery companies and logistics operators:** fewer failed first attempts, shorter and denser courier routes, lower cost per delivered parcel, and a fixed, verified drop point that removes incomplete-address failures.
- **E-commerce platforms:** a reliable pickup option that improves delivery success rates and customer satisfaction in an underpenetrated but fast-growing region.
- **Municipal decision-makers:** an evidence base for permitting locker sites, and support for National Address compliance and smart-city objectives.
- **Shopping malls and fuel stations:** additional footfall and a low-cost amenity that draws repeat visits, with lockers as revenue-sharing hosts.
- **Smart-city initiatives:** a replicable, data-driven siting model that generalizes to other Jazan Region cities.

Measurable benefits to track post-deployment: reduction in failed delivery attempts, reduction in last-mile cost per parcel, share of city population within a 10-minute catchment, National Address compliance coverage, and utilization rate per locker.

---

## 11. Limitations

- Without actual parcel-delivery records, the model estimates **potential** demand using proxy indicators. It does not measure real parcel volume at the neighborhood level, and no specific volume figures should be attributed to neighborhoods unless real operator data is supplied.
- POI, building-footprint, and OSM completeness vary across districts; newly built areas are often under-mapped, which can understate demand exactly where addressing gaps are worst. This should be checked and noted per district.
- Travel-time service areas depend on road-network quality and assumed speeds; results should be validated against local knowledge of traffic and access.
- Weights are expert-derived and should be stress-tested through sensitivity analysis, with the sensitivity results reported alongside the rankings.
- Final site feasibility (land ownership, power, permitting, security) requires field confirmation beyond the desktop model.

---

## 12. Final Recommendation: Jazan MVP Plan

A practical, staged pilot for Jazan City:

**Phase 1, Scope the urban core.** Define the analysis boundary around central Jazan, where population, commerce, and existing addressing are densest, and assemble the core datasets.

**Phase 2, Generate 50 candidate locations.** Combine anchor-based candidates (fuel stations, malls, government centers, parking) with grid-derived high-demand cells, then apply constraint masks.

**Phase 3, Score and select the best 10 pilot sites.** Run the MCDA, rank candidates, and apply the incremental-coverage pass so the ten selected lockers maximize added population and underserved-area coverage rather than clustering.

**Phase 4, Validate.** Confirm the ten sites through imagery review and, ideally, a short field visit checking parking, visibility, access, and 24/7 feasibility.

**Phase 5, Publish.** Deliver the interactive ArcGIS Online dashboard with the recommended sites, service areas, coverage metrics, and score breakdowns.

**Phase 6, Scale.** Use the validated Jazan City pilot as a template, then extend the same workflow to Sabya, Abu Arish, Samtah, and other Jazan Region cities, refining weights with any real utilization data gathered from the pilot.

The result is a repeatable, evidence-based siting engine, starting with ten defensible pilot lockers in Jazan City and designed from the outset to expand across the region.
