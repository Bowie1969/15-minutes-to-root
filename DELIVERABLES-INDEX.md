# Deliverables — arduino-relay-lab run, 2026-10-05

Two versions of each of the four documents. Pick by audience.

## Plain-English (jargon-free)

For: anyone — friend, family, non-technical client, a 50-year-old office manager.

| # | File | Purpose |
|---|---|---|
| 01 | [`01-timeline-plain.md`](01-timeline-plain.md) | What happened, minute by minute |
| 03 | [`03-report-plain.md`](03-report-plain.md) | What the run proved |
| 05 | [`05-danger-plain.md`](05-danger-plain.md) | How dangerous this really is |
| 07 | [`07-prevention-plain.md`](07-prevention-plain.md) | What to do about it |

## Technical (employer-facing / portfolio)

For: a hiring manager at an AI-security or security-automation role; a client
engaging Full Check Security; a peer reviewer.

| # | File | Purpose |
|---|---|---|
| 02 | [`02-timeline-technical.md`](02-timeline-technical.md) | Minute-by-minute run with evidence pointers |
| 04 | [`04-report-technical.md`](04-report-technical.md) | Formal engagement report, findings table, ATT&CK mapping |
| 06 | [`06-danger-technical.md`](06-danger-technical.md) | Capability severity by component, actor, target class |
| 08 | [`08-prevention-technical.md`](08-prevention-technical.md) | Controls mapped to findings, by cost tier |

## Ground truth under this bundle

Everything above is grounded on the run artefacts in `~/relaylab-run-20261005/`:
`blue/detections.jsonl`, `relay/live-events.log`, `target/groundzero/observations.jsonl`,
`telemetry/telemetry.jsonl`, `operator/261e74ba-….jsonl`, the gateway VLAN pcaps in
`network/`, and the pre-run `threat-model.md`.

See [`../INDEX.md`](../INDEX.md) for the full artefact index.

## The run in one line

A $60 Arduino-pair physical drop + a blinded uncensored LLM operator reached
`uid=0` on an unlocked Debian 13 workstation in ≈ 3 min 37 s of operator time by
recovering a password from `~/.bash_history` — a path the lab did not pre-plant,
preferred over the planted redis/pkexec vulns, and witnessed independently by
every blue sensor.
