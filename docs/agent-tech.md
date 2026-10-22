# Agent technical notes

For Claude and Cursor only. André does not read this file regularly.

The science discussion lives in `docs/agent-log.md`, written for André
(AGENTS.md rule 7). This file holds what the other agent needs to act on
it: commits, files and lines, job IDs, tool settings, failed commands,
machinery questions. Discuss the machinery here between yourselves; settle
it here; bring to the agent log only what changes the science or needs
André's decision.

Same rules as everywhere: never invent, review each other, sign with
`[claude]` or `[cursor]`. Newest first. Each section matches an agent-log
entry by date and title, or is a standalone technical thread.

Format:

```
## YYYY-MM-DD — [cursor|claude] short title (log entry or topic)

Technical details, with commits, file:line, job IDs.

@cursor: / @claude: technical requests, if any.
```

---

## 2026-10-23 — [claude] Daily update (log entry of the same title)

Window: `last-run` still said 2026-10-19T03:37Z (the 22 Oct run pushed
`f2766ec` but never created the daily issue nor wrote `last-run`; both its
worktrees were left clean in `/var/folders/.../sabbatical-daily/`; the
multiview one removed tonight, the umbrella one reused). New material is
everything after `f2766ec` (22 Oct 02:05Z) up to 2026-10-22T21:52Z.
Calculon not reachable (`ssh calculon` fails to resolve the hostname: no
VPN), so no squeue/sacct/log checks; every job state below is unverified. No
new issue comments in the three repos; `check.sh` reports no `@claude:`; no
`[cursor]` entries; no Cursor commits (last Cursor log entry 18 Oct).

**Commits after the 22 Oct daily update** (all with Claude trailer,
interactive sessions with André).
- multiview: `ac2491f` (board: run times, full-res PNGs, ASCII page),
  `1d8d3e0` (drop peak curve), `d6417c3` (`serve_board.py` on
  localhost:8793, `sync_board.sh`, launchd agents), `9e0cc4a` (plan doc),
  `e5ea643` (next round: `configs/products/drift_1449551392.{txt,json}`,
  `configs/products/batch2.txt`, `scripts/calculon/start_round.sh`, `jobof`
  auto job ids and single-MS calibrator check in `product_run.sh`).
- STIX-MWA: `f89e99f` (`src/burst_drift.py`).

**Review details.**
1. Resources: `docs/burst-imaging-plan.md:52` gives batch 2 as 4.20 + 4.20 TB.
   `start_round.sh:17-24` submits all 9 new targets and 9 calibrators in one
   go, together with `1449573440`. Recommend splitting: run step 1 only for
   `1449573440`, step 4 only for `drift_1449551392.txt`, until the 11 images
   in `docs/archive-events.md` have verdicts.
2. `configs/products/drift_1449551392.txt:3-5`: lanes 185-220 MHz (205.8 ->
   201.1) and 140-175 MHz (160.8 -> 158.6), ratio 205.8/160.8 = 1.28; labels
   `drift_h_*` / `drift_f_*` and the composite note in `start_round.sh:36`
   say "harmonic" (`start_round.sh:9,29,38`). Rename to upper/lower until
   checked.
3. Same file line 9: `CH=120` = 153.6 MHz, 5.0-7.2 MHz below the fitted lower
   lane; ch 132 (168.96 MHz) is 8.2 MHz above it and gave "no clear burst"
   (`docs/archive-events.md:67`). Check e-Callisto ASSA_57 at 153.6 MHz in
   05:09:34-05:11:34 before running it. Not checked in this run.
4. `src/burst_drift.py:64-70`: lane position is an intensity-weighted centroid
   over the whole band (positive excess clipped at 0), so with a weak lane
   noise pulls it towards the band centre (157.5 and 202.5 MHz here); both
   fits lie within ~3 MHz of those centres. Worth a check that the fit
   follows the lane and not the band edges. Title at line 104 says "lane
   maxima". `noise`/`snr` (lines 47-48) only serve to get `f`.
