# Science direction, sabbatical 2026/27 (draft v3, 29 September 2026)

References: [`references/science_direction_refs.ris`](../references/science_direction_refs.ris) (42 entries, tag `sabbatical-science-direction`, import into Zotero).

Basis: CESRA 2026 book of abstracts (73 abstracts, all read), latest publications of the presenting authors (section 5), discussion with A. Benz, sabbatical plan (umbrella repo i4Ds/solar-radio-imaging-spectroscopy, docs/sabbatical-plan.md, 29 Sep 2026), i4Ds/solar-burst-multiview, i4Ds/STIX-MWA.

This document belongs to item 13 of the sabbatical plan ("Science goals with CESRA"). It does not change the repo scoping: solar-burst-multiview stays MWA-only, STIX work stays in STIX-MWA, e-Callisto in ecallisto_ng.

## 1. Constraints taken as given

- Season I (Oct Perth, Nov-Dec Kanpur): MWA. Season II (Jan-Mar Mexico): e-Callisto. All MWA science must fit Season I.
- Theme: agentic software for solar radio imaging spectroscopy. Science must use items 4-7 of the plan, not compete with them for time.
- Status (repo, 29 Sep): items 1-3 done (MWA as user; stack on laptop, calculon, CSCS; Sharma 2022 reproduction). Item 4 (solar extension, self-cal) and item 5 (P-AIRCARS on the Mac) in progress. Item 6 (image all events in the MWA solar archive) not started.
- MWA solar data cover many periods: 2015 (legacy correlator, Sharma 2022 obsids in R. Sharma's tree on calculon) and G0002 regular solar observations 2022-2025 (MWAX; observation names Oberoi2022A/Oberoi2024A_reg_Sun).

## 2. MWA Phase III: what changes, what does not

(Not to be confused with "Phase 3" in solar-burst-multiview, which is the re-imaging validation step.)

- Phase III (completed 2025; MWAX correlator since early 2022) correlates all 256 tiles at once instead of 128. Sensitivity doubled, baselines ~4x (Tingay et al. 2026, arXiv 2606.01644).
- Maximum baseline unchanged, ~5.3 km. The paper gives no synthesized beam values.
- Order of magnitude (lambda/B_max, my calculation): ~78 arcsec at 150 MHz, ~39 arcsec at 300 MHz, i.e. ~57,000 km and ~28,000 km on the Sun. Phase II measured beam: 1.0 x 0.89 arcmin at 185 MHz (Wayth et al. 2018).
- Gain: uv coverage and dynamic range (compact hexes and long baselines together). Benchmark: MeerKAT flare imaging with dynamic range >1e3 (Luo et al. 2026).
- Benz's criterion (resolution < v_A dt < ~1000 km) cannot be met spatially by any metric array (scattering broadens sources to arcmin scales; Mondal et al. 2025; Clarkson & Kontar 2026). It can be met in the time domain (dt ~0.1-1 s gives 10-1000 km).
- To check per obsid: 128 vs 256 tiles, configuration, time/frequency resolution (plan open question 5: 2015 legacy vs 2024 MWAX differ in channelisation and metadata).

## 3. Decision

Track B first (it is where the repos already are), Track A as the use of item 6.

Track B, weak impulsive emission and type I noise storms (Benz's coronal heating topic), with R. Sharma (Kanpur).
- Direct continuation of the completed Sharma 2022 reproduction (items 3-4). Same method (visibility subtraction, maps, burst statistics in src/solarburst/), applied to MWAX data.
- State of the field 2025-26: noise-storm fine structure with uGMRT (Mondal et al. 2025), LOFAR (Clarkson & Kontar 2026; Dey et al., CESRA 2026); MeerKAT early results (Kansabanik et al. 2025); SKA simulations of weak bursts (Sharma et al. 2026, AASKA II, co-author M. Battaglia).
- Contribution compatible with Benz's criterion: fluctuation statistics at <1 s, compared with EUV, not spatial resolution of structures.
- STIX link to test (not established): STIX microflare occurrence in the same active region vs noise-storm variability (hard microflares rooted in sunspots: Battaglia et al. 2024; Saqri et al. 2024).
- Overlap risk: core topic of NCRA (Oberoi, Mondal, Dey, Patra) and Glasgow (Kontar, Clarkson). G0002 is an Oberoi-led programme. Do it with them via Sharma.

Track A, STIX flares in the MWA archive (uses item 6, lives in STIX-MWA).
- The cross-match is already done for G0002 in i4Ds/STIX-MWA (P. Matavulj, 2025, src/find_flares_in_mwa.py): 803 STIX flares visible from Earth with MWA overlap (2022: 27, 2023: 34, 2024: 550, 2025: 192; 681 C, 106 M, 3 X), plus 800 flares not visible from Earth. Location comparison exists (compare_mwa_stix_locations.py).
- Proposal for plan open question 2 ("what all events means"): define item 6's event list as this STIX-MWA list, starting with the 106 M and 3 X flares with the highest overlap.
- Imaging pipeline per open question 1 (P-AIRCARS vs Sharma-style subtraction + WSClean); not prescribed here.
- STIX side (STIX-MWA repo): spectral component imaging (Stiefel et al. 2025; Guidetti et al. 2026), 3D where HXI observed (Palumbo et al. 2026).
- Physics: timing and scattering-corrected centroids of type III/J/U bursts vs STIX nonthermal sources, as done with European arrays by Bhunia et al. 2025, Morosan et al. 2025, Zhang et al. (CESRA 2026). MWA covers the Australian/Asian daytime window those arrays miss. Corrections: Kontar et al. 2025; Clarkson & Kontar 2026; Chrysaphi (drift rates).
- The 800 flares not visible from Earth are a distinct sample: occulted events where MWA can only see high-coronal emission (compare Krucker & Masuda 2026, high-corona HXR sources in eruptions).
- Statistics of non-detections (STIX flares without MWA signal and vice versa, already seen in the STIX-MWA draft) connect to Paipa-Leon et al. 2026.

Season II, e-Callisto (ecallisto_ng): Benz's STIX-first statistical programme.
- STIX flare list -> e-Callisto -> multi-station correlation (only verified solar events) -> classification (type II, DCIM <10 min, spikes) -> statistical X-ray/radio comparison. Automated.
- References: Bussons Gordo et al. 2026 (e-Callisto SC24 catalogue, preprint); deARCE 2023; Benz et al. 2002; Battaglia & Benz 2009.
- Track A events are the imaging ground truth (plan item 18).

## 4. First step

Fits open questions 1-3 of the plan: finish item 4 on one G0002 MWAX obsid from the STIX-MWA list (an M-class flare with 100% overlap, e.g. 2022-09-30 M1.1, obsids 1348544016-1348546976, calibrators listed in the CSV). That tests the MWAX path, gives Track B its first non-2015 data set, and is the first Track A event.

## 5. Mapping of CESRA 2026 abstracts to latest publications

Status: MATCH = paper for the abstract content exists; RELATED = latest related paper by presenter; NONE = nothing found (search partly rate-limited, so NONE is not proof).

| # | Presenter | Status | Latest reference |
|---|---|---|---|
| 1 | Vecchio | MATCH | Pesini et al. 2026 A&A 709 A253; Kretzschmar et al. 2026 ApJL 1001; Vecchio et al. 2024 ApJL 974 L18 |
| 2 | Emslie | MATCH | Emslie & Kontar 2026 ApJ 1001, 47 |
| 3 | Krafft | MATCH | Polanco-Rodriguez et al. 2026 arXiv 2601.09368 |
| 4 | Marongiu | RELATED | Marongiu et al. 2024 A&A (arXiv 2401.13198); Marongiu et al. 2026 Sol Phys 301, 117 |
| 5 | Gordovskyy | RELATED | Bate et al. 2026 ApJ 1006, 127; Gordovskyy et al. 2023 ApJ 952 |
| 6 | Bhunia | MATCH | Bhunia et al. 2025 A&A 695 A136 |
| 7 | S. Yu | NONE | (co-author: Kaltman et al. 2026 A&A 707 A158) |
| 8 | S. White | RELATED | Bastian et al. 2025 ApJ 980, 60 |
| 9 | Pellizzoni | RELATED | Mulas et al. 2026 Sci Rep 15, 44237 |
| 10 | Nindos | RELATED | Bastian et al. 2025 ApJ 980, 60 |
| 11 | Warmuth | MATCH | Warmuth et al. 2025 A&A 701 A20 |
| 12 | Nadiger | NONE | |
| 13 | Benz | MATCH | Benz et al. 2024 Sol Phys 299, 146 |
| 14 | Koval | RELATED | Koval et al. 2023 ApJ 952, 51 |
| 15 | Bouratzis | RELATED | Armatas et al. 2022 A&A 659 A198 |
| 16 | Lorfing | RELATED | Lorfing et al. 2023 ApJ 959 |
| 17 | Vocks | NONE (1st author) | co-author Morosan et al. 2025 A&A 693 A296 |
| 18 | Marque | MATCH | Marque et al. 2026 JSWSC 16, 24 |
| 19 | Alissandrakis | RELATED | Bastian et al. 2025 ApJ 980, 60 |
| 20 | Clarkson | MATCH | Clarkson & Kontar 2026 ApJ 1005 (arXiv 2605.31450) |
| 21 | Banys | NONE | |
| 22 | Napolitano | NONE | |
| 23 | Patra | MATCH | Kansabanik et al. 2025 FrASS 12, 1666743 |
| 24 | Kansabanik | RELATED | Kansabanik et al. 2026 arXiv 2606.31440 (MWA 60 Rsun FR result not yet found) |
| 25 | Morosan | MATCH | Morosan et al. 2025 A&A 693 A296 |
| 26 | Oberoi | MATCH | Patra et al. 2026 PASA 43 e011 (STORMY) |
| 27 | Kontar | MATCH | Kontar et al. 2025 ApJL 991 L57 |
| 28 | Nita | RELATED | Nita et al. 2026 arXiv 2607.21874; pyCHMP Zenodo 2026 |
| 29 | Fleishman | MATCH | Fleishman et al. 2026 Nat Astron 10, 363 |
| 30 | Gimenez de Castro | NONE | |
| 31 | Krishnan | NONE | |
| 32 | Bhati | NONE | |
| 33 | Hudson | RELATED | Cliver, Hudson, Fletcher 2026 Phil Trans A 384, 20250291 |
| 34 | Gan | NONE | |
| 35 | Hannah | RELATED | Bajnokova et al. 2025 ApJL 992 L1 |
| 36 | Magdalenic | RELATED | Deshpande et al. 2025 A&A 704 A95 |
| 37 | Jinge Zhang | RELATED | Zhang et al. 2024 ApJ 965 (type U imaging); timing paper not yet found |
| 38 | Martinez del Hoyo / Bussons | RELATED | Bussons Gordo et al. 2026 Research Square (e-Callisto SC24 catalogue) |
| 39 | Borque | NONE | |
| 40 | Garcia Delgado | RELATED | Bussons Gordo et al. 2026 (2nd author) |
| 41 | Bhandari | MATCH | Bhandari et al. 2026 A&A (arXiv 2512.21846) |
| 42 | Deshpande | RELATED | Deshpande et al. 2025 A&A 704 A95 |
| 43 | Shibasaki | NONE | |
| 44 | Limin Zhao | MATCH | Zhao et al. 2026 ApJS 285 |
| 45 | Prieto | MATCH | Gomez-Tornero et al. 2025 IEEE OJAP 6, 1535 |
| 46 | Minghui Zhang | NONE | |
| 47 | Barta | NONE | |
| 48 | Sherif | NONE | |
| 49 | Stiefel | MATCH | Stiefel et al. 2025 A&A 704 A316; Guidetti et al. 2026 arXiv 2609.17178 |
| 50 | Vilmer | MATCH | Paipa-Leon et al. 2026 A&A 710 A213; Pesini et al. 2026 |
| 51 | Chrysaphi | RELATED | Chrysaphi et al. 2024 A&A 687 L12 |
| 52 | Thepthong | NONE | |
| 53 | Massa | MATCH | Palumbo et al. 2026 A&A 710 A105 |
| 54 | Zucca | RELATED | Kumari et al. 2025 A&A 700 A274 |
| 55 | P. Singh | NONE | |
| 56 | Majee | RELATED | Morosan et al. 2026 arXiv 2603.28408 |
| 57 | R. Sharma | MATCH | Sharma et al. 2026 arXiv 2606.28782 |
| 58 | Farrugia | NONE | |
| 59 | Krucker | MATCH | Krucker & Masuda 2026 A&A 710 A107 |
| 60 | Normo | RELATED | Normo et al. 2025 A&A 698 A175 |
| 61 | Mulas | MATCH | Mulas et al. 2026 Sci Rep 15, 44237 |
| 62 | Dey | NONE | (Mondal et al. 2025 Sol Phys 300, 109) |
| 63 | Song Tan | RELATED | Tan et al. 2025 A&A 702 A189 |
| 64 | Suli Ma | MATCH | Ma et al. 2026 Nat Commun 17, 5131 |
| 65 | S. Mondal | RELATED | Mondal et al. 2025 Sol Phys 300, 109 |
| 66 | Kaltman | MATCH | Kaltman et al. 2026 A&A 707 A158 |
| 67 | Bastian | NONE | |
| 68 | Cuambe | NONE | |
| 69 | Verma | NONE | |
| 70 | X. Chen | RELATED | Chen et al. 2025 ApJL 990 L50 |
| 71 | Yi Chai | NONE | |
| 72 | Yihua Yan | MATCH | Yan et al. 2025 Space Weather 23, e2025SW004595 |
| 73 | Luo | MATCH | Luo et al. 2026 ApJL 998 L46 |

Note: Kansabanik 2022 in the sabbatical plan = ApJ 932, 110 (polarimetry algorithm), as in solar-burst-multiview/references/core_references.bib.
Unverified: the "Fleishman, Kaltman, Yu 2026 ApJ 999, 179" cited in Fleishman's abstract could not be confirmed.
