# 🧭 Pathfinder-gobag

**The IR and Threat Hunting Workbench — Illuminating the Path in the Data**

A single HTML file you grab and go. No install, no server, no account. Open it in a browser and you're working. Everything saves locally in your browser — nothing leaves your machine.

---

## What it is

Pathfinder-gobag is a practitioner-first workbench for incident responders and threat hunters. Four integrated tools in one file, built to support the full arc of an investigation — from initial scoping through hunting, timeline reconstruction, and lessons learned.

---

## The four tools

### 1 — ABCs of Incident Response

A structured evidence matrix built on Mike Melone's pivot-point framework from the Microsoft Defender Virtual Ninja Training series ([watch the video](https://youtu.be/TTqFlnlwch0?si=hSDYUJe3Ywi4dCGK)).

Map what you know across four categories:

| Category | Key question |
|---|---|
| **A** — Authentication | What identities were used, stolen, or modified — including any account that launched a malicious process? |
| **B** — Back Doors | What mechanism(s) did the attacker use to control the endpoint? |
| **C** — Communication Channels | What sources and destinations were used to carry out the attack? |
| **D** — Data | What was the confidentiality, integrity, or availability impact of any data lost? |

Each category is tracked across three timeframes: **Initial Connection/Access**, **Post-Breach**, and **Impact Assessment**. When the web of indicators stops growing, the incident is fully scoped.

**IOC auto-detection** runs on everything you type. IPs, domains, hashes (MD5/SHA1/SHA256), file names, email addresses, URLs, CVEs, and phone numbers are detected and surfaced in a live IOC panel with per-type colour highlighting. Detection also runs on imported events and when you edit existing evidence — the IOC list stays current automatically.

**Defanged IOC support** — indicators written in defanged format are detected correctly. `hxxp://` and `hxxps://` are treated as URLs; domains with `[.]` or `(.)` dot substitution (e.g. `evil[.]com`) are matched as domains.

**IOC type override** — if auto-detection classifies an indicator incorrectly, click the ⇄ button on any IOC chip to reclassify it. The override persists across sessions and is included in JSON exports.

**Scope indicator** at the bottom of the grid turns green per-letter as evidence is added and shows a "Fully Scoped" badge when all four categories have at least one entry.

**Timeline Anchors & Metrics** — a collapsible section (collapsed by default) holds five timestamp anchors: Compromised, Detected, Contained, Eradicated, and Recovered. These feed the Incident Metrics panel which calculates MTTD, MTTC, MTTE, MTTR, and total incident duration. The collapsed header shows how many anchors are filled and the current MTTD at a glance. Expanding the section auto-opens all anchors and metrics.

**Timeline Event Builder** — a dedicated builder panel in the ABCs tab lets you compose timeline events from selected evidence cards and push them to the Incident Timeline.

---

### 2 — Hunting Theory Crafter

A hypothesis-driven threat hunt planner aligned with TaHiTI, PEAK, and SANS FOR508 methodology.

Each hunt theory has:

**Header fields**
- Icon, title (hypothesis statement), and status
- Hunt trigger: Intel-driven · Anomaly-driven · Situational-awareness · Model-assisted
- Hypothesis type: Analytics-based · Intel-based · Situational-awareness
- Hypothesis confidence: High · Medium · Low
- CVE reference(s) and threat intel URLs
- Premise (why we believe this is occurring)
- Hunt scope (environment, assets, data sources, timeframe)
- Evidence time window (start and end date)
- Data sources needed vs. data sources available

**MITRE ATT&CK technique chain** — add techniques from the full embedded ATT&CK Enterprise library (~310 techniques and sub-techniques, searchable by name or T-number, filterable by tactic). For each technique, record:
- Sensor coverage: **Detect** (alert exists) · **Observe** (logs present, no alert) · **Gap** (no visibility)
- Expected IOCs
- Analyst notes
- Query ideas

A **coverage summary bar** at the top of the chain shows the breakdown across all techniques at a glance.

