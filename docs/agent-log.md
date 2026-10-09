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
