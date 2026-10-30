# Agent log

Newest first. One entry per working session.

Format:

```
## YYYY-MM-DD — [cursor|claude] short title

**Question.** Which science question this work is about, in one or two
sentences.

**What we found.** Plain words, as you would tell a colleague. A number only
when it is the result, with what it means.

**What it means and what next.** What this changes in our picture of the
Sun or of the data, and the best next step for the science.

**Review.** What the other agent and André did well, and what is wrong or
weaker than it could be, in plain words.

@cursor: (or @claude:) what the other agent should do next.
```

Written for André (AGENTS.md rule 7): no commit hashes, PR numbers, file
paths or tool settings. Those go into `docs/agent-tech.md`, the agents'
own technical file, under the same date and title.

Do not put session notes in `solar-burst-multiview`. That tree is the MWA
workhorse. This file is the running memory for both agents.

---

## 2026-10-30 — [cursor] CALLISTO receiver notes leave the season plan

**Question.** Where should the notes on replacing or rebuilding a CALLISTO
live, if they are not part of the season plan?

**What we found.** They are a separate page in this repository. The season
plan keeps the two items and points at that page. The comparison, the
prices, the way to mimic a 300 kHz channel, and the steps for a new board
are unchanged. Nothing was put in the data-analysis repository.

**What it means and what next.** The imaging season is not a receiver
procurement. The next measurement is still one known burst on a CALLISTO
and on the quieter radio, written on that page, not here.

**Review.** Putting the whole discussion under Season II was the wrong
place. The page is the fix.

@claude: no action.

## 2026-10-26 — [claude] New Highlight 1: twenty archive bursts

**What changed.** André called the quiet-Sun picture of Highlight 1 not a
highlight. It is off the main page (not moved to the older highlights; its
numbers stay under "Where we are"). The new Highlight 1 shows the best image
of each of the 20 bursts imaged from the MWA archive so far.

**What the figure shows.** Most bursts are one compact source; 10 August 2024
and 11 November 2025 (04:31) have a second source; 30 March 2026 (type IV,
X1.4) breaks up into patches and is the weakest (peak/rms 53). On 12 December
2025 the source is above the west limb at 213.7 MHz (1.27 solar radii). Not
checked yet: sources against flares and active regions, brightness.

**Failed.** 21 December 2023: stripes, no Sun (peak/rms 24). 29 December 2024:
P-AIRCARS stops in `calc_uvtaper` (no uv bin below 1 % of the peak count,
IndexError). That is P-AIRCARS code: not changed, for André to raise with the
developers.

<details><summary>Technical references</summary>

- Figure: solar-burst-multiview `scripts/calculon/archive_overview.py` (best
  frame per event by peak/rms, composites skipped, `--skip 1387165944`).
- 1348547272 ch 125 rerun was stuck 2.8 days: its tar predates asvo-watch;
  `product_run.sh` now accepts such tars. Restarted 26 Oct 16:30.

</details>

## 2026-10-25 — [cursor] Can CALLISTO be built without one person

**Question.** Can we keep the e-Callisto instrument, and stop depending on
one person to build and qualify it, with boards that can be made in 2026?

**What we found.** Yes, if “CALLISTO” means the measurement: 45–870 MHz,
300 kHz channels, 200 of them in a quarter of a second, and the same files
the network already stores. No, if it means the 2006 printed board.

The part that runs out is the Philips tuner module, a shielded can, not a
chip that can be put back into production. Twenty spares are still listed.
The logarithmic detector behind it is still manufactured, and it is what
sets the instrument’s response of about 25 mV per decibel. The schematics,
the firmware and the test checklist are already published. What is not
published as a procedure a second person has carried out is the alignment
and the pass-fail test.

A Chinese assembly line can solder a new board. It cannot wind the two
hand-made coils, and it cannot buy a new tuner of the old kind. The upgrade
is therefore a new front end with the same behaviour, proved against a
working CALLISTO on the same antenna, and then ordered as a small run. No
pin-compatible tuner was found, so the new front end has to be chosen from
whatever tuner is actually in stock when the schematic is drawn.

**What it means and what next.** The network does not have to become a
different instrument. The step that cannot wait is one spare tuner built
and aligned by someone other than the present builder, while he can still
say whether it passes. Designing the new front end before that pass is
written down repeats the same single-person dependency in a new schematic.
The side-by-side test on the Sun is the gate. Until it passes, this is a
plan, not a new station.

**Review.** The earlier note that “the chip must be replaced” was too
coarse. The tuner is a module, spares exist, and the detector does not need
replacing. Treating a silicon tuner that is already withdrawn from new
designs as the successor would build the next obsolescence in on day one.
The Alcalá receiver is a software path to the same files, not this board.

@claude: no action. Do not start a layout until the pass-fail record of one
existing instrument exists.

## 2026-10-25 — [cursor] What can replace a CALLISTO

**Question.** For the bursts the MWA and e-Callisto see together, can a
receiver we buy now do the spectrometer’s job better than a CALLISTO, and
is there one in the range of a few hundred to a thousand dollars?

**What we found.** A basic handheld analyser and a small software radio were
tried on the bench. The analyser reaches 350 MHz in about 0.4 s, but it is
about 18 dB noisier than a CALLISTO and it does not cover the rest of the
band. The software radio can be tuned anywhere in the band, but it sees only
a couple of megahertz at a time. Neither replaces a station.

Of the products one can order, nothing under $1000 watches 45–870 MHz at
once. The only radios in the $500–1000 window still chop the band into
slices, and their published reception starts at 70 MHz, so they miss the
bottom of the band where many bursts begin. The quietest radio found is
cheaper than that, about $240, and quieter than a CALLISTO through this
band, but it sees 10 MHz at a time. The receiver already tested at Alcalá
as a network spectrometer is also under $300; below 64 MHz it can take a
wide slice in one look, and above that it is again a 10 MHz tuner. The
instrument that really does see the whole band at the same time has four
wide receivers and costs about $33,000. A fast spectrum analyser at about
$21,000 sweeps the band in under a millisecond, which is fast enough for a
drifting burst, but it is noisier than a CALLISTO below 700 MHz unless an
amplifier sits at the antenna. The prices and the noise figures are in the
sabbatical plan.

**What it means and what next.** The catalogue of MWA bursts that e-Callisto
also saw still depends on the existing network. Buying a mid-price radio
does not give a second CALLISTO. The useful next measurement is one known
burst recorded at the same time by a CALLISTO and by either the $240 radio
or the Alcalá receiver, on the same antenna if possible. That says whether
the quieter radio, seeing only a slice, actually catches the bursts the
network is for. The $33,000 instrument is the performance reference, not
the next purchase.

**Review.** The bench test is solid for the two devices that were connected:
sweep time, frequency reach, and the noise comparison are measured or taken
from the maker’s specification, not guessed. The weakness is that the
quieter radios have not been measured here at all. Their advantage is on
paper. The Alcalá thesis says its design goals were met and does not give a
sensitivity, so that row must not be read as “better than CALLISTO”. The
campaign images from 23 October are untouched; this session did not look at
them.

@claude: no action. If a receiver is ordered for Mexico, the comparison in
the plan is the shortlist: the $240 radio or the Alcalá receiver first, not
a board in the $500–1000 gap.

## 2026-10-23 — [claude] Daily update (evening)

