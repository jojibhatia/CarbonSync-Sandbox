# CarbonSync™ — Live Intelligence Platform
**by Daxem Labs · Patent Filed GB2602946.2**

> The first platform to directly measure, verify, and tokenise emissions reductions from UK HGV fleets in real time.

---

## Live Demo

**[Launch CarbonSync™ Sandbox →](https://jojibhatia.github.io/CarbonSync-Sandbox/carbonsync-demo.html)**

### Pre-Configured Client Links

| Client | Direct Link |
|--------|------------|
| Pickfords (700 vehicles) | [Open Pickfords Dashboard](https://jojibhatia.github.io/CarbonSync-Sandbox/carbonsync-demo.html?company=Pickfords&fleet=700&sector=removals&autolaunch=true) |
| O'Donovan Waste (85 vehicles) | [Open O'Donovan Dashboard](https://jojibhatia.github.io/CarbonSync-Sandbox/carbonsync-demo.html?company=O%27Donovan+Waste&fleet=85&sector=waste&autolaunch=true) |
| Generic (any fleet) | [Open with Login Screen](https://jojibhatia.github.io/CarbonSync-Sandbox/carbonsync-demo.html) |

---

## What Is This?

This is a fully interactive sandbox demonstration of the CarbonSync™ platform — built for fleet operators, investors, and pilot partners to experience the product before a Sentinel Rig is installed.

It runs on simulated data that mirrors exactly what the live platform produces when a real rig is connected. Every number, calculation, and credit figure uses real methodology — DEFRA 2024 emission factors, Verra VCS standards, and live ACX market pricing.

**When a real Sentinel Rig connects, nothing in the UI changes. The simulation data is simply replaced with live packets.**

---

## Platform Overview

| Tab | What It Shows |
|-----|--------------|
| **Overview** | Live credits, fuel savings, revenue, and the full verification pipeline from sensor to blockchain |
| **Live Telemetry** | Per-vehicle data feed — fuel flow, CO₂, OBD-II signals, cryptographic signatures |
| **Carbon Credits** | Credit breakdown by source category, Verra VCS methodology, CBAM alignment |
| **Blockchain Ledger** | Real-time ERC-1155 minting events on Polygon — transaction hashes, block numbers |
| **Revenue** | 3-year financial projection, 70/15/10/5 revenue split, hardware payback period |
| **⚠ CBAM Exposure** | UK Carbon Border Adjustment Mechanism liability calculator — live from January 2027 |

---

## How It Works

```
Sentinel Rig  →  MQTT / 4G  →  Dataloom™ AI  →  CarbonSync™ Ledger  →  Polygon Blockchain  →  Carbon Market
   (HGV)          (signed)       (validated)        (cryptographic)          (ERC-1155)           (revenue)
```

---

## Connecting a Real Sentinel Rig

The platform is production-ready for live rig integration. Full technical specification in `CarbonSync_Rig_Brief.docx`.

**WebSocket endpoint (backend engineer):**
```
wss://api.daxem.ai/rigs/live
```

**Expected packet format (embedded engineer):**
```json
{
  "rigId":          "SENT-001",
  "vehicleId":      "PK001",
  "timestamp":      1700000000000,
  "fuelPulse_L":    1.234,
  "obdRpm":         1450,
  "obdSpeed_kmh":   62,
  "obdEngineLoad":  0.67,
  "gpsLat":         51.5074,
  "gpsLng":         -0.1278,
  "gpsAccuracy_m":  4.2,
  "signalStrength": -73,
  "sdCardHash":     "sha256:...",
  "hash":           "sha256:...",
  "firmwareVersion":"1.0.3"
}
```

**Developer Mode:** Press `Ctrl+D` inside the dashboard to open the dev panel — WebSocket status, live connection log, packet schema, and computation constants. Switch between simulation and live mode without touching code.

> Simulation mode is on by default. No console errors in demo mode.

---

## Financial Model

| Parameter | Value | Source |
|-----------|-------|--------|
| CO₂ emission factor | 2.68 kg/litre | DEFRA 2024 |
| Efficiency baseline | 18% improvement | Verified methodology |
| Carbon credit price | £72/tCO₂e | ACX voluntary market |
| Fleet revenue share | 70% | CarbonSync™ standard |
| Platform fee | 15% | Daxem Labs |
| Driver incentives | 10% | Fleet operator distributed |
| Reserve fund | 5% | Buffer |
| Hardware cost | £250/vehicle | Sentinel Rig inc. installation |

---

## CBAM Compliance

The **⚠ CBAM Exposure** tab calculates each fleet's liability under the UK Carbon Border Adjustment Mechanism, effective **1 January 2027**.

- Without verified data: HMRC default penalty rates applied to all emissions
- With CarbonSync™: Verified actual data = lowest possible CBAM charge + carbon credit income offsets liability
- UK ETS rate: £40–£100/tonne (adjustable in the calculator)

---

## Technology

| Layer | Technology |
|-------|-----------|
| Hardware | Sentinel Rig — fuel flow sensor, OBD-II, GPS, 4G (Quectel BG96) |
| Data capture | MQTT over TLS, SHA-256 signed at source, SD card local backup |
| AI validation | Dataloom™ — anomaly detection, efficiency scoring |
| Blockchain | Polygon Mainnet — ERC-1155 tokens, gas-efficient minting |
| Verification | Verra VCS methodology, DEFRA 2024 emission factors |
| Frontend | Vanilla JS, no dependencies, WebSocket live data layer |

---

## Repository Structure

```
CarbonSync-Sandbox/
├── carbonsync-demo.html     ← Full interactive platform (single file)
├── README.md                 ← This file
```

---

## About Daxem Labs

Daxem Labs is a UK-based climate technology company building the FerroGuard AI platform — of which CarbonSync™ is the flagship product.

- **Stage:** Pre-pilot · Q2 2026 launch
- **Pilot partner:** Strategic validation partner (to be announced)
- **Target market:** UK HGV fleet operators — waste, logistics, construction, removals
- **Patent:** GB2602946.2 (filed February 2026)
- **Contact:** [daxem.ai](https://daxem.ai)

---

*CarbonSync™ and FerroGuard AI are trademarks of Daxem Labs. All rights reserved.*
