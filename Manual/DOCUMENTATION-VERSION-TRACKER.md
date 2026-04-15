# RAIL CONTROL — DOCUMENTATION VERSION TRACKER

**Version:** v1.0

> Master reference for all RAIL CONTROL documentation versions. Update this file whenever any document is released or revised.
>
> **Last updated:** April 2026

---

## 1. VERSION TABLE

### Core Testing Regime (Stages 0–7)

| Document | File | Latest Version | Status | Last Updated |
|---|---|---|---|---|
| **Initial Setup** | `SETUP.md` | v1.0 | Stable | April 2026 |
| **Testing Index** | `TESTING-00-INDEX.md` | v1.0 | Stable | April 2026 |
| **Stage 0: Pi Baseline** | `TESTING-01-STAGE-0.md` | v1.0 | Stable | April 2026 |
| **Stage 1: I²C & Detection** | `TESTING-02-STAGE-1.md` | v1.0 | Stable | April 2026 |
| **Stage 2: Output Verification** | `TESTING-03-STAGE-2.md` | v1.0 | Stable | April 2026 |
| **Stage 3: LED Drivers** | `TESTING-04-STAGE-3.md` | v1.0 | Stable | April 2026 |
| **Stage 4: Relay & Turnout** | `TESTING-05-STAGE-4.md` | v1.0 | Stable | April 2026 |
| **Stage 5: Hall Sensors** | `TESTING-06-STAGE-5.md` | v1.0 | Stable | April 2026 |
| **Stage 6: Integration Soak** | `TESTING-07-STAGE-6.md` | v1.0 | Stable | April 2026 |
| **Stage 7: Stripboard Build** | `TESTING-08-STAGE-7.md` | v1.0 | Stable | April 2026 |

### Supplementary Guides (1–9)

| Document | File | Latest Version | Status | Last Updated |
|---|---|---|---|---|
| **Guide #1: Soldering** | `SOLDERING-GUIDE.md` | v1.0 | Stable | April 2026 |
| **Guide #2: Stripboard Layout** | `STRIPBOARD-LAYOUT-GUIDE.md` | v1.0 | Stable | April 2026 |
| **Guide #3: Multimeter Reference** | `MULTIMETER-REFERENCE-GUIDE.md` | v1.0 | Stable | April 2026 |
| **Guide #4: Troubleshooting Decision Tree** | `TROUBLESHOOTING-DECISION-TREE.md` | v1.0 | Stable | April 2026 |
| **Guide #5: Component Identification & Verification** | `COMPONENT-IDENTIFICATION-VERIFICATION.md` | v1.0 | Stable | April 2026 |
| **Guide #6: Breadboard Best Practices** | `BREADBOARD-BEST-PRACTICES.md` | v1.0 | Stable | April 2026 |
| **Guide #7: Cable Termination & Connector Prep** | `CABLE-TERMINATION-CONNECTOR-PREP.md` | v1.0 | Stable | April 2026 |
| **Guide #8: Enclosure Weatherproofing** | `ENCLOSURE-WEATHERPROOFING.md` | v1.0 | Stable | April 2026 |
| **Guide #9: GPIO Pinout Reference Card** | `GPIO-PINOUT-REFERENCE-CARD.md` | v1.0 | Stable | April 2026 |

### System Record & Tracking

| Document | File | Latest Version | Status | Last Updated |
|---|---|---|---|---|
| **System Record Sheet** | `SYSTEM-RECORD-SHEET.md` | v1.0 | Stable | April 2026 |
| **Documentation Version Tracker** | `DOCUMENTATION-VERSION-TRACKER.md` | v1.0 | Stable | April 2026 |

---

## 2. VERSIONING SCHEME

**Format:** `vX.Y`

- **v1.0** → Initial release
- **v1.1** → Minor fixes (typos, clarifications, no structural changes)
- **v1.2** → Bug fixes in procedures (corrections to measurements, pinouts, etc.)
- **v2.0** → Major revision (new sections, restructured content, significant updates)

---

## 3. CHANGELOG

### SETUP.md

| Version | Date | Changes |
|---|---|---|
| v1.0 | Apr 2026 | Initial release. Validated against live Pi 5 hardware. |