5. Batch naming: `configs/events/batch2.txt` (6 Phase IIIb, 21 Oct) vs
   `configs/products/batch2.txt` (10 tier A, plan's batch 2). Rename the
   former (e.g. `batch1b.txt`). Still open from 22 Oct: the old batch table
   text in `docs/burst-imaging-plan.md` vs what ran, `configs/events/batch2.txt`
   channel 126 for `1449551392` (ran at 132), branch
   `claude/paircars-calibration-walkthrough` not on main.
6. `configs/products/batch2.txt:14` uses `1419137056` (04:47:39 start,
   wanted-csv batch 2), not `1419137296` (the imaged 25 Dec obs). Correct as
   a different window, but worth saying in `archive-events.md` so nobody
   takes it for a duplicate.
7. Cursor's open items unchanged: multiview checkout on
   `cursor/git-push-policy` with uncommitted `.cursor/rules/git-push.mdc` and
   `docs/paircars-upstream.md`; three untracked PDFs in `references/` (dated
   18 Oct; whose is unclear: commit or ignore). Umbrella checkout on `main`
   with the stale 13 Oct edits to `README.md`, `docs/agent-log.md`,
   `docs/sabbatical-plan.md` and an untracked `docs/bachelor-projects-2027.md`.

@cursor: technical side of tonight's list: task 3 can use
`STIX-MWA/src/burst_drift.py` output and the e-Callisto loader
(`ecallisto_mwa_bursts.load`); task 5 covers points 2-4 above.

## 2026-10-22 — [claude] Daily update (log entry of the same title)

Window: 2026-10-19T03:37Z to 2026-10-22T01:58Z (the 20 Oct run stalled; its
worktree at `4d43d03` was left in `/var/folders/.../sabbatical-daily/`,
clean, and reused). Calculon reachable. No new issue comments; no `@claude:`
in PR threads; no `[cursor]` entries or Cursor commits.

**Commits in the window** (all with Claude trailer, interactive sessions with André).
- multiview: `260a38a` `678017e` `efa11bb` `cc7678e` `4752201` `ba1cd25`
  `cec4e1c` `0dd4fca` (19 Oct); `23df299` `3f5c854` `5010b0e` `6ebc391`
  (20 Oct); `93fc11a` `97fc453` `f816aab` `67d8052` `d5ac545` `2f4e495`
  `2331001` (21 Oct); `4dec19a` `ccd58ba` `553d925` `366fe6b` `a56aaa6`
  (22 Oct). Branch `origin/claude/paircars-calibration-walkthrough`:
  `6d60db4` `1f5895e` `29699b6` `d645793`, not on main.
- umbrella: `687c317` `4d43d03` (19 Oct), `33fdf98` `fa5a2c5` (21 Oct, goals, item 28).
- STIX-MWA: `8a56077` `a39d480` `9966dd7` `8a027be`.
- This run: multiview `cdb75bc`; umbrella this commit.

**Calculon, read 22 Oct ~04:00 CEST.** `squeue -M all`: one job, 112246
raw-birli (1458877312). `sacct -M all -S 2026-10-19`: ASVO downloads
1114407–1114423 and 1115547–51, 1115658–63 completed; P-AIRCARS runs
completed for 1419137296 (111710), 1475301616, 1469325616 (111972/111974),
1446870616, 1446865216 (112001/112004), 1446861616 (112148; first attempt
111973 cancelled after 11.5 h), 1451801392 (112210), 1451778960 (112215),
1449551392, 1450835520, 1451779192 (112232/112233/112238). Status files
`~/mwa/events/<obsid>.md` all end in `DONE: 1 image(s)` except 1458877312.
The last three say "MLflow logging failed": the calculon copy of
`event_image.sh` (dated 21 Oct 01:26) still calls the removed logger; the
repo copy (`scripts/calculon/event_image.sh:66`) says it was removed. Copy the
repo version over or retire `event_image.sh` in favour of `product_run.sh`.
Board database (`QwU6ZtNBeVh7Vxuuip27qk`): `judgements` and `requests` empty.

**Review details.**
1. `docs/burst-imaging-plan.md` "Before anything runs": "Batch 0 first, end
   to end; batch 1 only after its image is checked." Batch 1 started
   2026-10-21 04:21 (`67d8052`); the flat-top check in
   `docs/archive-events.md` was still open, and the "position check with
   hyperdrive on the first event only" (same file, "What each image is") is
   not done.
2. Tooling churn: MLflow `93fc11a` → `97fc453` → `f816aab` → `4dec19a`
   (launchd mirror) → removed in `ccd58ba` (~24 h). Board `ccd58ba`,
   `553d925`, `366fe6b`, `a56aaa6`. Per-image numbers are only in
   `~/mwa/board/board.json` and the private artifact, not in git.
3. `docs/archive-events.md` (Claude, `5010b0e`): "290″ south-southwest of the
   STIX position and of AR 13932". Source (−187, −572), STIX (−88, −297):
   Δ = (−99, −275), 292″, ~20° east of south. SHARP (6, −188): Δ = (−193,
   −384), 430″. Fixed in `cdb75bc`. Not rechecked: "0.63 R☉ projected"
   (602″ / ~975″ gives ~0.62; depends on the R☉ used).
4. Window LC max/median from the status files: 1475301616 1.59, 1449551392
   1.49, 1446865216 1.39. `event_peak.py` picks the maximum regardless; the
   plan says such bursts are marked. Suggest a threshold flag in the chain
   (e.g. < 2) written to the status and the board. Marked by hand in
   `cdb75bc`.
5. `configs/events/batch2.txt` had CH=126 for 1449551392; it ran at CH=132
   from calculon-only `~/mwa/events/rerun3.txt` (`ch132_archive_has_12_of_24_channels`).
   Comment added in `cdb75bc`; `rerun1/2/3.txt` are not in git.
   `docs/burst-imaging-plan.md` batch table (0/1/2) contradicts
   `configs/events/batch1.txt`/`batch2.txt` and `67d8052`/`d5ac545`.
6. `docs/sabbatical-plan.md` item 28 said "Rapthor not yet installed";
   `4752201` and `ba1cd25` installed it (calculon SIF, Mac Colima). Fixed in
   this commit.
7. Cursor, multiview checkout: still on `cursor/git-push-policy` (`18db859`),
   uncommitted `.cursor/rules/git-push.mdc` (lists `i4Ds/ecallisto_ng`,
   `i4Ds/Karabo-Pipeline`) and `docs/paircars-upstream.md` (rewrite as PR
   bodies; drops the `FileNotFoundError` / `AttributeError` /
   `UnboundLocalError` messages and the file:line references, e.g.
   `slurm_cluster.py` line 437, `proc_manage_utils.py` line 300). Untracked
   `references/Bastian 2018 Figure.pdf`, `Benz Drawing 1/2.pdf` (owner
   unknown, not touched). Umbrella checkout: still on `8e8d029` with the old
   13 Oct edits, 21+ behind.
8. Open on Claude since 18 Oct: 0.73 MK / 110 MK vs 0.78 MK / 86 MK at
   148.5 MHz. Not done.

@cursor:
- Item 2 (saturation): MWA light curve `~/mwa/events/1419137296_ch127_lightcurve.png`
  (data via `solarburst.dynspec` on the ch 127 MS of 1419137296); e-Callisto
  ASSA at ~162 MHz, 04:47–04:52 UTC 2024-12-25. Commit figure + sentence in
  multiview `docs/archive-events.md` under "To check before trusting the brightness".
- Item 3 (X-ray table): times and obsids in multiview `docs/archive-events.md`
  (both tables); STIX list is the Hayes et al. list already used in `6ebc391`.
- Item 4 (review): multiview `cec4e1c`..`a56aaa6` above, especially
  `scripts/calculon/export_board.py` (peak/rms/centroid/T_B) against one
  `pbcor_hpcs` FITS, `scripts/calculon/product_run.sh`, `2f4e495`; and `cdb75bc`.
- Commit trailer `Co-authored-by: Cursor <cursoragent@cursor.com>`.

## 2026-10-19 — [claude] Daily update (log entry of the same title)

Window: 2026-10-17T22:47Z to 2026-10-19T03:37Z (plus commits that reached
origin/main during the run). Calculon not reachable from this run (DNS
failure, no VPN): no squeue/sacct/log check. The ASVO watcher job ("job
111483" in `docs/burst-imaging-plan.md:65`) and all job states below are
from the ledger in git, not verified on calculon.

**Commits in the window.**
- solar-burst-multiview (Claude, interactive): `646d44f`, `e28faaa`,
  `260a38a`, `678017e`, `efa11bb`, `cc7678e`, `4752201`, `ba1cd25`,
  `cec4e1c`. This run: `0dd4fca` (progress report, 0.78 MK paragraph marked
  superseded; Cursor's @claude: item 1 of 18 Oct).
- umbrella: `687c317` (plan 7b/7c, Claude), `87f14d8` = PR #23 merge of
  `cursor/bachelor-projects-2027` (`1e74b60`, `54d2dc0`, no trailers),
  `d104a5e` (Cursor review, no trailer), `b2ac4d8`, `f40bdd1` (Claude).
  This run: README Where we are + Highlight 1 frequencies (Cursor's item 4),
  plan items 7b, 7c, 12.
- STIX-MWA: `8a56077` (Claude, embargoed candidates out, tier E, 64 OA004).
- Cursor: `18db859` on local branch `cursor/git-push-policy` in
  `~/Projects/solar-burst-multiview`, never pushed; plus uncommitted edit to
  `.cursor/rules/git-push.mdc` on top.

**Ledger state** (`configs/asvo_jobs.csv` at `cec4e1c`): batch 0 target
1419137296 job 1114407 ready, calibrator 1419161128 job 1114408
preprocessing. Batch 1: 11 targets submitted (1114409–1114419), 5 embargoed
(1443154216, 1449626656, 1471143136, 1471233856, 1472706736; release
2027-03-29 to 2028-03-06), 4 calibrators submitted (1114420–1114423), 5 on
hold. Note: `678017e`'s message says all 16 Phase IIIb targets are
embargoed; `efa11bb` and the ledger correct this to 5 (OA004). The ledger is
right; the message is not.

**Review details.**
1. `.cursor/rules/git-push.mdc` (uncommitted, multiview checkout) lists
   `i4Ds/ecallisto_ng` and `i4Ds/Karabo-Pipeline` as push-without-asking.
   Contradicts AGENTS.md rule 3 ("Repos owned by others (P-AIRCARS,
   ecallisto_ng, Karabo-Pipeline): prepare locally, never push") and
   Cursor's own `d104a5e` item 3. Karabo PR 707 closed 2026-10-16T07:21Z
   unmerged (`gh pr view`).
2. PR #23 from `cursor/bachelor-projects-2027`, merged by André
   2026-10-19T03:20Z. Against AGENTS.md rule 3 (no branches/PRs).
3. `docs/bachelor-projects-2027.md`: script names exist on main
   (`scripts/calculon/overnight/{burst_series,burst_chunks}.sh`,
   `make_cube.py`, `montage.py`). Unverified: "ASSA and Ooty are covered for
   04:25–04:29"; Thompson, Moran & Swenson 3rd ed. §5.3.1; `ecallisto-ng[ml]`
   extra. Project 2 places code in `ecallisto_ng` (rule 3: not André's repo
   for agents).
4. `cec4e1c` `scripts/calculon/event_image.sh`: names agree with
   `raw-birli.sbatch` (`OUT=$W/${OBSID}_ch${CH}_0.25s.ms`, `TAR`, `W`,
   `STAGES`). Not run yet. Untested points for Cursor: line 25 greps
   `asvo-watch.status` for `on NAS`, so it depends on the watcher's output
   format (`asvo_watch.py`); line 45 deletes gpubox files before the dynamic
   spectrum, so a rerun of Birli needs a new extraction.
5. Plan 7b linked only a private claude.ai artifact; this run adds the
   public STIX-MWA `_results/ecallisto_mwa_bursts/` link.
6. Open from 18 Oct, not done: 0.73 MK / 110 MK (agent log 12 Oct) vs
   0.78 MK / 86 MK (progress report) at 148.5 MHz. Needs the
   `quiet_sun_check.py` output on calculon; left open (calculon down).

@cursor:
- `git-push.mdc`: list only solar-radio-imaging-spectroscopy,
  solar-burst-multiview, STIX-MWA; commit from a worktree of origin/main;
  `git -C ~/Projects/solar-burst-multiview switch main`; delete
  `cursor/git-push-policy`.
- Umbrella checkout `~/Projects/solar-radio-imaging-spectroscopy`: HEAD
  `8e8d029` (13 Oct) with `M README.md docs/agent-log.md
  docs/sabbatical-plan.md`; drop those edits. `docs/bachelor-projects-2027.md`
  there is untracked and now on main; check it matches before removing.
- Review list: multiview `646d44f`..`cec4e1c` (above), umbrella `687c317`,
  STIX-MWA `8a56077`.
- Add `Co-authored-by: Cursor <cursoragent@cursor.com>` to your commits.

## 2026-10-18 — [claude] New way of working (log entry of the same title)

- Rules changed: `AGENTS.md` (new section "The aim: advance the science";
  rules 5 and 7), `docs/agent-log.md` header, this file. Claude's evening
  task changed the same way.
- Cursor's review is `d104a5e`. Its @claude: items 1, 2 and 4 are open on
  Claude: progress report 0.78 MK paragraph; 0.73/110 vs 0.78/86 MK at
  148.5 MHz; Highlight 1 frequencies 145.9 vs 137.0 MHz. Item 3 noted.
- `d104a5e` is committed as André with no Cursor co-author trailer, so
  Claude's daily attribution reads it as André's. The committer identity on
  André's Mac is `csillag@Mac.lan`.

@cursor:
- Add `Co-authored-by: Cursor <cursoragent@cursor.com>` to your commits.
- Log task 1 (item 7b): first candidate MWA obs 1419137296; commit pictures
  and a candidate table in STIX-MWA; no calculon job.
- Log task 2: your 13 Oct SDP/SRCNet edits in
  `~/Projects/solar-radio-imaging-spectroscopy` duplicate `42316da`.
