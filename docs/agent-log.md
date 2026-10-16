# Agent log

Newest first. One entry per working session.

Format:

```
## YYYY-MM-DD — [cursor|claude] short title

Facts only. What changed, which repo, what to read next.
```

Do not put session notes in `solar-burst-multiview`. That tree is the MWA
workhorse. This file is the running memory for both agents.

---

## 2026-10-16 — [claude] Daily update

First run of the evening docs task; it covers 14–16 Oct (no earlier daily
update exists).

**Today**

- solar-burst-multiview (all Claude, with André): quiet-Sun brightness tests.
  2015-12-03 quiet Sun (Sharma & Oberoi 2020, obs 1133149192) through our
  P-AIRCARS chain (`1291cdb`, `e4baef9`, `9d52914`): 0.58 MK against the
  published 0.63. Tests on why 2022 is fainter: short baselines (`742f6d7`,
  2 % change), 2015 imaging settings (`a04b238`, 0.30 vs 0.31 MK), 13 min
  integration (`75c5bb7`, `a04b238`, 0.23 MK), self-cal on/off (`29e6390`:
  calibrator only 0.37 MK in 2015 and 0.42 MK in 2022; self-cal gives 0.58 and
  0.35). Progress report `f110705`. Tooling: `31a78e1`, `2a09678`, `d245b9b`.
- Umbrella (Claude): Highlight 1 (`baa85d0`, `6eb4c9b`); item 7 split into
  7a–7d (`e98567f`); SKAO items 24/27 split, notes and issues #11–#20
  (`42316da`, `ff45a27`); AGENTS.md worktree rule (`6bd39e5`).
- Umbrella, this run: André caught a wrong sentence in Highlight 1. It said
  self-cal "does not" raise the 2022 brightness because of the noise storm.
  The data (`29e6390`) show self-cal *lowers* 2022 (0.42 → 0.35 MK, ×0.85), and
  the storm is a likely cause, not a demonstrated one (f110705 lists it as
  open). Fixed in Highlight 1 (`84c91d3`), the README progress item and the
  umbrella progress report (`dc3d0e5`). The error was Claude's (`6eb4c9b`).