**Status options**

| Status | Meaning |
|---|---|
| Open | Not yet started |
| Active | Hunt in progress |
| Confirmed | Threat activity found |
| Denied | Theory ruled out |
| Inconclusive | Insufficient data |
| Productized | Converted to a scheduled detection or playbook |
| Data Gap | Hunt revealed missing telemetry; log source required |

When a theory is **Confirmed**, a "Push to ABCs" button appears that pre-fills an ABCs evidence entry with the theory title, icon, CVE, and notes.

**Chain layout** toggles between vertical (full-width cards, detail-focused) and horizontal (scrollable strip, kill-chain overview).

---

### 3 — Incident Timeline

A chronological event log with two display modes:

- **Vertical** — stacked cards, best for active note-taking and detail work
- **Angled** — diagonal fishbone layout, best for screenshots and presentations

Events carry: timestamp, icon, title, source label, description/notes, ABCD category badge, and an optional **Impact** flag (💥) for high-significance pivots.

**Executive Summary** — add a plain-text executive summary from the timeline toolbar. In vertical mode it appears as a pinned panel at the top of the timeline. In angled mode it becomes a draggable, resizable floating panel (drag the title bar to move, drag the corner grip to resize) — useful for keeping context visible during a briefing. The panel can be toggled on and off without losing the text, and is included in JSON exports.

**Full-screen Presentation overlay** — renders the timeline in a clean, screenshot-ready view. Use it for briefings, screenshots into reports, or sharing with leadership.

**Copy Table** — copies the timeline as a formatted HTML table for direct paste into Word or OneNote, preserving column structure. Falls back to tab-separated plain text if the clipboard API is unavailable.

**Timezone Adjust** — a nav bar button opens a stepper that shifts every timestamp in the incident (timeline events, evidence cards, and anchor rows) by ±1–48 hours with correct date rollover. Use it when you receive data in a different timezone and need to normalise everything at once.

---

### 4 — Incident Checklist

The SANS PICERL lifecycle as an interactive reference:

**P**reparation · **I**dentification · **C**ontainment · **E**radication · **R**ecovery · **L**essons Learned

The I and C phases are fully interactive checklists with pre-loaded SANS-sourced items, checkboxes, timestamps, and a notes field per item. P, E, R, and L are reference cards covering what should happen in each phase. Lessons Learned captures sustain/improve observations for the post-incident review.

---

## Getting started

### Windows and macOS

1. Download `pathfinder-gobag.html`
2. Double-click to open in your browser
3. Type an incident name in the header bar at the top
4. Start working — all data auto-saves as you type

### Linux

Double-click the file to open it in your browser. Works correctly in Chrome, Chromium, and system-package Firefox.

> **Firefox Flatpak on Wayland note:** If clicks don't register at the correct position, this is a known coordinate mapping issue with Flatpak-sandboxed Firefox and `file://` URLs on Wayland. Fix: run `python3 -m http.server 8080` in the directory and open `http://localhost:8080/pathfinder-gobag.html` instead. Non-Flatpak browsers are unaffected.

---

## Using the nav bar

The navigation bar at the top provides global controls available from any tab:

| Button | What it does |
|---|---|
| **TZ Adjust** | Shift all incident timestamps (timeline, evidence, anchors) forward or back by a set number of hours — handles date rollover automatically |
| **Export MD** | Export the full incident as a Markdown document — paste into Obsidian, Confluence, GitHub, or any report |
| **Export JSON** | Export all data as JSON for backup, handoff, or re-import into another Pathfinder instance |
| **Import** | Import a previously exported JSON file — choose to replace all data, merge (add all), merge (skip duplicates), or review event-by-event |
| **Import Single Event** | Import a single shared event or hunt theory copied from a colleague's clipboard |
| **✕ Clear All** | Reset everything to a blank slate — first click arms it (glows red, asks "You sure?"), second click clears |
| 🌙 / ☀ | Toggle between dark and light mode |

