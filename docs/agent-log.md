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