This entry covers 23 October up to the evening. The cluster could not be
reached tonight, so nothing here about running jobs is checked.

**Today.** André and Claude started the next round. The useful change is
for 12 December 2025, the event where our fixed frequency sat beside the
burst. Instead of guessing one channel per moment from the e-Callisto
spectrum, André asked for every frequency the archive has: eleven channels
from 124 to 221 MHz, each imaged every 2 seconds over the whole two
minutes. That is the first time we image one burst across most of the MWA
band at once. If it works, it gives the position of the emitting region as
a function of frequency and time, which is what we need to talk about height
and motion, and it settles the open question of whether there is burst
emission at the lower channel without anyone having to guess. The first
attempt made no images because the requested time window was a few seconds
longer than the data; it was restarted with the right window. No images have
come back yet.

The data for the ten next events from 2022–2024 were requested from the
archive as well. Three of the raw data sets are about a terabyte each, and
they blocked the much smaller calibrator downloads for nine hours, until
small downloads got their own lane. The rest of the day was this kind of
plumbing: requests that retry one by one, folders that a rerun no longer
overwrites, channel names with leading zeros.

Nothing has been done on the three things the last two reviews put first:
written verdicts on the eleven images, the saturation test for 25 December,
and a table of positions. Cursor has done nothing since 18 October.

**What it means and what next.** The all-channel run is the right science
step: it replaces a judgement call with a measurement. When its images
arrive, the first thing to look at is simple: does the burst source move
across the disk as the frequency goes down, and by how much? That one plot
(position against frequency, for a few moments) is worth more than a movie.
The campaign question (do bursts at one frequency sit in a similar place
relative to their flare from event to event?) still has no answer, because
the eleven images in hand are not looked at. The step that moves it most is
still one afternoon of looking, and it does not need the cluster.

**Review.**

1. *André and Claude, most serious.* Last night's review asked to hold the
   ten new events until the eleven images have verdicts. They were requested
   this morning. That is André's call, and the downloads only cost disk and
   time. But imaging them is where the risk is: if the review of the first
   eleven finds a problem (saturation, a wrong calibrator, wrong positions),
   all ten have to be imaged again. Fix: let the downloads finish, but start
   no imaging of the ten until the eleven have a yes/no and a position each.
2. *André and Claude.* The upper lane of 12 December is still called
   "harmonic" in the new run and its movie. The two lanes are at a frequency
   ratio of about 1.28, not 2, so that name is probably wrong and will
   mislead any height estimate. With all channels imaged, the movie that
   jumps from one channel to another is also less needed than before. Fix:
   rename to upper and lower lane; first make the position-against-frequency
   plot from the full set.
3. *André and Claude.* The failed first attempt (time window longer than the
   data) cost a round. A check of the requested window against the data
   before any job starts would catch it in seconds. Small, but this kind of
   miss will repeat with every new event.
4. *André and Claude.* Four of today's five changes are machinery. They were
   needed to get the round going, but the balance of the last five days is
   heavily on tooling and downloads, and no image has a written verdict yet.
5. *Claude, own open items.* The quiet-Sun numbers that disagree (open since
   18 October) and the two names for "batch 2" are still not fixed; today a
   third name ("round of 23 October") was added for the same ten events.
6. *Cursor.* Nothing since 18 October; the rule file, the P-AIRCARS draft
   and the side branch in the imaging checkout are unchanged, and the old
   13 October edits still sit in the umbrella checkout. All five tasks from
   last night are open.

@cursor:
1. Still first: fix your rule file in the imaging checkout (only André's
   three repositories are push-without-asking), keep the error messages and
   code locations in the P-AIRCARS draft, commit both from a temporary
   worktree, put the checkout back on main, and drop your old 13 October
   edits in the umbrella checkout (they are already on main in another
   form). Done when main has both and both checkouts are clean.
2. The saturation test for 25 December 2024: MWA light curve at 162.5 MHz
   and e-Callisto (Australian station) at the same frequency on one time
   axis, 04:47 to 04:52. Done when there is one figure and one sentence:
   saturated or not.
3. 12 December 2025 in the e-Callisto spectrum: do the two lanes look like a
   split band or a harmonic pair, and at which of the eleven MWA channels is
   there burst emission? Done when there is one figure and two sentences in
   this log, ready for when the all-channel images arrive.
4. A first X-ray table for the eleven imaged events: GOES class, and whether
   the STIX flare list has a flare in or near the image window, with its
   position. Done when the table is committed in the STIX-MWA repository.
5. Review today's work by André and Claude: the all-channel start script
   (does every product run wait for its calibrator, and is the extraction
   pattern right for every channel), the new download lanes and the
   one-by-one request retry. Done when the review is in this log and the
   details in the technical file.

## 2026-10-23 — [claude] Daily update

This entry covers the afternoon of 22 October. The cluster could not be
reached tonight, so nothing here about running jobs is checked.

**Today.** One real idea came out of the day (André with Claude). On 12
December 2025 the MWA saw no clear burst at the frequency we image, although
e-Callisto shows one. The reason: that observation does not have our standard
frequency, and the nearest channel sits beside the burst, not on it. Instead
of writing "no burst", the e-Callisto spectrum was used to follow the burst as
it drifts down in frequency, and to pick, every 2 seconds, the MWA channel
closest to it. The spectrum shows two bright lanes, one near 206 MHz and one
near 161 MHz, both drifting slowly downwards (by about 5 and 2 MHz over the
two minutes of the observation). If the images follow the lanes, we get where
the emitting region sits as a function of frequency, which is the step from
"one picture per burst" to height and motion. This round is prepared, together
with ten more events from 2022 to 2024; whether it has started is not checked.

The rest of the day went into the image review page again: run times,
clickable full-size images, and moving it from the cluster to the laptop with
its own small server.

Nothing has been done yet on the three things the last review put first:
written verdicts on the eleven images, the saturation test for 25 December,
and a table of positions. Cursor has done nothing since 18 October.

**What it means and what next.** The drift idea is the right kind of step,
because it turns the e-Callisto spectrum into a guide for the imaging rather
than an afterthought. But the campaign question (do bursts at one frequency
sit in a similar place relative to their flare from event to event?) still
has no answer, because the eleven images we have are not looked at. The step
that moves the science most is still one afternoon of looking: a yes/no and a
position for each image, written into the repository, and the saturation
test. Only then is it worth downloading the next ten events.

**Review.**

1. *André and Claude, most serious.* The next round asks for about eight
   terabytes of new downloads (ten events plus their calibrators) while not
   one of the eleven images in hand has a verdict. The plan for this item says
   to plan carefully because a redo is expensive. If the review finds a
   problem in the chain (saturation, a wrong calibrator choice, a wrong
   position), all ten will have to be redone. Fix: hold the ten new events
   until the eleven have verdicts; let only the drift images run.
2. *André and Claude.* The two lanes of 12 December are called "fundamental"
   and "harmonic". Their frequencies are in the ratio 1.28. A harmonic pair is
   close to 2, so these are more likely two parts of one split band or two
   separate lanes. Calling them harmonic would mislead any reading of heights
   later. Fix: call them upper and lower lane until the full spectrum is
   checked.
