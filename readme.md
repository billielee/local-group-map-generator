# Local Group Map Generator

A single‑file Leaflet app that draws ZIP code areas from U.S. Census ZCTAs, lets you group them by color with custom labels, and exports a PNG that includes a grayscale basemap, city names, state outlines, and your colored groups.

## Quick start

1. Download the file named **Local Group Map Generator** as `index.html`.

2. Open it in a browser. If tiles fail to load on `file://`, run a tiny local server:

   ```bash
   python3 -m http.server 8080
   ```

   Then visit `http://localhost:8080/index.html`.

3. Click **Load sample** to see it in action.

## How to use

* **Add group**: creates a card for one group of ZIPs.

  * **Label**: the text that appears on the map.
  * **Color** and **Opacity**: controls the fill and outline color for that group.
  * **ZIP codes**: paste a messy list. The app normalizes to five digits.
* **Show all groups**: queries ZCTAs and renders each group as a separate layer.
* **Clear map**: removes current layers and labels.
* **Use basemap**: toggles the grayscale base and city‑name label tiles.
* **Show group labels**: toggles visibility of label text centered in each colored group.
* **Download PNG**: opens a new tab with the composed image and a download link.

## What’s included on the map

* **ZCTAs** (Census 2020 via ACS 2022 TIGERweb service) for boundaries and fill.
* **State outlines** as a thin vector layer for context.
* **City names** from the Esri Light Gray Reference tile set.
* **Basemap**: Esri Light Gray Canvas so your group colors stand out.

## Accuracy notes

* USPS ZIP codes are delivery routes, not strict polygons. ZCTAs are the best available public approximation. Expect small differences at edges. Do not use for legal boundaries.

## Export behavior

* PNG includes the basemap, city names, state outlines, and all group polygons.
* The export opens in a new tab. Some browsers block auto‑downloads. Use the visible link if needed.
* If you see a blank export: allow popups, serve the file over `http://localhost`, and try again.

## Limits and performance

* **URL length**: Large groups may exceed a safe GET URL length. Keep individual groups to a few hundred ZIPs or split them.
* **Rendering**: Thousands of ZCTAs can feel sluggish in the browser. Performance depends on geometry complexity and your machine.
* **Server**: The Census service is generous, but network latency adds up. Be patient with big requests.

## Customization

All edits are in `index.html`.

* **Default view**: change the `setView([lat, lng], zoom)` values.
* **Sample groups**: edit the two calls inside the `#sample` button handler.
* **Label placement**: labels are placed at an interior point of each group using Turf’s `pointOnFeature()`; the app falls back to the bounds center if needed.
* **Colors**: each group has an independent color. The legend mirrors these.
* **Basemap and labels**: swap tile URLs if you prefer a different provider that permits canvas export.
* **Add counties**: create another pane and load a county boundaries GeoJSON service, similar to `stateLayer`.

## Known issues and fixes

* **Popup blocked** on download: allow popups for the page. The app opens a tab first to ensure user gesture context.
* **Slow responses**: ZCTA queries are remote. Large lists will take longer. If you plan very large scopes, consider simplifying geometries.
* **Safari quirks**: if PNG is empty, serve over `http://localhost` and retry.

## Data sources and attributions

* **ZCTAs**: U.S. Census Bureau TIGERweb (ACS 2022 2020‑vintage ZCTA polygons).
* **Basemap**: Esri Canvas Light Gray Base.
* **City labels**: Esri Canvas Light Gray Reference.
* **State boundaries**: Esri USA State Boundaries FeatureServer.
* OpenStreetMap attribution applies to underlying Esri content where OSM is a contributor. Respect each provider’s terms of use.

## Dev notes

* No backend. Everything runs client‑side in the browser.
* Uses Leaflet for maps and leaflet‑image for raster export.
* Uses Turf.js to compute interior points for group label placement.
* Vector drawing uses the Leaflet Canvas renderer for reliable PNG captures.

## Roadmap ideas

* Single union outline around all selected ZIPs.
* Shareable URLs that preload groups, colors, and zoom.
* CSV upload with validation feedback.
* SVG export route that temporarily switches to an SVG renderer.

## License

MIT for the app code below. Replace if you prefer something else.

```
MIT License

Copyright (c) 2025

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