---

## Import and export in detail

### Export JSON — for re-import and handoff

Exports the complete incident state: incident name, all ABCDs evidence, timeline events, hunt theories, PICERL checklist state, IOC settings, type overrides, and the executive summary. The file includes a version marker for forward compatibility.

**Use it for:**
- Handing off to another analyst mid-incident
- Saving a snapshot before making major changes
- Moving data between machines or browsers
- Sharing a full incident package with your team

### Export MD — for reports and documentation

Exports a structured Markdown document containing:
- Key timestamps (Detection, Containment)
- Full incident timeline as a table
- ABCDs evidence matrix per category and timeframe
- IOC list by type
- Hunt theories with technique chains and findings

**Use it for:**
- Pasting into Obsidian, Confluence, or Notion
- Drafting incident reports
- Archiving in a ticket or case management system

### Import

When you import a JSON file, you choose how it merges with your current data:

| Mode | Behaviour |
|---|---|
| **Replace all** | Clears everything and loads the imported file — equivalent to starting fresh from the export |
| **Merge — add all** | Keeps existing data and appends all incoming events regardless of overlap |
| **Merge — skip duplicates** | Keeps existing data and skips incoming events with titles that already exist |
| **Review and pick** | Steps through each incoming event one at a time — import or drop individually |

If two events share a timestamp but have different titles, you're prompted to resolve the conflict before it's added. IOC detection re-runs automatically on all imported events and evidence cards.

### Import Single Event — single-event sharing

Every event card and hunt theory card has a share button (⤴) that copies a machine-readable text block to your clipboard. Paste it into a chat or email. Your colleague clicks **Import Single Event** and pastes it — the event imports directly without needing to exchange JSON files.

---

## IOC highlighting

IOC detection runs automatically on all free-text fields across ABCDs, Timeline, and Hunt Theories — including on import and on every edit. Detected types:

- IP addresses
- Domains and hostnames — including defanged formats with `[.]` or `(.)` substitution
- URLs — including `hxxp://` and `hxxps://` defanged variants
- MD5, SHA1, and SHA256 hashes
- File names and executables (exe, dll, ps1, docm, and many more)
- Email addresses
- CVE identifiers
- Phone numbers

**Interacting with highlights:**
- **Click a highlight** — cycles through four display styles: coloured background → bold → bold + background → plain
- **Click 🎨 Colors** in the timeline or ABCs toolbar — opens a per-type colour picker. Each button opens its own picker independently; changing colours updates live across the entire workbench
- **Click ⇄ on an IOC chip** — opens a type picker to reclassify the indicator (e.g. if a file name was detected as a domain). The override is saved and exported with the incident
- **Click the eye icon** next to an IOC in the IOC panel — hides that specific indicator from the panel without removing it from the evidence

All colour choices and type overrides persist in localStorage alongside your data.

---

## Data and privacy

Everything lives in your browser's `localStorage` under the `ir-` key prefix. Nothing is transmitted anywhere. The only network requests are:

- Loading **Tailwind CSS** from `cdn.tailwindcss.com` on first open (layout and styling)
- Any external links you click

The file works fully offline once it has been opened once with internet access (Tailwind is cached by the browser). For a fully air-gapped environment, open it once with internet access, then export and use the cached version.

---

## Framework credits

- **ABCs of Incident Response** — Mike Melone, Microsoft Defender Virtual Ninja Training ([watch the video](https://youtu.be/TTqFlnlwch0?si=hSDYUJe3Ywi4dCGK))
- **PICERL lifecycle** — SANS Incident Handler's Handbook
- **MITRE ATT&CK** — MITRE Corporation
- **TaHiTI** — ING / Fox-IT threat hunting methodology
- **PEAK** — Permiso Security threat hunting framework
- **SANS FOR508** — Advanced Incident Response, Threat Hunting, and Digital Forensics

---

*Built for the analyst in the field who needs to think clearly under pressure.*
