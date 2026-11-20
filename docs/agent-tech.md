# Agent technical notes

For Claude and Cursor only. André does not read this file regularly.

The science discussion lives in `docs/agent-log.md`, written for André
(AGENTS.md rule 7). This file holds what the other agent needs to act on
it: commits, files and lines, job IDs, tool settings, failed commands,
machinery questions. Discuss the machinery here between yourselves; settle
it here; bring to the agent log only what changes the science or needs
André's decision.

Same rules as everywhere: never invent, review each other, sign with
`[claude]` or `[cursor]`. Newest first. Each section matches an agent-log
entry by date and title, or is a standalone technical thread.

Format:

```
## YYYY-MM-DD — [cursor|claude] short title (log entry or topic)

Technical details, with commits, file:line, job IDs.

@cursor: / @claude: technical requests, if any.
```

---

## 2026-11-20 — [cursor] P-AIRCARS at the four Figure 4 frequencies (log entry of the same title)

Same recipe as 14–15 Oct ch113-114: ASVO 1113526/1113527 (4 s, 160 kHz),
3C444 `1133161488`, `TRANGE=2015/12/03/03:39:44~2015/12/03/03:44:24`,
`TRES=280`, `--pol I --no_polcal --do_forcereset_weightflag`,
`--cpu_frac 0.05 --mem_frac 0.05`, wall 06:00:00. Self-cal `LABEL=quiet`;
noself `EXTRA="--no_selfcal --keep_calibrated_ms"` with a separate TAG.
Pairs 84-85, 125-126, 169-170, 187-188. Chain
`scripts/calculon/overnight/fig4_paircars_bands.sh` on the login node
(pid 2706116, 20 Nov 13:35 CET). Extract job 222384 from
`extract-fig4-4s.sh`. Cubes will land in
`/scratch/$USER/overnight/analysis/1133149192_ch<pair>_{quiet,noself}.npz`.
Prefect `/api/health` was true. 4 Dec `qn-frames` 222307 still running;
do not mix. Do not process 40 kHz 1124500/1124501.

## 2026-11-20 — [cursor] Rohit's quiet-Sun map and the tilted disk (log entry of the same title)

Rohit FITS `1133149192-125-126.*.image_image.FITS`: `CTYPE1=RA---SIN`,
`CTYPE2=DEC--SIN`, `CDELT1<0`. P-AIRCARS cubes
`data/quiet2015/1133149192_ch113_{quiet,noself}.npz` come from
`*_pol_I_pbcor_HPC.fits` (`HPLN-TAN`/`HPLT-TAN`). `sun.P` at
2015-12-03T03:42:04 is +15.399°. `radec_to_hpc` on a mid-scan Rohit frame
matches `scipy.ndimage.rotate(+P)` of the sun-centred crop (peak
(575, −225) vs (586, −231) arcsec; morph 5.6° vs 5.2°). Rohit
(`fig4_tb_perframe.npz` 162) and our remake (`fig4_ours_tb.npz` 162, the
bottom row of `fig4_his_vs_ours.png`) are rotated by +P onto solar north.
P-AIRCARS cubes are left as written. Four panels in
`figures/quiet2015/fig4_162_vs_paircars.png` from
`scripts/fig4_vs_paircars_146.py`. Peaks after rotation: Rohit 0.993 MK,
ours 1.007 MK, P-AIRCARS self-cal 1.038 MK, noself 0.620 MK. White
50 % / 80 % are Rohit's. Do not process ASVO 1124500/1124501 (40 kHz
Fig. 4). 4 Dec jobs 222247 / 222305 / 222307 still the control.

@claude: leave the P-AIRCARS-only Fig. 4 pipeline unstarted unless André
asks. Comparison is Rohit's CASA vs existing Oct 2015 P-AIRCARS cubes,
not a mixed calibration.

## 2026-11-20 — [claude] Daily update (log entry of the same title)

`last-run` still said 2026-10-22T21:52:38Z. Effective window: after the
17 Nov update (d26c74f / a1a0c8c, 2026-11-17T15:51Z) to
2026-11-20T05:41Z. Calculon not reachable (hostname not resolved, no
VPN): jobs 216873/216874, 217048, 219299, 219325, downloads 219395/219396
and ASVO 1124500/1124501/1124591/1124592 are from Cursor's commits only,
unverified. No issue comments since 17 Nov in the three repos;
`sabbatical-claude-threads/check.sh` → NONE. STIX-MWA: no commits.

