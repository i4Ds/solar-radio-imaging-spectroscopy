# Handover: first MWA image of the 2022-09-30 M1.1 flare on calculon

From: [claude] Cowork session, 2026-10-05. To: Claude Code (on the Mac or on calculon).

## Goal

Make **one** WSClean image of the Sun at the STIX flare peak, on calculon,
from data that is already in the FHNW MWA archive. Nothing more yet.

## Target (from ASVO TAP metadata, verified)

| | Solar obs | Calibrator |
|---|---|---|
| obs_id | **1348545200** | **1348522216** |
| name | `Oberoi2022A_reg_Sun_Ch[58,61,...,210,226]_30_of_61` | `Oberoi2022A_Cal_PKS0408-65_Ch[58,61,...,210,226]` |
| project | G0002 | G0002 |
| UTC | 2022-09-30 03:53:02 – 03:57:58 (296 s) | 2022-09-29 21:29:58 – 21:33:58 (240 s) |
| array | Phase II Extended | Phase II Extended |
| channels | 24 coarse channels, 74–290 MHz (same list for both) | same |
| raw resolution | 0.25 s, 10 kHz | 0.25 s, 10 kHz |
| tiles | 111 good | 112 good |
| quality | Good | Good |
| files we hold | `ms.tar` + `vis_meta.tar` | `ms.tar` only |

- Flare: STIX 2209300352, GOES M1.1. STIX peak **at Earth: 03:57:23 UTC**,
  i.e. about 261 s into 1348545200 (within the last ~35 s of the obs).
- The STIX list peak is 4–10 keV (thermal). Radio bursts are likely
  stronger earlier in the impulsive phase (flare start at Earth 03:33).
  André asked for the peak time for this first image; a dynamic spectrum later
  will show the radio peak.
- Calibrator choice: PKS0408-65 is the only same-day calibrator with quality
  Good and the same channel list. HerA (1348569080) and 3C444 (1348574416) are
  "Some Issues"; 3C444 uses a different channel set ([101..187]).

## Unknowns to resolve first (not verified)

1. Where the tarballs are on calculon. Probably the shared archive
   `/mnt/nas05/data02/MWA_data/data/mwa_data` (from
   `solar-burst-multiview/docs/reproduction-plan.md`), but the listing André
   sent (`out.txt`) did not include its path.
2. What is inside each `ms.tar`: one MS or one per coarse channel? Has ASVO
   averaged time/frequency? Does the calibrator tar include a metafits?
   (Hyperdrive in `di-calibrate.sbatch` is called with MS + metafits.)
3. Whether `~/mwa/cal/GGSM_updated.fits` and
   `~/mwa/smoke/mwa_full_embedded_element_pattern.h5` still exist.

Inspection command (calculon login node), asked of André but not yet run:

```bash
A=/mnt/nas05/data02/MWA_data/data/mwa_data
ls -la $A | grep -E '1348545200|1348522216'
for f in $A/1348545200_*ms.tar $A/1348545200_*vis_meta.tar $A/1348522216_*ms.tar; do
  echo "== $f"; tar -tvf "$f" | awk '{print $3, $6}' | grep -v -E '/table\.f[0-9]|/table\.(lock|dat|info)$' | head -40
done
ls ~/mwa/cal/GGSM_updated.fits ~/mwa/smoke/mwa_full_embedded_element_pattern.h5
```

## Planned job (to write after the inspection)

One sbatch on the GPU cluster, modelled on
`solar-burst-multiview/scripts/calculon/di-calibrate.sbatch` (same SIF
`$MWA_DEMO_SIF`, `source ~/mwa/mwa-env.sh`, `singularity exec --nv`, sky
model `GGSM_updated.fits`, same tile flags unless metafits says otherwise):

1. Extract both obs to `/scratch/$USER/mwa/flare20220930/` (never write in
   the archive or in `/mnt/nas05/data02/rohit`).
2. `hyperdrive di-calibrate` on 1348522216 (PKS0408-65).
3. `hyperdrive solutions-apply` to 1348545200.
4. `wsclean` one image: ~1 s (4 × 0.25 s) centred on 03:57:23, one coarse
   channel near 150 MHz, field of a few degrees centred on the Sun
   (pointing is 3.7° from the Sun; use `-shift` or phase-rotate to the Sun).
   Convert time → timestep index from the MS TIME column, not by assumption.
5. Quick-look PNG plus the FITS. Then compare with the laptop test in
   `docs/calculon-mwa.md` (obs 1424757616: about 2′ beam, ch 113).

Partitions: `debug` is 30 min, one GPU; longer runs use `p3080` (as in
`di-calibrate.sbatch`) or `performance`. Memory under `MaxMemPerCPU`.

## What was done in this session

- Decoded André's archive listing (`out.txt`, 5,046 ASVO tarballs, 2,546 obs)
  and looked up every obs in ASVO TAP with the new script
  `STIX-MWA/src/mwa_obs_lookup.py` (untracked, not committed). Run it in conda
  env `solar-burst-multiview` (no `stixmwa` env exists on the Mac).
- Results, in `STIX-MWA/_results/my_mwa_obs/` (not committed):
  - `mwa_obs_inventory.csv`: one row per obs, all TAP columns, plus `files`,
    `stix_flares`, `use`.
  - `mwa_g0002_MX_flare_candidates.csv`: 24 M/X flares covered by G0002 Sun obs.
- Content: G0002 Sun-pointed 742, G0002 calibrators 119, G0060 IPS 1,241,
  C001 tests 144, other non-solar 300. G0002 Sun obs by array: Phase II
  Extended (2022, 2024) 261, Phase II Compact (2023) 271, Phase III (late
  2024–2025) 209, Phase I (2015) 1.
- Next flares after this one: 2024-05-14 X1.7 (3 obs, HydA same day), 2024-12-11
  M2.0 (24 YAMAGAWA trigger obs, Phase III, no same-day calibrator in our set).

## Rules reminder (AGENTS.md)

Branch `claude/<topic>`, never main; PR reviewed by Cursor; André merges.
Do not touch credentials or compute allocations without André asking; here he
asked for the calculon run. Add the agent-log entry below to
`solar-radio-imaging-spectroscopy/docs/agent-log.md` on a `claude/` branch
(the working tree is currently on `cursor/paircars-mac-cap`).

```
## 2026-10-05 — [claude] MWA archive inventory, first flare target

Looked up all 2,546 obs from André's ASVO download listing in TAP
(STIX-MWA/src/mwa_obs_lookup.py, untracked). 742 are G0002 Sun pointings,
1,241 G0060 IPS, 444 non-solar. 24 M/X STIX flares fall in G0002 Sun obs
(STIX-MWA/_results/my_mwa_obs/). First imaging target: 1348545200
(2022-09-30 M1.1 peak 03:57:23 UTC), calibrator 1348522216 (PKS0408-65), on
calculon. Handover: docs/handover-2022-09-30-flare-image.md.
```