### TESTING-00-INDEX.md

| Version | Date | Changes |
|---|---|---|
| v1.0 | Apr 2026 | Initial release. Eight-stage testing regime overview. |

### TESTING-01-STAGE-0.md

| Version | Date | Changes |
|---|---|---|
| v1.0 | Apr 2026 | Initial release. Pi baseline verification. |

### TESTING-02-STAGE-1.md

| Version | Date | Changes |
|---|---|---|
| v1.0 | Apr 2026 | Initial release. I²C bus and MCP23017 detection. |

### TESTING-03-STAGE-2.md

| Version | Date | Changes |
|---|---|---|
| v1.0 | Apr 2026 | Initial release. Output pin walk test. |

### TESTING-04-STAGE-3.md

| Version | Date | Changes |
|---|---|---|
| v1.0 | Apr 2026 | Initial release. LED driver circuit verification. |

### TESTING-05-STAGE-4.md

| Version | Date | Changes |
|---|---|---|
| v1.0 | Apr 2026 | Initial release. Relay module and turnout motor testing. |

### TESTING-06-STAGE-5.md

| Version | Date | Changes |
|---|---|---|
| v1.0 | Apr 2026 | Initial release. Hall effect sensor configuration and testing. |

### TESTING-07-STAGE-6.md

| Version | Date | Changes |
|---|---|---|
| v1.0 | Apr 2026 | Initial release. Integrated 30-minute soak test. |

### TESTING-08-STAGE-7.md

| Version | Date | Changes |
|---|---|---|
| v1.0 | Apr 2026 | Initial release. Stripboard build and mechanical verification. |

### SOLDERING-GUIDE.md

| Version | Date | Changes |
|---|---|---|
| v1.0 | Apr 2026 | Initial release. Through-hole soldering techniques and troubleshooting. |

### STRIPBOARD-LAYOUT-GUIDE.md

| Version | Date | Changes |
|---|---|---|
| v1.0 | Apr 2026 | Initial release. Stripboard design, track cutting, component placement. British English: "organised" not "organized". |

### MULTIMETER-REFERENCE-GUIDE.md

| Version | Date | Changes |
|---|---|---|
| v1.0 | Apr 2026 | Initial release. Digital multimeter usage for voltage, current, continuity, resistance. British English: "colour" not "color". |

### TROUBLESHOOTING-DECISION-TREE.md

| Version | Date | Changes |
|---|---|---|
| v1.0 | Apr 2026 | Initial release. 10 decision trees covering all common RAIL CONTROL faults. Symptom-driven diagnosis. |

### COMPONENT-IDENTIFICATION-VERIFICATION.md

| Version | Date | Changes |
|---|---|---|
| v1.0 | Apr 2026 | Initial release. Visual ID and pre-installation verification for all 11 component types. Pinouts and functional tests. |

### BREADBOARD-BEST-PRACTICES.md

| Version | Date | Changes |
|---|---|---|
| v1.0 | Apr 2026 | Initial release. Breadboard organisation, power distribution, wire routing, testing techniques, stripboard migration. |

### CABLE-TERMINATION-CONNECTOR-PREP.md

| Version | Date | Changes |
|---|---|---|
| v1.0 | Apr 2026 | Initial release. Wire stripping, screw terminal installation, strain relief, weatherproofing, connector assembly. |

### ENCLOSURE-WEATHERPROOFING.md

| Version | Date | Changes |
|---|---|---|
| v1.0 | Apr 2026 | Initial release. IP65/IP68 enclosure selection, cable gland installation, DIN rail layout, thermal management, maintenance. |

### GPIO-PINOUT-REFERENCE-CARD.md

| Version | Date | Changes |
|---|---|---|
| v1.0 | Apr 2026 | Initial release. Printable quick-reference for Pi 5 GPIO, MCP23017 ports, I²C addresses, power budgets, electrical specs. |

### SYSTEM-RECORD-SHEET.md

| Version | Date | Changes |
|---|---|---|
| v1.0 | Apr 2026 | Initial release. Fillable form for GPIO assignments, LED mapping, cable routing, power config, stripboard build, testing milestones, issues/resolutions. |

---