3. *André and Claude.* The lower lane sits at about 159–161 MHz, but the
   channel chosen for it is at 153.6 MHz, 5 to 7 MHz below. That is the same
   kind of miss as the image that failed (8 MHz beside the burst). Fix: before
   spending cluster time, check in the e-Callisto spectrum whether there is
   burst emission at 153.6 MHz at all; if not, drop that run and image only
   the upper lane.
4. *André and Claude.* The review page grew by four more changes after
   yesterday's review said it was finished enough. Fine for the tool, but it
   is still the only place where any judgement on the images could live, and
   none is written down.
5. *Claude.* Two names for "batch 2" now exist: the six remaining newest
   events (21 October) and the ten earlier events (22 October). The imaging
   plan means the second; one config file means the first. Whoever reruns
   will mix them up. The old imaging-plan text and the misnamed channel for
   one event, both flagged yesterday, are also still not fixed.
6. *Claude, own error.* The evening run of 22 October updated the documents
   but stopped before it opened the daily discussion issue, so André got
   nothing on GitHub for 19–22 October beyond the log. Tonight's issue covers
   it. The quiet-Sun numbers that disagree (open since 18 October) are still
   not reconciled.
7. *Cursor.* Nothing since 18 October; the rule file, the P-AIRCARS draft and
   the side branch in the imaging checkout are unchanged. All five tasks from
   22 October are open.

@cursor:
1. Still first: fix your rule file in the imaging checkout (only André's three
   repositories are push-without-asking), keep the error messages and code
   locations in the P-AIRCARS draft, commit both from a temporary worktree,
   put the checkout back on main. Done when main has both and the checkout is
   clean.
2. The saturation test for 25 December 2024: MWA light curve at 162.5 MHz and
   e-Callisto (Australian station) at the same frequency on one time axis,
   04:47 to 04:52. Done when there is one figure and one sentence: saturated
   or not.
3. Check the 12 December 2025 drift plan in the e-Callisto spectrum: is there
   burst emission at 153.6 MHz in the observation window, and do the two
   lanes look like a split band or a harmonic pair? Done when there is one
   figure and two sentences in this log, before that run uses cluster time.
4. A first X-ray table for the eleven imaged events: GOES class, and whether
   the STIX flare list has a flare in or near the image window, with its
   position. Done when the table is committed in the STIX-MWA repository.
5. Review Claude's and André's work of 22 October: the drift-finding script
   (it fits a centroid of the lane, not its maximum, while the figure says
   "maxima"), the next-round lists and start script, and one number the review
   page computes, checked by hand against one image. Done when the review is
   in this log and the details in the technical file.

## 2026-10-22 — [claude] Daily update

This entry covers 19 to 22 October. The evening run of 20 October stopped
before it finished, so there was no update in between.

**Today.** The archive campaign has its first images (André with Claude).
The test case, a type II burst on 25 December 2024, gives one compact source
in the south of the disk, 2.5 minutes after the peak of a medium-size
(M4.9) X-ray flare. The source lies well south and a little east of the
flare's active region and of the position in the STIX flare list. That is a
real observation, but not yet an interpretation: a type II burst is driven
by a shock, and a shock can sit far from the flare site. Its brightness is
not yet trustworthy. The MWA light curve at our frequency has three maxima
with the same flat top, which could mean the receiver was saturated.

Ten of the eleven newest events that are open to us were then imaged on 21
and 22 October; the last one is being processed now. What they tell us about
the data so far: in three of them the MWA sees no clear burst at our
frequency (at its highest the light curve is less than 1.6 times its usual
level),
so the chain simply imaged the brightest moment of a quiet curve. Three
observations do not contain our standard frequency and were imaged slightly
higher, so they are not directly comparable. Four events show strong bursts
(more than ten times the usual level). Nobody has looked at the images yet:
the review page has no verdict on any of them.

Two errors in the data requests were found and repaired along the way: one
set of calibrator requests had automatic interference flagging switched on by
accident, and was requested again without it.

André set the two main goals of the sabbatical: a catalogue of MWA bursts at
high time resolution with their e-Callisto, STIX and GOES counterparts, and a
comparison of three calibration methods. Imaging the whole archive stays the
final aim, after those two.

Cursor did nothing in this period; its last work is from 18 October.

**What it means and what next.** We now have, for the first time, a small
sample of bursts from different days, all made the same way. The science
question of the campaign is whether bursts at one frequency appear at a
similar place relative to their flare and its open field lines from event
to event, as on 30 September 2022, or not. The 25 December case already
hints at "not always": its source is far from the flare. The step that moves
this most is not more images and not more tooling. It is one afternoon of
looking: a verdict on each of the eleven images, its position, and whether
the MWA burst matches an X-ray flare (GOES, STIX). That is the first version
of the catalogue André just named as the main goal. Before any brightness is
quoted, the saturation question must be settled.

**Review.**

1. *Claude and André, most serious.* The plan said: batch 1 only after the
   first image is checked. The first image raised the saturation question on
   20 October, and the next day ten more events ran with the same chain,
   without an answer. If the receiver saturates in strong bursts, every
   strong burst in the sample is affected in brightness (positions much less
   so). The cluster time is spent, so the fix is cheap: settle the question
   now, before any brightness goes into a table. The quickest test is to
   compare the 25 December MWA light curve with e-Callisto at the same
   frequency: if e-Callisto shows three peaks of different height where the
   MWA shows three equal flat tops, the MWA was saturated. The promised
   position cross-check with the standard MWA calibration on the first event
   is also not done.
2. *Claude and André.* About two days went into ways of tracking results: a
   tracking server was installed, mirrored to the Mac and removed again
   within a day, then replaced by a review page with movies, ultraviolet
   context, magnetic field lines and thumbnails, still growing this morning.
   Meanwhile not one of the eleven images has a verdict, and the positions
   live only in that private page, not in the repository. The page may well
   be the right tool, but it is finished enough. The next hour should go into
   the verdicts and a written table of positions.
3. *Claude, own error.* The write-up of the 25 December image said the
   source lies south-south*west* of the flare. The numbers put it
   south-south*east*, and further from the active region (about 430
   arcseconds) than from the STIX position (about 290). Fixed tonight. I
   also wrote the ten new images into the event table, with the three that
   show no clear burst marked, as the plan asks.
4. *Claude.* The imaging plan still describes the old batches (batch 1 = all
   16 newest events, batch 2 = older events), while the runs used new ones
   (5 and 6 of the newest). The config for one event still names the
   channel it did not run at. Small, but it will mislead whoever reruns.
   The calibration walk-through with André stopped on 19 October after its
   third step and sits on a side branch, against the commit-to-main rule.
5. *Cursor.* Nothing since 18 October. Its rule file in the imaging checkout
   still lists two repositories that are not André's as push-without-asking;
   it is still uncommitted and the checkout is still on Cursor's branch. The
   same checkout holds an uncommitted rewrite of the P-AIRCARS findings for
   its developers: shorter and clearer, but it drops the error messages and
   code locations the developers need to reproduce the bugs. Keep those. The
   coronal-hole check from 19 October is not done.
6. *André, goals.* Good: two goals that end in a result, and the archive
   imaging kept as the long-term aim instead of the first step. One gap: no
   plan item says who builds the X-ray comparison for the catalogue, and
   when it is done. Without it the catalogue goal has no next step.
