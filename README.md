# Carbon Alert — Incident & Evacuation Table

Public-facing incident and evacuation display for [Carbon Alert](https://carbon-alert-carbonmt.hub.arcgis.com), Carbon County Montana's emergency information site.

Hosted via GitHub Pages at: **https://pb1268901.github.io/carbon-county-incidents/**

Embedded on the Carbon Alert Hub site via an IFrame card.

---

## What it does

`index.html` queries two live feature layers from Carbon County's ArcGIS Online organization and renders:

- **Evacuation cards** — color-coded by level (Order / Warning / Advisory), shown only when active zones exist
- **Incident table** — all non-draft incidents with status badges, flags for active evacuations and road closures, search, and active count
- **Auto-refresh** — both sections reload every 5 minutes automatically

---

## Data source

Both layers are queried from the public view layer:

```
https://services1.arcgis.com/lo6DwqkHoBfbpsNX/ArcGIS/rest/services/Carbon_County_Incidents-_Public/FeatureServer
```

| Index | Layer | Purpose |
|-------|-------|---------|
| /0 | Carbon County Incident Points | One record per incident |
| /1 | Carbon County Incident Evacuations | One polygon per evacuation zone |

This is a read-only public view of the private source layer `Carbon_County_Incidents`. Staff update data via Survey123, which writes to the source layer. The public view reflects those changes automatically.

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

---

## Staff workflow

Data is managed via Survey123 from the Carbon Alert staff page:
`https://carbon-alert-carbonmt.hub.arcgis.com/pages/staff`

The staff page requires an ArcGIS Online org account to access.

---

## Maintainer

Patrick Benson, GIS Coordinator — Carbon County Montana  
pbenson@carbonmt.gov — 406-445-7271
