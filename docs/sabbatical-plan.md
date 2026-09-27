# Sabbatical plan (Aug 2026)

Source: *csillaghy sabbatical plan aug 2026* (OneDrive PDF). This file is the
GitHub copy, annotated with where the work actually lives and what is already
done.

**Theme:** agentic software aspects of solar radio indirect imaging and
spectroscopy — André Csillaghy.

## General idea

Software development and data analysis are shifting from “pure” engineering
toward designing functions (sometimes called a rebirth of requirements
engineering). New observatories are starting (SKA, SMILE), while MWA, Solar
Orbiter, and e-Callisto already work. Current software may not scale; the
largest models cannot run in-house; agentic AI changes how scientists touch
data.

The six-month goal is to see how those pieces fit, accelerate heliophysics
tooling, and open Swiss collaborations on solar radio imaging.

## Season I — Imaging (October Perth – November/December Kanpur)

Based on science (Kansabanik et al. 2022, Sharma et al. 2022) and SKAO / SRCNet
engineering in Switzerland.

| Item | Repo | Status |
|------|------|--------|
| Design a system for seamless solar radio images (MCP server as a later idea) | umbrella + solar-burst-multiview | not started (MCP); imaging path started |
| Start from existing MWA packages; 2015 → 2024 campaign | [solar-burst-multiview](https://github.com/i4Ds/solar-burst-multiview) | 2015 Sharma reproduction in progress; 2024 not started |
| Calibration (MWA and SKA-Low teams) | solar-burst-multiview | not started as a gate |
| MWA stack on calculon, transfer to CSCS, extend for solar | solar-burst-multiview `docs/calculon-mwa.md` | containers on calculon GPU nodes; CSCS batch still open |
| SRCNet solar Detailed Science Cases | — | not started |
| Instrument-agnostic parts; SKA-MID / MeerKAT / ALMA / eOVSA (with Rohit) | — | not started |
| Scalability, CSCS, SKA-SDP, data-flow hardware | Karabo-Pipeline, CSCS | not started |
| Science goals with Benz, Krucker, CESRA | — | discussion, not in git |

**Realistic near-term checklist (slide 4 of the PDF), mapped:**

- Understand MWA as a user — in progress (Sharma 2022 products + notebooks)
- Demos — MWA demo uvfits on besso/calculon exists; solar imaging demo still blocked on data layout
- Calibration / self-cal — later phase
- Solar-specific software — `src/solarburst/` in solar-burst-multiview
- KARABO simulations — [Karabo-Pipeline](https://github.com/i4Ds/Karabo-Pipeline)
- SKA software stack — same
- Side project: SKA calibration on new hardware; install simulation pipeline

## Season II — e-Callisto (January–March, Mexico)

| Item | Repo | Status |
|------|------|--------|
| e-Callisto as context / alarm / interval selection | [ecallisto_ng](https://github.com/i4Ds/ecallisto_ng) | package exists; sabbatical use not started |
| AI burst classification (Vincenzo), paper, Mexican stations | ecallisto_ng | not started here |
| Dynamic range of Mexican stations | — | not started |
| Compare e-Callisto and imagers; network calibration | ecallisto_ng + solar-burst-multiview | not started |
| SDR tests (Spain / Udaipur / Mexico) as long-term replacement | — | not started |

## Outcomes

- Continuation student works 2027–2030
- Working prototype for agentic solar interferometry
- Updated e-Callisto analysis package
- Case study: 3–5 events jointly across facilities
- Publication and/or documentation
- Dummy proposal for an e-Callisto EU future (FASR-style)
- Basis for SNSF and/or EU data-analysis proposal

## What this umbrella is not

It is not a rename of solar-burst-multiview. That repo stays the MWA imaging
workhorse. e-Callisto, Karabo, and STIX stay in their own repositories.
