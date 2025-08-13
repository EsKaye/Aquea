
# 🌊 Project Aquea — Open Water Reclamation (AI + Open Hardware)

**Mission:** Free, clean water for everyone via open-source, AI-managed reclamation & purification that scales from a single home to an entire city.

This repo is scaffolded to be a **monorepo** (inspired by our **NessHash** patterns). It includes:
- Edge firmware for low-cost sensors/actuators (ESP32/STM32).
- A secure ingestion gateway (FastAPI), device hub (MQTT↔DB), and an ML microservice for anomaly detection & predictive maintenance.
- A simulator for local testing.
- Open hardware docs (KiCad placeholders) and calibration scripts.
- Dockerized infra: Postgres, Mosquitto (MQTT).

> **NessHash reference**: All records are hashed with a pluggable adapter. If your `nesshash` library is present, we’ll use it; otherwise we fall back to standard cryptographic hashing. See `libs/nesshash-adapter`.

---

## Quickstart (Dev)

1) **Prereqs**: Docker + Docker Compose, Python 3.11+ (optional).
2) Copy env:  
   ```bash
   cp .env.example .env
   ```
3) Start stack:  
   ```bash
   docker compose up --build
   ```
4) In a new shell, start the simulator (or bring your own device):  
   ```bash
   docker compose run --rm simulator python publisher.py
   ```

## Data Flow

```
[ESP32 sensors] --MQTT--> [Mosquitto] --sub--> [device-hub] --SQL--> [Postgres]
                                          \--> [gateway API] <--> [ml-service]
```

**Topic format**: `aquea/site/{site_id}/device/{device_id}/measurement`

**JSON payload** (example):
```json
{
  "ts": "2025-08-13T17:00:00Z",
  "site_id": "demo-site",
  "device_id": "esp32-01",
  "sensors": {
    "ph": 7.2,
    "tds_ppm": 190.3,
    "turbidity_ntu": 1.1,
    "temp_c": 22.4,
    "flow_lpm": 3.2,
    "pressure_kpa": 210.0
  },
  "meta": {"firmware": "0.1.0", "lat": 0.0, "lon": 0.0}
}
```

## Repo Layout

```
/services
  /gateway         # FastAPI ingestion + ledger (NessHash-integrated)
  /device-hub      # MQTT consumer → Postgres
  /ml-service      # Dummy model + anomalies (extend to MPC/control)
  /simulator       # Test publisher
/libs
  /nesshash-adapter  # Pluggable hashing for integrity ledger
/infra
  /db/init.sql     # Tables + indices
/mqtt            # Mosquitto config
/hardware
  /kicad           # Placeholders for schematics/PCBs
  /firmware/esp32  # PlatformIO skeleton (placeholder)
/docs              # Specs, safety, ops guides
```

## Licenses

- **Code:** AGPL-3.0-or-later (to keep derivatives open).
- **Hardware:** CERN-OHL-S-2.0 (strongly reciprocal for hardware).

See `LICENSES/` and file headers with `SPDX-License-Identifier` tags.

## Contributing

See [`CONTRIBUTING.md`](CONTRIBUTING.md). We welcome NGOs, researchers, makers, & civic hackers.

## Roadmap (MVP → v0.1)

- [ ] Baseline sensors: pH, TDS, turbidity, temperature, flow, pressure.
- [ ] Ingestion + integrity ledger (NessHash).
- [ ] Anomaly detection (simple stats → autoencoder).
- [ ] Local control loop (PID) with safe bounds.
- [ ] Open hardware rev A (KiCad) + BOM.
- [ ] Calibration & QA procedures.
- [ ] Deployment recipes (Pi + ESP32; solar option).