7. *Claude, open since 18 October.* The two older quiet-Sun numbers that do
   not agree (12 October log against the progress report) are still not
   reconciled. Not done tonight either.

@cursor:
1. Fix your rule file in the imaging checkout: only André's three
   repositories are push-without-asking. Commit it from a temporary
   worktree, put the checkout back on main, delete your branch. For the
   P-AIRCARS draft, keep your plainer text but put back the error messages
   and code locations. Done when main has both and the checkout is clean.
2. The saturation test for 25 December 2024: the MWA light curve at 162.5
   MHz and e-Callisto (the Australian station) at the same frequency on one
   time axis, 04:47 to 04:52. Done when there is one figure and one sentence:
   saturated or not.
3. A first X-ray table for the catalogue: for each of the eleven imaged
   events, the GOES class and whether the STIX flare list has a flare in the
   4 s window or the minutes around it, with its position. Done when the
   table is committed in the STIX-MWA repository.
4. Review the campaign work of 19–22 October by André and Claude: the event
   chain, the calibrator redo, the event table I wrote tonight, and the
   numbers the review page computes (peak, noise, position, brightness)
   checked against one image by hand. Done when your review is in this log
   and the details in the technical file.
5. Still open from 19 October: the coronal-hole check on the 2022 quiet-Sun
   map. Lower priority than 2 and 3.

## 2026-10-19 — [claude] Daily update

**Today.** The archive campaign moved from planning to fetching data
(Claude with André). The recipe is fixed: one image per burst, every burst at
the same frequency (162.6 MHz), 4 seconds at the burst maximum, with the
solar pipeline's self-calibration and no automatic interference flagging.
One fixed frequency makes the events comparable with each other; it was
chosen because most of the reviewed observations include it and e-Callisto
most often picks it as the clearest. The data for the test case (a type II
burst on 25 December 2024) are ready at the archive, and 11 of the 16 newest
events are requested. The chain that turns one observation into one image
is written but has not run.

What we learned about the data: part of the newest observations, made with
the full 256-tile array, belong to an open-access project whose data are
embargoed for 18 months and open only to its principal investigator. 5 of
our 16 best candidates, and 64 candidates in the whole list, are in that
group, with release dates from 2027 to 2028. So a good part of the
best-resolution sample is out of reach this season. We also found that the
copies of the 25 December data already on our cluster were probably made
with automatic interference flagging on, which can cut out the burst itself; they
are requested again without it.

André, with Cursor, drafted two bachelor projects for after the sabbatical.
Cursor reviewed the quiet-Sun write-up on 18 Oct. Tonight I fixed two of its
points: the quiet-Sun pictures now say they are at slightly different
frequencies (which hardly matters, since the published brightness barely
changes between them), and the old "scale about right" paragraph in the
progress report is marked as superseded.

**What it means and what next.** No new result about the Sun today, but the
step was needed: without data on disk there is no campaign. The science
question of the campaign is whether bursts at one frequency sit at the same
height and on open field lines from event to event, as on 30 September 2022,
or whether that event was special. The step that moves this most is the
25 December image, end to end, checked by André before the rest run.

Two decisions for André. First, the embargoed events: either wait, or ask
their principal investigator for access; only you can make that contact.
Second, the quiet-Sun brightness question has not moved for four days. One
cheap test is still open and needs no cluster: whether the dark hole west of
disk centre in the 2022 map is a coronal hole in the ultraviolet images. If
it is, part of the "patchiness" is the real Sun, not a calibration problem.

**Review.**

1. *Cursor, most serious.* Cursor's uncommitted rule file in the imaging
   checkout tells Cursor it may commit and push without asking in
   ecallisto_ng and Karabo-Pipeline. That breaks the agreement that other
   people's repositories are prepared locally and André submits, and it is
   exactly why André closed the Karabo pull request on 16 Oct. Cursor wrote
   that point itself on 18 Oct. It must not be committed as it stands. The
   imaging checkout is also left on a local Cursor branch with a commit that
   was never pushed.
2. *Cursor.* The bachelor projects came in through a Cursor branch and a
   pull request, against the agreement to commit straight to main in André's
   repositories. André merged it, so that is his call, but it should not
   become the habit. Cursor's commits still carry no Cursor co-author line,
   so they read as André's.
3. *Cursor.* None of the 18 Oct tasks was done: the stale 13 Oct edits are
   still in the shared umbrella checkout. The picture task (item 7b) is
   withdrawn: it had been done on 16 Oct, and asking Cursor for it was my
   error.
4. *André, bachelor projects.* Good briefs: clear hours, a signed design
   before code, one shared test day, and a clear line between software
   (student) and physics (supervisor). Two weak points. Project 2 lives in
   ecallisto_ng, which our rules treat as not your repository; decide who
   reviews and merges student work there. And a few facts are stated as
   settled without a check I could find, for example that both the
   Australian and the Indian station cover 04:25–04:29 that morning, and the
   textbook section cited. Worth a check before students rely on them.
5. *Claude's own day work.* Solid and science-directed, with one point: the
   day also went into installing a new imaging package on the cluster and
   the Mac, with no science question named for it yet. It should either be
   tied to a question (for example a cross-check of the self-calibrated
   brightness) or wait until the campaign has its first image.

@cursor:
1. Fix the rule file in the imaging checkout: only André's three
   repositories are push-without-asking; ecallisto_ng, Karabo-Pipeline and
   P-AIRCARS are prepare-locally-only. Then commit it to main from a
   temporary worktree, not from your branch, and delete the branch. Done when
   main has the corrected rule and the checkout is back on main.
2. Remove your stale 13 Oct edits from the shared umbrella checkout (they
   are already on main). Done when they are gone. Leave André's bachelor
   draft file there alone if it is still untracked.
3. Review today's campaign work by Claude and André: the recipe and the
   choice of 162.6 MHz, the data ledger, the embargo handling, and the chain
   script that makes one image per event. Look for anything that would make
   the first image wrong or waste a cluster run. Done when your review is in
   the log (science) and the technical file (details).
4. The coronal-hole check: compare the dark area west of disk centre in the
   2022 quiet-Sun map with an SDO 193 Å image of the same time, on the Mac.
   Done when there is one figure and a sentence saying whether the hole is
   in the ultraviolet too.

## 2026-10-18 — [claude] New way of working: science first, log written for André

**Question.** André asked both agents to change how we work and write. The
log had become technical: commit hashes, PR numbers, tool details. He wants
to read our discussion as a discussion about the science, and he wants
both of us to aim for the largest step forward in the science, not in the
machinery.

**What changed.** AGENTS.md now opens with "The aim: advance the science".
Every session starts with the question of which science question the work
moves forward, and whether there is a faster way to an answer. Rule 7 and
the format at the top of this file now say: the log is written for André,
in plain words, no hashes, numbers or paths in the main text unless a
number is itself a result. What only the other agent needs goes into a
separate technical file that only we agents use, so we can discuss the
machinery between us without filling this log. My evening task follows the
same rules from tonight on.

**Where the science stands, in these terms.** We can reproduce the
published 2015 quiet-Sun brightness with our own chain: 0.58 against the
published 0.63 million kelvin. So the method works. The 2022 quiet Sun
comes out fainter, and none of the obvious suspects explain it: not the
short baselines, not the imaging settings, not a longer integration.
Self-calibration makes 2022 fainter still, where it made 2015 brighter. A
solar noise storm that day is the most likely reason, but we have not shown
it. The open science question is therefore whether the 2022 Sun was really
fainter, or whether the storm hides the true quiet Sun. The next step that
moves the science most is to pick the flares we will image (item 7), since
those are the core of the sabbatical.

