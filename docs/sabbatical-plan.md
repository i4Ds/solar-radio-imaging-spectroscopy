# Sabbatical plan (29 September 2026)

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
tooling, and open Swiss collaborations on solar radio imaging. That includes
going into the large volume of MWA solar observations and making images of
all events.

## Season I — Imaging (October Perth – November/December Kanpur)

Based on science (Kansabanik et al. 2022, Sharma et al. 2022) and SKAO / SRCNet
engineering in Switzerland.

🟢 done · 🟡 in progress · 🔴 not started

| # | Item | Repo | Status |
|---|------|------|--------|
| 1 | Understand MWA as a user | [solar-burst-multiview](https://github.com/i4Ds/solar-burst-multiview) | 🟢 done |
| 2 | Understand the P-AIRCARS imaging pipeline | solar-burst-multiview | 🔴 not started |
| 3 | MWA stack on the laptop, calculon, and CSCS (non-solar demo, then solar) | solar-burst-multiview `docs/calculon-mwa.md` | 🟡 in progress (containers on all three; one solar snapshot imaged, disk not recovered) |
| 4 | Reproduce Sharma et al. 2022 from existing MWA packages | solar-burst-multiview | 🟡 in progress (Phases 0–1 done; Phase 2 open) |
| 5 | Solar-specific software (`src/solarburst/`) | solar-burst-multiview | 🟡 in progress |
| 6 | Calibration / self-cal (MWA and SKA-Low teams) | solar-burst-multiview | 🔴 not started |
| 7 | 2024 campaign: image all events in the MWA solar archive | solar-burst-multiview | 🔴 not started |
| 8 | Design a system for seamless solar radio images (MCP later) | umbrella + solar-burst-multiview | 🔴 not started |
| 9 | SRCNet solar Detailed Science Cases | — | 🔴 not started |
| 10 | Instrument-agnostic parts (SKA-MID / MeerKAT / ALMA / eOVSA, with Rohit) | — | 🔴 not started |
| 11 | Scalability: CSCS, SKA-SDP, KARABO, data-flow hardware | [Karabo-Pipeline](https://github.com/i4Ds/Karabo-Pipeline), CSCS | 🔴 not started |
| 12 | SKA software stack; side project: calibration on new hardware | Karabo-Pipeline | 🔴 not started |
| 13 | Science goals with Benz, Krucker, CESRA | — | 🟡 in progress (discussion, not in git) |

### Imaging pipeline setup (29 September 2026)

Item 3. The containers run on the laptop (Docker/`mwa` under Colima), on
calculon (Singularity), and on CSCS (Podman). The laptop notes and the solar
trial are in solar-burst-multiview
[`docs/calculon-mwa.md`](https://github.com/i4Ds/solar-burst-multiview/blob/cursor/calculon-mwa-containers/docs/calculon-mwa.md).

One archive scan was used to test that laptop path, not to reproduce Sharma.
Observation **1424757616** (Oberoi2024B_Sun, 2025-02-28, 176 s) was copied
from the shared calculon archive and calibrated from Pictor A **1424775768**,
five hours later. ASVO had already run birli. Hyperdrive `solutions-apply`
worked: the peak went from speckles at about 4 Jy/beam to about 2×10⁵ Jy/beam.

The image is still a grating-lobe stripe about 1.6° from the Sun. The scan is
a picket fence of 1.28 MHz channels with gaps of about 8 MHz, so extra
channels do not smear the lobe, and three minutes of Earth rotation do not
fill the uv plane. A solar disk is not recoverable from this snapshot.
Self-calibration on the Sun has not been tried. Item 6 stays not started.

## Season II — e-Callisto (January–March, Mexico)

| # | Item | Repo | Status |
|---|------|------|--------|
| 14 | e-Callisto as context / alarm / interval selection | [ecallisto_ng](https://github.com/i4Ds/ecallisto_ng) | 🔴 not started |
| 15 | Dynamic range of Mexican stations | — | 🔴 not started |
| 16 | AI burst classification (Vincenzo), paper, Mexican stations | ecallisto_ng | 🔴 not started |
| 17 | Compare e-Callisto and imagers; network calibration | ecallisto_ng + solar-burst-multiview | 🔴 not started |
| 18 | Test the SDR solution with the mini-spectrum analyzer | — | 🔴 not started |
| 19 | SDR tests (Spain / Udaipur / Mexico) as a long-term replacement | — | 🔴 not started |

## Open questions

1. **Imaging path for the archive.** Is P-AIRCARS (item 2) the pipeline for
   item 7 (all MWA solar events), or do we stay with Sharma-style scan-mean
   subtraction plus WSClean, or run both?
2. **What “all events” means.** Every solar obsid in the archive, or only
   burst-detected intervals? Cadence, bands, and Stokes still unset.
3. **Validation before scale.** Does Phase 3 (re-image Sharma visibilities
   with our stack) have to pass before item 7, or is matching Sharma’s
   figures (item 4) enough to move on?
4. **Where production imaging runs.** Calculon, CSCS, or both? The login node
   on calculon cannot run containers; jobs have to be `sbatch`/`srun`.
5. **Self-cal for 2024 MWAX data.** Item 6 is later; 2024 is MWAX, 2015 is
   legacy, and they differ in channelisation and metadata.
6. **MCP / agentic interface.** Item 8 is “later” — this season, or after
   images exist?
7. **How far to generalise.** MWA-first until item 7 works, or start
   instrument-agnostic design (item 10) in parallel?
8. **Sharma Figures 7–12 / Table 2.** Region centres and counts are still
   approximate without Rohit’s mapping. Is that required, or is the present
   match good enough?

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
