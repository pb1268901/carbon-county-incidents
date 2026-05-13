# Carbon Alert — Incident, Evacuation & Road Closure Table

Public-facing incident, evacuation, and road closure display for [Carbon Alert](https://carbon-alert-carbonmt.hub.arcgis.com), Carbon County Montana's emergency information site.

Hosted via GitHub Pages at: **https://pb1268901.github.io/carbon-county-incidents/**

Embedded on the Carbon Alert Hub site via an IFrame card.

---

## What it does

`index.html` queries live feature layers from Carbon County's ArcGIS Online organization and renders:

- **Evacuation cards** — color-coded by level (Order / Warning / Advisory), shown only when active zones exist
- **Incident table** — all non-draft incidents with status badges, flags for active evacuations and road closures, search, and active count
- **Road closure table** — all road blocks with active/inactive badges, full closure tags, alternate routes, and active closure count
- **Auto-refresh** — all sections reload every 5 minutes automatically

---

## Data sources

### Incidents & Evacuations

```
https://services1.arcgis.com/lo6DwqkHoBfbpsNX/ArcGIS/rest/services/Carbon_County_Incidents-_Public/FeatureServer
```

| Index | Layer | Purpose |
|-------|-------|---------|
| /0 | Carbon County Incident Points | One record per incident |
| /1 | Carbon County Incident Evacuations | One polygon per evacuation zone |

This is a read-only public view of the private source layer `Carbon_County_Incidents`. Staff update data via Survey123, which writes to the source layer. The public view reflects those changes automatically.

### Road Closures

```
https://services1.arcgis.com/lo6DwqkHoBfbpsNX/arcgis/rest/services/Road_Closures_View/FeatureServer
```

| Index | Layer | Purpose |
|-------|-------|---------|
| /0 | RoadBlock | One record per road closure |

This is a read-only public view of the private `Road_Closures` source layer. Staff update data directly in ArcGIS Online or via the staff page. The public view reflects those changes automatically.

---

## Incident fields used

| Field | Type | Notes |
|-------|------|-------|
| Incident_Name | String | Display name |
| Incident_Type | String | Wildfire, Flood, Hazmat, etc. |
| Status | String | Active, Contained, Closed |
| Start_Date | Date | |
| Location_Description | String | Plain language location |
| Size_Acres | Double | |
| Contact_Info | String | |
| Last_Updated | Date | |
| Evacuations | String | Yes/No — flags row in table |
| Road_Closures | String | Yes/No |
| RC_Details | String | Road closure description |

## Evacuation fields used

| Field | Type | Notes |
|-------|------|-------|
| Zone_Level | String | Evacuation Order, Warning, or Advisory |
| Status | String | Active or Lifted — only Active records shown |
| Affected_Area | String | Plain language area description |
| Public_Message | String | Full public-facing message |
| Effective_Date | Date | |

## Road closure fields used

| Field | Type | Notes |
|-------|------|-------|
| BLOCKNM | String | Road block name |
| LOCDESC | String | Plain language location description |
| ACTIVE | Integer | 1 = Active (red badge), 0 = Inactive (grey badge) |
| FULLCLOSE | String | Yes/No — shows FULL CLOSURE tag on row |
| DIRECTION | String | Direction of closure if partial |
| BLOCKOCCUR | String | Occurrence type |
| ALTROUTE | String | Alternate route description |
| STARTDATE | Date | |
| ENDDATE | Date | |
| CONTACT | String | |
| INCIDENTNM | String | Associated incident name if applicable |
| LASTUPDATE | Date | |

---

## Updating the file

Edit `index.html` directly and push to main. GitHub Pages deploys within a few minutes. Hard refresh the live URL with Ctrl+Shift+R to bypass cache.

If the table shows **Error loading incident data** check:
1. The public view layer is still shared publicly in AGOL
2. The BASE URL in the script matches `Carbon_County_Incidents-_Public/FeatureServer`
3. The browser developer console for JavaScript errors

If evacuations are not showing check:
1. The evacuation record has `Status = Active` in Survey123
2. The public view layer exposes all evacuation fields — query `/1/query?where=1%3D1&outFields=*&f=json` directly to verify

If road closures are not showing check:
1. The `Road_Closures_View` item is shared publicly in AGOL — this is a view layer and must be shared independently of the source `Road_Closures` layer
2. The ROAD_BASE URL in the script matches `Road_Closures_View/FeatureServer`
3. Verify the layer is accessible by running in the browser console:
   `fetch('https://services1.arcgis.com/lo6DwqkHoBfbpsNX/arcgis/rest/services/Road_Closures_View/FeatureServer/0/query?where=1%3D1&outFields=ACTIVE&f=json').then(r=>r.json()).then(d=>console.log(d))`
   If you see `{error: {code: 499, message: 'Token Required'}}` the view layer is not yet public

---

## Staff workflow

Data is managed via Survey123 from the Carbon Alert staff page:
`https://carbon-alert-carbonmt.hub.arcgis.com/pages/staff`

The staff page requires an ArcGIS Online org account to access.

---

## Maintainer

Patrick Benson, GIS Coordinator — Carbon County Montana  
pbenson@carbonmt.gov — 406-445-7271