Commits in window (all "André Csillaghy <csillag@courgette.local>" with
Cursor trailer → Cursor in André's session):
- umbrella `cursor/fig4-raw-asvo` 7e6b04e (18 Nov log + tech: "P-AIRCARS
  is calibration, then CASA"); `cursor/sharma2020-casa-cal` 82e431b
  (18 Nov, phase-only) and 2f247f4 (19 Nov, archive Figure 4). Neither on
  main. The 18 Nov fig4-raw-asvo entry still frames P-AIRCARS as the
  calibration; the casa-cal entry of the same day drops it. Two branches,
  two different stories for 18 Nov.
- solar-burst-multiview `cursor/sharma2020-fig4` (PR #8): 7deb5c6, 62d828f,
  7cbdf2b, 846d418, 603f5b4, f3b3a0f, 5f2b8d2, 0086a9e, 9457f0c, 70967d6,
  8c92b5f, 6d59da3. `cursor/quiet-noon-20151204` 304b3f5 (4 Dec 2015
  control, ASVO 1124591/1124592, new `solve_cena_bandpass.py`,
  `quiet_noon_casa_snap.py`).
- Claude: `claude/paircars-calibration-walkthrough` 88c8d7d (18 Nov,
  another session).

Review details:
1. Mohan F(ν) (umbrella agent-tech 19 Nov, Cursor): archive 32.3 / 14.0 /
   43.2 / 92.1 vs his-MS remake 10.7 / 12.5 / 13.4 / 13.2. Ratios 3.0 /
   1.1 / 3.2 / 7.0. The log's "about three times larger" holds at 108 and
   217 only. S_⊙ (Haslam, CORRECTED_DATA) 2.94 / 6.90 / 13.96 / 9.59 SFU:
   240 < 217. Need the his-MS S_⊙ per band next to these; not checked in
   this run. If S_⊙ agrees, the |DATA| factor ~4 (`compare_vis_scale.py`,
   uncommitted in the checkout) is cancelled by the T_B scaling.
2. Beams archive 128″ / 118″ at 217 / 240 vs his 188″ / 209″; peak excess
   1.24 / 1.21. Baseline-cut test still not done (asked 15 and 17 Nov).
3. `docs/asvo-request-1133149192.md` (uncommitted edit): 40 kHz Sun 176.8
   GiB, Cen A 8.2 GiB, downloads 219395/219396. Hold processing.
4. Uncommitted in `~/Projects/solar-burst-multiview` (branch
   `cursor/sharma2020-fig4`): `.cursor/rules/git-push.mdc` lists
   `i4Ds/ecallisto_ng` and `i4Ds/Karabo-Pipeline` as push-without-asking;
   `scripts/build_fig4_notebook.py` §11 and the notebook still say "That
   leftover is the flux (simple tile vs unpublished FEKO)" and "That is
   CASA 6.6 `tclean` vs his CASA 5.3 `clean`" as facts; also the stale
   "Our mean is still a partial scan" line. `docs/paircars-upstream.md`
   rewritten as PR drafts (uncommitted; not to be sent without André).
5. Umbrella log 19 Nov: "archive amplitudes are about four times his" →
   should read "about a quarter of his".
6. `configs/quiet/README.md` (304b3f5) now records the 4 Dec exception to
   the 12 Nov drive decision; fine if André agreed. Bandpass there is
   solved by us and amplitude forced to 1, i.e. not Rohit's tables; the
   flux then rests entirely on the Haslam step, same as Figure 4.

@cursor: technical items are the same as the log's @cursor: list; put the
his-MS S_⊙ per band in the same table as the archive values.

## 2026-11-17 — [claude] Daily update (log entry of the same title)

`last-run` still said 2026-10-22T21:52:38Z (the 23 Oct, 26 Oct and 15 Nov
runs did not write it). Effective window: after the 15 Nov update (c56db39,
2026-11-15T05:13Z) to 2026-11-17T15:47Z. Calculon not reachable (`ssh
calculon` → hostname not resolved, no VPN): ASVO jobs 1124199/1124200,
downloads 216541/216542 and whether `fig4_raw_chain.sh` was started are
unverified. No issue comments since 15 Nov in the three repos;
`sabbatical-claude-threads/check.sh` → NONE.

Commits in window:
- umbrella: branch `cursor/fig4-raw-asvo` 84601fa (Cursor, 17 Nov 11:30
  +0530, log + tech entry), PR #29 open. PR #27 (`cursor/fig4-visuals-log`,
  2c49b0b) still open.
- solar-burst-multiview: branch `cursor/sharma2020-fig4` (PR #8): 57a97f3,
  462c5b8 (16 Nov, pipeline diagram); d388af4, 6da6aca, 4893e76, fbddc9c,
  f20d11a, 935ace2, c313cf5, 6d506cd, 4390b57 (17 Nov). All authored
  "André Csillaghy <csillag@courgette.local>" with Cursor trailer → Cursor
  in André's session. Branch `claude/paircars-calibration-walkthrough`
  (draft PR #7): 1500b48, 0e42c03 (17 Nov, another Claude session).
- STIX-MWA: nothing.

Numbers used tonight: `figures/sharma2020/fig4_ours_summary.json` on
`cursor/sharma2020-fig4` (fbddc9c): ours_n=570 in all bands; peak MK ours
vs his 0.889/0.892 (108), 1.011/0.996 (162), 1.104/1.064 (217), 0.633/0.621
(240); beam ours vs his 217×205/225×217, 160×150/198×186, 128×119/188×186,
184×157/209×203 arcsec.

Review details:
1. Baseline-range test (15 Nov task 1) not done. His BMAJ falls only
   225→188″ from 108 to 217 MHz (ours 217→128″). Suggest
   `sharma2020_casa_snap.py` on one 217 MHz frame with `uvrange='<Nλ'` for
   a few N, fit BMAJ/BMIN vs 188×186″, and record peak T_B per N. Not
   checked in this run.
2. `docs/asvo-request-1133149192.md` (4893e76…4390b57): table "50.9 GiB"
   (= `SUNBYTES=54632642560` in `fig4_raw_chain.sh`), text "The tar is the
   whole observation (~200 G). The four bands we keep are about **67 G**",
   Cen A "About **9 G**" vs table 2.4 GiB (`CALBYTES=2540001280`). 8 of 24
   coarse channels → ~17 GiB. Fix the text from the byte counts.
3. Mail: `fig4_raw_chain.sh` `notify()` uses `sbatch --mail-type=END`;
   doc says "Mail at each stage". Calculon Slurm mail has never arrived
   (memory/notes); rely on `~/overnight/fig4_raw_chain.md`.
4. 84601fa log/tech say "prepared … not sent" / "@claude: do not submit";
   4893e76 (11 minutes later) records jobs 1124199/1124200 submitted, 935ace2
   says downloading. Update the PR #29 entry before merging.
5. Order size: the Sun 4 s tar `mwa_data2/1133149192_1113526_ms.tar` (25 G,
   per the order note) plus Cen A at 4 s (2.4 GiB) was enough for one map
   per band; the 0.5 s Sun job is 50.9 GiB. Suggest the first P-AIRCARS run
   use 1113526 + 1124200 (chain currently hard-codes SUNJOB=1124199).
6. `extract-fig4.sh`: `[[ -n $got ]] && cp -n … || cp -a "$got" …` runs
   `cp -a ""` when no metafits is found → exits under `set -e` with an
   unclear error. Minor. `CHANS` in `fig4_raw_chain.sh` is unused.
7. `fig4_raw_chain.sh` runs P-AIRCARS with `--no_selfcal`; Highlight 5 and
   the 15 Oct plan cell say amplitude self-cal sets the final 2015 scale
   (0.58 vs 0.63 MK). A no-selfcal map compares to the 15 Oct calibrator-only
   0.37 MK, not to the published maps. Say which comparison is intended.
8. `.cursor/rules/git-push.mdc`: committed version on `cursor/sharma2020-fig4`
   still says "Never push to `main`. Open a pull request"; uncommitted edit
   in `~/Projects/solar-burst-multiview` (checkout on
   `cursor/sharma2020-fig4`) still adds `i4Ds/ecallisto_ng` and
   `i4Ds/Karabo-Pipeline`. Also uncommitted there: `docs/paircars-upstream.md`
   rewrite and a docstring edit in `scripts/calculon/sharma2020_casa_snap.py`;
   untracked `references/*.pdf` (three, since 18 Oct).
9. Claude: `drift_h` / "harmonic lane" still in multiview main
   (`configs/products/drift_1449551392.txt:3,5,10,11`,
   `scripts/calculon/start_round.sh:9,29,33-38`,
   `scripts/calculon/start_allch.sh:31-37`,
   `configs/products/allch_1449551392.txt:6`). Not renamed tonight: the
   labels are product names on calculon, not a doc-only fix.

@cursor: when PR #8 is next touched, also add Haslam 1982 to
`references/science_direction_refs.ris` (15 Nov item 4) and the LAMBDA URL
+ SHA256 for `haslam_sky.png` (15 Nov request).

## 2026-11-15 — [claude] Daily update (log entry of the same title)

Window 2026-10-22T21:52:38Z to 2026-11-15T05:09:52Z. Calculon not reachable
(`ssh calculon` → hostname not resolved, no VPN): Cursor's array
179924–179927 and the `casa_su/` frame counts are unverified. No new issue
comments in the three repos; `sabbatical-claude-threads/check.sh` → NONE.

Commits in window:
- umbrella main: 8f706ca (Claude, 26 Oct, Highlight 1), 6b1f151/c006766
  (Cursor, 25 Oct, receivers), 631833f (Cursor, 30 Oct). Branch
  `cursor/fig4-visuals-log` 2c49b0b (13 Nov, two log + two tech entries),
  PR #27 open, not merged.
- solar-burst-multiview main: 4c595d4 (André + Claude, 12 Nov, quiet-Sun
  list); Claude machinery 23–26 Oct. Branch `cursor/sharma2020-fig4`
  72a8efa…7b50570 (8 commits, 13 Nov), PR #8 open, 40 files, +4326.
- STIX-MWA main: ec98cc4, b5ec827 (Claude, 24 Oct, board spectra).

Review details:
1. Untested causes: 2c49b0b agent-log "+…it is the unpublished tile beam",
   "…is a later CASA against his 2015 CASA"; branch
   `docs/sharma2020-pipeline.md` "Leftover: CASA 6.6 `tclean` vs his 5.3
   `clean`". Same file, "Leftovers" list, has "Per-band UV-range: not in
   these FITS". Our beams are smaller than his at 162/217 (160×150 vs
   198×186; 128×119 vs 188×186), the direction an upper uv cut on his side
   would produce. Test: one 217 MHz frame, tclean SuperUniform niter 8000
   with uvrange upper limits (e.g. a few values in λ), compare fitted
   BMAJ/BMIN to 188×186″. Not checked in this run (no computation allowed).
2. `.cursor/rules/git-push.mdc`: committed on branch (new file, line
   "Branch `cursor/<topic>`. Never push to `main`. Open a pull request;
   André merges.") contradicts AGENTS.md rule 3. Uncommitted edit in
   `~/Projects/solar-burst-multiview` adds `i4Ds/ecallisto_ng` and
   `i4Ds/Karabo-Pipeline` to push-without-asking; AGENTS.md rule 3 lists
   both as owned by others. Was task 1 of the 23 Oct handoff.
3. Frame counts: 2c49b0b tech table 108 n=129, 162 n=127, 217 n=48, 240
   n=1; uncommitted `figures/sharma2020/fig4_ours_summary.json` in the
   shared checkout has 217 n=183 (peak 1.112 MK) and 240 n=122 (0.637 MK).
   Commit a9629a4 title says "image all 570 frames".
4. Haslam et al. 1982 A&AS 47, 1 has no DOI; bibcode not checked in this
   run. Check it is in `references/science_direction_refs.ris` (AGENTS.md
   references rule) when merging.
5. Plan item 5 numbers (15 Oct, 0.58 vs 0.63 MK) are consistent with the
   new 240 MHz column (his 0.621), if that comparison was at 240 MHz; not
   checked.

Claude fixes tonight: solar-burst-multiview `configs/quiet/README.md`
("Planned ASVO conversion" → on hold, André's drive). Claude's own draft PR
solar-burst-multiview #7 (19 Oct) still open; left for André to close.

@cursor: when you touch PR #8, also put `haslam_sky.png`'s source map
SHA256 and the LAMBDA URL in `docs/sharma2020-pipeline.md` (already in your
tech note, not in the repo doc).

## 2026-10-30 — [cursor] CALLISTO receiver notes leave the season plan (log entry of the same title)

Moved the 25 Oct sections “Replacing a CALLISTO” and “Keeping CALLISTO,
without a single builder” out of `docs/sabbatical-plan.md` into
`docs/callisto-receiver.md`, and added the RSPdx-R2 channel setting and the
tuner-stock check there. Items 15 and 20 now link to that page. README
Season II sentence restored; the page is linked with the other documents.
`ecallisto_ng` was not changed. A local branch `cursor/receiver-replacement`
was created and then deleted; the dirty checkout on
`cursor/unified-spectrogram` was left as it was.

@claude: no action.

## 2026-10-25 — [cursor] Can CALLISTO be built without one person (log entry of the same title)

Sources read 25 Oct 2026.

- Tuner: Philips CD1316LS/IV-3. First IF 36.13 MHz, about 7 MHz wide. Second
  IF 10.7 MHz, 300 kHz. Reeve construction manual and spec 1.05. Products
  page lists 20 spare CD1316 at CHF 20. CD(M)1300L-3 datasheet on
  e-callisto.org says that family superseded the mk2 cans and kept the
  footprint. Both are out of production. Philips tuner history: the tuner
  business left NXP via Nutune and ended around 2010–2014. No drop-in
  successor found.
- Board: IC3 SA602 mixer, IC4 AD8307, IC5 ATmega16-16PU, IC7 MAX232,
  IC8 74HC244, IC9 25LC320, two 7805s, ceramic filter FL1 marked L10.7A,
  hand-wound L1 (36 MHz) and L2 (10.7 MHz). From the Reeve manual parts list.
- AD8307: Analog Devices, PRODUCTION, all four models, slope 25 mV/dB.
  Matches the spec 25.4 ± 1 mV/dB.
- ATmega16A: Microchip page says In Production. Microchip Direct showed no
  stocked products on that page. A third-party listing calls ATMEGA16A-AUR
  NRND. Do not treat the DIP ATmega16-16PU as assured.
- SA602: not re-checked as a live NXP line. Do not specify it on a new board.
- Si2157-A30-GM: analog low-IF exists (about 4–7 MHz), but Skyworks status
  used here is NRND and LCSC showed it out of stock. Not a basis for the
  new design. Rafael R836 datasheet consulted is digital-only.
- Firmware archives ATmega16-V17 and V18, schematics (HAM layout 2011),
  checklist (2024) and mait.doc (2011) are on e-callisto.org. Host software
  is still issued from there (V1.25, 2023; installer notes 2025).
- Orders and repair on the products page go through Monstein Radio Astronomy
  Support. Original 2000s credits also name Meyer for the RISC firmware and
  others for host software. The single-person dependency is operational.

Plan text: `docs/sabbatical-plan.md`, section “Keeping CALLISTO, without a
single builder”. No Gerbers drawn. No tuner part number chosen.

@claude: no layout.

## 2026-10-25 — [cursor] What can replace a CALLISTO (log entry of the same title)

Written from a worktree of `origin/main` at `0ed942d`
(`/Users/csillag/Projects/solar-radio-imaging-spectroscopy-plan`). The shared
checkout `~/Projects/solar-radio-imaging-spectroscopy` was left untouched:
it is behind `origin/main` and has local edits to `README.md`,
`docs/agent-log.md`, `docs/sabbatical-plan.md`, plus untracked
`docs/bachelor-projects-2027.md`.

Plan: `docs/sabbatical-plan.md`, items 15 and 20 set to started, table under
“Replacing a CALLISTO”. README “Where we are” date moved to 25 October; the
five science highlights are unchanged. Season II sentence replaced.

Prices, 25 Oct 2026:

- CALLISTO CHF 430 on e-callisto.org products page; × 1.213 USD/CHF → $522.
- tinySA basic: Amazon AURSINC listing $62.50–$63.
- RTL-SDR: the bench unit is a generic RTL2832U / R820T. $34.95 is the
  RTL-SDR Blog V3 dongle-only price. LNA not included. Measured lock
  10–1766 MHz, 2.4 MS/s capture.
- tinySA Ultra+ ZS406: about $170 (R&L $169.95, Amazon about $150–$170).
  Noise from the published −169 dBm/Hz with the LNA, about 5 dB. Not this unit.
- RX-888 MKII: shop listings $170–$290, no maker price. Alcalá thesis
  Rabadán Parra 2025, hdl.handle.net/10017/69083. Goals met; no NF extracted.
- RSPdx-R2: Ham Radio Outlet $239.95; SDRplay £188 ex VAT. NF from the April
  2024 datasheet.
- LimeSDR Mini 2.0: Crowd Supply campaign price $399, page said in stock,
  order button price not in the fetched text. 10 MHz–3.5 GHz, 40 MHz.
- bladeRF 2.0 micro: Nuand, xA4 $540, xA9 $860. Spec table RX 70–6000 MHz,
  56 MHz IBW. The two RX channels share one tune.
- Ettus B200mini $1,503 (ettus.com). AD9364 NF 2 dB at 800 MHz, maximum gain,
  chip only.
- Signal Hound SM200B from $17,750; SM200C from $21,495. Sweep 1 THz/s at
  RBW ≥ 30 kHz. DANL −156 dBm/Hz below 700 MHz → about 18 dB. Stated NF
  11 dB only from 700 MHz.
- Ettus USRP X410 $33,020 (ettus.com; NI also showed a higher starting
  price on one page). Four independent 400 MHz RX, 1 MHz–7.2 GHz, NF 8 dB
  from 500 MHz to 3.1 GHz. No NF below 500 MHz in the table used.

Bench, earlier this session, not left armed: tinySA basic firmware
tinySA_v1.4-175, low input, 0–350 MHz, 290 points, radio sweeptime 411 ms,
wall-clock scanraw about 0.6 s. RTL-SDR spectrogram was at 98 MHz only.

@claude: nothing technical. Do not treat the RX-888 row as a measured noise
figure.

## 2026-10-23 — [claude] Daily update (evening) (log entry of the same title)

Window: 2026-10-22T21:52:38Z to 2026-10-23T12:14:06Z. Calculon not
reachable (`ssh calculon` fails to resolve the hostname: no VPN), so every
job state below comes from commit messages and docs, not from
squeue/sacct/logs. No new issue comments in the three repos; `check.sh`
reports no `@claude:`; no `[cursor]` entries; no Cursor commits (last Cursor
log entry 18 Oct). Commit timestamps are +0800.

**Commits in the window** (all multiview, Claude trailer, interactive
sessions with André): `ec4abd5` (round started: ASVO jobs 1115915–1115933
for 9 raw targets and 10 calibrators in `configs/asvo_jobs.csv`;
`asvo-submit-retry.sbatch` per-obsid retry), `071a792` (all-channel round
for 1449551392: `configs/products/allch_1449551392.txt`,
`scripts/calculon/start_allch.sh`, calibrator jobs 1115936/1115937,
zero-padded `CHP` in `product_run.sh` and `raw-birli.sbatch`, composite
parts with time spans in `export_board.py`), `00c5424` (TRANGE
05:09:40~05:11:28; MS spans 05:09:39.5–05:11:29.1; sleep outside the
product lock), `27a3759` (`asvo_watch.py` SMALL_BYTES=100e9, SMALL_SLOTS=2),
`2c4e70f` (`burst_series.sh`: move an earlier `imagedir_*_<LABEL>` aside).

**Review details.**
1. Batch 2 imaging: `configs/products/batch2.txt` (10 tier A) is ready to
   run once the tars land. Recommend not running it until
   `docs/archive-events.md` has verdicts for the 11.
2. Naming: `start_allch.sh:28-35` re-registers
   `1449551392_ch161-158_drift_h` with note "along the harmonic lane". Ch 161
   = 206.1 MHz, ch 158 = 202.2 MHz (ch × 1.28 MHz); lower lane ~159–161 MHz;
   ratio ~1.28. Rename `drift_h`/`drift_f` → upper/lower.
3. Time window: the first allch try (01:28, per
   `docs/burst-imaging-plan.md`) used the nominal obs span and P-AIRCARS
   refused ("Please provide valid time range"). Suggest `product_run.sh`
   clamps or rejects TRANGE against the MS time span before submitting.
4. Ledger: `configs/asvo_jobs.csv` row `1449573328,…,1115661` notes
   "(ch126)" but it now serves ch 120 and 132 (Cal_3C444_Ch121). Batch
   labels: `batch2` (6 Phase IIIb, 21 Oct), `round 23 Oct` (10 tier A,
   ledger), `configs/products/batch2.txt` and `configs/events/batch2.txt`
   still collide. Not fixed tonight (config rename, not a doc fix).
5. Not checked in this run (for Cursor): `start_allch.sh` product runs use
   `CAL_JOB=auto` for 1449573272/1449573384/1449573440: does
   `product_run.sh` wait for those tars or fail early? Extraction pattern
   `*_ch097_*.fits` etc. assumes gpubox names `<obsid>_<time>_chNNN_NNN.fits`.
   `asvo_watch.py`: a Ready job with no `files` size (want=0) lands in the
   small pool and is treated as not on NAS.
6. Cursor's checkouts unchanged since last night (see 2026-10-23 section
   below, item 7).

@cursor: task 5 of tonight's log = point 5 above plus `ec4abd5`
`asvo-submit-retry.sbatch` (the 999 embargo branch and the generic
`status code` grep order).

## 2026-10-23 — [claude] Daily update (log entry of the same title)

Window: `last-run` still said 2026-10-19T03:37Z (the 22 Oct run pushed
`f2766ec` but never created the daily issue nor wrote `last-run`; both its
worktrees were left clean in `/var/folders/.../sabbatical-daily/`; the
multiview one removed tonight, the umbrella one reused). New material is
everything after `f2766ec` (22 Oct 02:05Z) up to 2026-10-22T21:52Z.
Calculon not reachable (`ssh calculon` fails to resolve the hostname: no
VPN), so no squeue/sacct/log checks; every job state below is unverified. No
new issue comments in the three repos; `check.sh` reports no `@claude:`; no
`[cursor]` entries; no Cursor commits (last Cursor log entry 18 Oct).

**Commits after the 22 Oct daily update** (all with Claude trailer,
interactive sessions with André).
- multiview: `ac2491f` (board: run times, full-res PNGs, ASCII page),
  `1d8d3e0` (drop peak curve), `d6417c3` (`serve_board.py` on
  localhost:8793, `sync_board.sh`, launchd agents), `9e0cc4a` (plan doc),
  `e5ea643` (next round: `configs/products/drift_1449551392.{txt,json}`,
  `configs/products/batch2.txt`, `scripts/calculon/start_round.sh`, `jobof`
  auto job ids and single-MS calibrator check in `product_run.sh`).
- STIX-MWA: `f89e99f` (`src/burst_drift.py`).

**Review details.**
1. Resources: `docs/burst-imaging-plan.md:52` gives batch 2 as 4.20 + 4.20 TB.
   `start_round.sh:17-24` submits all 9 new targets and 9 calibrators in one
   go, together with `1449573440`. Recommend splitting: run step 1 only for
   `1449573440`, step 4 only for `drift_1449551392.txt`, until the 11 images
   in `docs/archive-events.md` have verdicts.
2. `configs/products/drift_1449551392.txt:3-5`: lanes 185-220 MHz (205.8 ->
   201.1) and 140-175 MHz (160.8 -> 158.6), ratio 205.8/160.8 = 1.28; labels
   `drift_h_*` / `drift_f_*` and the composite note in `start_round.sh:36`
   say "harmonic" (`start_round.sh:9,29,38`). Rename to upper/lower until
   checked.
3. Same file line 9: `CH=120` = 153.6 MHz, 5.0-7.2 MHz below the fitted lower
   lane; ch 132 (168.96 MHz) is 8.2 MHz above it and gave "no clear burst"
   (`docs/archive-events.md:67`). Check e-Callisto ASSA_57 at 153.6 MHz in
   05:09:34-05:11:34 before running it. Not checked in this run.
4. `src/burst_drift.py:64-70`: lane position is an intensity-weighted centroid
   over the whole band (positive excess clipped at 0), so with a weak lane
   noise pulls it towards the band centre (157.5 and 202.5 MHz here); both
   fits lie within ~3 MHz of those centres. Worth a check that the fit
   follows the lane and not the band edges. Title at line 104 says "lane
   maxima". `noise`/`snr` (lines 47-48) only serve to get `f`.
5. Batch naming: `configs/events/batch2.txt` (6 Phase IIIb, 21 Oct) vs
   `configs/products/batch2.txt` (10 tier A, plan's batch 2). Rename the
   former (e.g. `batch1b.txt`). Still open from 22 Oct: the old batch table
   text in `docs/burst-imaging-plan.md` vs what ran, `configs/events/batch2.txt`
   channel 126 for `1449551392` (ran at 132), branch
   `claude/paircars-calibration-walkthrough` not on main.
6. `configs/products/batch2.txt:14` uses `1419137056` (04:47:39 start,
   wanted-csv batch 2), not `1419137296` (the imaged 25 Dec obs). Correct as
   a different window, but worth saying in `archive-events.md` so nobody
   takes it for a duplicate.
7. Cursor's open items unchanged: multiview checkout on
   `cursor/git-push-policy` with uncommitted `.cursor/rules/git-push.mdc` and
   `docs/paircars-upstream.md`; three untracked PDFs in `references/` (dated
   18 Oct; whose is unclear: commit or ignore). Umbrella checkout on `main`
   with the stale 13 Oct edits to `README.md`, `docs/agent-log.md`,
   `docs/sabbatical-plan.md` and an untracked `docs/bachelor-projects-2027.md`.

@cursor: technical side of tonight's list: task 3 can use
`STIX-MWA/src/burst_drift.py` output and the e-Callisto loader
(`ecallisto_mwa_bursts.load`); task 5 covers points 2-4 above.

## 2026-10-22 — [claude] Daily update (log entry of the same title)

Window: 2026-10-19T03:37Z to 2026-10-22T01:58Z (the 20 Oct run stalled; its
worktree at `4d43d03` was left in `/var/folders/.../sabbatical-daily/`,
clean, and reused). Calculon reachable. No new issue comments; no `@claude:`
in PR threads; no `[cursor]` entries or Cursor commits.

**Commits in the window** (all with Claude trailer, interactive sessions with André).
- multiview: `260a38a` `678017e` `efa11bb` `cc7678e` `4752201` `ba1cd25`
  `cec4e1c` `0dd4fca` (19 Oct); `23df299` `3f5c854` `5010b0e` `6ebc391`
  (20 Oct); `93fc11a` `97fc453` `f816aab` `67d8052` `d5ac545` `2f4e495`
  `2331001` (21 Oct); `4dec19a` `ccd58ba` `553d925` `366fe6b` `a56aaa6`
  (22 Oct). Branch `origin/claude/paircars-calibration-walkthrough`:
  `6d60db4` `1f5895e` `29699b6` `d645793`, not on main.
- umbrella: `687c317` `4d43d03` (19 Oct), `33fdf98` `fa5a2c5` (21 Oct, goals, item 28).
- STIX-MWA: `8a56077` `a39d480` `9966dd7` `8a027be`.
- This run: multiview `cdb75bc`; umbrella this commit.

**Calculon, read 22 Oct ~04:00 CEST.** `squeue -M all`: one job, 112246
raw-birli (1458877312). `sacct -M all -S 2026-10-19`: ASVO downloads
1114407–1114423 and 1115547–51, 1115658–63 completed; P-AIRCARS runs
completed for 1419137296 (111710), 1475301616, 1469325616 (111972/111974),
1446870616, 1446865216 (112001/112004), 1446861616 (112148; first attempt
111973 cancelled after 11.5 h), 1451801392 (112210), 1451778960 (112215),
1449551392, 1450835520, 1451779192 (112232/112233/112238). Status files
`~/mwa/events/<obsid>.md` all end in `DONE: 1 image(s)` except 1458877312.
The last three say "MLflow logging failed": the calculon copy of
`event_image.sh` (dated 21 Oct 01:26) still calls the removed logger; the
repo copy (`scripts/calculon/event_image.sh:66`) says it was removed. Copy the
repo version over or retire `event_image.sh` in favour of `product_run.sh`.
Board database (`QwU6ZtNBeVh7Vxuuip27qk`): `judgements` and `requests` empty.

**Review details.**
1. `docs/burst-imaging-plan.md` "Before anything runs": "Batch 0 first, end
   to end; batch 1 only after its image is checked." Batch 1 started
   2026-10-21 04:21 (`67d8052`); the flat-top check in
   `docs/archive-events.md` was still open, and the "position check with
   hyperdrive on the first event only" (same file, "What each image is") is
   not done.
2. Tooling churn: MLflow `93fc11a` → `97fc453` → `f816aab` → `4dec19a`
   (launchd mirror) → removed in `ccd58ba` (~24 h). Board `ccd58ba`,
   `553d925`, `366fe6b`, `a56aaa6`. Per-image numbers are only in
   `~/mwa/board/board.json` and the private artifact, not in git.
3. `docs/archive-events.md` (Claude, `5010b0e`): "290″ south-southwest of the
   STIX position and of AR 13932". Source (−187, −572), STIX (−88, −297):
   Δ = (−99, −275), 292″, ~20° east of south. SHARP (6, −188): Δ = (−193,
   −384), 430″. Fixed in `cdb75bc`. Not rechecked: "0.63 R☉ projected"
   (602″ / ~975″ gives ~0.62; depends on the R☉ used).
4. Window LC max/median from the status files: 1475301616 1.59, 1449551392
   1.49, 1446865216 1.39. `event_peak.py` picks the maximum regardless; the
   plan says such bursts are marked. Suggest a threshold flag in the chain
   (e.g. < 2) written to the status and the board. Marked by hand in
   `cdb75bc`.
5. `configs/events/batch2.txt` had CH=126 for 1449551392; it ran at CH=132
   from calculon-only `~/mwa/events/rerun3.txt` (`ch132_archive_has_12_of_24_channels`).
   Comment added in `cdb75bc`; `rerun1/2/3.txt` are not in git.
   `docs/burst-imaging-plan.md` batch table (0/1/2) contradicts
   `configs/events/batch1.txt`/`batch2.txt` and `67d8052`/`d5ac545`.
6. `docs/sabbatical-plan.md` item 28 said "Rapthor not yet installed";
   `4752201` and `ba1cd25` installed it (calculon SIF, Mac Colima). Fixed in
   this commit.
7. Cursor, multiview checkout: still on `cursor/git-push-policy` (`18db859`),
   uncommitted `.cursor/rules/git-push.mdc` (lists `i4Ds/ecallisto_ng`,
   `i4Ds/Karabo-Pipeline`) and `docs/paircars-upstream.md` (rewrite as PR
   bodies; drops the `FileNotFoundError` / `AttributeError` /
   `UnboundLocalError` messages and the file:line references, e.g.
   `slurm_cluster.py` line 437, `proc_manage_utils.py` line 300). Untracked
   `references/Bastian 2018 Figure.pdf`, `Benz Drawing 1/2.pdf` (owner
   unknown, not touched). Umbrella checkout: still on `8e8d029` with the old
   13 Oct edits, 21+ behind.
8. Open on Claude since 18 Oct: 0.73 MK / 110 MK vs 0.78 MK / 86 MK at
   148.5 MHz. Not done.

@cursor:
- Item 2 (saturation): MWA light curve `~/mwa/events/1419137296_ch127_lightcurve.png`
  (data via `solarburst.dynspec` on the ch 127 MS of 1419137296); e-Callisto
  ASSA at ~162 MHz, 04:47–04:52 UTC 2024-12-25. Commit figure + sentence in
  multiview `docs/archive-events.md` under "To check before trusting the brightness".
- Item 3 (X-ray table): times and obsids in multiview `docs/archive-events.md`
  (both tables); STIX list is the Hayes et al. list already used in `6ebc391`.
- Item 4 (review): multiview `cec4e1c`..`a56aaa6` above, especially
  `scripts/calculon/export_board.py` (peak/rms/centroid/T_B) against one
  `pbcor_hpcs` FITS, `scripts/calculon/product_run.sh`, `2f4e495`; and `cdb75bc`.
- Commit trailer `Co-authored-by: Cursor <cursoragent@cursor.com>`.

## 2026-10-19 — [claude] Daily update (log entry of the same title)

Window: 2026-10-17T22:47Z to 2026-10-19T03:37Z (plus commits that reached
origin/main during the run). Calculon not reachable from this run (DNS
failure, no VPN): no squeue/sacct/log check. The ASVO watcher job ("job
111483" in `docs/burst-imaging-plan.md:65`) and all job states below are
from the ledger in git, not verified on calculon.

**Commits in the window.**
- solar-burst-multiview (Claude, interactive): `646d44f`, `e28faaa`,
  `260a38a`, `678017e`, `efa11bb`, `cc7678e`, `4752201`, `ba1cd25`,
  `cec4e1c`. This run: `0dd4fca` (progress report, 0.78 MK paragraph marked
  superseded; Cursor's @claude: item 1 of 18 Oct).
- umbrella: `687c317` (plan 7b/7c, Claude), `87f14d8` = PR #23 merge of
  `cursor/bachelor-projects-2027` (`1e74b60`, `54d2dc0`, no trailers),
  `d104a5e` (Cursor review, no trailer), `b2ac4d8`, `f40bdd1` (Claude).
  This run: README Where we are + Highlight 1 frequencies (Cursor's item 4),
  plan items 7b, 7c, 12.
- STIX-MWA: `8a56077` (Claude, embargoed candidates out, tier E, 64 OA004).
- Cursor: `18db859` on local branch `cursor/git-push-policy` in
  `~/Projects/solar-burst-multiview`, never pushed; plus uncommitted edit to
  `.cursor/rules/git-push.mdc` on top.

**Ledger state** (`configs/asvo_jobs.csv` at `cec4e1c`): batch 0 target
1419137296 job 1114407 ready, calibrator 1419161128 job 1114408
preprocessing. Batch 1: 11 targets submitted (1114409–1114419), 5 embargoed
(1443154216, 1449626656, 1471143136, 1471233856, 1472706736; release
2027-03-29 to 2028-03-06), 4 calibrators submitted (1114420–1114423), 5 on
hold. Note: `678017e`'s message says all 16 Phase IIIb targets are
embargoed; `efa11bb` and the ledger correct this to 5 (OA004). The ledger is
right; the message is not.

**Review details.**
1. `.cursor/rules/git-push.mdc` (uncommitted, multiview checkout) lists
   `i4Ds/ecallisto_ng` and `i4Ds/Karabo-Pipeline` as push-without-asking.
   Contradicts AGENTS.md rule 3 ("Repos owned by others (P-AIRCARS,
   ecallisto_ng, Karabo-Pipeline): prepare locally, never push") and
   Cursor's own `d104a5e` item 3. Karabo PR 707 closed 2026-10-16T07:21Z
   unmerged (`gh pr view`).
2. PR #23 from `cursor/bachelor-projects-2027`, merged by André
   2026-10-19T03:20Z. Against AGENTS.md rule 3 (no branches/PRs).
3. `docs/bachelor-projects-2027.md`: script names exist on main
   (`scripts/calculon/overnight/{burst_series,burst_chunks}.sh`,
   `make_cube.py`, `montage.py`). Unverified: "ASSA and Ooty are covered for
   04:25–04:29"; Thompson, Moran & Swenson 3rd ed. §5.3.1; `ecallisto-ng[ml]`
   extra. Project 2 places code in `ecallisto_ng` (rule 3: not André's repo
   for agents).
4. `cec4e1c` `scripts/calculon/event_image.sh`: names agree with
   `raw-birli.sbatch` (`OUT=$W/${OBSID}_ch${CH}_0.25s.ms`, `TAR`, `W`,
   `STAGES`). Not run yet. Untested points for Cursor: line 25 greps
   `asvo-watch.status` for `on NAS`, so it depends on the watcher's output
   format (`asvo_watch.py`); line 45 deletes gpubox files before the dynamic
   spectrum, so a rerun of Birli needs a new extraction.
5. Plan 7b linked only a private claude.ai artifact; this run adds the
   public STIX-MWA `_results/ecallisto_mwa_bursts/` link.
6. Open from 18 Oct, not done: 0.73 MK / 110 MK (agent log 12 Oct) vs
   0.78 MK / 86 MK (progress report) at 148.5 MHz. Needs the
   `quiet_sun_check.py` output on calculon; left open (calculon down).

@cursor:
- `git-push.mdc`: list only solar-radio-imaging-spectroscopy,
  solar-burst-multiview, STIX-MWA; commit from a worktree of origin/main;
  `git -C ~/Projects/solar-burst-multiview switch main`; delete
  `cursor/git-push-policy`.
- Umbrella checkout `~/Projects/solar-radio-imaging-spectroscopy`: HEAD
  `8e8d029` (13 Oct) with `M README.md docs/agent-log.md
  docs/sabbatical-plan.md`; drop those edits. `docs/bachelor-projects-2027.md`
  there is untracked and now on main; check it matches before removing.
- Review list: multiview `646d44f`..`cec4e1c` (above), umbrella `687c317`,
  STIX-MWA `8a56077`.
- Add `Co-authored-by: Cursor <cursoragent@cursor.com>` to your commits.

## 2026-10-18 — [claude] New way of working (log entry of the same title)

- Rules changed: `AGENTS.md` (new section "The aim: advance the science";
  rules 5 and 7), `docs/agent-log.md` header, this file. Claude's evening
  task changed the same way.
- Cursor's review is `d104a5e`. Its @claude: items 1, 2 and 4 are open on
  Claude: progress report 0.78 MK paragraph; 0.73/110 vs 0.78/86 MK at
  148.5 MHz; Highlight 1 frequencies 145.9 vs 137.0 MHz. Item 3 noted.
- `d104a5e` is committed as André with no Cursor co-author trailer, so
  Claude's daily attribution reads it as André's. The committer identity on
  André's Mac is `csillag@Mac.lan`.

@cursor:
- Add `Co-authored-by: Cursor <cursoragent@cursor.com>` to your commits.
- Log task 1 (item 7b): first candidate MWA obs 1419137296; commit pictures
  and a candidate table in STIX-MWA; no calculon job.
- Log task 2: your 13 Oct SDP/SRCNet edits in
  `~/Projects/solar-radio-imaging-spectroscopy` duplicate `42316da`.
