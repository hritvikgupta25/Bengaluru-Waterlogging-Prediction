# 🌊 Bengaluru Commuter Flood Radar

A dual-mode, real-time road waterlogging and flood forecast radar for commuters across Bengaluru.

When you search for any location (college, office, tech park, metro station, or cafe), the app analyzes every street within a **1 km radius** using live weather and road elevation data to show you which roads are safe and which ones are flooded.

---
Currently live on: https://waterlogging-prediction.streamlit.app/
---


## 🚦 Commuter Dashboard Features

### 🟢 1. Current Road Conditions
* **24-Hour Antecedent Rainfall:** Checks the last 24 hours of precipitation (`past_1h`, `past_3h`, `past_6h`, `past_24h`) to calculate ground saturation, muddy roads, and residual water depth.
* **Underpass & Dip Retention:** Accounts for water trapped in underpasses, railway bridges, and low-lying topography long after rain has stopped.
* **Smart Night & Drizzle Filter:** Differentiates between light mist/dew and genuine road accumulation to prevent false alarms.

### 🔮 2. Waterlogging Forecast (+30m & +60m)
* **High-Resolution Precipitation Projections:** Takes live nowcasts and forecasts forward in 15-minute intervals.
* **Physical Drainage Decay Model:**
  * **Underpasses (Slow Drainage):** Water pools quickly and drains slowly (~95% water retention over 30 mins).
  * **Residential Layouts:** Retains puddles and mud for hours after rain due to unpaved shoulders and flat gutters (~92% retention).
  * **Arterial Roads & Highways:** Storm drains clear standing water steadily (~86% retention).
  * **Flyovers & Bridges:** Rapid gravity runoff clears water almost instantly (~15% retention).
* **Actionable Commuter Trends:**
  * 📈 **Rapidly Flooding** (Incoming heavy rain)
  * ⚠️ **Water Accumulating** (Rain exceeding drain capacity)
  * 🌊 **Standing Water & Mud** (No new rain, but persistent puddles from recent downpours)
  * 📉 **Slowly Draining** (Water level receding)
  * 🟢 **Clearing Rapidly** (Dry roads returning)

### 🌦️ 3. Live Weather & 24-Hour Rainfall Radar
* Real-time temperature, humidity, wind, and precipitation rates.
* 24-hour historical rainfall tracker showing precipitation over the last 1h, 3h, 6h, and 24h.
* Ground Saturation & Mud Index advising on puddle risk and soil wetness.
* Interactive 24-hour precipitation bar chart.

---

## 🚦 Road Condition & Safety Guide (5 Levels)

* 🟢 **Clear / Low Risk (< 35%)**: `#10B981` (Emerald Green) — Normal driving conditions. Flyovers and elevated roads.
* 🔵 **Wet Road / Minor Puddles (35% – 49%)**: `#0284C7` (Ocean Blue) — Damp surface and small puddles. Normal caution.
* 🟡 **Caution / Water Accumulation & Mud (50% – 69%)**: `#EAB308` (Golden Yellow) — Significant puddles, muddy shoulders, and pooling water. Slow down, especially for two-wheelers.
* 🟠 **High Risk / Deep Waterlogging (70% – 84%)**: `#FF8C00` (Tangerine Orange) — Deep water accumulation. High risk of stalling for two-wheelers and sedans.
* 🔴 **Severe Hazard / Flooded (≥ 85%)**: `#B91C1C` (Deep Crimson Red) — Impassable road or flooded underpass. Extreme danger. Seek alternative route.

---

---

## 🛠️ Tech Stack

* **Frontend & Dashboard:** `streamlit`, `streamlit-searchbox`, `streamlit-folium`
* **Geospatial Mapping:** `folium` *(Zero heavy GIS dependencies like GDAL, OSMnx, or GeoPandas)*
* **External HTTP APIs:** `requests`
* **Configuration:** `python-dotenv`
* **Data Processing & Analytics:** `pandas`, Python `math` module

---

## 🌐 External APIs Grounding

| API | Service Provider | Purpose | Cache Policy |
| :--- | :--- | :--- | :--- |
| **Search Box API v1** | Mapbox | Live address/POI autocomplete in Bengaluru | Real-time (300ms debounce) |
| **Multi-Model Forecast API** | Open-Meteo (DWD ICON, Météo-France, ECMWF, GFS) | Live convective rain, 15m nowcast & 24h storm history | `@st.cache_data(ttl=300)` |
| **Overpass Interpreter** | OpenStreetMap | 1km street polylines & waterway coordinates | `@st.cache_data(ttl=3600)` |
