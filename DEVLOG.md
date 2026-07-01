# Devlog — iPhone LiDAR Reverse

_Chronological, append-only log. Old entries are never edited — each session adds a new one below._

---

## Session 2026-07-01 — Day 1

### How this started

Came up while discussing building a DIY AGV (ROS + Nav2 + LiDAR) as a learning
project, separate from unrelated professional work. While looking at cheap
LiDAR options, the idea of reusing the LiDAR sensor from a broken iPhone
(12 Pro and later) came up — researched and confirmed:

- The sensor is "parts paired" to the specific device's serial number
- No known public project has already solved standalone reuse of this sensor
  (unlike screens/batteries, where commercial tools like JCID/Qianli iCopy
  already exist)
- This is a genuine research gap, not an already-solved problem

### Scope decision

- Open-ended research project, no deadline — could take years, that's fine
- Own hardware only (purchased/acquired broken iPhones), nothing third-party
- Building a full AGV was explicitly ruled out — conflict of interest with
  unrelated professional work
- Public repo from day 1 — legal risk assessed as low (reverse-engineering own
  hardware, with "right to repair" precedent in favor), documenting everything
  transparently (failures included)

### TODO

- [ ] Get the first iPhone 12 Pro (broken/parts) to start disassembly
- [ ] Research the specific LiDAR sensor chip on the 12 Pro (datasheet, protocol)
- [ ] Check whether any existing programmer tool (JCID/Qianli) already reads/
      writes this specific sensor's identity chip, even for a different purpose
- [ ] Define the first concrete experiment (e.g. reading raw sensor data while
      still inside the iPhone, before attempting extraction)

### Related prior art found

- **JCID/Qianli iCopy Plus** (commercial parts-pairing bypass tools): no public
  teardown or reverse-engineering of the tools themselves found anywhere —
  confirms this specific angle (their internal chip-cloning method) is closed
  and undocumented. Closest adjacent open project: `iCopy-X-Community/icopyx-teardown`
  on GitHub, but that's a *different* device (RFID/NFC card cloner built on a
  NanoPi NEO + Proxmark3), unrelated despite the similar name.
- **[Arvos](https://github.com/jaskirat1616/Arvos)** (Jaskirat Singh) — real,
  verified open-source project. Runs a WebSocket server *inside* the iPhone
  using ARKit/AVFoundation/CoreMotion (no hardware extraction, no parts-pairing
  involved at all), streaming raw LiDAR depth (0.5-5m, 10fps), RGBD (30fps),
  and IMU (200Hz) data over WiFi to any computer. Records in MCAP format,
  directly compatible with ROS 2 / Foxglove.
  - This solves the *practical* goal (cheap-ish LiDAR data for a robotics
    project) immediately, without touching hardware reverse-engineering at all.
  - Trade-off: requires a **working** iPhone (not broken parts) running the
    app — doesn't reduce hardware cost, just sidesteps the whole parts-pairing
    problem by not extracting anything.
  - Doesn't replace this project's original goal (standalone hardware reuse
    of a *broken* sensor), but is worth tracking as the closest thing to
    "someone already using iPhone LiDAR outside the box."

### Confirmed: exact chip breakdown of the LiDAR module

Verified against real sources (TechInsights, System Plus Consulting/Yole,
CNBC) — this is a real breakthrough, since it turns "the iPhone's LiDAR chip"
from a generic unknown into a specific, searchable set of parts:

| Component | Manufacturer | Notes |
|---|---|---|
| ToF sensor (SPAD array) | **Sony** | Sony IMX591 family, ~10.1µm pixel pitch. Sony won this socket over STMicroelectronics (who supplied the older Face ID proximity sensor) |
| VCSEL emitter | **Lumentum** | Custom multi-electrode design, >$100M deal with Apple from this alone |
| VCSEL driver IC | **Texas Instruments** | WLCSP (wafer-level chip-scale package), 5-side molded |
| Diffractive optical element (DOE) | **Himax** | Sits on top of the VCSEL, creates the dot pattern |

First appeared in the 2020 iPad Pro, carried over unchanged to iPhone 12 Pro.

Note: exact resolution figure is slightly inconsistent across sources (one
says "0.01MP" ≈ 10,000 px, a secondary summary said "30K array") — minor
discrepancy, doesn't affect the manufacturer identification above.

Next step this unlocks: search for the **Sony IMX591** datasheet/interface
docs directly, instead of searching generically for "iPhone LiDAR chip."

### Public commercial equivalent found — biggest lead so far

The exact Apple-customized chip (IMX590 for 12/13/14 Pro, IMX591 for 15 Pro
Max) has no public datasheet — Apple's version is proprietary/restricted, and
firms like TechInsights only sell the teardown data as expensive paid reports.

However, Sony sells **the same underlying 10µm SPAD Cu-Cu-stacked dToF
architecture** commercially for the automotive/industrial market, under
different part numbers, with real public documentation:

- **Sony IMX459** — automotive-grade stacked SPAD dToF sensor, 597x168
  (~100K pixels), 300m range, 905nm, ~6ns response. Distributed by
  RESTAR FRAMOS Technologies. Public brief datasheet found:
  [IMX459-AAMV-W-Rev.0.005 (PDF)](https://www.sunnywale.com/uploadfile/2024/0517/IMX459-AAMV-W-Rev.0.005_BSY%E7%AE%80%E4%BB%8B.pdf)
- **Sony IMX479** — similar family, up to 20fps, 520 dToF pixels using
  3x3 SPAD pixel groups for accuracy.

**Confirmed interface (verified against multiple sources):** MIPI CSI-2
serial output, 4-lane or 2-lane configurable, 10/12-bit RAW data, up to 60fps
support in the interface itself. Power: 3.3V (analog), 1.1V (digital), 1.8V
(interface I/O).

**Why this matters:** these are the closest real, publicly-documented proxy
for how the Apple LiDAR chip actually talks over the wire (MIPI CSI-2 for the
depth data payload, I2C/I3C for config/control) — since Apple's own chip
shares the same sensor architecture, just customized/relabeled. This is a
much better starting point than reverse-engineering from zero.

### Follow-up checks: dev kit availability + physical connector

- **No dev kit / breakout board found for IMX459 or IMX479.** Framos sells
  evaluation kits for many other Sony sensors (IMX412, IMX415, IMX455 — the
  common Raspberry Pi/industrial camera ones), but nothing for these two.
  Makes sense: they're B2B-only parts sold directly to automotive LiDAR
  manufacturers, not stocked for hobbyist purchase. Getting one to experiment
  with won't be as simple as ordering from a normal distributor.
- **No public pinout found for the iPhone's LiDAR module connector.**
  Confirmed it's a friction-fit connector (no screws), and that the iPhone 12
  (non-Pro) and 12 Pro share the same camera socket design despite different
  camera counts — but no pin-level documentation surfaced anywhere.
- Net effect: the chip identity (Sony SPAD family + MIPI CSI-2 protocol) is
  now solid, but the *practical path to get hands-on with the same silicon
  cheaply* is still an open problem — automotive parts aren't hobbyist-
  accessible the way Raspberry Pi camera sensors are.

---

_Each session adds a new entry below with the date and what was done._
