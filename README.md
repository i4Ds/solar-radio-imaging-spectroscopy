# Solar radio imaging spectroscopy

Umbrella for André Csillaghy’s 2026 sabbatical: **agentic software for solar
radio indirect imaging and spectroscopy** (FHNW / i4DS).

This repository is the map, not the code. Science and engineering live in the
repos below. Track work on the
[GitHub Project](https://github.com/orgs/i4Ds/projects/18).

## Repositories

| Repo | Role |
|------|------|
| [solar-burst-multiview](https://github.com/i4Ds/solar-burst-multiview) | MWA solar imaging: Sharma et al. 2022 reproduction, then 2024 campaign data. Calculon/WSClean stack. |
| [ecallisto_ng](https://github.com/i4Ds/ecallisto_ng) | e-Callisto data access and analysis. Mexico season (Jan–Mar). |
| [Karabo-Pipeline](https://github.com/i4Ds/Karabo-Pipeline) | SKA digital twin / Karabo experiments. |
| [STIX-MWA](https://github.com/i4Ds/STIX-MWA) | Earlier STIX–MWA coincidence scripts. Not the 2024 MWA science path. |
| [mwa-demo](https://github.com/i4Ds/mwa-demo) | Non-solar MWA shell pipeline (birli / hyperdrive / wsclean). |

Do not fold those trees into this repo.

## Where we are (27 September 2026)

**Imaging (Perth / Kanpur, Oct–Dec)** is started on the laptop against Sharma 2022:

- Phase 0–1 done: visibility subtraction matches Sharma’s `_sub.ms` bit-exactly (`scan_mean`, not the paper’s 15 s median).
- Phase 2 in progress: `python -m solarburst.figures` rebuilds paper Figures 3, 5, 7, 9, 11, 12. Figure 3 peaks match the caption to 15%.
- MWA containers run on calculon GPU nodes (Singularity); login node cannot. See `docs/calculon-mwa.md` in solar-burst-multiview.

**e-Callisto (Mexico, Jan–Mar)** has not started in this programme. Use `ecallisto_ng`.

Detail: [docs/sabbatical-plan.md](docs/sabbatical-plan.md). The MWA reproduction
mechanics stay in
[solar-burst-multiview/docs/reproduction-plan.md](https://github.com/i4Ds/solar-burst-multiview/blob/main/docs/reproduction-plan.md).

## Outcomes (target)

- Working prototype for agentic solar interferometry
- Continuation student work 2027–2030
- Updated e-Callisto analysis package
- 3–5 events analysed jointly across facilities
- Basis for an SNSF and/or EU e-Callisto data-analysis proposal
