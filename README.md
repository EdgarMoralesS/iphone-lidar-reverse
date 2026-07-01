# iPhone LiDAR Reverse — reusing the LiDAR sensor from broken iPhones

## Goal

Investigate whether the LiDAR sensor in an iPhone (12 Pro and later) can be
extracted and reused standalone, outside the Apple ecosystem, with your own
electronics (Arduino/ESP32/Jetson/whatever).

This is an open-ended research project, no deadline — progress happens step by
step, session by session, and it might take years. That's fine.

## Why

- The sensor is "parts paired" to the specific device (serial number) — it
  doesn't even work properly when swapped between two identical iPhones.
- No public project (hacker, researcher, or repair community) was found that
  has already solved standalone reuse of this sensor — this is a genuine gap,
  not a solved problem that just needs better searching.
- The real cost isn't the hardware (a broken iPhone is cheap/free) — it's the
  reverse-engineering time the repair industry already invested for screens/
  batteries (see JCID, Qianli iCopy tools) but nobody invested for the LiDAR.

## Scope — what this IS and ISN'T

- ✅ Own hardware: broken iPhones purchased/acquired by the author, or loose parts
- ✅ Documenting the full process, unrushed, failures included
- ❌ NOT a project to build a full AGV from scratch (explicitly ruled out —
  direct conflict of interest with the author's day job working on Dahua/
  Hikvision AGVs)
- ❌ Does NOT involve copying/extracting proprietary third-party software
  (Apple, Dahua, etc.)

## Current status

Day 1 — repo created, no hardware in hand yet. See `DEVLOG.md` for the progress log.

## License

MIT — see [LICENSE](LICENSE).
