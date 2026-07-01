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

---

_Each session adds a new entry below with the date and what was done._