## 4. HOW TO USE THIS TRACKER

**When updating a document:**

1. Increment the version number in the document footer: `v1.0` → `v1.1`
2. Update the "Latest Version" column in Section 1 (this file)
3. Add a new row to the document's changelog in Section 3
4. Update the "Last updated" date in the table (Section 1)
5. Commit to Git with a message like: `Update SETUP.md to v1.1 — clarify I2C wiring`

**When creating a new document:**

1. Set initial version to `v1.0`
2. Add a row to the version table (Section 1)
3. Create a new changelog section in Section 3
4. Add the document to Git

---

## 5. DOCUMENT STATUS MEANINGS

| Status | Meaning | Action |
|---|---|---|
| **Stable** | Tested and verified in the field. Unlikely to change. | Use as reference. Report issues via GitHub. |
| **In Review** | Recently updated, awaiting field validation. | Use with caution. May contain errors. |
| **Draft** | Incomplete or untested content. | Do not rely on this document yet. |
| **Deprecated** | Replaced by a newer version. Kept for historical reference. | Do not use. Reference the newer version instead. |

---

## 6. RELATED DOCUMENTATION

The following documents are not versioned here but are part of the RAIL CONTROL ecosystem:

- **README.md** — Project overview and bill of materials (in `/mnt/project/`)
- **rail-control-design-spec.md** — UI/UX specification (in `/mnt/project/`)
- **TSANA_FORGE_BRAND_GUIDE.md** — Branding and style standards (in `/mnt/project/`)

---

## 7. GITHUB RELEASE NOTES

When publishing a release to GitHub, reference this file in the release notes:

```
RAIL CONTROL v1.0 Release — Complete Documentation Suite
========================================================

**Total documents:** 24 (14 core + 9 guides + 1 record sheet)

### Core Testing Regime (Stages 0–7)
- SETUP.md (v1.0)
- TESTING-00-INDEX.md (v1.0)
- TESTING-01-STAGE-0.md through TESTING-08-STAGE-7.md (all v1.0)
- All validated against Raspberry Pi 5 hardware (rev 1.0+)

### Supplementary Guides (1–9)
- Soldering Guide (v1.0) — Through-hole techniques, troubleshooting
- Stripboard Layout Guide (v1.0) — Design, track cutting, component placement
- Multimeter Reference (v1.0) — Voltage, current, continuity, resistance measurement
- Troubleshooting Decision Tree (v1.0) — 10 symptom-driven diagnostic trees
- Component Identification & Verification (v1.0) — Pre-installation checks for all 11 components
- Breadboard Best Practices (v1.0) — Organisation, wiring, power distribution, testing
- Cable Termination & Connector Prep (v1.0) — Wire stripping, screw terminals, weatherproofing
- Enclosure Weatherproofing (v1.0) — IP65/IP68, cable glands, DIN rail, thermal management
- GPIO Pinout Reference Card (v1.0) — Printable quick-lookup, all pin assignments

### System Documentation
- System Record Sheet (v1.0) — Fillable form for configuration, testing, issues, sign-off
- Documentation Version Tracker (v1.0) — Master version control and changelog

### Key Features
- ✓ Tsana Forge branded throughout (consistent style, tone, visual identity)
- ✓ Metric-first measurements (mm, °C, metres) with imperial secondary
- ✓ GBP-first currency (£) with USD secondary
- ✓ British English throughout (colour, organised, metre, etc.)
- ✓ GNU GPL v3 licensed (free to use, modify, distribute)
- ✓ Ready for GitHub publication
- ✓ Designed for printable distribution and on-site reference
- ✓ Cross-referenced throughout for easy navigation

### Getting Started
1. Print or read SETUP.md for Raspberry Pi 5 initial setup
2. Follow TESTING-00-INDEX.md for stage-by-stage testing plan
3. Reference the appropriate Testing Stage guide as you build
4. Use supplementary guides (e.g. Soldering, Troubleshooting) as needed
5. Fill in System Record Sheet to document your specific configuration

See DOCUMENTATION-VERSION-TRACKER.md for complete version history and changelog.
```

---

*Tsana Forge — RAIL CONTROL | Documentation Version Tracker | v1.0 | https://tsana.net*
