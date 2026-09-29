# Solar radio imaging spectroscopy

Umbrella for André Csillaghy’s 2026 sabbatical: **agentic software for solar
radio indirect imaging and spectroscopy** (FHNW / i4DS).

This repository is the map, not the code. Science and engineering live in the
repos below. Track work on the
[GitHub Project](https://github.com/orgs/i4Ds/projects/18).

## Repositories

| Repo | Role |
|------|------|
| [solar-burst-multiview](https://github.com/i4Ds/solar-burst-multiview) | MWA solar imaging: Sharma et al. 2022, non-solar MWA demo, P-AIRCARS on the Mac. Calculon/CSCS containers. |
| [ecallisto_ng](https://github.com/i4Ds/ecallisto_ng) | e-Callisto data access and analysis. Mexico season (Jan–Mar). |
| [Karabo-Pipeline](https://github.com/i4Ds/Karabo-Pipeline) | SKA digital twin / Karabo experiments. |
| [STIX-MWA](https://github.com/i4Ds/STIX-MWA) | Earlier STIX–MWA coincidence scripts. Not the 2024 MWA science path. |

Do not fold those trees into this repo.

## Where we are (29 September 2026)

**Imaging (Perth / Kanpur, Oct–Dec)** is underway:

- Sharma 2022 reproduction (items 1–3) done; solar extension and calibration (item 4) in progress.
- P-AIRCARS for SKA-Low (item 5) runs on this Mac. See `docs/paircars-mac.md` in solar-burst-multiview.
- MWA containers run on the laptop (Docker), calculon GPU nodes (Singularity), and CSCS (Podman).

**e-Callisto (Mexico, Jan–Mar)** has not started in this programme. Use `ecallisto_ng`.

Detail: [docs/sabbatical-plan.md](docs/sabbatical-plan.md). Science
direction (item 13): [docs/science-direction.md](docs/science-direction.md). The MWA reproduction
mechanics stay in
[solar-burst-multiview/docs/reproduction-plan.md](https://github.com/i4Ds/solar-burst-multiview/blob/main/docs/reproduction-plan.md).

## Outcomes (target)

- Working prototype for agentic solar interferometry
- Continuation student work 2027–2030
- Updated e-Callisto analysis package
- 3–5 events analysed jointly across facilities
- Basis for an SNSF and/or EU e-Callisto data-analysis proposal
