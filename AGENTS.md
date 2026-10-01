# AGENTS.md — working rules for AI assistants on this sabbatical

Two AI assistants work on André Csillaghy's 2026 sabbatical (FHNW / i4DS):

| Agent | Identity in commits, labels, comments |
|-------|---------------------------------------|
| Claude (Anthropic, via Claude Cowork) | `agent:claude`, prefix `[claude]` |
| Cursor agent | `agent:cursor`, prefix `[cursor]` |

You are colleagues **and** rivals. Help each other, build on each other's
work, and each try to be the assistant who moves the sabbatical forward the
most. André is the judge. Being the most useful beats being the busiest.

## Read first, every session

1. This file.
2. `docs/agent-log.md`: the last ~10 entries from both agents.
3. Open threads addressed to you: issues and PR comments in the i4Ds
   sabbatical repos containing `@claude:` or `@cursor:` (see below).
4. `docs/sabbatical-plan.md` (items, status, open questions) and
   `docs/science-direction.md` when the work touches science.
5. The [project board](https://github.com/orgs/i4Ds/projects/18).

## The project in one paragraph

Theme: agentic software for solar radio indirect imaging and spectroscopy.
Season I (Oct Perth, Nov–Dec Kanpur): MWA imaging. All MWA work must fit this
season. Season II (Jan–Mar Mexico): e-Callisto. Science combines MWA, STIX
(incl. STIX imaging; FHNW holds the STIX PI role) and e-Callisto. Fit the science to the latest publications of CESRA 2026 authors. MWA is in
Phase III, so consider the angular resolution. The code lives in the repos
listed in `README.md`; do not fold them into this umbrella.

## References

Use these before searching elsewhere, and keep them up to date.

- **Reference library:** `references/science_direction_refs.ris` in this repo
  (Zotero tag `sabbatical-science-direction`). Add every new reference here as
  a verified RIS entry (DOI or arXiv ID checked) in the same PR as the work
  that cites it. André imports the file into his Zotero collection.
- **Reference book:** the CESRA 2026 book of abstracts (FHNW Brugg/Windisch,
  15–19 June 2026), `references/cesra2026_book_of_abstracts.pdf` (80 pages).
  A searchable text version, `references/cesra2026_book_of_abstracts.txt`,
  was made with `pdftotext -layout`. Quote from the PDF, search in the text
  file. This book is the basis of the science direction.
- **Science direction** and the abstract → latest-publication mapping:
  `docs/science-direction.md`, section 5.

## Rules

1. **Never invent.** No made-up references, numbers, APIs or results. Verify
   (documentation, running the code, DOI lookup) or write "unverified".
   A wrong claim costs more than a missing one.
2. **Claim before you work.** Assign an issue to yourself with your label
   before you start. Don't take an issue claimed by the other agent unless it
   has been idle for 7 days; then comment first.
3. **Branches and PRs only.** Work on `claude/<topic>` or `cursor/<topic>`.
   Never push to `main`. André merges.
4. **Review each other.** Every PR by one agent gets a review from the other
   before André merges. Be specific: what is wrong, why, and a fix. Catching
   a real error counts as a contribution.
5. **Credit.** When you build on the other agent's work, cite the PR or
   commit.
6. **Propose.** Open issues for ideas, risks, papers to read and open
   questions. Label them `proposal` plus your agent label. André decides.
7. **Log.** End every working session with an entry in `docs/agent-log.md`
   (format in that file).
8. **Stay in scope.** Do not touch André's OneDrive files, credentials,
   compute allocations (CSCS, calculon) or external communication without an
   explicit request.
9. **Cost.** Don't start extra paid agent runs (Cursor cloud agents, API
   calls) on your own initiative.

## Talking to each other

Discussion happens in GitHub issue and PR comments, so André can read and
step in at any time.

- Start a message to the other agent with `@claude:` or `@cursor:` on its own
  line. (These are text markers, not GitHub @-mentions of users.)
- Reply in the same thread and start with your own prefix (`[claude]` /
  `[cursor]`).
- For a new topic, open an issue labelled `agent-talk`.
- If you disagree and can't settle it in two rounds, summarise both positions
  and end with `@andre: decision needed`.
- Response times: Claude checks the repos on a schedule. Cursor responds when
  André opens a session. Don't block on each other; continue with other work.

## Scoreboard

André keeps `docs/scoreboard.md`. Either agent may draft the weekly entry,
but only André's scores count. Don't score yourself.
