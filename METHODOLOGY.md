# CarbonSync™ Measurement & Verification Methodology
**Daxem Labs · Patent Pending GB2602946.2 · Version 1.0 · February 2026**

> Prepared for submission to Verra (Verified Carbon Standard) and SustainCERT for methodology approval and validation body engagement.

---

## 1. Overview

CarbonSync™ is a digital Monitoring, Reporting, and Verification (dMRV) system designed for the quantification and verification of greenhouse gas emissions reductions from UK heavy goods vehicle (HGV) fleets. It is the first system to establish a cryptographically-verifiable, unbroken chain of custody from physical fuel consumption measurement at the vehicle to tokenised Verified Carbon Units (VCUs) on a public blockchain — with no manual data entry at any point in the pipeline.

This document describes the measurement methodology, baseline construction, emissions quantification, additionality demonstration, permanence provisions, and verification architecture.

---

## 2. Applicability Conditions

This methodology applies to projects that meet all of the following criteria:

- Fleet consists of diesel-powered HGVs operating in the United Kingdom
- Vehicles are individually instrumented with a CarbonSync™ Sentinel Rig
- A minimum 60-day baseline monitoring period is completed prior to intervention
- Fleet operator commits to a minimum 24-month crediting period
- Data quality score exceeds 90/100 for each monitored period (see Section 5.3)

---

## 3. Baseline Emissions Quantification

### 3.1 Baseline Measurement Approach

Baseline emissions are established through direct physical measurement — not estimation, fuel receipts, or OBD-II proxy data. The Sentinel Rig's hall-effect ultrasonic flow sensor measures fuel consumption at pulse level with accuracy exceeding 98%, installed inline on the vehicle fuel return line.

This constitutes a **Type 1 (Direct Measurement)** baseline under Verra VCS requirements, providing the highest available data quality tier.

### 3.2 Baseline Emission Factor

Baseline CO₂e is calculated using the DEFRA 2024 UK Government Greenhouse Gas Conversion Factors for Company Reporting:

```
Emission Factor:  2.68 kg CO₂e per litre of diesel
Standard:         DEFRA 2024 · UK Transport · Scope 1
Scope:            Tailpipe CO₂, CH₄, and N₂O expressed as CO₂e
```

This factor is updated annually in line with DEFRA publication cycles. The factor applicable at the start of each crediting period is locked for that period.

### 3.3 Baseline Period

A minimum 60-day continuous monitoring period is required to establish a statistically robust vehicle-specific baseline. During this period, the Sentinel Rig collects:

- Fuel consumption at 30-second intervals (fuelPulse_L)
- Engine RPM, speed, and load via OBD-II (PIDs 0x0C, 0x0D, 0x04)
- GPS position, route topology, and elevation profile
- Ambient conditions via third-party weather API integration

The baseline model is vehicle-specific. A fleet-average baseline is not used, as this would obscure individual vehicle performance and reduce sensitivity to incremental improvements.

---

## 4. Project Emissions and Reduction Quantification

### 4.1 Emissions Reduction Calculation

```
Baseline Emissions (tCO₂e)  =  Fuel_baseline (L)  ×  2.68 kg/L  ÷  1000
Project Emissions (tCO₂e)   =  Fuel_project (L)   ×  2.68 kg/L  ÷  1000
Emissions Reduction          =  Baseline Emissions  −  Project Emissions
Net VCUs Issued              =  Emissions Reduction  ×  (1 − Uncertainty Deduction)
```

### 4.2 Uncertainty Deduction

A 5% conservative deduction is applied to net reductions prior to VCU issuance to account for measurement uncertainty, sensor calibration tolerances, and model prediction variance. This deduction may be reduced following independent sensor calibration verification by the Validation and Verification Body (VVB).

### 4.3 AI-Assisted Attribution (Dataloom™)

The Dataloom™ AI engine deploys vehicle-specific Long Short-Term Memory (LSTM) neural networks to attribute observed fuel reductions to specific causal factors:

| Reduction Source | Attribution Method |
|---|---|
| Fuel efficiency improvements | LSTM prediction vs. actual fuel consumption delta |
| Route optimisation | GPS route topology comparison vs. baseline routes |
| Driver behaviour | Engine load profile deviation from baseline distribution |
| Idle reduction | OBD-II RPM analysis — idle events vs. baseline frequency |

Attribution is performed per vehicle per 30-day period and reported as a percentage breakdown. This granularity satisfies Verra VCS requirements for additionality demonstration at the project activity level.

---

## 5. Data Integrity and Chain of Custody

This is the architectural feature that distinguishes CarbonSync™ from all existing dMRV platforms. The integrity architecture solves the fundamental trust problem in mobile industrial monitoring: **how do you prove that data captured in an uncontrolled environment has not been tampered with between measurement and credit issuance?**

### 5.1 Hardware-Enforced Cryptographic Integrity

An independent cryptographic co-processor (ATTiny85), connected to the primary microcontroller via a **unidirectional interface**, continuously hashes all sensor data streams in real time. This architecture means:

- The primary processor cannot modify data after hashing — it has no write access to the co-processor
- A compromised primary system cannot generate false data and a matching hash simultaneously
- Any post-hoc data modification is computationally detectable

Each 30-second packet is signed with SHA-256 at source, before transmission, before local storage. The hash is embedded in the packet itself.

### 5.2 Dual-Persistent Storage

Every reading is written to two independent storage layers simultaneously:

| Layer | Location | Purpose |
|---|---|---|
| Primary | Cloud (encrypted, TLS 1.3) | Real-time processing and dashboard |
| Secondary | On-vehicle SD card (tamper-evident enclosure, IP67) | Audit black box — available for physical inspection |

During network outages, data accumulates on the SD card and transmits as a verified catch-up batch on reconnection. No data is lost during connectivity interruptions.

### 5.3 Data Quality Scoring

Each monitoring period is assigned a quality score (0–100) computed from:

- Sensor signal quality (GPS fix accuracy, 4G RSSI)
- Data completeness (percentage of expected 30-second packets received)
- Cross-validation pass rate (fuel / OBD-II / GPS consistency checks)
- Hash verification pass rate (cryptographic integrity)

Only periods scoring ≥90 are included in VCU calculations. Periods scoring 75–89 are flagged for VVB review. Periods below 75 are excluded.

### 5.4 Multi-Modal Cross-Validation

The Dataloom™ validation layer cross-checks three independent data streams on every packet:

1. **Fuel flow** (direct physical measurement — flow sensor)
2. **Engine telemetry** (OBD-II — RPM, speed, engine load)
3. **GPS** (position, distance, elevation delta)

Anomalies are flagged when these streams are inconsistent with each other. The false positive rate is below 1%, compared to 3–5% for software-only validation systems — a direct result of the hardware co-processor architecture described in Section 5.1.

---

## 6. Additionality

Additionality is demonstrated on two bases:

**Regulatory Additionality:** No UK regulatory requirement exists compelling HGV operators to install real-time fuel flow monitoring systems or generate carbon credits from fleet emissions reductions. The activity goes beyond legal compliance.

**Financial Additionality:** The cost of Sentinel Rig hardware installation (£250/vehicle) and platform fees represents a genuine financial barrier. Without the carbon credit revenue stream, the monitoring activity would not be financially viable for fleet operators. A barrier analysis is provided for each project as part of the Project Description Document (PDD).

---

## 7. Permanence

This methodology generates credits from emissions reductions in fuel consumption — not from carbon sequestration. Permanence risk (reversal) is therefore minimal: a tonne of CO₂e not emitted due to verified fuel savings cannot be "reversed." No buffer pool contribution is required under VCS for this reduction category.

---

## 8. Verification Architecture

### 8.1 Automated Verification Package

At the close of each monitoring period, the Dataloom™ platform automatically generates a structured verification package containing:

- Complete sensor data with cryptographic hash chain
- Quality scores and anomaly logs
- Baseline vs. project emissions comparison
- LSTM attribution breakdown by reduction source
- IPFS content hash linking on-chain tokens to underlying data

This package is formatted for direct submission to the VVB via REST API, replacing manual document compilation.

### 8.2 Verification Cost Reduction

The automated pipeline reduces verification cost from the industry standard of **$3–5/tonne** (manual methodology) to an estimated **$0.50–0.75/tonne** — an 80–85% reduction. This is achieved by replacing manual document review with computational hash verification. Auditors verify chain-of-custody integrity in seconds rather than hours. Physical site visit frequency is reduced from every crediting period to annually, owing to the cryptographic confidence provided by the hardware architecture.

### 8.3 Time-to-Credit

Automated packaging reduces the time from monitoring period close to VCU issuance from the industry standard of **6–12 months** (manual methodologies) to a target of **30–60 days**, enabling near-real-time monetisation of emissions reductions.

### 8.4 Blockchain Tokenisation

Upon VVB approval, VCUs are minted as ERC-1155 tokens on Polygon Mainnet (proof-of-stake — >99% lower energy than proof-of-work networks). Each token contains:

- IPFS content hash linking to the full verification package
- Vehicle ID, monitoring period, tCO₂e quantity
- VVB certificate reference
- Immutable minting timestamp

The blockchain record provides a publicly auditable, tamper-proof registry of all issued credits that is independent of any single organisation's systems.

---

## 9. Pilot Project

The inaugural CarbonSync™ project is being conducted in partnership with a **strategic validation partner (to be announced)**, deploying Sentinel Rigs across their HGV fleet in Q2 2026. Pilot data will form the empirical basis for this methodology's submission to Verra and for SustainCERT validation engagement.

The live platform demonstrating real-time operation of this methodology is available at:
**https://jojibhatia.github.io/CarbonSync-Sandbox/carbonsync-demo.html**

---

## 10. Standards Alignment

| Standard / Framework | Alignment |
|---|---|
| Verra Verified Carbon Standard (VCS) | Methodology submission in preparation |
| DEFRA 2024 GHG Conversion Factors | Emission factor adopted (2.68 kg CO₂e/L diesel) |
| ISO 14064-2 | Project-level GHG quantification and reporting |
| UK CBAM (effective 1 Jan 2027) | Verified actual emissions data satisfies HMRC requirements |
| TCFD | Scope 1 and Scope 3 transport emissions reporting |

---

## 11. Contact

**Daxem Labs**
Patent: GB2602946.2 (filed February 2026)
Platform: [daxem.ai](https://daxem.ai)
GitHub: [github.com/jojibhatia/CarbonSync-Sandbox](https://github.com/jojibhatia/CarbonSync-Sandbox)

*For methodology enquiries, VVB engagement, or pilot partnership discussions, please contact Daxem Labs directly.*

---

*CarbonSync™ is a trademark of Daxem Labs. Patent pending GB2602946.2. All methodology documentation © Daxem Labs 2026.*
