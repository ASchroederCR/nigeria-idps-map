# Nigeria IDP Origin/Destination Map — North-East Region (March 2025)

**Live map:** https://aschroedercr.github.io/nigeria-idps-map/

## Data source

Data comes from WorldPop's (University of Southampton) **NGA Spatially Integrated
Displacement Datasets**, GRID3 Phase 2 Scaling initiative
(DOI: [10.5258/SOTON/WP00826](https://doi.org/10.5258/SOTON/WP00826)), specifically
the `ne_adm3.gpkg` layer covering ward-level (Admin 3) administrative boundaries in
North-East Nigeria.

The underlying displacement counts are integrated from IOM's Displacement Tracking
Matrix (DTM) assessments and spatially harmonized to GRID3 administrative
boundaries via a combination of administrative identifier matching, name matching,
and coordinate-based geocoding.

This map uses the most recent reporting period available for both origin and
destination in the dataset: **March 2025** (`idps_orig_2503` / `idps_dest_2503`).

Flood risk data comes from a separate UNICEF LGA-level risk analysis dataset
(`unicef_lga_risk_analysis.geojson`), covering riverine, flash, and coastal
flood risk categories alongside several conflict and disease risk fields
(only the flood risk fields are used in this map).

## Map contents

The map shows four independently toggleable layers:

- **Origin** (viridis palette, on by default): number of IDPs whose origin
  (place of displacement *from*) is recorded in each ward.
- **Destination** (plasma palette, on by default): number of IDPs currently
  residing in (displaced *to*) each ward.
- **Riverine Flood Risk (UNICEF LGA)** (blue color ramp, **off by default**):
  LGA-level riverine flood risk category.
- **Flash Flood Risk (UNICEF LGA)** (blue color ramp, **off by default**):
  LGA-level flash flood risk category.

The Origin and Destination layers use a log10 color scale, since IDP counts
range from single digits to roughly 200,000 per ward. Clicking any ward opens
a popup showing that ward's **Origin IDPs**, **Destination IDPs**, and
**Net (destination − origin)** — a positive net indicates the ward is a net
*receiver* of displaced people; a negative net indicates it is a net *sender*.

### Flood risk layers

The UNICEF dataset classifies each LGA's riverine, flash, and coastal flood
exposure as `None`, `Low`, `Medium`, or `High`. Two of these three risk types
are shown as map layers (coastal flood risk is not currently mapped as its
own layer, though all three risk fields are included in the flood layer
popups):

- **Filtered to the IDP data's extent:** The source dataset is national
  (774 LGAs). Both flood layers are clipped to the rectangular bounding box
  of the North-East ward-level IDP data, reducing this to 355 candidate LGAs.
- **`None`-risk LGAs are removed, not colored:** Rather than showing a
  "no risk" color, LGAs classified as `None` for the relevant flood type are
  dropped from that layer entirely, leaving 110 LGAs in the Riverine layer
  and 173 in the Flash Flood layer. Because filtering is done independently
  per risk type, an LGA may appear in one flood layer but not the other.
  Coloring runs light-to-dark blue across the remaining `Low` / `Medium` /
  `High` categories.
- **Off by default:** Both flood layers are hidden on load to keep the
  initial view focused on displacement data; check them in the layer control
  to enable. Each layer's legend only appears while that layer is visible,
  to avoid legend clutter when multiple layers are toggled on/off.

## Known data limitations

- **59 wards excluded:** These have `ward == "unknown"` and empty geometry —
  LGA-level assessment records that couldn't be allocated to a specific ward.
  Excluding them drops ~25% of the total origin count and ~6% of the total
  destination count from the ward-level map (concentrated in Borno LGAs such as
  Damboa and Ngala). LGA-level aggregates (not shown on this map) would capture
  these records.
- **Uneven temporal coverage:** Origin counts are reported for 4 periods,
  destination for 7; not every ward has data for every period.
- **Ward-level precision:** Source assessments are often reported at higher
  administrative levels (LGA) and allocated to wards through a spatial
  harmonisation procedure, so ward-level figures carry more uncertainty than
  LGA- or state-level figures.
- **Not an official statistic:** This is a derived product for exploratory
  spatial analysis, not an authoritative displacement count.

## Reproducing this map

Built in R using `sf`, `leaflet`, and `htmlwidgets`, reading directly from
`ne_adm3.gpkg`. The rendered HTML in `docs/index.html` is self-contained
(no external JS/data dependencies) and is served via GitHub Pages from the
`docs/` folder on `master`.
