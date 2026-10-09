# Handover: science results for the 2022-09-30 flare period (MWA, STIX, AIA, e-Callisto)

From: [claude] Claude Code session, 5–9 Oct 2026. To: the science chat that
wrote `handover-2022-09-30-flare-image.md`.

All figures are on branch `claude/flare-20220930` of i4Ds/solar-burst-multiview
(PR #3), folder `figures/flare20220930/`, unless noted. STIX and e-Callisto code
and plots are in i4Ds/STIX-MWA (PR #4).

## Short version

1. **The M1.1 flare (STIX 2209300352) is radio-quiet** at 129–240 MHz and in
   e-Callisto during its rise and peak. STIX places it on the NE limb, matching
   GOES/HEK and the hot AIA 131 Å loops.
2. **The radio activity that morning comes from AR 13110**, near disk centre:
   a persistent source at 144 MHz (03:57) and a strong type III group at
   04:25–04:29, at the time of a C5.3 flare in AR 13110 (HEK 04:26–04:30).
3. **The 04:28 type III** was imaged in 24 channels (129–239 MHz) and as ten
   4 s frames at 148.5 MHz. It sits ≈ 90–125″ SW of AR 13110, at the edge of the
   coronal hole, on open PFSS field lines leaving the west edge of the AR.
4. **Height ≈ 1.2 R☉** (≈ 140,000 km above the photosphere), from two
   independent estimates that agree: plasma frequency + density model, and the
   projected position of open field lines.

## Event and data

| | |
|---|---|
| Flare | STIX 2209300352, GOES M1.1, start 03:33:48 (Earth), STIX 4–10 keV peak 03:57:23, GOES peak 04:01 |
| Second flare | GOES C5.3 in AR 13110, 04:26–04:30 (HEK SSW Latest Events, (+239″, +158″)) |
| MWA | G0002 solar series (Phase II Extended, MWAX 0.25 s / 10 kHz raw), archive copies averaged by ASVO to 4 s / 160 kHz. 1348545200 (03:53–03:58, ch58–226); 1348547272 (04:27:34–04:32:30, ch101–187; the channel list changes at 04:27:34) |
| Calibrators | PKS0408-65 1348522216 (21:30, ch58–226); 3C444 1348574416 (12:00, ch101–187, ASVO quality "Some Issues", 8.4 h after the burst) |
| STIX | L1 pixel data, request 2209305292 (03:29–04:27 at Solar Orbiter) |
| Geometry | Solar Orbiter at Stonyhurst lon −178.8°, lat −7.9°, 0.401 AU: almost behind the Sun; light-time difference to Earth 299.5 s. Solar P angle 25.9° |

## Results

### M1.1 flare position (STIX, AIA)

- STIX 4–10 keV CLEAN (Earth 03:57:04–03:57:44, detectors 10–3): from Solar
  Orbiter the source is on the **west limb, footpoints occulted**. Reprojected to
  Earth view with a plane-of-sky assumption (well posed here, the views are
  nearly opposite): **(−869″, +412″)**, 15″ from the HEK/GOES position
  (−867″, +397″), on the hot AIA 131 Å loops. Figure:
  `figures/flare20220930/1348545200_ch113_peak_hpc_aia.png` (bottom row).
- 15–25 keV: no significant source (thermal phase; footpoints occulted).

### MWA at the STIX peak (03:57:24, 144.6 MHz)

- Only one bright source, **on AR 13110** (+215″, +95″); nothing at the flare
  (49 Jy/beam ≈ 3σ). The faint quiet-Sun disk is centred within ≈ 1′ of disk
  centre, so the frame is not shifted (tested because the offset from the
  flare looked suspicious). A J2000-vs-date frame error would give ≈ 1,100″ in
  solar X only and does not fit.

### Dynamic spectra (03:18–04:42)

- MWA 74–290 MHz (17 obs, 100 shortest baselines, per-channel median
  normalised) combined with e-Callisto ASSA (15–87, 108–524 MHz) and OOTY
  (45–165 MHz): STIX-MWA `_results/plots/flare_20220930_spectrogram_mwa.png`.
- M1.1 rise and peak (03:33–04:20): nothing strong in e-Callisto; MWA shows
  many short weak bursts at 80–200 MHz.
- 04:22–04:27: strong broadband emission in MWA, strongest at 80–130 MHz.
- 04:25–04:29: type III group at all e-Callisto stations (C5.3 in AR 13110).
- At 140–150 MHz the peak is 04:28:36 (e-Callisto) and **04:28:44 (MWA)**; the
  MWA light curve rises at 04:28:32, stays at 6–6.4× the median until
  04:28:56 and is back at background by 04:29:08.

### The 04:28:44 type III in images

| image | calibration | beam | peak/rms | peak (solar X, Y) |
|---|---|---|---|---|
| hyperdrive + wsclean, 143.4 MHz | 3C444 DI | 2.0′ × 1.3′ | 56 | (+365″, +185″) (double maximum) |
| P-AIRCARS, 143.4 MHz | 3C444 only | 2.5′ × 1.3′ | 70 | (+331″, +101″) |
| P-AIRCARS, 143.4 MHz | 3C444 + self-cal | 2.5′ × 1.3′ | **312** | (+306″, +101″) |

The 50 % contours of all three coincide (`paircars_vs_hyperdrive_042844.png`).
Self-calibration raises the dynamic range 4.5× and moves the peak by ≈ 25″.

- **24 channels at 04:28:44** (`paircars_burst_24ch_042844.png`): the burst is in
  21 channels from 129 to 233 MHz at ≈ (+300″, +100″); ch173, 177 and 187 fail.
- **Ten 4 s frames at 148.5 MHz** (ch116, best SNR, 465): 04:28:24–04:29:00
  (`paircars_burst10_ch116.png`, movies `burst10_ch116_aia.mp4`,
  `burst10_ch116_mwa.mp4`). Weak emission before (1.4–1.8 × 10⁵ Jy/beam), a
  jump to 9.3 × 10⁵ at 04:28:32, maximum 1.0 × 10⁶ at 04:28:44, decay to
  2.2 × 10⁵ at 04:29:00. Position stable within ≈ 100″; a weaker tail extends
  north to ≈ +500″ during 04:28:36–44. On the Stonyhurst grid the source is at
  ≈ 45° W, 5–10° N.

### Height

- **Position vs frequency** (19 channels, SNR > 50, mean of 04:28:36–44;
  `burst_height_vs_freq.png`, `centroids_042836_44.json`): 346″ ± 22″ from disk
  centre. If radially above AR 13110 (287″ projected), **r ≈ 1.21 R☉**. This
  lies between fundamental emission in a 2 × Newkirk corona and harmonic
  emission in a 1 × Newkirk corona.
- No frequency trend is measurable: the models predict 20–40″ across
  129–215 MHz; the channel-to-channel scatter (±22″) is systematic
  (statistical errors < 1″), most likely independent self-cal per channel.
  An earlier single-channel value of 1.37 R☉ (ch116) was the outlier.
- Density-model heights at 148.5 MHz (for reference): fundamental
  (n_e = 2.7 × 10⁸ cm⁻³) 1.13 / 1.23 / 1.35 R☉ for 1 / 2 / 4 × Newkirk;
  harmonic (6.8 × 10⁷ cm⁻³) 1.35 / 1.48 / 1.66 R☉; Saito (quiet equator)
  harmonic 1.16 R☉, fundamental not reachable.

### Magnetic field

- PFSS (GONG synoptic 04:04 UTC, source surface 2.5 R☉, sunkit-magex), 169 lines
  traced from around AR 13110: 36 open, 133 closed (`pfss_ar13110.png`). Open
  lines leave the **west edge** of the AR and fan out west/south-west and north.
  **All radio centroids lie on the west/south-west open bundle.** The open line
  passing closest to the mean centroid in projection does so at
  **r ≈ 1.20 R☉**, matching the plasma-frequency estimate.

## Caveats

- **Absolute positions** are not yet validated against background sources: the
  GLEAM/GGSM check failed in the solar field (dynamic range; even 3C273 at 4.9°
  and Virgo A at 15° were not detected), and the 3C444-field check crashed
  (script bug, fixable). Expect ≈ 1′ systematic (calibration, ionosphere).
- **Flux scale** is not established. P-AIRCARS applies a calibrator attenuation
  scaling; peak fluxes jump by up to ×100 between neighbouring channels, which
  points to a per-channel calibration problem. The quiet-Sun check was not
  completed (quiet-interval run stopped on request).
- **Fundamental or harmonic** is open: polarisation calibration was off; Stokes
  V would decide.
- **Scattering** displaces and broadens sources outward; geometric heights are
  partly upper limits. PFSS is static and current-free near an AR that had just
  flared.
- **Calibrator 3C444** is 8.4 h away and flagged "Some Issues" in ASVO.
- **AOFlagger** in the ASVO products flagged and zero-weighted 95–98 % of the
  burst time steps; all burst images use reflagged data (dead tiles only) or
  P-AIRCARS with `--do_forcereset_weightflag`.

## Pending

- **0.25 s data:** ASVO conversion job **1109734** (obs 1348547272, 0.25 s,
  160 kHz, no RFI flagging, ≈ 120–130 GB) submitted 9 Oct 15:18, state
  "Staging". 0.1 s does not exist (correlator 0.25 s).
- Polarisation calibration (P-AIRCARS with polcal) for F/H.
- Per-channel flux consistency; quiet-Sun flux check; GLEAM astrometry on the
  3C444 field and on a Sun-subtracted solar field.
- The 03:57 image (ch113) was made before reflagging; redo with
  `RESET_FLAGS=1`.

## Suggested science questions

1. Is the 04:28 type III the radio signature of the C5.3 in AR 13110, and why is
   the larger M1.1 on the limb radio-quiet at these frequencies?
2. Does the 04:22–04:27 broadband emission (80–130 MHz) belong to the same
   event, and where is it?
3. With 0.25 s images: does the source move during the rise (04:28:30–34)?
4. With Stokes V: fundamental or harmonic, and hence 1.2 R☉ in a 2 × Newkirk
   or 1 × Newkirk corona?
