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

