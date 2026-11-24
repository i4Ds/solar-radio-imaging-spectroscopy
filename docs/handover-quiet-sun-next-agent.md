# Handover for the next Cursor agent — quiet Sun

Workhorse document (software, jobs, data):
`../solar-burst-multiview/docs/quiet-sun-handover.md`.

Notebook with pictures:
`../solar-burst-multiview/notebooks/06_quiet_sun_pipelines.ipynb`.

Three pipeline files:

- `scripts/pipelines/paper_2020.py` — Sharma & Oberoi 2020
- `scripts/pipelines/casa_remake.py` — CASA SuperUniform remake
- `scripts/pipelines/paircars.py` — P-AIRCARS
- `scripts/pipelines/diagram.py` — three-column figure

Read the workhorse handover in full. Summary of how to behave:

1. Lead with what a map is (day, time interval, method, what was averaged).
2. Always be imaging one event. After status, start or continue the next job.
3. If a job fails: log, path, next command, then run it.
4. Show the maps.
5. Paper citations on the gallery; published maps / CASA remake / P-AIRCARS.
6. Visibilities already on disk stay on disk (no new ASVO for 3 Dec noon, 4 Dec, 8 Dec).
7. Isolated worktree; `cursor/<topic>` pull request; gallery `i4Ds/mwa-quiet-sun` pushes `main`.
8. `figures/sharma2020/fig4.png` stays as it is.

Running when this was written: P-AIRCARS for 8 December 2015, 03:36–03:41 UT,
108 MHz, Slurm 266429. Next: remaining 8 December bands, then 4 December,
then 3 December 03:49 with `MEMFRAC=0.10`.
