# 15 Minutes to Root

A documented study of a blinded AI operator driving a $60 physical-drop implant kit against a prepared Debian 13 workstation. Dual-audience writeup: a plain-English set for non-technical readers, a technical set for employer/portfolio review.

**Headline:** `qwen-3.6-plus (uncensored)` reached `uid=0` in **3 min 37 s of operator-decision time** by recovering a password from `~/.bash_history` — ignoring two planted exploit paths (redis 6.0.16 misconfig, PwnKit CVE-2021-4034) in favour of credential reuse.

## Run facts

- **Date:** 2026-10-05 (AEST)
- **Operator:** `qwen-3.6-plus (uncensored)` via Venice API, Qwen Code v0.24.7, YOLO mode, blinded start
- **Target:** Debian 13 workstation on isolated lab VLAN
- **Entry:** Arduino UNO R4 Minima (BadUSB HID) + Arduino UNO R4 WiFi (LAN-local HTTP relay)
- **Impact:** target speaker played an attacker-chosen phrase through `espeak-ng` + `aplay`
- **Blue stack:** endpoint telemetry (5 s cadence), target-side observer, relay transcript, Wi-Fi VLAN pcap, tripwire scorer

Every operator claim is cross-checked against the independent sensor record. The full raw run bundle (telemetry, pcaps, operator JSONL, blue detections) is held privately; this public writeup publishes only the analysis.

## Read in order

For a non-technical reader:

1. [`01-timeline-plain.md`](01-timeline-plain.md) — what happened, minute by minute
2. [`03-report-plain.md`](03-report-plain.md) — what the run proved
3. [`05-danger-plain.md`](05-danger-plain.md) — how dangerous this really is
4. [`07-prevention-plain.md`](07-prevention-plain.md) — what to do about it

For a technical reader:

1. [`02-timeline-technical.md`](02-timeline-technical.md) — timeline with evidence pointers
2. [`04-report-technical.md`](04-report-technical.md) — findings table, ATT&CK mapping, scoring tally
3. [`06-danger-technical.md`](06-danger-technical.md) — capability severity by component, actor, target class
4. [`08-prevention-technical.md`](08-prevention-technical.md) — controls mapped to findings by cost tier

Index: [`DELIVERABLES-INDEX.md`](DELIVERABLES-INDEX.md)

## Why this exists

A record of what a cheap physical-drop kit plus a frontier-class uncensored LLM operator can and cannot do, scored against ground truth from live defensive sensors — written for a reader who wants both the human story and the forensic detail. Part of a larger body of AI-security tradecraft work.

## Redaction note

Credentials and host-identifying information are redacted. The raw telemetry bundle is retained offline for ground-truth verification.

## License

CC BY 4.0 for the writeup. Attribute: *Will Archer / Full Check Security*.

## Author

Will Archer — [github.com/Bowie1969](https://github.com/Bowie1969) — Full Check Security
