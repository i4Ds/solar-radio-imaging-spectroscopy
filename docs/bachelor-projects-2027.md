# Bachelor projects, from spring 2027

Two projects for computer science students, after the sabbatical (return
March 2027). One student each. They run in parallel. Each project is **150
hours** of the student's own time: reading, design, implementation, review,
and a run on real data. That is a semester project, not a script written in
a week.

Claude Code and Cursor are allowed, and expected. The 150 hours include the
time spent directing an agent, reading what it wrote, and rejecting what
does not match the design. The student signs the stage diagram (project 1)
or the schema (project 2) before implementation starts, and must be able to
explain every boundary in that document without the agent. Tests and the run
on real data are not delegated.

The students build software. The 2022-09-30 observation is the test fixture.
Solar physics stays with the supervisor.

By March 2027 the sabbatical will have left a working imaging path and one
event processed end to end (MWA observation 1348547272, 30 September 2022,
burst at 04:28 UTC), plus STIX and e-Callisto spectra for the same morning.
Imaging is a set of one-off scripts under
`scripts/calculon/overnight/` in
[solar-burst-multiview](https://github.com/i4Ds/solar-burst-multiview)
(`burst_series.sh`, `burst_chunks.sh`, `make_cube.py`, `montage.py`, and
others). Flare lists and overlays are in
[STIX-MWA](https://github.com/i4Ds/STIX-MWA). Spectrum access, including an
existing detector (`ecallisto-ng[ml]`), is in
[ecallisto_ng](https://github.com/i4Ds/ecallisto_ng). Season II
(January–March 2027) will have started the e-Callisto work. Project 2
continues that package.

Students do not push to `main` and do not push to P-AIRCARS. Any change to
flagging, averaging, or calibration is reviewed before it is run. Both
projects treat those settings as inputs.

The contract between the two projects is one JSON record: a time window, an
MWA observation id, a calibrator id, and the e-Callisto stations that cover
the window, each marked covered or missing. Project 2 produces records of
that shape. Project 1 consumes one. Neither student blocks on the other:
2022-09-30 is the shared example, fixed before either project starts.

---

## 1. A staged run for one solar observation

**Engineering problem.** Producing the 30 September image cube and PNG takes
several programs, each with its own environment variables, launched by hand
on the calculon login node. A failed step is restarted from the beginning,
including the imaging job, which runs for hours. Nothing checks that a later
edit still produces the 30 September products. A second observation is a copy
of the script.

**Repo.** [solar-burst-multiview](https://github.com/i4Ds/solar-burst-multiview).
The heavy imaging step stays the existing P-AIRCARS launch. The student does
not modify P-AIRCARS.

**What 150 hours is spent on.**

| Hours | Work |
|------:|------|
| 20 | Read `scripts/calculon/overnight/` and write the stage diagram: each stage, the files it reads, the files it writes, and how a failed stage is detected. The supervisor signs this before coding. |
| 25 | Config schema and validation, command-line interface, one directory per run (config, logs, products, git revision). |
| 30 | A runner with a lock and a log. A failed stage can be repeated without repeating stages that already succeeded. The imaging job is one stage; it is not restarted because a later plot failed. |
| 25 | Connect the existing cube, dynamic-spectrum, and PNG steps as further stages. Regression on 2022-09-30, channel 116, 04:28:20–04:29:10: cube metadata, peak position within a tolerance the supervisor sets, PNG present. |
| 30 | Run a second observation, named by the supervisor, through the same program. Fix the runner where that run exposes a gap. An imager failure is in scope only as a logged stage failure. |
| 20 | Tests that do not need calculon (config, lock, resume) and a README a classmate can follow for the 30 September config. |

**Done when.** A classmate can run the 30 September config from the README.
The regression passes. The second observation is a different config file, not
an edited program. A run that dies in a late stage resumes at that stage, and
a test on the laptop proves the resume without submitting a cluster job.

**Out of scope.** New calibration, Stokes V, a web UI, imaging a flare list,
training a model, and any change upstream in P-AIRCARS.

**Domain, one paragraph.** An MWA tile sums 16 dipoles into a beam tens of
degrees wide. The images use a different, much smaller beam, set by the
distances between tiles. The student needs that only so the two are not mixed
when reading headers. Thompson, Moran, and Swenson, *Interferometry and
Synthesis in Radio Astronomy*, 3rd ed., §5.3.1.

---

## 2. Which stations covered this flare

**Engineering problem.** A STIX flare time and the e-Callisto files for that
time are in different packages. Resolving "which stations have data, and
which do not" is a notebook per flare. A station with no file is easy to
record as a quiet Sun. Doing this for one year of flares means downloads,
partial files, retries, and a batch that can be stopped and continued.
`ecallisto_ng` already fetches spectra and has a detector. The missing piece
is a reliable join over a list of flares.

**Repos.** [ecallisto_ng](https://github.com/i4Ds/ecallisto_ng) for spectra
and the existing detector. [STIX-MWA](https://github.com/i4Ds/STIX-MWA) for
the flare list. The schema and the batch live in `ecallisto_ng`. STIX-MWA
calls them.

**What 150 hours is spent on.**

| Hours | Work |
|------:|------|
| 20 | Read `ecallisto_ng` and the STIX flare list. Write the schema, including the difference between "no file" and "file present, nothing detected". The supervisor signs this before coding. |
| 25 | One flare: stations, time overlap, frequency range, path or an explicit missing status. Cache of files already fetched. |
| 25 | Idempotent download: retry, partial file, unreadable FITS, station that does not observe that hour. Each of these has a test. |
| 25 | Batch over one calendar year of the STIX list (2022 unless Season II has replaced that choice). Restartable. A progress file so a crash does not refetch the year. |
| 20 | Call the existing `ecallisto-ng[ml]` detector behind an interface. Score it only on the 30 September fixture and a small labelled set the supervisor provides. Do not train a new model. A stored label file is an acceptable stand-in if the detector is not wired up in time; the batch and the schema still have to exist. |
| 20 | Command-line output of the JSON record Project 1 reads. Laptop tests. A short note of every failure mode the year run actually hit. |
| 15 | Review of agent-written code against the signed schema. Delete what the tests do not justify. |

**Done when.** The laptop tests pass without calculon. The 30 September
morning is produced by the same code path as every other flare in the year.
ASSA and Ooty are covered for 04:25–04:29, and a station with no file is
missing, not quiet. The year table states how many flares had no station.
The JSON for 30 September validates against the schema Project 1 reads.

**Out of scope.** Imaging, a new neural network, the full solar cycle, SDR
hardware, and station dynamic-range measurements. `radio_sunburst_detector`
and FlareSense stay as they are; this project does not merge them.

**Domain, one paragraph.** A dynamic spectrum is frequency against time. On
30 September, 04:25–04:29 is covered by more than one e-Callisto station and
contains a burst; an earlier window that morning does not. Those two windows
are fixtures. The student does not classify burst types.

---

## After both

Project 2's year table says which flares have station coverage. Project 1
can take one of those records as a config. Pointing the runner at further
flares is a later project. It is not part of these 150 hours.
