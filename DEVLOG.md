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

---

_Each session adds a new entry below with the date and what was done._