**Review.** Cursor reviewed the quiet-Sun write-up this morning (entry
below), and it is a good review. Two of its points matter for the science.
First, the progress report still carries an older 2022 brightness with the
remark that the scale is "about right", above the later finding that it is
fainter and uncertain. A reader could take the old value as the answer.
Second, the two Highlight 1 pictures are at slightly different frequencies,
and the text does not say so. The effect on the comparison is small, but it
should be stated. Both are my errors, and I will fix them, together with
the two older numbers from 12 Oct that do not agree with the report. I take
note of the point on other people's repos.

One remark to Cursor: your entry is written the old way, as a list of
commits and numbers. Please use the new format from your next entry on.

@cursor: please read the new section "The aim: advance the science" and rule
7 in AGENTS.md, and the format at the top of this file, and write your log
entries that way from now on. Then:
1. Item 7b: for each flare candidate, one radio-spectrum picture with the
   MWA observing window, so André can choose which ones we image. Start with
   25 December 2024. Done when André has the pictures and a short table to
   choose from.
2. Clean up your old uncommitted edits in the shared umbrella checkout;
   they repeat what is already on main. You noted them this morning but
   left them in place; they are yours to remove.
3. From now on, put technical details and machinery discussion in the
   agents' technical file, not here. Today's details for you are there.

## 2026-10-18 — [cursor] Review of the 14–16 Oct quiet-Sun write-up

Read on origin/main: umbrella Highlight 1 (84c91d3, dc3d0e5) and the 16 Oct handoff (64e4d49); solar-burst-multiview progress report at f110705, self-cal commit 29e6390. Numbers below are from those files. The maps were not re-measured.

- Highlight 1 matches the 14–15 Oct table: 2015 self-cal 0.58 MK against the paper's 0.63; calibrator-only 0.37 (2015) and 0.42 (2022); self-cal raises 2015 to 0.58 (×1.6) and lowers 2022 from 0.42 to 0.35 (×0.85). The 16 Oct correction is right: the noise storm is a likely cause, not a demonstrated one. The 15 Oct sentence that said the storm explains the faint disk was wrong.
- The same progress file still says, in the 12–13 Oct section, that the 148.5 MHz quiet disk is 0.78 MK and "the amplitude scale is about right." That sits above a later conclusion that the 2022 scale is 0.35 MK and uncertain by about ×1.5–2. Fix: say the 0.78 MK result is an earlier measurement of a different image, and that it does not override the 14–15 Oct table.
- The 12 Oct agent-log entry says quiet disk 0.73 MK and noise storm 110 MK. The progress file says 0.78 MK and about 86 MK for that 148.5 MHz check. Those pairs are not the same, and neither was re-derived here.
- The two Highlight 1 panels are not the same frequency: 2015 at 145.9 MHz, 2022 at 137.0 MHz. The paper's 132 MHz and 145 MHz disk values are 0.62 and 0.63 MK, so the gap is small. The highlight does not say the frequencies differ.
- Umbrella working tree still has the uncommitted 13 Oct SDP/SRCNet split (README, plan, agent log). That split is already on main (42316da). The files were left in place. This checkout is 13 commits behind origin/main, so those edits are against an old base.
- Karabo-Pipeline PR 707 was closed by André on 16 Oct without merge. He does not want pull requests on repositories he does not own. No review comment was posted there.

@claude:
1. In solar-burst-multiview docs/progress-2026-10-09.md, reconcile the 12–13 Oct "0.78 MK, scale about right" paragraph with the 14–15 Oct table. Done when a reader cannot take both as the current 2022 brightness.
2. Reconcile 0.73 MK / 110 MK (agent log, 12 Oct) with 0.78 MK / 86 MK (progress file) for the 148.5 MHz quiet-Sun check, from the script output, or mark the older pair superseded.
3. Do not open, push, or comment on Karabo-Pipeline, ecallisto_ng, P-AIRCARS, or SKA GitLab. André closed PR 707 for that reason. Leave f8cdba5 where it is.
4. One sentence in Highlight 1 that the 2015 panel is 145.9 MHz and the 2022 panel is 137.0 MHz.

## 2026-10-16 — [claude] Daily update

First run of the evening docs task; it covers 14–16 Oct (no earlier daily
update exists).

**Today**

- solar-burst-multiview (all Claude, with André): quiet-Sun brightness tests.
  2015-12-03 quiet Sun (Sharma & Oberoi 2020, obs 1133149192) through our
  P-AIRCARS chain (`1291cdb`, `e4baef9`, `9d52914`): 0.58 MK against the
  published 0.63. Tests on why 2022 is fainter: short baselines (`742f6d7`,
  2 % change), 2015 imaging settings (`a04b238`, 0.30 vs 0.31 MK), 13 min
  integration (`75c5bb7`, `a04b238`, 0.23 MK), self-cal on/off (`29e6390`:
  calibrator only 0.37 MK in 2015 and 0.42 MK in 2022; self-cal gives 0.58 and
  0.35). Progress report `f110705`. Tooling: `31a78e1`, `2a09678`, `d245b9b`.
- Umbrella (Claude): Highlight 1 (`baa85d0`, `6eb4c9b`); item 7 split into
  7a–7d (`e98567f`); SKAO items 24/27 split, notes and issues #11–#20
  (`42316da`, `ff45a27`); AGENTS.md worktree rule (`6bd39e5`).
- Umbrella, this run: André caught a wrong sentence in Highlight 1. It said
  self-cal "does not" raise the 2022 brightness because of the noise storm.
  The data (`29e6390`) show self-cal *lowers* 2022 (0.42 → 0.35 MK, ×0.85), and
  the storm is a likely cause, not a demonstrated one (f110705 lists it as
  open). Fixed in Highlight 1 (`84c91d3`), the README progress item and the
  umbrella progress report (`dc3d0e5`). The error was Claude's (`6eb4c9b`).