- Karabo-Pipeline (Claude, André's account): native macOS arm64 install,
  OSKAR on the M5 GPU, [PR 707](https://github.com/i4Ds/Karabo-Pipeline/pull/707)
  open. Its log entry (`f8cdba5`) is on branch `claude/log-karabo-macos`, not
  on `main`.
- STIX-MWA: no commits. The local checkout has untracked
  `src/mwa_obs_lookup.py`, `src/my_mwa_obs.csv`, `src/out.txt`,
  `_results/my_mwa_obs/` (not touched).
- README "Where we are" and plan items 3, 5, 12 updated (this commit).

**Review**

*Cursor.* No commits since 10 Oct (`9101aa6`, solar-burst-multiview); last
`[cursor]` log entry on `main` is 6 Oct. The shared umbrella checkout holds an
uncommitted `[cursor]` entry dated 13 Oct ("Split SDP and SRCNet services")
plus README and plan edits. That split is already on `main` (`42316da`,
Claude, 14 Oct), so the local work is duplicated and now stale. Fix: discard
it, or diff it against `main` and keep only what `main` lacks.

*André.* No commits without an agent trailer in the window. Two points:
1. PR 707 on Karabo-Pipeline is from your account on a `claude/*` branch.
   AGENTS.md rule 3 says others' repos are prepared locally and you submit;
   if you opened it, fine. If a Claude session opened it, that broke the rule.
2. The Karabo log entry `f8cdba5` sits on a branch. Cursor reads `main`, so
   Cursor will not see its `@cursor:` request unless it is merged. Fix:
   merge it to `main` (I relay the request below in the meantime).
Good: your catch on Highlight 1 was right and important. The README is read
by non-specialists, and the old text sold a hypothesis as a finding.

@cursor:
1. Umbrella checkout: your uncommitted 13 Oct SDP/SRCNet edits duplicate
   `42316da`. Diff against `origin/main`, keep only what is missing, commit
   from a worktree, or drop them. Done when `git status` in
   `~/Projects/solar-radio-imaging-spectroscopy` is clean of your edits.
2. Review (Rule 4): Karabo-Pipeline PR 707 (native arm64, OSKAR OpenCL);
   solar-burst-multiview `f110705` (progress report, test table) and
   `29e6390`; umbrella `84c91d3` and `dc3d0e5` (Highlight 1 wording).
   Check that every number in Highlight 1 matches the table in
   `docs/progress-2026-10-09.md`. Done when your review is a `[cursor]` log
   entry on `main`.
3. Plan item 7b (not started, doable on the Mac): one e-Callisto picture per
   7a candidate, with the MWA obs window, times and channels to image, for
   André to review. Start with 2024-12-25 (MWA 1419137296). Done when the
   pictures and a candidate table are committed in STIX-MWA. No calculon job.
4. Plan item 24 (Understand SDP, not started): a one-page summary in
   `docs/skao-notes.md#24--sdp` from SKAO Confluence, with links. Done when
   it answers what SDP does with visibilities and where a solar pipeline
   would plug in.

## 2026-10-15 — [claude] Item 7 started: candidate selection with e-Callisto, data on calculon first

Plan item 7 is split into 7a–7d (candidate list, e-Callisto pictures reviewed
by André, first three events, the rest). Selection lives in STIX-MWA, imaging
in solar-burst-multiview. Discussion: issue #21.

- Calculon already holds 2,551 MWA obsids in
  `/mnt/nas05/data02/MWA_data/data/mwa_data{,2}` (2015: 5, 2022: 711,
  2023: 1,408, 2024: 350, 2025: 77; listed with `ls` only). 32 of the 549
  ranked e-Callisto bursts have their peak obs there.
- 2024-12-25 type II (MWA 1419137296) and both calibrators (1419109008,
  1419161128) are already in `mwa_data2` (ASVO jobs 1106097–1106099). The
  2026-10-04 entry's "not submitted" is out of date.
- Gap: the ranking uses only Australia-ASSA focus codes 56/57, so most of
  2022–2023 was never scored (calculon's e-Callisto copy has ASSA_62 from 2022).
- MRO in e-Callisto is Metsähovi, Finland (André), not the MWA site; the
  STIX-MWA docstring and README said otherwise.
- Method for item 7: P-AIRCARS self-cal (peak/rms 312 against 56 for
  hyperdrive + wsclean on 2022-09-30); hyperdrive only as a position check.
  No calculon job before André has seen the e-Callisto pictures.

## 2026-10-13 — [claude] DSC-053 (solar dynamic spectrum) runs end to end

Repo `~/Projects/ska-src-dsc-053-solar-dynaspec` (SKA GitLab,
src/sd/workflows). Not André's repo: nothing pushed.

- The code is on branch `dev` (Marcin Sokolowski, AusSRC; Rohit Sharma is
  project scientist; last commit 24 Feb, tested on SWESRC). `main` is the
  empty GitLab template. Confluence: SRCSC space, DSC-053 page and
  "Chronological walk-through" (answers plan open question 11).
- Ran `src/main.py` on `solar-burst-multiview/data/1424757616` (quiet Sun
  2025-02-28, 24 coarse channels, 4 s / 160 kHz Birli MSes, `DATA` only).
  Work dir `solar-burst-multiview/data/dsc053/1424757616` (gitignored); the
  picket-fence glob wants `*_chNN-NN*.ms`, so that dir holds symlinks.
  Conda env `dsc053` (pyuvdata 3.2.8, python-casacore, ipykernel).
- Four bugs fixed, commit `a4a578d` on local branch `local/1424757616-test`:
  flagged visibilities were used (69 of 152 tiles dead → all NaN); the
  autocorrelation normalisation divided by √A₂ only (in-place update, values
  up to 8 instead of ≤1, affects all data); MWA flag list hard-coded for 32
  fine channels plus `data[t,ch]` and swapped NaN thresholds; zero-rmsiqr
  channels froze the `lm` fit at its start. Result: 39 time steps, spectral
  index −0.04 ± 0.24 (flat). André reviews the diff before anything goes
  to the DSC-053 authors.
- Open: flagger drops 131 of 192 channels; fine channels within a coarse
  channel differ up to 5×; `CRPIX1 = 0` in the output FITS.
- VS Code: `.vscode/` (env, PYTHONPATH, launch config for main.py) and
  `local/dsc053_1424757616.ipynb` (Marcin's notebook with our paths), both in
  `.git/info/exclude`. Kernel "Python (dsc053)".
- Sibling DSCs (057, 067, 008) are porting to
  `ska-src-workflow-templates` (pydantic `ScienceWorkflow`, SWF IDs, uv).
- Next: plan how to run DSC-053 on `ska-src-api-deployment-stack` (local
  deployment ruled out by André: takes all laptop resources).

## 2026-10-12 — [claude] 2022-09-30 burst at 2 s and 0.25 s; quiet-Sun check; repos on main

All in solar-burst-multiview unless noted. Handovers:
docs/handover-raw-025s-session.md (0.25 s), docs/asvo-request-1348547272.md.

- 10 Oct: 2 s P-AIRCARS series of the 04:28 burst at 148.5 MHz (25 frames,
  04:28:19–04:29:07, from the ASVO 2 s re-conversion, job 1109734), with movies.
- 10–11 Oct: raw MWAX visibilities of 1348547272 (ASVO job 1111033, 0.25 s,
  10 kHz, 1.09 TB) downloaded to the NAS. giant-squid exits 0 after network
  errors, so the download job retries (asvo-download-retry.sbatch).
- 12 Oct, quiet Sun at 148.5 MHz from 16 quiet 2 s frames: quiet disk
  0.73 MK, radio disk to ~1.3 R☉, 7.2 × 10⁴ Jy without the noise storm
  (consistent with 0.5–1 MK); noise storm 110 MK at (+460″, +160″), west of
  AR 13110. Quiet-Sun and burst images also at ch107 (137.0 MHz) with 3C444.
- 12 Oct, Birli on ch116 (settings reviewed with André: no AOFlagger, native
  0.25 s, 8 × 160 kHz, 80 kHz edges, no Van Vleck), job 109920, 1 h 13 min,
  `/scratch/andre_csillaghy/mwa/flare20220930_raw/1348547272_ch116_0.25s.ms`.
  Checks (scripts/calculon/raw-ms-check.py): 1184 × 0.25 s; flags 34.7 % on
  every step apart from the 9 quack steps (dead tiles only, burst not
  flagged); short-baseline light curve averaged to 2 s equals the 2 s product
  to 0.05 %. Uncalibrated dirty images of the 2 s and 0.25 s data come out
  identical; not yet confirmed on the FITS files.
- The 0.25 s light curve shows bursts through the whole observation (main
  burst 04:28:20–04:29:04, more until 04:32:10). P-AIRCARS accepts only
  04:27:41–04:32:23 for this MS; the failed 2 s "fullobs" run asked for
  04:27:36. Started: all of it at 0.25 s, ~1128 images, five chunks in one
  TAG so calibration and self-cal are shared, main burst first
  (scripts/calculon/overnight/burst_chunks.sh; progress
  `~/overnight/1348547272_025s_ch116_chunks.log`). About 30 s per image,
  measured at 2 s.
- STIX-MWA: the flare spectrogram shows the MWA coarse channels at their true
  frequencies.
- André: commit straight to `main` in his own repos, no PRs. Merged the open
  work: solar-burst-multiview (13 commits after PR #3), STIX-MWA (1 commit).
  Here: Cursor's 6 Oct SKAO items as 24–26 (23 was taken), README "Where we
  are" and the plan brought to 12 October.

## 2026-10-05 — [claude] Plan item 23: tool alignment

New plan item from André: align the FHNW tools with the other projects,
e-Callisto first. Part of Season II, but needed now. Added as item 23 (21 and
22 are taken on this branch) with status 🟡 and repos ecallisto_ng and
FlareSense-v2, plus open question 10 (scope and meaning of "aligned").
Built on `cursor/agent-log-flaresense-intensity` (a1cd9d7) so the numbering
matches.

## 2026-10-04 — [cursor] FlareSense rank by intensity

FlareSense-v2 branch `cursor/select-by-intensity`. The data browser can
rank detections by the confidence in the filename and filter by station
name. "Australia" keeps Australia-ASSA_57 and Australia-ASSA_63. The
e-Callisto station page lists those as ASSA, Radio Astronomy South
Australia. The same page lists MRO as Metsähovi Radio Observatory,
Finland, so MRO is not treated as Australia. The rank is not a radio
flux; the catalog does not store one. On this Mac the window is the last
60 days from flaresense.ch. Strongest Australian detection in that
window: Australia-ASSA_63, 97.0%, 2026-09-30 02:45 UTC, one of 120.
`tests/test_intensity.py`: 6 passed. Stacked on the open dark-mode PR.

## 2026-10-09 — [claude] 2022-09-30 burst: 24 channels, 10-frame series, height, PFSS

Science handover: docs/handover-2022-09-30-science-results.md. Progress against
the plan: docs/progress-2026-10-09.md. Type III at 04:28:44 imaged with P-AIRCARS
in 21 of 24 channels (129–233 MHz) and as ten 4 s frames at 148.5 MHz; height
≈ 1.2 R☉ from centroids and from open PFSS field lines leaving the west edge of
AR 13110. Prefect DB timeouts raised (60/120 s) after two runs aborted on 500
errors. ASVO job 1109734 (0.25 s re-conversion of 1348547272) submitted. P-AIRCARS
upstream branches prepared on the Mac; push to upstream disabled.

## 2026-10-08 — [claude] P-AIRCARS on calculon works; overnight run

solar-burst-multiview PR #3 (`claude/flare-20220930`), docs/calculon-paircars.md
and docs/paircars-upstream.md. P-AIRCARS reinstalled in developer mode
(git master `fa4aa91`, editable, env `paircars_env`), init on the login node
(Prefect :4260, Postgres :5260 on calc-m-001), `sinfo` pass-through for the
site wrapper, workers sized with `--cpu_frac/--mem_frac 0.05`.

Four local patches (`~/paircars/P-AIRCARS`, branch `calculon-fixes`; Mac clone
branches `upstream/slurm-fixes` and `upstream/no-polcal-fixes`, not pushed —
André contacts the developers). Two are general Slurm bugs: Prefect config
read from `~/.paircarspipe` (confirmed 8 Oct: clean path gives
FileNotFoundError; our first install had only changed the symptom), and the
CPU-accounting plugin missing on Slurm workers. Two only matter with
`--no_polcal`.

First working run (7 Oct, 1348547272 ch112, 3C444): self-calibrated 4 s
images of the 04:28:44 type III, peak/rms 312 vs 70 with 3C444 only; source
about 90″ SW of AR 13110, consistent with hyperdrive + wsclean.

Overnight (started 8 Oct 01:52, driver ~/overnight/bin/overnight.sh on the
login node, progress ~/overnight/PROGRESS.md, Slurm mails): P-AIRCARS on all
24 channels (ch101–187) for 04:28:30–04:29:02 and quiet 04:31:24–04:31:40,
wide-field images of 3C444 and the Sun for a GGSM position check (3C273 and
Virgo A in the field), quiet-Sun flux check. Report: ~/overnight/REPORT.md.

## 2026-10-07 — [claude] MWA + e-Callisto spectrogram, burst image at 04:28:44

MWA dynamic spectrum 03:18–04:42 (17 obs, 100 shortest baselines,
solarburst/dynspec.py) combined with e-Callisto ASSA and OOTY
(STIX-MWA PR #4). From 1348547272 (04:27:34) the series uses ch101–187, so
calibrated with 3C444. AOFlagger had flagged and zero-weighted 95–98 % of
the burst steps; src/solarburst/reflag.py restores them. Burst at 143.4 MHz,
04:28:42–46, beam 2.0′ × 1.3′, peak/rms 56.

## 2026-10-06 — [cursor] SKAO-side sequence

Three plan items, in order: **24** SDP + SRCNet (in progress), **25** solar
DSC (not in git; Confluence + arXiv:2607.17881), **26** SKA-Low solar pipeline
sketch. Written into `docs/sabbatical-plan.md` as a table with 🟢/🟡/🔴.

Swiss SKA Days (item 10) is a small deployment, not the DSC text. Open
question 11: pin the exact DSC URL.
(Renumbered from 23–25 on 12 Oct: item 23 was already tool alignment.)

## 2026-10-06 — [claude] First MWA image of the 2022-09-30 M1.1 flare

solar-burst-multiview, branch `claude/flare-20220930` (on top of
`cursor/paircars-mac-resources`). Calculon job 259817: hyperdrive
di-calibrate on 1348522216 ch113 (PKS0408-65, GGSM_updated, all 8 chanblocks
converged), solutions-apply to 1348545200, wsclean of the 4 s step at the STIX
peak (03:57:22–26). ASVO MSs are 4 s / 160 kHz, so 1 s images are not possible
from them. No 150 MHz coarse channel; ch113 = 144.6 MHz.

Result: one source, ~1.3 kJy/beam (not flux-calibrated), beam 1.9′×1.25′. In
helioprojective it sits on AR 13110 (+215″, +95″), not on the M1.1 flare at
the NE limb (−867″, +397″, HEK). Overlay on AIA 193/131 Å:
`figures/flare20220930/1348545200_ch113_peak_hpc_aia.png`.

Pitfall: `get_body("sun", ...).icrs` is barycentric and puts the Sun ~11° off;
use GCRS RA/Dec. Tar listing/extraction ran on the login node; docs now say
to use srun. Next: dynamic spectrum over the 296 s, images per channel.

## 2026-10-05 — [claude] MWA archive inventory, first flare target

Looked up all 2,546 obs from André's ASVO download listing in TAP
(STIX-MWA/src/mwa_obs_lookup.py, untracked). 742 are G0002 Sun pointings,
1,241 G0060 IPS, 444 non-solar. 24 M/X STIX flares fall in G0002 Sun obs
(STIX-MWA/_results/my_mwa_obs/). First imaging target: 1348545200
(2022-09-30 M1.1 peak 03:57:23 UTC), calibrator 1348522216 (PKS0408-65), on
calculon. Handover: docs/handover-2022-09-30-flare-image.md.
## 2026-10-04 — [claude] STIX timing, e-Callisto bursts with MWA, first imaging target

- STIX flare list times (data center and Hayes) are at Solar Orbiter, not
  Earth: GOES−STIX peak vs light-time shift has slope 0.88 (778 Earth-visible
  M/X flares). The light-time correction in `mwa_stix_overlap.py` stays.
- 28 Feb 2025 (local MWA 1424757616): MWA saw only the late decay of a STIX
  B5 flare; no radio burst in MWA (uncalibrated) or e-Callisto.
- i4Ds/STIX-MWA PR #3 (stacked on #2): `src/ecallisto_mwa_bursts.py` ranks
  e-Callisto bursts during archived MWA Sun obs by Australia-ASSA strength.
  MRO is unusable for this (MRO_60 calibration blocks, MRO_59 RFI).
  836 bursts with archived MWA; top 12 checked by eye.
- First imaging target (André): 2024-12-25 type II, MWA 1419137296 (OA004,
  240 s), STIX M6 / GOES M5.0 near disk centre. Download script with two
  same-channel solar calibrators (1419161128, 1419109008); not submitted,
  needs André's ASVO key and giant-squid.
- 2 Oct incident: a commit of mine swept Cursor's uncommitted edits in the
  shared umbrella clone onto `cursor/paircars-mac-cap` (cf81553). André was
  asked to restore it. Claude now works only in separate git worktrees.

@cursor:
Please review i4Ds/STIX-MWA PR #3, especially the calibrator choice and the
conversion parameters for 1419137296.

## 2026-10-02 — [claude] Setup from Claude Code desktop

- Working from André's Mac (Claude Code desktop), pushing with his `gh`
  login. The Cowork git-proxy block does not apply here.
- Rebased `claude/agent-log-entries` on `main` (after PR #1) and opened a PR
  with the 30 Sep / 1 Oct Claude entries, `docs/scoreboard.md` and the CESRA
  2026 book of abstracts.
- Pushed `claude/mwa-stix-overlap` to i4Ds/STIX-MWA and opened a PR
  (`src/mwa_stix_overlap.py` plus the AGENTS.md pointer).
- Created labels `agent:claude`, `agent:cursor`, `proposal`, `agent-talk` in
  the four sabbatical repos.

@cursor:
Please review both PRs (Rule 4), and check the five points on
`find_flares_in_mwa.py` in the 1 Oct entry below.
## 2026-10-02 — [cursor] Close of day: guest, postgres, channel 127

Native macOS still cannot install P-AIRCARS. `pip install '.[dev]'` on
this Mac fails twice: base Python is 3.13.13, and conda env `paircars`
(Python 3.10.21) then has no wheel for `casatools==6.6.0.20`. Plan items
21 and 22 in `docs/sabbatical-plan.md` are that native Mac build and a
Metal GPU build. Both are not started.

The working install is the Colima `paircars` guest: Ubuntu 22.04.5,
x86_64, 16 vCPU, 40 GiB. The container was recreated so these mounts
exist:

| In the guest | On the Mac |
|---|---|
| `/obs` | `~/Projects/solar-burst-multiview/data` |
| `/data` | `~/Projects/P-AIRCARS-data` |
| `/opt/P-AIRCARS` | `~/Projects/P-AIRCARS` |

Closing the terminal does not stop the guest. `1424757616` is visible at
`/obs/1424757616` (24 measurement sets).

`init-paircars-setup --init` with no `--configdir` uses
`~/.paircarspipe`. A `pip install` into the `paircars` user site shadowed
the conda package and started PostgreSQL with proot, which fails under
Rosetta. The user-site copy was patched to fakechroot, and the guest
`udocker` command rewrites `--execmode=P1` to `F1` and passes a TCP port.
PostgreSQL then logged ready on port 5260 at 05:43 UTC. Those two fixes
are only in the running container. A later init died because `lsof` was
missing; it is installed now. `solar-burst-multiview`
`scripts/paircars-mac/install-inside.sh` lists `lsof` so the next image
build has it.

Channel 127 image, no calibrator, Stokes I, 4 s at 2025-02-28 06:00:40.
Job `20261002055510809`. The log header shows 15 CPUs and 35.09 GB.
Intensity self-cal dynamic range 10.51. The master flow then says
"Self-calibration subflow is not successful. No solutions are available
to apply." No bandpass table. Imaging still wrote one image and removed
the raw FITS. Primary-beam correction wrote 1 image and warned "No disk
detected image is present to estimate phase shift." Overlay PNG:
`P-AIRCARS-data/out/1424757616-ch127/20250228/1424757616_target/imagedir_f_1.28_t_4.0_pol_I_w_briggs_0.0/overlay_pngs/time_20250228060040.0_freq_162.48_pol_I_pbcor.png`.
Pipeline banner: finished successful. The self-cal solutions were not
applied. Read the master log
`P-AIRCARS-data/pipeline-work/1424757616-ch127/main_paircars_20261002055510809.log`.

## 2026-10-02 — [cursor] Plan: native Mac P-AIRCARS, and the Linux guest

Added sabbatical plan items 21 and 22 in `docs/sabbatical-plan.md`.
Item 21 is a P-AIRCARS that runs on macOS. Item 22 is a P-AIRCARS that
uses Mac GPUs. Both are not started. Numbers sit after Season II so
items 15–20 stay put.

Host `pip install '.[dev]'` still cannot finish. Base is Python 3.13.13.
Conda env `paircars` is Python 3.10.21, and pip then stops because
`casatools==6.6.0.20` has no macOS arm64 wheel.

The Linux guest for that install is already up. Colima profile
`paircars` (16 vCPU, 40 GiB). Container `paircars` is Ubuntu 22.04.5
x86_64. Inside it, conda env `paircars` is Python 3.10.21 and already
has paircars 3.0.7 plus the dev extra (pytest 8.3.3, black 24.4.2,
Sphinx 8.1.3, ruff 0.16.9). Source checkout is `/opt/P-AIRCARS`.

## 2026-10-02 — [cursor] Mac P-AIRCARS worker ceiling

This Mac is an M5 Pro: 18 cores, 64 GB, 20-core Metal GPU. Colima
`paircars` is running at 16 vCPU and 40 GiB. Colima `default` is running
at 4 vCPU and 8 GiB. With both up, the compressor held about 19.5 GiB and
swap was idle.

Channel 127 (`1424757616`, job `20261002035203048`) finished with exit 0.
The log recorded 12 CPUs and 31.27 GiB, which is 80% of that guest (the
guest reports 39.09 GiB). WSClean ran `-gridder wgridder -j 12 -abs-mem
31.27`. One Stokes I FITS was written. The primary-beam step reported 0
corrected images.

IDG's GPU modes are CUDA. The x86 guest cannot see Metal. The 20 GPU cores
are idle for this pipeline. The virtual machine was not resized, and
`default` was not stopped.

`solar-burst-multiview` branch `cursor/paircars-mac-resources`:
`scripts/paircars-mac/mac_resources.py` and `tune.sh`. Applied in the
running container. A later worker that asks for 0.8 gets 15 vCPUs and
35.09 GiB (1 vCPU and 4 GiB stay inside the guest for PostgreSQL and
Prefect). `run-sample.sh` still requests 0.5 and 0.6. Read
`docs/paircars-mac.md`, section "This Mac".

Claude's 2 Oct entry on `claude/agent-log-entries` asks for a review of
the umbrella PR and the STIX–MWA overlap PR, and for a check of
`find_flares_in_mwa.py`. Not done in this session.

## 2026-10-01 — [cursor] End of day

Wrap of 27 Sep–1 Oct. Facts below. Local umbrella clone:
`~/Projects/solar-radio-imaging-spectroscopy`. Do not treat
`solar-burst-multiview/.umbrella-staging/` as git.

### Repos

| Repo | Branch | What landed |
|------|--------|-------------|
| [solar-burst-multiview](https://github.com/i4Ds/solar-burst-multiview) | `cursor/calculon-mwa-containers` | Sharma package + figures; MWA-only README; P-AIRCARS on the Mac; hyperdrive DI-cal on calculon; P-AIRCARS calculon notes (CPU cluster, separate from Singularity) |
| [solar-radio-imaging-spectroscopy](https://github.com/i4Ds/solar-radio-imaging-spectroscopy) | `main` has plan through 29 Sep; local `cursor/agent-log` has `AGENTS.md`, this log, and the 1 Oct plan | Numbered plan, no person names, general idea at the end; science direction (item 14) |
| ecallisto_ng, Karabo-Pipeline, STIX-MWA | unchanged this week | Season II / SKA / old coincidence scripts |

No `i4Ds/mwa-demo`. Non-solar demo is in solar-burst-multiview. Upstream image:
[MWATelescope/mwa-demo](https://github.com/MWATelescope/mwa-demo).

### Plan status (items)

🟢 1 Understand MWA as a user  
🟢 2 Stack on laptop / calculon / CSCS (non-solar demo)  
🟡 3 Understand MWA calibration (hyperdrive Pictor A trial; 2025 pair did not yield a solar disk)  
🟢 4 Reproduce Sharma et al. 2022 from existing products  
🟡 5 Solar extension of the MWA stack + calibration / self-cal with teams  
🟡 6 P-AIRCARS for SKA-Low (runs on this Mac; calculon CPU install started, login-node udocker unconfirmed)  
🔴 7–13, 15–20 as in `docs/sabbatical-plan.md`  
🟡 14 Science goals with CESRA (`docs/science-direction.md` draft v3)

### Sharma (solar-burst-multiview)

`src/solarburst/`: `stage`, `subtract`, `maps`, `bursts`, `figures`.
`scan_mean` matches Sharma `_sub.ms` bit-exactly. Paper 15 s median is
`--method running_median` only. `imaging.tb_scale: 9.48`. Time average is
the median. `python -m solarburst.figures` rebuilds Figs 3, 5, 7, 9, 11, 12.
Table 2 region counts still approximate.

### Imaging stack

- Laptop Docker/Colima; calculon GPU Singularity (`docs/calculon-mwa.md`);
  CSCS Podman working.
- Notebook 03: `HYPERDRIVE=local|calculon`. Calculon job:
  `scripts/calculon/di-calibrate.sbatch`. Sky model `GGSM_updated.fits`.
- One 2025 solar/cal pair: after DI-cal the peak was ~2×10⁵ Jy/beam, ~2′
  beam; not a solar disk. Notebook note 1 Oct: that calibrator obsid is the
  wrong grid; no matching cal files for the solar scan → self-cal /
  P-AIRCARS.

### P-AIRCARS

- Mac: `docs/paircars-mac.md`, Colima linux/amd64. Zenodo sample
  `1111474560` / `1111476056`: Stokes IQUV from intensity self-cal.
  Polarisation self-cal, dynamic spectra, EUV overlays did not succeed.
- Calculon: `docs/calculon-paircars.md`. PyPI 3.0.6 in Miniforge on
  `calc-cpu` / `cpu-daily`, not the SIF images. Init and `run-mwa-paircars`
  scripts under `scripts/calculon/`. Login-node udocker still to confirm.

### Conventions

- Date the plan on the day it is edited. 🟢/🟡/🔴. General idea at the end.
- No person names in `docs/sabbatical-plan.md`.
- Tests of solar-specific software stay in solar-burst-multiview.
- Do not commit kernel-metadata notebooks, `Untitled`, `display_images.ipynb`,
  `test.png`.

## 2026-10-01 — [claude] STIX–MWA overlap script, GitHub access

New working plan (André): image the Sun when STIX saw a flare while MWA
observed.

- `mwa_stix_overlap.py`, intended for STIX-MWA (André's decision). Branch
  `claude/mwa-stix-overlap` prepared, not pushed. Step `mwa`: MWA TAP
  daytime obs (`sun_elevation > 0`), kept if pointed within 15° of the Sun or
  named sun/solar, any project. Step `match`: STIX science flare list
  (hayesla/stix_flarelist_science, 33,076 flares to 2026-02-28), Solar Orbiter
  times shifted to Earth by (d_Earth − d_SolO)/c (−15 to +355 s), overlap
  with 60 s padding. Tested on synthetic MWA data; TAP step not run (network).
- Read STIX-MWA `src/find_flares_in_mwa.py` (source of the 803/800 G0002
  lists): older flare list (to 2025-02-28); no light-time correction (imports
  unused, docstring says "optionally applies"); sunrise/sunset computed but
  unused, so no daylight filter; G0002 only; reads `../files/` but the repo
  folder is `_files/`.
- Blocked in Claude's sessions: vo.mwatelescope.org,
  datacenter.stix.i4ds.net, mwatelescope.atlassian.net. Pushes refused by the
  git proxy until a session starts with the repos attached.

@cursor:
Please check the five points on `find_flares_in_mwa.py` and say if any is
wrong. Claude will review `cursor/agent-log` and `cursor/calculon-mwa-containers`
once it can comment on GitHub.

## 2026-09-30 — [cursor] Plan, repo split, P-AIRCARS on the Mac

Local clone of this umbrella: `~/Projects/solar-radio-imaging-spectroscopy`.
Do not use `solar-burst-multiview/.umbrella-staging/` as the git copy; that
folder is a stale mirror.

### Where things live

| Thing | Place |
|-------|--------|
| Sabbatical plan (numbered items, 🟢/🟡/🔴) | `docs/sabbatical-plan.md` in this repo |
| Science direction (item 13) | `docs/science-direction.md` |
| Agent rules | `AGENTS.md` in this repo |
| Project board | https://github.com/orgs/i4Ds/projects/18 |
| Sharma 2022 + MWA demo + P-AIRCARS Mac install | [i4Ds/solar-burst-multiview](https://github.com/i4Ds/solar-burst-multiview) |
| e-Callisto | `ecallisto_ng` |
| KARABO | `Karabo-Pipeline` |
| STIX coincidence scripts | `STIX-MWA` (not the 2024 MWA path) |

There is no `i4Ds/mwa-demo`. The non-solar birli / hyperdrive / wsclean demo
lives in solar-burst-multiview. Upstream image:
[MWATelescope/mwa-demo](https://github.com/MWATelescope/mwa-demo).

solar-burst-multiview is MWA only. No STIX, no e-Callisto, no multi-instrument
notebooks as the current path. Leftover notebooks under `notebooks/` are not
the architecture.

Solar-specific tests stay in solar-burst-multiview (same configs and staged
Sharma data). Do not invent a third i4Ds MWA repo for that.

### Sharma 2022 (plan items 1–3: done)

- Env, `src/solarburst/`, stage from calculon: done.
- `python -m solarburst.subtract` vs Sharma `_sub.ms`: bit-exact with
  `scan_mean`. The paper’s 15 s running median is `--method running_median`
  and is **not** this ground truth. CASA logs: `subvs` `mode="linear"` over
  the scan.
- `imaging.tb_scale: 9.48` recovers the pickled T_B scale. Rayleigh–Jeans
  from FITS `BMAJ`/`BMIN` is ~9.5× too low.
- Time average of T_B is the **median** (a mean is wrecked by a few
  hundred-MK frames).
- Figures 3, 5, 7, 9, 11, 12 rebuild via `python -m solarburst.figures`.
  Plan item 3 is marked done.

Mechanics: `solar-burst-multiview/docs/reproduction-plan.md`.

### Containers (plan item 2: done)

- Laptop: Docker / Colima.
- Calculon GPU: Singularity SIF under `~/mwa/images`. Login node cannot run
  containers; jobs are `sbatch`/`srun`. Notes:
  `solar-burst-multiview/docs/calculon-mwa.md`.
- CSCS (Besso): Podman, installed and working.

### P-AIRCARS (plan item 5: in progress, runs on this Mac)

Apple Silicon install is Colima `linux/amd64`, not the calculon Singularity
path. Instructions: `solar-burst-multiview/docs/paircars-mac.md`. Scripts:
`solar-burst-multiview/scripts/paircars-mac/`.

Tested 29 Sep 2026 on Zenodo sample (solar `1111474560`, calibrator
`1111476056`): intensity self-cal produced Stokes IQUV cubes. Pictor A
import, polarisation self-cal, dynamic spectra, and EUV overlays did not
succeed on that setup. Images are self-calibrated, not flux-calibrated from
the calibrator.

Plan item 4 (solar extension of the MWA stack + calibration / self-cal)
does **not** include `solarburst`; that package is the Sharma reproduction.

### Plan editing conventions

- Date the plan heading on the day it is edited.
- Number items; 🟢 done, 🟡 in progress, 🔴 not started.
- General idea sits at the **end** as reference.
- No person names in `docs/sabbatical-plan.md` (paper citations like
  Sharma et al. 2022 stay).
- Item 9: SRCNet small deployment shown at Swiss SKA Days (August 2026).
- Item 14 is first in Season II: mini-spectrum-analyzer SDR test.

### Do not commit in solar-burst-multiview

Jupyter kernel-metadata-only diffs, `Untitled`,
`notebooks/display_images.ipynb`, `notebooks/test.png`, and
`.umbrella-staging/`.

## 2026-09-30 — [claude] Agent setup drafts

- Drafted `AGENTS.md` (same text as on this branch), the first version of this
  log, and `docs/scoreboard.md`. Added the CESRA 2026 book of abstracts
  (`references/cesra2026_book_of_abstracts.pdf` and `.txt`, pdftotext
  -layout), approved by André for this repo.

