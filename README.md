# SWIFT — Secure Weather Information Fetch & Transfer

## Overview
SWIFT is a 24x7x365 automated pipeline that retrieves, formats, and delivers METAR/TAF weather data for 18 aviation hubs, replacing a fully manual process that consumed roughly 15 hours of analyst time every single day.

## The Problem
Our team monitors weather data for 18 ICAO stations from multiple countries across rotating 24/7 shifts. Every 30 minutes, an analyst had to manually fetch the latest METAR and TAF reports for each station, format them to the client's required specification, and upload them via SFTP. This ran continuously — nights, weekends, holidays — with zero exceptions.

The manual cycle consumed roughly one full analyst position's worth of capacity every day, time that should have gone toward actual NOTAM analysis rather than repetitive data movement.

## The Solution
SWIFT automates the complete fetch → format → sync → upload cycle with zero manual intervention. It runs continuously, handling all 18 stations on a 30-minute cycle without an analyst touching it.

## Key Features
- **24x7x365 autonomous operation** — no manual triggering required
- **Intelligent error classification** — distinguishes genuinely missing or offline data from expected exemptions such as irregular reporting stations, weekend-only schedules, and hourly-only stations, preventing false-alarm fatigue
- **Automated shift reporting** — sends a clear formatted summary every cycle so the incoming team knows station status at a glance without digging through raw logs
- **Self-healing session management** — automatically detects and recovers from session and connectivity issues mid-cycle rather than requiring manual restart
- **State tracking** — persists last-known timestamps per station so reruns never duplicate or miss data
- **Auto-deprioritization** — automatically reduces retry cycles for stations that go offline, preventing wasted processing time

## Data Sourcing — A Note on Responsible Access
Several monitored stations do not have a standard public API. Where this was the case, I proactively flagged the access approach to management as an area requiring oversight rather than treating it as a purely technical implementation detail. In parallel, I contacted the source meteorological authority directly to request a sanctioned public API for ongoing access — treating data governance as part of the engineering responsibility, not an afterthought. This request is currently pending response.

## Technical Stack
Python, Playwright, curl_cffi, python-dotenv, smtplib, pyautogui

## Impact

| Metric | Result |
|---|---|
| Manual effort eliminated | ~15 hours per day |
| Annual hours saved | 5,000+ |
| Stations covered | Scaled from 5 to 18 |
| Manual intervention required | Zero |
| Recognition | Star Performer of the Month — March 2026 |

## Current Status
SWIFT is currently operational in production across all 18 ICAO stations. Following a brief 24-48 hour pause after a WAF update to Indonesian data sources introduced intermittent blocking, the system resumed normal operations without script modification — suggesting the interruption was a temporary enforcement fluctuation rather than a permanent access change. Monitoring continues, and investigation into formal API partnerships with source meteorological authorities remains ongoing as a long-term sustainable sourcing solution.

## What I Learned
Building and operating SWIFT taught me that automation is not just about writing a script that works once. It is about building something a team can trust in production — clear error reporting, graceful degradation when a data source becomes unreliable, and being honest about the sustainability of an access method rather than quietly patching around problems indefinitely. When the primary access method became less reliable, the right response was to evaluate alternate data feeds and pursue the formal API request rather than escalating workarounds.
