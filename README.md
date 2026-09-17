# MEDISYNC

**Regional Medicine Shortage Intelligence & Intervention System**

*From one empty shelf to a regional shortage — before it happens.*

Built for **Manipal Hackathon 2026** · Problem Statement: *Good Health and Well-being (SDG 3)* · Team **code4life**

---

## The problem

A hospital running low on a medicine can look like a one-off inventory issue. But when several nearby facilities decline together, it's often an early signal of a wider supply disruption — one that spreads before health authorities have enough time to respond. Meanwhile, usable surplus frequently sits unused elsewhere, because no one has a shared, real-time view of demand and supply across the region.

## The solution

MEDISYNC builds that shared view, and reasons about it in five stages:

| Stage | What it does |
|---|---|
| **1. Detect** | Monitors stock, consumption, and replenishment signals across every facility in a region |
| **2. Forecast** | Estimates consumption rate and projects days-to-stockout for every facility–medicine pair |
| **3. Propagate** | Correlates declining stock across nearby facilities to tell an isolated dip apart from a spreading regional shortage |
| **4. Intervene** | Matches at-risk facilities against facilities with usable surplus, weighing distance, urgency, and transport feasibility, and ranks redistribution, emergency procurement, or demand-smoothing actions |
| **5. Verify** | Lets a decision-maker simulate an intervention before committing to it, and reports risk alongside confidence — never as certainty |

## What's in this repo

This repo currently contains the **working prototype**: a single self-contained `medisync-app.html` file that implements the full Detect → Forecast → Propagate → Intervene → Verify pipeline client-side, over a deterministic simulated dataset (30 facilities across 6 districts, 12 medicines).

Nothing here is hardcoded set-dressing — stock forecasts, risk classification, cluster detection, surplus/redistribution scoring, and simulation results are all calculated live in the browser from the seeded data.

### Try it

No install, no server. Open the file in a browser:

```
open medisync-app.html      # macOS
start medisync-app.html     # Windows
xdg-open medisync-app.html  # Linux
```

Click **Use Demo Account**, then set **Region → Tambaram** and **Medicine → Amoxicillin 500mg** to see the seeded demo scenario: three facilities declining together with a nearby depot holding transferable surplus.

### What's implemented

- Login screen → sidebar app shell (Overview, Regional Risk Map, Inventory, Forecasts, Propagation, Interventions, Simulator, Facilities, Medicines, Alerts, Analytics, Settings, Help)
- Forecast engine — rolling consumption rate, days-to-stockout, confidence score from demand volatility
- Risk engine — configurable Critical / High / Medium / Healthy thresholds (editable live in Settings)
- Propagation engine — clusters facilities by district + medicine, distinguishes "potential regional shortage cluster" from "likely isolated shortage," using real geographic distance (haversine)
- Surplus & recommendation engine — safety-stock-aware surplus detection, urgency/distance/criticality scoring, with a plain-language explanation for every recommendation
- Simulator — before/after stock, days remaining, risk level, and stockout date; changes only persist if "Apply" is clicked
- Schematic geographic map built from facility coordinates (no external map dependency required to run)

## Planned production architecture

The prototype's engines are written as isolated functions with clear inputs/outputs specifically so they port cleanly to real services. The target architecture:

```
Data sources → PostgreSQL → FastAPI → Intelligence layer → REST API → Next.js dashboard
```

| Layer | Technology |
|---|---|
| Frontend | React / Next.js |
| Backend | Python, FastAPI |
| Forecasting | Time-series models (baseline moving average, scikit-learn / XGBoost where sufficient history exists) |
| Propagation graph | Facilities as geographic nodes, edges weighted by distance & transport feasibility |
| Optimization | OR-Tools for redistribution matching, with a deterministic scoring fallback |
| Database | PostgreSQL |
| Maps | OpenStreetMap |

## Feasibility

- Scoped as a 36-hour MVP across four phases: **Data & Network → Intelligence → Optimization → Dashboard**
- Targets a manageable regional network (10–50 facilities, multiple medicines) rather than a full national system
- Uses simulated or public data throughout, as the problem statement permits
- Every alert reports **risk level, time horizon, confidence, and contributing factors** — MEDISYNC never presents a forecast as certainty

## Users & impact

**Primary:** state / regional health authorities.
**Secondary:** hospital supply-chain managers, hospital networks, central medical warehouses, NGOs and public-health programs, healthcare logistics providers.

The same facility-graph and forecasting engine generalizes beyond medicines to vaccines, blood units, and equipment — a path from district pilot to state and national deployment, supporting SDG 3 through fewer preventable stockouts and more equitable medicine access.

## Important note

This is a hackathon prototype built on **simulated data**. It is a supply-chain intelligence system, not a diagnosis, treatment, or patient medical advice system, and forecasts are decision-support estimates to be reviewed by authorized personnel — not guarantees.

## Team code4life

Arya Singh Vishen · Pranay Jain · Manya Sinha · Harishma Venati · Riya Maheshwari
