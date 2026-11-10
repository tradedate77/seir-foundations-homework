# Module 1 — Week 4

## Summary

_What this week covered:_

Lesson 2 homework — "Seven Alerts, Seven Wins". Seven scripts run against the
Balerica AI EC2 instance, each one sending an alert to the `#balerica-alerts`
Slack channel through `send_alert()` from `slack.py`. The set builds from a
single hard-coded greeting (hw1) up to a multi-line monitoring report generator
(hw7) that measures the real disk and load on the server.

Assignment: https://github.com/BalericaAI/SEIR-Foundations/tree/main/module1/lesson2_slack/homework

## Files

| File | What it is |
|------|------------|
| `Week4Terminal_Screenshot.jpg` | EC2 shell showing all seven scripts run in order, plus the Lesson 1 ID line |
| `Slack1.jpg` | `#balerica-alerts` — connectivity test (TEST-001) and the hw2 severity tour |
| `Slack2.jpg` | `#balerica-alerts` — hw3, hw4, hw5, and the first two hw6 incidents |
| `Slack3.jpg` | `#balerica-alerts` — hw6 incident drill (all three) and the hw7 morning report |

## Screenshots

_named for the step each one verifies:_

| File | Shows |
|------|--------|
| `Week4Terminal_Screenshot.jpg` | `hw1`–`hw7` each reporting `Delivered to Slack`, and the ID line `whoami && TZ='America/New_York' date && ec2-metadata -i` returning `root`, `Sun Nov 8 22:59:41 EST 2026`, `instance-id: i-0a0216044abc01834` |
| `Slack1.jpg` | TEST-001 connectivity test, then the four hw2 severities: INFO, WARNING, ERROR, CRITICAL (all HW-002) |
| `Slack2.jpg` | HW-003 (`hw3_server_time.py`), HW-004 (`hw4_disk_watch.py`), HW-005 (`hw5_load_check.py`), DRILL-3940 and DRILL-4539 (`hw6_incident_drill.py`) |
| `Slack3.jpg` | DRILL-3940, DRILL-4539, DRILL-6262 (`hw6_incident_drill.py`), and HW-007 (`hw7_morning_report.py`) |

## Notes

### Q1 (`hw2_severity_tour.py`) — Which severities arrived, and why those?

Four severities arrived: **INFO, WARNING, ERROR, CRITICAL**. These are the four
that were set up during the install process on the nano screen. hw2 sends one
message at each level, so all four should reach Slack — and all four did.

### Q2 (`hw3_server_time.py`) — How many hours apart are server time and your wall clock?

My clock said 10:44 pm Eastern on Sunday night, November 8th.

The server reported Monday, November 9, 2026 at 03:42:58 UTC.

That is **five hours apart** — Eastern is UTC−5.

### Q3 (`hw4_disk_watch.py`) — Why is a quiet INFO here good design, not a failure?

The screen showed the disk at **25.6% used**, with **5.9 GB free**.

It is good design because a quiet INFO indicates the message was delivered
successfully to Slack — the measurement ran, my if/else chose the severity, and
the alert went out. The message in Slack confirms the same thing. Nothing is
wrong with the disk, so nothing should shout.

### Q4 (`hw5_load_check.py`) — What would make this number spike on the Balerica bank?

If the load was a higher capacity than the traffic the bank could handle.

### Q5 (`hw6_incident_drill.py`) — Which of these fake incidents deserves to wake a human at 3 AM?

**1. COBOL batch job BATCH-EOD running 12 minutes longer than usual.**
It delivered successfully, so no need to wake a human at 3 am.

**2. FastAPI `/payments` endpoint responded 500 twice in one minute.**

**3. SWIFT traffic generator queue depth climbing: 42 messages waiting.**
This is the one that should wake a human up. It means SWIFT messages carrying
bank instructions are arriving but not being processed — payment instructions
sitting unprocessed. That can lead to trade settlement fails, which can lead to
fail claims that have to be paid by whoever was at fault. This deserves a human
at 3 am.

### Q6 (`hw7_morning_report.py`) — Prediction vs. result (experiment 1)

**Prediction:** Only one of the four messages would arrive. With
`SLACK_ALERT_SEVERITY=CRITICAL`, the threshold sits above INFO, WARNING and
ERROR, so those three are filtered out before they ever reach Slack. The
severity setting is a floor applied on send, not a label that upgrades a
message — so the CRITICAL alert goes through and the other three do not.

**Result:** That is exactly what happened — the CRITICAL alert arrived, and the
less serious alerts below the threshold did not.
