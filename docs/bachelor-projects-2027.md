# Bachelor projects, from spring 2027

Two one-semester projects for computer science students, after the sabbatical
(return March 2027). One student each. They run in parallel.

The students build software. The 2022-09-30 observation is the test fixture,
not a research question they have to answer. Solar physics stays with the
supervisor. What they need of it is written below in one paragraph.

By March 2027 the sabbatical will have left a working imaging path and one
event processed end to end (MWA observation 1348547272, 30 September 2022,
type III burst at 04:28 UTC), plus STIX and e-Callisto spectra for the same
morning. The code is a set of scripts with environment variables
(`scripts/calculon/overnight/burst_series.sh` and the `scripts/*.py` entry
points in
[solar-burst-multiview](https://github.com/i4Ds/solar-burst-multiview)), a
separate overlay stack in
[STIX-MWA](https://github.com/i4Ds/STIX-MWA), and e-Callisto access in
[ecallisto_ng](https://github.com/i4Ds/ecallisto_ng). Season II
(January–March 2027) will have started the e-Callisto work. Project 2
continues that package; it does not wait for it to be redesigned.

Students do not push to `main` and do not push to P-AIRCARS. Any change to
flagging, averaging, or calibration is reviewed before it is run. Both
projects treat those settings as inputs, not as something to invent.

The contract between the two projects is one JSON record: a time window, an
MWA observation id, a calibrator id, and the list of e-Callisto stations that
cover the window. Project 2 produces it. Project 1 consumes it. Neither
student blocks on the other, because 2022-09-30 is the shared example record,
fixed before either project starts.

---

## 1. One command for one solar observation

**Engineering problem.** Going from an MWA observation to a small image cube
and a PNG is today a shell script driven by environment variables, started
with `setsid nohup` on the calculon login node, with status in a markdown
file under `~/overnight/`. A second observation means editing those
variables. There is no test that a later change still produces the
30 September products.

**Repo.** [solar-burst-multiview](https://github.com/i4Ds/solar-burst-multiview).
Calculon is where the heavy step runs. The student does not modify
P-AIRCARS.

**Work.**

1. Replace the environment-variable interface with a config file (the JSON
   record above, plus channel, time range, and time resolution). One command
   reads it and runs the existing imaging steps.
2. Give each run its own directory, a lock, and a log. Two runs must not
   write the same output. This is the behaviour `burst_series.sh` already
   aims at; the project makes it a tested program rather than a script that
   only one person knows how to launch.
3. A regression test on 2022-09-30, channel 116, 04:28:20–04:29:10. The test
   checks products the current scripts already write (cube metadata, peak
   position within a tolerance the supervisor sets, PNG exists). It does not
   re-judge the solar result.
4. A second config, for an observation the supervisor names, that runs
   without editing the program. If that observation fails in the imager, the
   log names the failing step. That failure is a successful test of the
   software.

**Done when.** A classmate can run the 30 September config from the README
and get the logged products, the regression test passes in CI or in a
documented calculon job, and the second config is only a different file.

**Out of scope.** New calibration, Stokes V, a web UI, imaging the flare
list, and any change upstream in P-AIRCARS.

**The one paragraph of domain.** An MWA tile sums 16 dipoles into a beam tens
of degrees wide. The images the pipeline writes use a different, much
smaller beam, set by the distances between tiles. The student needs that
distinction only so they do not mix the two when they read headers. The
reference is Thompson, Moran, and Swenson, *Interferometry and Synthesis in
Radio Astronomy*, 3rd ed., §5.3.1.

---

## 2. From a flare time to the spectra that cover it

**Engineering problem.** STIX flare times and e-Callisto dynamic spectra live
in different packages and file layouts. Asking "which stations have data for
this interval, and what do those spectra look like?" is a notebook each
time. Missing stations are easy to confuse with a quiet Sun.

**Repos.** [ecallisto_ng](https://github.com/i4Ds/ecallisto_ng) for reading
spectra, [STIX-MWA](https://github.com/i4Ds/STIX-MWA) for the flare list.
The library and the schema live in `ecallisto_ng`. STIX-MWA only calls them.

**Work.**

1. A schema and a function: flare time interval in, list of station spectra
   out. Each item says whether the station has a file, the time overlap, and
   a path or an explicit "no coverage". No coverage is not the same status
   as a quiet spectrum.
2. Tests fixed to the morning of 2022-09-30. ASSA and Ooty cover 04:25–04:29.
   The test checks stations, time bounds, and that a station with no file is
   reported as missing. The supervisor supplies the expected station list.
3. A command-line interface that prints the JSON record Project 1 accepts,
   filled for one flare from the STIX list (observation id and calibrator
   left blank when the MWA overlap list has none).
4. An optional detector behind an interface, burst versus quiet, scored only
   on that fixture. A documented manual label file is an acceptable detector
   if a classifier does not beat it on the fixture. The grade is the
   interface and the tests, not the detection rate.

**Done when.** The test suite passes on a laptop without calculon, the
30 September fixture is recovered by the same code path as any other flare,
and the JSON for that morning validates against the schema Project 1 reads.

**Out of scope.** Imaging, a full-cycle catalogue, SDR hardware, and station
dynamic-range measurements.

**The one paragraph of domain.** A dynamic spectrum is frequency against
time. On 30 September the 04:25–04:29 window contains a burst seen by more
than one station; the earlier M1.1 window does not. The student uses those
two windows as fixtures. They do not classify burst types.

---

## What this is not

Not two more science case studies. Not the sabbatical item that images every
MWA solar observation. A later project can point Project 1 at flares that
Project 2 marks as covered. That is after these two, and it is still a
software task unless a scientist picks up the interpretation.
