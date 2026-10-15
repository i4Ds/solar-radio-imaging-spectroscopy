# Sabbatical plan (15 October 2026)

**Theme:** agentic software aspects of solar radio indirect imaging and
spectroscopy.

## Season I — Imaging (October Perth – November/December Kanpur)

Based on science (Kansabanik et al. 2022, Sharma et al. 2022) and SKAO / SRCNet
engineering in Switzerland.

🟢 done · 🟡 in progress · 🔴 not started

| # | Item | Repo | Status |
|---|------|------|--------|
| 1 | Understand MWA as a user | [solar-burst-multiview](https://github.com/i4Ds/solar-burst-multiview) | 🟢 done |
| 2 | MWA stack on the laptop, calculon, and CSCS (non-solar demo) | solar-burst-multiview `docs/calculon-mwa.md` | 🟢 done |
| 3 | Understand MWA calibration | solar-burst-multiview | 🟡 in progress |
| 4 | Reproduce Sharma et al. 2022 from existing MWA packages | solar-burst-multiview | 🟢 done |
| 5 | Solar extension of the MWA stack, and calibration / self-cal (MWA and SKA-Low teams) | solar-burst-multiview | 🟡 in progress (12 Oct: first quiet-Sun check of the brightness scale, 0.73 MK quiet disk at 148.5 MHz) |
| 6 | P-AIRCARS for MWA | P-AIRCARS | 🟡 in progress (runs on calculon since 8 Oct, 4 local patches; see solar-burst-multiview `docs/calculon-paircars.md`) |
| 7 | 2024 campaign: image all events in the MWA solar archive | STIX-MWA (selection) + solar-burst-multiview (imaging) | 🟡 in progress. Plan carefully first: it can use huge resources if it has to be redone. Start with a few test cases (strong bursts and quiet Sun); see [#21](https://github.com/i4Ds/solar-radio-imaging-spectroscopy/issues/21) |
| 7a | Candidate list: MWA Sun obs with e-Callisto-confirmed bursts, data already on calculon first | STIX-MWA `src/ecallisto_mwa_bursts.py` | 🟡 in progress (15 Oct: 32 ranked bursts already on calculon; the ranking misses 2022–2023 because it uses only ASSA_56/57) |
| 7b | e-Callisto picture per candidate, with the times and channels to image; reviewed before any calculon job | STIX-MWA | 🔴 not started |
| 7c | First three events imaged (P-AIRCARS self-cal), starting with 2024-12-25 type II, MWA 1419137296 (data and calibrators on calculon) | solar-burst-multiview | 🔴 not started |
| 7d | Rest of the reviewed candidates | solar-burst-multiview | 🔴 not started |
| 8 | Design a system for seamless solar radio images (MCP later) | umbrella + solar-burst-multiview | 🔴 not started |
| 9 | SRCNet solar Detailed Science Cases | same work as item 25 | 🟡 in progress (DSC-053, see item 25) |
| 10 | Work with the SRCNet small deployment shown at Swiss SKA Days (August 2026) | SRCNet (hands-on for item 27) | 🔴 not started |
| 11 | Instrument-agnostic parts (SKA-MID / MeerKAT / ALMA / eOVSA) | — | 🔴 not started |
| 12 | KARABO | [Karabo-Pipeline](https://github.com/i4Ds/Karabo-Pipeline) | 🔴 not started |
| 13 | Scalability: CSCS, SKA-SDP, SKA software stack, data-flow hardware, calibration on new hardware | Karabo-Pipeline, CSCS (after items 24 and 27) | 🔴 not started |
| 14 | Science goals with CESRA | [docs/science-direction.md](science-direction.md) | 🟡 in progress (draft v3) |
| 21 | A P-AIRCARS version that runs on a Mac | P-AIRCARS | 🔴 not started (no macOS wheel for `casatools`; runs in an Ubuntu container) |
| 22 | A P-AIRCARS version optimised for Mac GPUs | P-AIRCARS | 🔴 not started |
| 24 | Understand SDP | SKAO Confluence; [notes](skao-notes.md#24--sdp) | 🔴 not started |
| 27 | Understand SRCNet services | SKAO Confluence; items 10 and 12; [notes](skao-notes.md#27--srcnet-services) | 🔴 not started |
| 25 | Understand the solar DSC | DSC-053 on SKA GitLab; [notes](skao-notes.md#25--solar-dsc) | 🟡 in progress (13 Oct: runs end to end on an MWA scan, four local fixes) |
| 26 | Sketch a SKA-Low solar pipeline | this umbrella `docs/`; [notes](skao-notes.md#26--ska-low-solar-pipeline-sketch-one-page) | 🔴 not started |

Items 21 and 22 are numbered after Season II so items 15–20 stay as they are.

## SKAO side (items 24–27)

Order: SDP (24), SRCNet services (27), then the SKA-Low pipeline sketch (26).
The solar DSC (25) is already running and does not wait for 24 or 27.
Notes and findings: [skao-notes.md](skao-notes.md).

## Season II — e-Callisto (January–March, Mexico)

| # | Item | Repo | Status |
|---|------|------|--------|
| 15 | Test the SDR solution with the mini-spectrum analyzer | — | 🔴 not started |
| 16 | e-Callisto as context / alarm / interval selection | [ecallisto_ng](https://github.com/i4Ds/ecallisto_ng) | 🔴 not started |
| 17 | Dynamic range of Mexican stations | — | 🔴 not started |
| 18 | AI burst classification, paper, Mexican stations | ecallisto_ng | 🔴 not started |
| 19 | Compare e-Callisto and imagers; network calibration | ecallisto_ng + solar-burst-multiview | 🔴 not started (the e-Callisto confirmation from items 7a and 7b is reused here) |
| 20 | SDR tests (Spain / Udaipur / Mexico) as a long-term replacement | — | 🔴 not started |
| 23 | Align the FHNW tools with the other projects, e-Callisto first | [ecallisto_ng](https://github.com/i4Ds/ecallisto_ng), [FlareSense-v2](https://github.com/i4Ds/FlareSense-v2) | 🟡 started early (needed now, not only in January) |

Item 23 belongs to Season II but starts now, in parallel with Season I.

## Open questions

Discussed as GitHub issues with the label `question`:
[open questions](https://github.com/i4Ds/solar-radio-imaging-spectroscopy/issues?q=is%3Aissue+label%3Aquestion).

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
workhorse (Sharma reproduction and the non-solar MWA demo). e-Callisto,
Karabo, and STIX stay in their own repositories.

## General idea (reference)

Software development and data analysis are shifting from “pure” engineering
toward designing functions (sometimes called a rebirth of requirements
engineering). New observatories are starting (SKA, SMILE), while MWA, Solar
Orbiter, and e-Callisto already work. Current software may not scale; the
largest models cannot run in-house; agentic AI changes how scientists touch
data.

The six-month goal is to see how those pieces fit, accelerate heliophysics
tooling, and open Swiss collaborations on solar radio imaging. That includes
going into the large volume of MWA solar observations and making images of
all events.
