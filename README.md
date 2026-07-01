# Pathfinder-gobag

**The IR and Threat Hunting Workbench: Illuminating the Path in the Data**

A single HTML file you grab and go. No install, no server, no account. Open it in a browser and you are working. Everything saves locally in your browser and nothing leaves your machine. Sample threat hunt and incident JSON files are included in this repository for self-driven demos of all features.

---

## What it is

Pathfinder-gobag is a practitioner-first workbench for incident responders and threat hunters. Four integrated tools in one file, built to support the full arc of an investigation: from initial scoping through hunting, timeline reconstruction, and lessons learned.

---

## The four tools

### 1. Incident War Room

A place to digitally track evidence like sticky notes on a whiteboard. Track IOCs, evidence, pivots, and case notes using MIND and ABCs, create and track events in a timeline table, then push chosen events to a presentation ready killchain style timeline.

#### MIND IOC Tracker

Implements the MIND framework by [Patterson Cake (securecake.com)](https://securecake.com/f/mind-your-iocs), a four-category IOC organiser designed to surface what matters fast:

| Category | What goes here |
|---|---|
| **M** Memory | Processes, services, and loaded modules running in memory |
| **I** Identity | Compromised usernames, emails, accounts, and credentials |
| **N** Network | IPs, domains, URLs, C2 channels, and exfil destinations |
| **D** Disk | File hashes, malicious files, persistence paths, and registry keys |

IOCs detected in your ABCs evidence cards auto-populate the corresponding MIND category. You can also type directly into any MIND category and IOC detection runs on those notes automatically. Use MIND as a rapid reference during threat intel lookups or when briefing other analysts.

#### ABCs of Incident Response

A structured evidence matrix built on Mike Melone's pivot-point framework from the Microsoft Defender Virtual Ninja Training series ([watch the video](https://youtu.be/TTqFlnlwch0?si=hSDYUJe3Ywi4dCGK)).

Map what you know across four categories:

| Category | Key question |
|---|---|
| **A** Authentication | What identities were used, stolen, or modified, including any account that launched a malicious process? |
| **B** Back Doors | What mechanism did the attacker use to control the endpoint? |
| **C** Communication Channels | What sources and destinations were used to carry out the attack? |
| **D** Data | What was the confidentiality, integrity, or availability impact on data? |

Each category is tracked across three timeframes: **Initial Connection/Access**, **Post-Breach**, and **Impact Assessment**. When the web of indicators stops growing, the incident is fully scoped.

#### IOC Auto Detection

IOC detection runs automatically on everything you type. IPs, domains, hashes (MD5/SHA1/SHA256), file names, email addresses, URLs, CVEs, and phone numbers are detected and surfaced in a live IOC panel with per-type colour highlighting. Detection also runs on imported events and on every edit so the IOC list stays current automatically.

Defanged indicators are detected correctly. `hxxp://` and `hxxps://` are treated as URLs; domains with `[.]` or `(.)` dot substitution are matched as domains.

If auto-detection classifies an indicator incorrectly, click the ⇄ button on any IOC chip to reclassify it. The override persists across sessions and is included in JSON exports.

#### Timeline Anchors and Metrics

A collapsible section holds five timestamp anchors: Compromised, Detected, Contained, Eradicated, and Recovered. These feed the Incident Metrics panel which calculates MTTD, MTTC, MTTE, MTTR, and total incident duration.

#### Working Timeline

A timeline event builder at the bottom of the War Room lets you compose events from evidence cards and push chosen events to the Incident Timeline tab. Toggle the **Pres.** checkbox on each row to control which events appear in the presentation view. Paste screenshots directly into description fields with Ctrl+V.

---

### 2. Incident Timeline

The Incident Timeline tab is the presentation view: a curated, screenshot-ready render of the events you have toggled on in the War Room working timeline. Use this tab for briefings, screenshots into reports, and sharing with leadership. Add and manage events in the War Room.

Two display modes:

**Vertical** stacks cards in a scrollable list. Best for reading through the sequence and for screenshots of a complete timeline.

**Angled** uses a diagonal fishbone layout where events alternate above and below a central spine. Best for visual presentations and infographic style screenshots.

Events carry: timestamp, icon, title, source label, description/notes, and an optional Impact flag (💥) for high-significance pivots.

#### Angled Mode Controls

**Stagger** sets whether the first event card starts above or below the spine. All subsequent cards flip accordingly.

**Spine length (− and +)** shrinks or lengthens the spine while keeping its angle constant. All events redistribute proportionally. Use this to compress a busy timeline to fit a screenshot frame, stretch a short one to fill the canvas, or create space around the executive summary panel for a clean presentation screenshot.

**Telescope** extends the left end of the spine and moves the first event further left without affecting any other events. Each click steps the telescope out one position (1 through 4); the button shows the current step. A small × button resets it to 0 in one click. Use telescope to open space on the left side for the executive summary floating panel.

**Per-card side pin** lets you hover any card and click the ↕/▲/▼ button to lock that card above or below the spine regardless of the global stagger pattern.

**Nudge controls** appear on hover (←→↑↓) and let you fine-tune any card's position. A reset button (⊙) snaps it back to its computed position.

#### Executive Summary

Click **👑 Exec Sum Edit** in the toolbar to write or paste a plain-text summary. In vertical mode it appears as a pinned banner at the top. In angled mode it becomes a draggable, resizable floating panel. Toggle it on and off during briefings without losing the text.

#### Full Page Overlay

Click **Full Page** to render the timeline in a clean, borderless view filling the entire browser window. Pan by dragging, zoom with the toolbar +/− buttons. All toolbar controls including stagger, spine length, telescope, exec summary, card size, and highlights are available inside Full Page.

---

### 3. Hunting Theory Crafter

A hypothesis-driven threat hunt planner aligned with TaHiTI, PEAK, and SANS FOR508 methodology.

Each hunt theory has:

**Header fields**
- Icon, title (hypothesis statement), and status
- Hunt trigger: Intel-driven, Anomaly-driven, Situational-awareness, or Model-assisted
- Hypothesis type: Analytics-based, Intel-based, or Situational-awareness
- Hypothesis confidence: High, Medium, or Low
- CVE reference and threat intel URLs
- Premise (why we believe this is occurring)
- Hunt scope (environment, assets, data sources, timeframe)
- Evidence time window (start and end date)
- Data sources needed vs. data sources available

**MITRE ATT&CK Technique Chain**

Add techniques from the full embedded ATT&CK Enterprise library (over 310 techniques and sub-techniques, searchable by name or T-number, filterable by tactic). For each technique, record:

- Sensor coverage: **Detect** (alert exists), **Observe** (logs present, no alert), or **Gap** (no visibility)
- Expected IOCs
- Analyst notes
- Query ideas

A **coverage summary bar** at the top of the chain shows the Detect/Observe/Gap breakdown across all techniques at a glance.

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

When a theory is **Confirmed**, a **→ Push to ABCDs** button appears that pre-fills an ABCs evidence entry with the theory title, icon, CVE, and notes.

Chain layout toggles between vertical (full-width cards, detail-focused) and horizontal (scrollable strip, kill-chain overview).

---

### 4. Incident Checklist

The SANS PICERL lifecycle as an interactive reference and tracking tool:

**P**reparation · **I**dentification · **C**ontainment · **E**radication · **R**ecovery · **L**essons Learned

The Identification and Containment phases are fully interactive checklists with pre-loaded SANS-sourced items, checkboxes, timestamps, and a notes field per item. Preparation, Eradication, Recovery, and Lessons Learned are structured reference cards covering what should happen at each phase. Lessons Learned captures sustain and improve observations for the post-incident review.

---

## Getting started

### Windows and macOS

1. Download `pathfinder-gobag.html`
2. Double-click to open in your browser
3. Type an incident name in the header bar at the top
4. Start working; all data auto-saves as you type

### Linux

Double-click the file to open it in your browser. Works correctly in Chrome, Chromium, and system-package Firefox.

> **Firefox Flatpak on Wayland note:** If clicks do not register at the correct position, this is a known coordinate mapping issue with Flatpak-sandboxed Firefox and `file://` URLs on Wayland. Fix: run `python3 -m http.server 8080` in the directory and open `http://localhost:8080/pathfinder-gobag.html` instead. Non-Flatpak browsers are unaffected.

---

## Using the nav bar

The navigation bar at the top provides global controls available from any tab:

| Button | What it does |
|---|---|
| **📋 Copy Timeline as Table** | Copy the full timeline as a formatted HTML table for paste into Word, OneNote, or Google Docs |
| **TZ Adjust** | Shift all incident timestamps forward or back by a set number of hours with correct date rollover |
| **Export MD** | Export the full incident as a Markdown document for paste into Obsidian, Confluence, or any report |
| **Export JSON** | Export all data as JSON for backup, handoff, or re-import into another Pathfinder instance |
| **Import** | Import a previously exported JSON file; choose to replace all data, merge (add all), merge (skip duplicates), or review event by event |
| **Import Event** | Import a single shared event or hunt theory copied from a colleague's clipboard |
| **Clear All** | Reset everything to a blank slate; first click arms it (glows red), second click clears |
| Light/Dark toggle | Switch between dark and light mode |

---

## Import and export in detail

### Export JSON

Exports the complete incident state: incident name, all ABCDs evidence, MIND tracker entries, timeline events, hunt theories, PICERL checklist state, IOC settings, type overrides, and the executive summary. The file includes a version marker for forward compatibility.

Use it for:
- Handing off to another analyst mid-incident
- Saving a snapshot before making major changes
- Moving data between machines or browsers
- Sharing a full incident package with your team

### Export MD

Exports a structured Markdown document containing:
- Key timestamps (Detection, Containment)
- Full incident timeline as a table
- ABCDs evidence matrix per category and timeframe
- IOC list by type
- Hunt theories with technique chains and findings

Use it for:
- Pasting into Obsidian, Confluence, or Notion
- Drafting incident reports
- Archiving in a ticket or case management system

### Import

When you import a JSON file, you choose how it merges with your current data:

| Mode | Behaviour |
|---|---|
| **Replace all** | Clears everything and loads the imported file |
| **Merge, add all** | Keeps existing data and appends all incoming events regardless of overlap |
| **Merge, skip duplicates** | Keeps existing data and skips incoming events with titles that already exist |
| **Review and pick** | Steps through each incoming event one at a time; import or drop individually |

IOC detection re-runs automatically on all imported events and evidence cards.

### Per-event sharing

Every event card and hunt theory card has a share button (⤴) that copies a machine-readable text block to your clipboard. Paste it into a chat or email. Your colleague clicks **Import Event** and pastes it; the event imports directly without exchanging JSON files.

---

## IOC Highlighting

IOC detection runs automatically on all free-text fields across ABCDs, MIND, Timeline, and Hunt Theories, including on import and on every edit. Detected types:

- IP addresses (IPv4 and IPv6)
- Domains and hostnames, including defanged formats with `[.]` or `(.)` substitution
- URLs, including `hxxp://` and `hxxps://` defanged variants
- MD5, SHA1, and SHA256 hashes
- File names and executables (exe, dll, ps1, docm, and many more)
- Email addresses
- CVE identifiers
- Phone numbers

**Interacting with highlights:**
- **Click a highlight** to cycle through four display styles: coloured background, bold, bold with background, plain
- **Click 🎨 Colors** in the toolbar to open a per-type colour picker; changes apply live across the entire workbench
- **Click ⇄ on an IOC chip** to reclassify the indicator if auto-detection got the type wrong; the override is saved and exported with the incident
- **Click the eye icon** next to an IOC in the IOC panel to hide it from the panel without removing it from the evidence

---

## Data and privacy

Everything lives in your browser's `localStorage` under the `ir-` key prefix. Nothing is transmitted anywhere. The only network requests are:

- Loading **Tailwind CSS** from `cdn.tailwindcss.com` on first open (layout and styling)
- Any external links you click

The file works fully offline once it has been opened once with internet access (Tailwind is cached by the browser). For a fully air-gapped environment, open it once with internet access, then use the cached version thereafter.

---

## Framework credits

- **ABCs of Incident Response** — Mike Melone, Microsoft Defender Virtual Ninja Training ([watch the video](https://youtu.be/TTqFlnlwch0?si=hSDYUJe3Ywi4dCGK))
- **MIND Framework** — Patterson Cake ([securecake.com](https://securecake.com/f/mind-your-iocs)) ([Rapid Incident Response video](https://www.youtube.com/live/XfUjST9kXdU?si=1ykMZNeV9NQCk40B))
- **PICERL lifecycle** — SANS Incident Handler's Handbook
- **MITRE ATT&CK** — MITRE Corporation
- **TaHiTI** — ING / Fox-IT threat hunting methodology
- **PEAK** — Permiso Security threat hunting framework
- **SANS FOR508** — Advanced Incident Response, Threat Hunting, and Digital Forensics

---

*Built for the analyst in the field who needs to think clearly under pressure.*
