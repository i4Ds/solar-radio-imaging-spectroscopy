# SKAO side: working notes (plan items 24–27)

Background, reading and findings for the SKAO items of
[the sabbatical plan](sabbatical-plan.md). Discussion goes in the GitHub
issues of this repo, not here.

## 24 — SDP

**SDP** (Science Data Processor) is the telescope-side pipeline: visibilities
in, Observatory Data Products out.

## 27 — SRCNet services

**SRCNet** is the regional network: archive, reprocessing, and the science
platform. It is not the correlator and it is not SDP.

Hands-on: the small SRCNet deployment shown at Swiss SKA Days (August 2026),
item 10. Swiss software context: KARABO, item 12. Item 13 (CSCS, SKA software
stack, hardware) is what comes after this understanding, not instead of it.

Read: [SRCNet Documents](https://confluence.skatelescope.org/display/SNC/SRCNet+Documents)
(SKAO Confluence, often restricted); [SRCNet update (Jan 2025)](https://www.skao.int/sites/default/files/documents/SKA%20SRCNet%20Update%202025Jan.pdf).
v0.1 science test cases are generic (imaging, catalogues), not solar:
[uksrc-developers/science-testcases-srcnetv01](https://github.com/uksrc-developers/science-testcases-srcnetv01).

## 25 — Solar DSC

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

## 26 — SKA-Low solar pipeline (sketch, one page)

After 24, 27 and 25. Data flow, not a new repo yet:

1. SKA-Low visibilities → SDP (item 24; standard cal/image as far as it goes).
2. Solar-specific layer (self-cal, high cadence, Stokes): precursor is
   P-AIRCARS on MWA, item 6.
3. Products into SRCNet (item 27) so the solar DSC (item 25) can be run there.

Do not fold this into `solar-burst-multiview`. That repo stays MWA. Put the
sketch in this umbrella under `docs/` until it needs code.