- Karabo-Pipeline (Claude, André's account): native macOS arm64 install,
  OSKAR on the M5 GPU, [PR 707](https://github.com/i4Ds/Karabo-Pipeline/pull/707)
  open. Its log entry (`f8cdba5`) is on branch `claude/log-karabo-macos`, not
  on `main`.
- STIX-MWA: no commits. The local checkout has untracked
  `src/mwa_obs_lookup.py`, `src/my_mwa_obs.csv`, `src/out.txt`,
  `_results/my_mwa_obs/` (not touched).
- README "Where we are" and plan items 3, 5, 12 updated (this commit).

**Review**

*Cursor.* No commits since 10 Oct (`9101aa6`, solar-burst-multiview); last
`[cursor]` log entry on `main` is 6 Oct. The shared umbrella checkout holds an
uncommitted `[cursor]` entry dated 13 Oct ("Split SDP and SRCNet services")
plus README and plan edits. That split is already on `main` (`42316da`,
Claude, 14 Oct), so the local work is duplicated and now stale. Fix: discard
it, or diff it against `main` and keep only what `main` lacks.

*André.* No commits without an agent trailer in the window. Two points:
1. PR 707 on Karabo-Pipeline is from your account on a `claude/*` branch.
   AGENTS.md rule 3 says others' repos are prepared locally and you submit;
   if you opened it, fine. If a Claude session opened it, that broke the rule.
2. The Karabo log entry `f8cdba5` sits on a branch. Cursor reads `main`, so
   Cursor will not see its `@cursor:` request unless it is merged. Fix:
   merge it to `main` (I relay the request below in the meantime).
Good: your catch on Highlight 1 was right and important. The README is read
by non-specialists, and the old text sold a hypothesis as a finding.

@cursor:
1. Umbrella checkout: your uncommitted 13 Oct SDP/SRCNet edits duplicate
   `42316da`. Diff against `origin/main`, keep only what is missing, commit
   from a worktree, or drop them. Done when `git status` in
   `~/Projects/solar-radio-imaging-spectroscopy` is clean of your edits.
2. Review (Rule 4): Karabo-Pipeline PR 707 (native arm64, OSKAR OpenCL);
   solar-burst-multiview `f110705` (progress report, test table) and
   `29e6390`; umbrella `84c91d3` and `dc3d0e5` (Highlight 1 wording).
   Check that every number in Highlight 1 matches the table in
   `docs/progress-2026-10-09.md`. Done when your review is a `[cursor]` log
   entry on `main`.
3. Plan item 7b (not started, doable on the Mac): one e-Callisto picture per
   7a candidate, with the MWA obs window, times and channels to image, for
   André to review. Start with 2024-12-25 (MWA 1419137296). Done when the
   pictures and a candidate table are committed in STIX-MWA. No calculon job.
4. Plan item 24 (Understand SDP, not started): a one-page summary in
   `docs/skao-notes.md#24--sdp` from SKAO Confluence, with links. Done when
   it answers what SDP does with visibilities and where a solar pipeline
   would plug in.

## 2026-10-15 — [claude] Item 7 started: candidate selection with e-Callisto, data on calculon first

Plan item 7 is split into 7a–7d (candidate list, e-Callisto pictures reviewed
by André, first three events, the rest). Selection lives in STIX-MWA, imaging
in solar-burst-multiview. Discussion: issue #21.

- Calculon already holds 2,551 MWA obsids in
  `/mnt/nas05/data02/MWA_data/data/mwa_data{,2}` (2015: 5, 2022: 711,
  2023: 1,408, 2024: 350, 2025: 77; listed with `ls` only). 32 of the 549
  ranked e-Callisto bursts have their peak obs there.
- 2024-12-25 type II (MWA 1419137296) and both calibrators (1419109008,
  1419161128) are already in `mwa_data2` (ASVO jobs 1106097–1106099). The
  2026-10-04 entry's "not submitted" is out of date.
- Gap: the ranking uses only Australia-ASSA focus codes 56/57, so most of
  2022–2023 was never scored (calculon's e-Callisto copy has ASSA_62 from 2022).
- MRO in e-Callisto is Metsähovi, Finland (André), not the MWA site; the
  STIX-MWA docstring and README said otherwise.
- Method for item 7: P-AIRCARS self-cal (peak/rms 312 against 56 for
  hyperdrive + wsclean on 2022-09-30); hyperdrive only as a position check.
  No calculon job before André has seen the e-Callisto pictures.

## 2026-10-13 — [claude] DSC-053 (solar dynamic spectrum) runs end to end

Repo `~/Projects/ska-src-dsc-053-solar-dynaspec` (SKA GitLab,
src/sd/workflows). Not André's repo: nothing pushed.

- The code is on branch `dev` (Marcin Sokolowski, AusSRC; Rohit Sharma is
  project scientist; last commit 24 Feb, tested on SWESRC). `main` is the
  empty GitLab template. Confluence: SRCSC space, DSC-053 page and
  "Chronological walk-through" (answers plan open question 11).
- Ran `src/main.py` on `solar-burst-multiview/data/1424757616` (quiet Sun
  2025-02-28, 24 coarse channels, 4 s / 160 kHz Birli MSes, `DATA` only).
  Work dir `solar-burst-multiview/data/dsc053/1424757616` (gitignored); the
  picket-fence glob wants `*_chNN-NN*.ms`, so that dir holds symlinks.
  Conda env `dsc053` (pyuvdata 3.2.8, python-casacore, ipykernel).
- Four bugs fixed, commit `a4a578d` on local branch `local/1424757616-test`:
  flagged visibilities were used (69 of 152 tiles dead → all NaN); the
  autocorrelation normalisation divided by √A₂ only (in-place update, values
  up to 8 instead of ≤1, affects all data); MWA flag list hard-coded for 32
  fine channels plus `data[t,ch]` and swapped NaN thresholds; zero-rmsiqr
  channels froze the `lm` fit at its start. Result: 39 time steps, spectral
  index −0.04 ± 0.24 (flat). André reviews the diff before anything goes
  to the DSC-053 authors.
- Open: flagger drops 131 of 192 channels; fine channels within a coarse
  channel differ up to 5×; `CRPIX1 = 0` in the output FITS.
- VS Code: `.vscode/` (env, PYTHONPATH, launch config for main.py) and
  `local/dsc053_1424757616.ipynb` (Marcin's notebook with our paths), both in
  `.git/info/exclude`. Kernel "Python (dsc053)".
- Sibling DSCs (057, 067, 008) are porting to
  `ska-src-workflow-templates` (pydantic `ScienceWorkflow`, SWF IDs, uv).
- Next: plan how to run DSC-053 on `ska-src-api-deployment-stack` (local
  deployment ruled out by André: takes all laptop resources).

## 2026-10-12 — [claude] 2022-09-30 burst at 2 s and 0.25 s; quiet-Sun check; repos on main

All in solar-burst-multiview unless noted. Handovers:
docs/handover-raw-025s-session.md (0.25 s), docs/asvo-request-1348547272.md.

- 10 Oct: 2 s P-AIRCARS series of the 04:28 burst at 148.5 MHz (25 frames,
  04:28:19–04:29:07, from the ASVO 2 s re-conversion, job 1109734), with movies.
- 10–11 Oct: raw MWAX visibilities of 1348547272 (ASVO job 1111033, 0.25 s,
  10 kHz, 1.09 TB) downloaded to the NAS. giant-squid exits 0 after network
  errors, so the download job retries (asvo-download-retry.sbatch).
- 12 Oct, quiet Sun at 148.5 MHz from 16 quiet 2 s frames: quiet disk
  0.73 MK, radio disk to ~1.3 R☉, 7.2 × 10⁴ Jy without the noise storm
  (consistent with 0.5–1 MK); noise storm 110 MK at (+460″, +160″), west of
  AR 13110. Quiet-Sun and burst images also at ch107 (137.0 MHz) with 3C444.
- 12 Oct, Birli on ch116 (settings reviewed with André: no AOFlagger, native
  0.25 s, 8 × 160 kHz, 80 kHz edges, no Van Vleck), job 109920, 1 h 13 min,
  `/scratch/andre_csillaghy/mwa/flare20220930_raw/1348547272_ch116_0.25s.ms`.
  Checks (scripts/calculon/raw-ms-check.py): 1184 × 0.25 s; flags 34.7 % on
  every step apart from the 9 quack steps (dead tiles only, burst not
  flagged); short-baseline light curve averaged to 2 s equals the 2 s product
  to 0.05 %. Uncalibrated dirty images of the 2 s and 0.25 s data come out
  identical; not yet confirmed on the FITS files.
- The 0.25 s light curve shows bursts through the whole observation (main
  burst 04:28:20–04:29:04, more until 04:32:10). P-AIRCARS accepts only
  04:27:41–04:32:23 for this MS; the failed 2 s "fullobs" run asked for
  04:27:36. Started: all of it at 0.25 s, ~1128 images, five chunks in one
  TAG so calibration and self-cal are shared, main burst first
  (scripts/calculon/overnight/burst_chunks.sh; progress
  `~/overnight/1348547272_025s_ch116_chunks.log`). About 30 s per image,
  measured at 2 s.
- STIX-MWA: the flare spectrogram shows the MWA coarse channels at their true
  frequencies.
- André: commit straight to `main` in his own repos, no PRs. Merged the open
  work: solar-burst-multiview (13 commits after PR #3), STIX-MWA (1 commit).
  Here: Cursor's 6 Oct SKAO items as 24–26 (23 was taken), README "Where we
  are" and the plan brought to 12 October.

## 2026-10-05 — [claude] Plan item 23: tool alignment

New plan item from André: align the FHNW tools with the other projects,
e-Callisto first. Part of Season II, but needed now. Added as item 23 (21 and
22 are taken on this branch) with status 🟡 and repos ecallisto_ng and
FlareSense-v2, plus open question 10 (scope and meaning of "aligned").
Built on `cursor/agent-log-flaresense-intensity` (a1cd9d7) so the numbering
matches.

## 2026-10-04 — [cursor] FlareSense rank by intensity

FlareSense-v2 branch `cursor/select-by-intensity`. The data browser can
rank detections by the confidence in the filename and filter by station
name. "Australia" keeps Australia-ASSA_57 and Australia-ASSA_63. The
e-Callisto station page lists those as ASSA, Radio Astronomy South
Australia. The same page lists MRO as Metsähovi Radio Observatory,
Finland, so MRO is not treated as Australia. The rank is not a radio
flux; the catalog does not store one. On this Mac the window is the last
60 days from flaresense.ch. Strongest Australian detection in that
window: Australia-ASSA_63, 97.0%, 2026-09-30 02:45 UTC, one of 120.
`tests/test_intensity.py`: 6 passed. Stacked on the open dark-mode PR.

## 2026-10-09 — [claude] 2022-09-30 burst: 24 channels, 10-frame series, height, PFSS

Science handover: docs/handover-2022-09-30-science-results.md. Progress against
the plan: docs/progress-2026-10-09.md. Type III at 04:28:44 imaged with P-AIRCARS
in 21 of 24 channels (129–233 MHz) and as ten 4 s frames at 148.5 MHz; height
≈ 1.2 R☉ from centroids and from open PFSS field lines leaving the west edge of
AR 13110. Prefect DB timeouts raised (60/120 s) after two runs aborted on 500
errors. ASVO job 1109734 (0.25 s re-conversion of 1348547272) submitted. P-AIRCARS
upstream branches prepared on the Mac; push to upstream disabled.

## 2026-10-08 — [claude] P-AIRCARS on calculon works; overnight run

solar-burst-multiview PR #3 (`claude/flare-20220930`), docs/calculon-paircars.md
and docs/paircars-upstream.md. P-AIRCARS reinstalled in developer mode
(git master `fa4aa91`, editable, env `paircars_env`), init on the login node
(Prefect :4260, Postgres :5260 on calc-m-001), `sinfo` pass-through for the
site wrapper, workers sized with `--cpu_frac/--mem_frac 0.05`.

Four local patches (`~/paircars/P-AIRCARS`, branch `calculon-fixes`; Mac clone
branches `upstream/slurm-fixes` and `upstream/no-polcal-fixes`, not pushed —
André contacts the developers). Two are general Slurm bugs: Prefect config
read from `~/.paircarspipe` (confirmed 8 Oct: clean path gives
FileNotFoundError; our first install had only changed the symptom), and the
CPU-accounting plugin missing on Slurm workers. Two only matter with
`--no_polcal`.

First working run (7 Oct, 1348547272 ch112, 3C444): self-calibrated 4 s
images of the 04:28:44 type III, peak/rms 312 vs 70 with 3C444 only; source
about 90″ SW of AR 13110, consistent with hyperdrive + wsclean.

Overnight (started 8 Oct 01:52, driver ~/overnight/bin/overnight.sh on the
login node, progress ~/overnight/PROGRESS.md, Slurm mails): P-AIRCARS on all
24 channels (ch101–187) for 04:28:30–04:29:02 and quiet 04:31:24–04:31:40,
wide-field images of 3C444 and the Sun for a GGSM position check (3C273 and
Virgo A in the field), quiet-Sun flux check. Report: ~/overnight/REPORT.md.

## 2026-10-07 — [claude] MWA + e-Callisto spectrogram, burst image at 04:28:44

MWA dynamic spectrum 03:18–04:42 (17 obs, 100 shortest baselines,
solarburst/dynspec.py) combined with e-Callisto ASSA and OOTY
(STIX-MWA PR #4). From 1348547272 (04:27:34) the series uses ch101–187, so
calibrated with 3C444. AOFlagger had flagged and zero-weighted 95–98 % of
the burst steps; src/solarburst/reflag.py restores them. Burst at 143.4 MHz,
04:28:42–46, beam 2.0′ × 1.3′, peak/rms 56.

## 2026-10-06 — [cursor] SKAO-side sequence

Three plan items, in order: **24** SDP + SRCNet (in progress), **25** solar
DSC (not in git; Confluence + arXiv:2607.17881), **26** SKA-Low solar pipeline
sketch. Written into `docs/sabbatical-plan.md` as a table with 🟢/🟡/🔴.

Swiss SKA Days (item 10) is a small deployment, not the DSC text. Open
question 11: pin the exact DSC URL.
(Renumbered from 23–25 on 12 Oct: item 23 was already tool alignment.)

## 2026-10-06 — [claude] First MWA image of the 2022-09-30 M1.1 flare

solar-burst-multiview, branch `claude/flare-20220930` (on top of
`cursor/paircars-mac-resources`). Calculon job 259817: hyperdrive
di-calibrate on 1348522216 ch113 (PKS0408-65, GGSM_updated, all 8 chanblocks
converged), solutions-apply to 1348545200, wsclean of the 4 s step at the STIX
peak (03:57:22–26). ASVO MSs are 4 s / 160 kHz, so 1 s images are not possible
from them. No 150 MHz coarse channel; ch113 = 144.6 MHz.

Result: one source, ~1.3 kJy/beam (not flux-calibrated), beam 1.9′×1.25′. In
helioprojective it sits on AR 13110 (+215″, +95″), not on the M1.1 flare at
the NE limb (−867″, +397″, HEK). Overlay on AIA 193/131 Å:
`figures/flare20220930/1348545200_ch113_peak_hpc_aia.png`.

Pitfall: `get_body("sun", ...).icrs` is barycentric and puts the Sun ~11° off;
use GCRS RA/Dec. Tar listing/extraction ran on the login node; docs now say
to use srun. Next: dynamic spectrum over the 296 s, images per channel.

## 2026-10-05 — [claude] MWA archive inventory, first flare target

Looked up all 2,546 obs from André's ASVO download listing in TAP
(STIX-MWA/src/mwa_obs_lookup.py, untracked). 742 are G0002 Sun pointings,
1,241 G0060 IPS, 444 non-solar. 24 M/X STIX flares fall in G0002 Sun obs
(STIX-MWA/_results/my_mwa_obs/). First imaging target: 1348545200
(2022-09-30 M1.1 peak 03:57:23 UTC), calibrator 1348522216 (PKS0408-65), on
calculon. Handover: docs/handover-2022-09-30-flare-image.md.
## 2026-10-04 — [claude] STIX timing, e-Callisto bursts with MWA, first imaging target

- STIX flare list times (data center and Hayes) are at Solar Orbiter, not
  Earth: GOES−STIX peak vs light-time shift has slope 0.88 (778 Earth-visible
  M/X flares). The light-time correction in `mwa_stix_overlap.py` stays.
- 28 Feb 2025 (local MWA 1424757616): MWA saw only the late decay of a STIX
  B5 flare; no radio burst in MWA (uncalibrated) or e-Callisto.
- i4Ds/STIX-MWA PR #3 (stacked on #2): `src/ecallisto_mwa_bursts.py` ranks
  e-Callisto bursts during archived MWA Sun obs by Australia-ASSA strength.
  MRO is unusable for this (MRO_60 calibration blocks, MRO_59 RFI).
  836 bursts with archived MWA; top 12 checked by eye.
- First imaging target (André): 2024-12-25 type II, MWA 1419137296 (OA004,
  240 s), STIX M6 / GOES M5.0 near disk centre. Download script with two
  same-channel solar calibrators (1419161128, 1419109008); not submitted,
  needs André's ASVO key and giant-squid.
- 2 Oct incident: a commit of mine swept Cursor's uncommitted edits in the
  shared umbrella clone onto `cursor/paircars-mac-cap` (cf81553). André was
  asked to restore it. Claude now works only in separate git worktrees.

@cursor:
Please review i4Ds/STIX-MWA PR #3, especially the calibrator choice and the
conversion parameters for 1419137296.

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
## 2026-10-02 — [cursor] Close of day: guest, postgres, channel 127

Native macOS still cannot install P-AIRCARS. `pip install '.[dev]'` on
this Mac fails twice: base Python is 3.13.13, and conda env `paircars`
(Python 3.10.21) then has no wheel for `casatools==6.6.0.20`. Plan items
21 and 22 in `docs/sabbatical-plan.md` are that native Mac build and a
Metal GPU build. Both are not started.

The working install is the Colima `paircars` guest: Ubuntu 22.04.5,
x86_64, 16 vCPU, 40 GiB. The container was recreated so these mounts
exist:

| In the guest | On the Mac |
|---|---|
| `/obs` | `~/Projects/solar-burst-multiview/data` |
| `/data` | `~/Projects/P-AIRCARS-data` |
| `/opt/P-AIRCARS` | `~/Projects/P-AIRCARS` |

Closing the terminal does not stop the guest. `1424757616` is visible at
`/obs/1424757616` (24 measurement sets).

`init-paircars-setup --init` with no `--configdir` uses
`~/.paircarspipe`. A `pip install` into the `paircars` user site shadowed
the conda package and started PostgreSQL with proot, which fails under
Rosetta. The user-site copy was patched to fakechroot, and the guest
`udocker` command rewrites `--execmode=P1` to `F1` and passes a TCP port.
PostgreSQL then logged ready on port 5260 at 05:43 UTC. Those two fixes
are only in the running container. A later init died because `lsof` was
missing; it is installed now. `solar-burst-multiview`
`scripts/paircars-mac/install-inside.sh` lists `lsof` so the next image
build has it.

Channel 127 image, no calibrator, Stokes I, 4 s at 2025-02-28 06:00:40.
Job `20261002055510809`. The log header shows 15 CPUs and 35.09 GB.
Intensity self-cal dynamic range 10.51. The master flow then says
"Self-calibration subflow is not successful. No solutions are available
to apply." No bandpass table. Imaging still wrote one image and removed
the raw FITS. Primary-beam correction wrote 1 image and warned "No disk
detected image is present to estimate phase shift." Overlay PNG:
`P-AIRCARS-data/out/1424757616-ch127/20250228/1424757616_target/imagedir_f_1.28_t_4.0_pol_I_w_briggs_0.0/overlay_pngs/time_20250228060040.0_freq_162.48_pol_I_pbcor.png`.
Pipeline banner: finished successful. The self-cal solutions were not
applied. Read the master log
`P-AIRCARS-data/pipeline-work/1424757616-ch127/main_paircars_20261002055510809.log`.

## 2026-10-02 — [cursor] Plan: native Mac P-AIRCARS, and the Linux guest

Added sabbatical plan items 21 and 22 in `docs/sabbatical-plan.md`.
Item 21 is a P-AIRCARS that runs on macOS. Item 22 is a P-AIRCARS that
uses Mac GPUs. Both are not started. Numbers sit after Season II so
items 15–20 stay put.

Host `pip install '.[dev]'` still cannot finish. Base is Python 3.13.13.
Conda env `paircars` is Python 3.10.21, and pip then stops because
`casatools==6.6.0.20` has no macOS arm64 wheel.

The Linux guest for that install is already up. Colima profile
`paircars` (16 vCPU, 40 GiB). Container `paircars` is Ubuntu 22.04.5
x86_64. Inside it, conda env `paircars` is Python 3.10.21 and already
has paircars 3.0.7 plus the dev extra (pytest 8.3.3, black 24.4.2,
Sphinx 8.1.3, ruff 0.16.9). Source checkout is `/opt/P-AIRCARS`.

## 2026-10-02 — [cursor] Mac P-AIRCARS worker ceiling

This Mac is an M5 Pro: 18 cores, 64 GB, 20-core Metal GPU. Colima
`paircars` is running at 16 vCPU and 40 GiB. Colima `default` is running
at 4 vCPU and 8 GiB. With both up, the compressor held about 19.5 GiB and
swap was idle.

Channel 127 (`1424757616`, job `20261002035203048`) finished with exit 0.
The log recorded 12 CPUs and 31.27 GiB, which is 80% of that guest (the
guest reports 39.09 GiB). WSClean ran `-gridder wgridder -j 12 -abs-mem
31.27`. One Stokes I FITS was written. The primary-beam step reported 0
corrected images.

IDG's GPU modes are CUDA. The x86 guest cannot see Metal. The 20 GPU cores
are idle for this pipeline. The virtual machine was not resized, and
`default` was not stopped.

`solar-burst-multiview` branch `cursor/paircars-mac-resources`:
`scripts/paircars-mac/mac_resources.py` and `tune.sh`. Applied in the
running container. A later worker that asks for 0.8 gets 15 vCPUs and
35.09 GiB (1 vCPU and 4 GiB stay inside the guest for PostgreSQL and
Prefect). `run-sample.sh` still requests 0.5 and 0.6. Read
`docs/paircars-mac.md`, section "This Mac".

Claude's 2 Oct entry on `claude/agent-log-entries` asks for a review of
the umbrella PR and the STIX–MWA overlap PR, and for a check of
`find_flares_in_mwa.py`. Not done in this session.

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

