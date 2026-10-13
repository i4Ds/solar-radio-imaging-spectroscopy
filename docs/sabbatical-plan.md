# Sabbatical plan (13 October 2026)

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
| 6 | P-AIRCARS for MWA | P-AIRCARS | 🟡 in progress (runs on calculon since 8 Oct with 4 local patches, not upstream; self-calibrated images of the 2022-09-30 burst at 4 s and 2 s; 0.25 s series of the whole flare running since 12 Oct. Mac container runs) |
| 7 | 2024 campaign: image all events in the MWA solar archive | solar-burst-multiview | 🔴 not started |
| 8 | Design a system for seamless solar radio images (MCP later) | umbrella + solar-burst-multiview | 🔴 not started |
| 9 | SRCNet solar Detailed Science Cases | same work as item 25 | 🟡 in progress (DSC-053, see item 25) |
| 10 | Work with the SRCNet small deployment shown at Swiss SKA Days (August 2026) | SRCNet (hands-on for item 24) | 🔴 not started |
| 11 | Instrument-agnostic parts (SKA-MID / MeerKAT / ALMA / eOVSA) | — | 🔴 not started |
| 12 | KARABO | [Karabo-Pipeline](https://github.com/i4Ds/Karabo-Pipeline) | 🔴 not started |
| 13 | Scalability: CSCS, SKA-SDP, SKA software stack, data-flow hardware, calibration on new hardware | Karabo-Pipeline, CSCS (after item 24) | 🔴 not started |
| 14 | Science goals with CESRA | [docs/science-direction.md](science-direction.md) | 🟡 in progress (draft v3) |
| 21 | A P-AIRCARS version that runs on a Mac | P-AIRCARS | 🔴 not started (native macOS; `casatools==6.6.0.20` has no macOS wheel. Today it runs in an Ubuntu 22.04 x86_64 container) |
| 22 | A P-AIRCARS version optimised for Mac GPUs | P-AIRCARS | 🔴 not started (imaging is CPU WSClean in that guest; the Metal GPU is unused) |
| 24 | Understand SDP and SRCNet | SKAO Confluence; items 10 and 12 | 🟡 in progress |
| 25 | Understand the solar DSC | [ska-src-dsc-053-solar-dynaspec](https://gitlab.com/ska-telescope/src/sd/workflows/ska-src-dsc-053-solar-dynaspec) (SKA GitLab, branch `dev`); Confluence + [arXiv:2607.17881](https://arxiv.org/abs/2607.17881) | 🟡 in progress (13 Oct: DSC-053 runs end to end on an MWA picket-fence scan after four local fixes, not upstream) |
| 26 | Sketch a SKA-Low solar pipeline | this umbrella `docs/`; precursor item 6 | 🔴 not started |

Items 21 and 22 are numbered after Season II so items 15–20 stay as they are.

## SKAO side (now — items 24–26)

Do these three in order. They are the SKAO block of Season I.

| # | Item | Where | Status |
|---|------|--------|--------|
| 24 | Understand SDP and SRCNet | SKAO Confluence; Swiss SKA Days deployment (item 10); KARABO (item 12) | 🟡 in progress |
| 25 | Understand the solar DSC | DSC-053 workflow repo on SKA GitLab; Confluence space SRCSC. Public framing: [arXiv:2607.17881](https://arxiv.org/abs/2607.17881) | 🟡 in progress |
| 26 | Sketch a SKA-Low solar pipeline | this umbrella `docs/` until it needs code; precursor P-AIRCARS (item 6) | 🔴 not started |

### 24 — SDP and SRCNet

**SDP** (Science Data Processor) is the telescope-side pipeline: visibilities
in, Observatory Data Products out.

**SRCNet** is the regional network: archive, reprocessing, and the science
platform. It is not the correlator and it is not SDP.

Hands-on: the small SRCNet deployment shown at Swiss SKA Days (August 2026),
item 10. Swiss software context: KARABO, item 12. Item 13 (CSCS, SKA software
stack, hardware) is what comes after this understanding, not instead of it.

Read: [SRCNet Documents](https://confluence.skatelescope.org/display/SNC/SRCNet+Documents)
(SKAO Confluence, often restricted); [SRCNet update (Jan 2025)](https://www.skao.int/sites/default/files/documents/SKA%20SRCNet%20Update%202025Jan.pdf).
v0.1 science test cases are generic (imaging, catalogues), not solar:
[uksrc-developers/science-testcases-srcnetv01](https://github.com/uksrc-developers/science-testcases-srcnetv01).

### 25 — Solar DSC

Item 9 in the table above is the same work.

- **DSC-053 "Solar Dynamic Spectrum comparison with other observatories".**
  Workflow repo
  [ska-src-dsc-053-solar-dynaspec](https://gitlab.com/ska-telescope/src/sd/workflows/ska-src-dsc-053-solar-dynaspec),
  code on branch `dev` (`main` is empty). Text on Confluence:
  [DSC-053](https://confluence.skatelescope.org/display/SRCSC/DSC-053+Solar+Dynamic+Spectrum+comparison+with+other+observatories)
  and its
  [chronological walk-through](https://confluence.skatelescope.org/display/SRCSC/DSC-053+%3A+Chronological+walk-through).
  Steps: median over baselines of |V| normalised by the autocorrelations →
  dynamic spectrum FITS (24 picket-fence MSes merged) → RFI flagging →
  power-law fit per time step (spectral index vs time) → cut-out from another
  observatory (Yamagawa) for comparison.
- **13 Oct: runs end to end** on MWA 1424757616 (quiet Sun, 2025-02-28,
  picket fence, 4 s / 160 kHz), about 1 min on the laptop, conda env `dsc053`.
  Four bugs fixed on a local branch (`local/1424757616-test`, not pushed):
  flags ignored; autocorrelation normalisation divided by √A₂ only; flagger
  hard-coded for 32 fine channels with swapped axes; zero-σ channels stalled
  the fit. Still open: the flagger drops 131 of 192 channels; fine channels
  within one coarse channel differ by up to 5×; `CRPIX1 = 0` in the FITS
  header.
- **SRCNet workflow templates.** Sibling DSCs (057, 067, 008) are moving to
  [ska-src-workflow-templates](https://gitlab.com/ska-telescope/src/sd/utilities/ska-src-workflow-templates)
  (pydantic `ScienceWorkflow` steps with SWF IDs, uv, ruff). DSC-053 is not
  ported, and there is no dynamic-spectrum template yet.
- **Public science framing:** SKAO SHI working-group chapter for AASKA II,
  [arXiv:2607.17881](https://arxiv.org/abs/2607.17881). Standard SDP/SRCNet
  archive products do **not** cover solar flux/pol calibration, ≲50 ms burst
  imaging, or near-real-time space weather. A low-latency SHI layer *upstream
  of SRCNet* is likely needed.
- Item 10 is a **deployment**, not the DSC text.
- Item 14 (CESRA) is the physics. The DSC is the SRCNet/SKAO engineering case.

### 26 — SKA-Low solar pipeline (sketch, one page)

After 24 and 25. Data flow, not a new repo yet:

1. SKA-Low visibilities → SDP (standard cal/image as far as it goes).
2. Solar-specific layer (self-cal, high cadence, Stokes): precursor is
   P-AIRCARS on MWA, item 6.
3. Products into SRCNet so the solar DSC (item 25) can be run there.

Do not fold this into `solar-burst-multiview`. That repo stays MWA. Put the
sketch in this umbrella under `docs/` until it needs code.

## Season II — e-Callisto (January–March, Mexico)

| # | Item | Repo | Status |
|---|------|------|--------|
| 15 | Test the SDR solution with the mini-spectrum analyzer | — | 🔴 not started |
| 16 | e-Callisto as context / alarm / interval selection | [ecallisto_ng](https://github.com/i4Ds/ecallisto_ng) | 🔴 not started |
| 17 | Dynamic range of Mexican stations | — | 🔴 not started |
| 18 | AI burst classification, paper, Mexican stations | ecallisto_ng | 🔴 not started |
| 19 | Compare e-Callisto and imagers; network calibration | ecallisto_ng + solar-burst-multiview | 🔴 not started |
| 20 | SDR tests (Spain / Udaipur / Mexico) as a long-term replacement | — | 🔴 not started |
| 23 | Align the FHNW tools with the other projects, e-Callisto first | [ecallisto_ng](https://github.com/i4Ds/ecallisto_ng), [FlareSense-v2](https://github.com/i4Ds/FlareSense-v2) | 🟡 started early (needed now, not only in January) |

Item 23 belongs to Season II but starts now, in parallel with Season I.

## Open questions

1. **Imaging path for the archive.** Is P-AIRCARS (item 6) the pipeline for
   item 7 (all MWA solar events), or do we stay with Sharma-style scan-mean
   subtraction plus WSClean, or run both?
2. **What “all events” means.** Every solar obsid in the archive, or only
   burst-detected intervals? Cadence, bands, and Stokes still unset.
3. **Validation before scale.** Does Phase 3 (re-image Sharma visibilities
   with our stack) have to pass before item 7, or is matching Sharma’s
   figures (item 4) enough to move on?
4. **Where production imaging runs.** Calculon, CSCS, or both? The login node
   on calculon cannot run containers; jobs have to be `sbatch`/`srun`.
5. **Self-cal for 2024 MWAX data.** Item 5 is later; 2024 is MWAX, 2015 is
   legacy, and they differ in channelisation and metadata.
6. **MCP / agentic interface.** Item 8 is “later” — this season, or after
   images exist?
7. **How far to generalise.** MWA-first until item 7 works, or start
   instrument-agnostic design (item 11) in parallel?
8. **Paper Figures 7–12 / Table 2.** Region centres and counts are still
   approximate without a mapping from the original analysis. Is that
   required, or is the present match good enough?
9. **DI-cal vs P-AIRCARS for campaign data.** Hyperdrive on Pictor A for one
   2025 solar/cal pair did not produce a solar disk, and that calibrator
   obsid was the wrong grid. Self-cal (item 6) may be required before item 7.
10. **Tool alignment (item 23).** Which other projects and tools are in
    scope besides e-Callisto, and what does "aligned" mean: shared data
    formats, shared station metadata, a common API, or merged code?
11. **Where the solar DSC document is.** Answered 13 Oct: DSC-053 on
    Confluence (space SRCSC) and the workflow repo
    `ska-src-dsc-053-solar-dynaspec` (see item 25).

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
