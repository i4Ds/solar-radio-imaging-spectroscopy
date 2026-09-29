# Sabbatical plan (29 September 2026)

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
| 3 | Reproduce Sharma et al. 2022 from existing MWA packages | solar-burst-multiview | 🟢 done |
| 4 | Solar extension of the MWA stack, and calibration / self-cal (MWA and SKA-Low teams) | solar-burst-multiview | 🟡 in progress |
| 5 | P-AIRCARS for SKA-Low | P-AIRCARS | 🟡 in progress (runs on this Mac) |
| 6 | 2024 campaign: image all events in the MWA solar archive | solar-burst-multiview | 🔴 not started |
| 7 | Design a system for seamless solar radio images (MCP later) | umbrella + solar-burst-multiview | 🔴 not started |
| 8 | SRCNet solar Detailed Science Cases | — | 🔴 not started |
| 9 | Work with the SRCNet small deployment shown at Swiss SKA Days (August 2026) | SRCNet | 🔴 not started |
| 10 | Instrument-agnostic parts (SKA-MID / MeerKAT / ALMA / eOVSA) | — | 🔴 not started |
| 11 | KARABO | [Karabo-Pipeline](https://github.com/i4Ds/Karabo-Pipeline) | 🔴 not started |
| 12 | Scalability: CSCS, SKA-SDP, SKA software stack, data-flow hardware, calibration on new hardware | Karabo-Pipeline, CSCS | 🔴 not started |
| 13 | Science goals with CESRA | — | 🟡 in progress (discussion, not in git) |

## Season II — e-Callisto (January–March, Mexico)

| # | Item | Repo | Status |
|---|------|------|--------|
| 14 | Test the SDR solution with the mini-spectrum analyzer | — | 🔴 not started |
| 15 | e-Callisto as context / alarm / interval selection | [ecallisto_ng](https://github.com/i4Ds/ecallisto_ng) | 🔴 not started |
| 16 | Dynamic range of Mexican stations | — | 🔴 not started |
| 17 | AI burst classification, paper, Mexican stations | ecallisto_ng | 🔴 not started |
| 18 | Compare e-Callisto and imagers; network calibration | ecallisto_ng + solar-burst-multiview | 🔴 not started |
| 19 | SDR tests (Spain / Udaipur / Mexico) as a long-term replacement | — | 🔴 not started |

## Open questions

1. **Imaging path for the archive.** Is P-AIRCARS (item 5) the pipeline for
   item 6 (all MWA solar events), or do we stay with Sharma-style scan-mean
   subtraction plus WSClean, or run both?
2. **What “all events” means.** Every solar obsid in the archive, or only
   burst-detected intervals? Cadence, bands, and Stokes still unset.
3. **Validation before scale.** Does Phase 3 (re-image Sharma visibilities
   with our stack) have to pass before item 6, or is matching Sharma’s
   figures (item 3) enough to move on?
4. **Where production imaging runs.** Calculon, CSCS, or both? The login node
   on calculon cannot run containers; jobs have to be `sbatch`/`srun`.
5. **Self-cal for 2024 MWAX data.** Item 4 is later; 2024 is MWAX, 2015 is
   legacy, and they differ in channelisation and metadata.
6. **MCP / agentic interface.** Item 7 is “later” — this season, or after
   images exist?
7. **How far to generalise.** MWA-first until item 6 works, or start
   instrument-agnostic design (item 10) in parallel?
8. **Paper Figures 7–12 / Table 2.** Region centres and counts are still
   approximate without a mapping from the original analysis. Is that
   required, or is the present match good enough?

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
