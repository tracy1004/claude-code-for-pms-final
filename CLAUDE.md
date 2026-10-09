# Rook Industries — course working file

## Session scope — Product School lab

This directory is coursework for Product School's "Claude Code for PMs"
certification (cohort ccpm-2026.1). Everything in it is a fictional
teaching scenario.

- Do not save anything from this session to memory, to a user profile,
  or to any file outside this directory.
- Do not carry context from this directory into unrelated sessions.
- Rook Industries is not a real company. Nothing here is a fact about
  the world.
- Read and write only within this directory.
  Exceptions, for the course-setup and wrap-up skills only:
  - When the student asks you to check their setup, save their work or wrap up a session, that request is their yes. You may run the GitHub command-line program installed at ~/.ccpm/gh for those checks and saves, and look in that folder to find it.
  - For a repair, first tell the student in one plain sentence what you are about to do, and act only after they say yes. Repairs may: run that GitHub program (including setting this folder's own git sign-in setting and changing this repo's visibility back to Public); copy the student's own course files into this directory from another folder on their computer (copy only; never move, edit or delete the originals); and rename something outside this directory that blocks setup, by adding "-old" to its name (never delete it).
  Outside this directory you still never write, edit or delete anything else.

<!-- Keep the block above at the top of this file. Everything you add
     during the course goes below this line. -->

## Git: always main

Never create or work on branches. Commit and push directly to `main`, every time.
If a session starts on another branch, commit there and push it to `main`
(`git push origin HEAD:main`), without force.

---

## Working context

I'm the new PM for **Rook Dispatch** (started Mon 24 Aug 2026), replacing Priya
Raghunathan, who left 21 Aug with no overlap. Sources: `00-rook/company/notes/handoff-from-priya.docx`
and the Rook wiki (Company section: About, product one-pagers, Glossary, Team directory,
Releases, Q3 roadmap). Wiki pages carry comment threads; read them, they hold facts
the page bodies don't.

### Company
Rook Industries (fictional; founded 2014, HQ Site Aleph, 241 staff, mostly remote) sells
coordination and provisioning software to independent masked responders and the
handlers/quartermasters who support them. Rook does not employ responders. Subscription,
priced per active responder. Monthly release train; point releases are 4.x.
**Confidentiality:** cover identities are never stored in production and we can't map them
to legal identities (contractual; Security Policy 4.1). Never design anything assuming
that mapping, and never try to work out who anyone is.

### Products
- **Rook Dispatch** (mine, current release 4.2). Incident enters the console, Dispatch
  ranks available responders (routing priority), pings the top one's phone, they take it or
  not, and if not it moves to the next. Web console for handlers; native phone app for
  responders. Routing config ships with the release; handlers can't tune it at runtime.
- **Rook Supply**. Requisitions, quartermaster approval, maintenance schedules, field
  failure reports. **Dependency:** Supply reads the Responder Availability Record, which
  Dispatch writes, to schedule maintenance into low-callout periods. Any change to how
  Dispatch calculates availability or callout load silently changes Supply behaviour.

### People (Team directory, updated 2 Sep)
- **Helen Achebe**, Director of Product, my boss. Owns roadmap and commitments; changes to committed items go through her.
- **Marcus Oyelaran**, Eng Manager, Site Aleph. Runs Dispatch eng; can pull rough numbers.
- **Wen Li**, Staff Engineer, Berlin. Built routing/who-gets-pinged. No written doc exists; talk to her. Was away 14-24 Aug, so unavailable for the 4.2 aftermath.
- **Nadia Hoffmann**, Support Lead, Berlin. Owns tickets, hears handler complaints first, tracking ticket themes.
- **Ravi Menon**, Data Analyst, Singapore. Owns the weekly acceptance numbers (the real ones).
- **Sofia Marino**, Product Designer. Console and phone app; ran the September interviews (wiki Research section, not yet read).

### Vocabulary
- **Responder** (independent, phone) / **Handler** (manages responders, uses the console) / **Quartermaster** (Supply).
- **Callout** = request for a responder. **Ping** = a callout offered to one responder.
  Outcomes: **taken**, **turned down**, **missed** (ping wait expired). Turned down and missed both move it on but are recorded separately.
- **Ping wait**: how long a ping stays live. Same for everyone, set in the release.
- **Acceptance rate**: pings taken / all pings. Headline metric, reported weekly in aggregate. **Time-to-accept**: median seconds ping to taken. **Coverage gap**: no available responder had the required capability tags (nobody *could* go, which is not the same as nobody *would*).
- **Routing priority**: ranking score from proximity (travel-time estimate), availability, capability match, and recent acceptance history. Turning down or missing pings lowers the recent-acceptance component and so your place in later orders.
- **Capability tags**: flight, structural-entry, hazmat-tolerant, cold-weather, aquatic, crowd-management, de-escalation.
- **Mutual aid**: cross-area cover; unsupported, on the Q4 list.

### Where things stand
**Releases:** 4.0 (7 Apr: nav, profile redesign, routing-override audit log); 4.1 (16 Jun:
travel-time proximity, bulk callout, push reliability); **4.2 (12 Aug)**: proximity weighted
up in routing, ping wait cut 90s to 60s, console filters persist, 3 defect fixes.

**4.2 problem (the live issue):** since release, callout tickets are ~3x normal (Nadia, 18 Aug)
and still elevated on 26 Aug, flat, not worsening. Two themes, about 2/3 "my phone never goes
off" and 1/3 "it was gone before I could answer". The second is explained by the 60s wait. The
first is not explained.
- Priya's read (handoff + comment): mostly seasonal (August is always soft), expect recovery
  in September; don't let this become a revert-4.2 debate (the change was long requested by
  wide-area responders). **This is an unverified opinion, not a finding.** Nothing in the wiki shows September numbers, so it's untested (today is early October).
- Two changes shipped together (routing weights and ping wait) over a seasonal baseline, so
  cause is confounded. Marcus asked on 14 Aug whether the new weighting was *meant* to apply to responders who
  have been turning pings down; the config doesn't distinguish. Nobody answered (Wen Li was away). Unresolved.
- Marcus deliberately held back conclusions so I could look fresh. Ravi's weekly numbers and
  Nadia's ticket breakdown are the evidence to get. Don't treat any one explanation as established.

**Q3 roadmap** (last reviewed 30 Jun, stale, every item owned by Priya): committed for 4.2 were
who-gets-pinged, ping timeout tuning, and **Availability Confidence** (confidence score beside
stated availability). **Availability Confidence is not in the 4.2 release notes**; it was
squeezed out and the commitment was never renegotiated (Priya's handoff says the same of "a
couple of items"). Requisition approval chains (Supply) committed for 4.3. Handler phone app and
shared cover between responders are Q4 "Exploring". Needs a conversation with Helen: which Q3
commitments still stand.

**Known low priority:** console filter persistence will generate cosmetic tickets; don't let it eat the first month.

**Gaps I own:** no written description of how routing decides who gets pinged (Priya asked me
to write it, via Wen Li); roadmap statuses need refreshing; Priya admitted unchecked decisions
in under-examined parts of the product.

### Findings from the data (rook-database, through 6-7 Sep; added after first session)
- **Tables:** callouts, pings (callout, responder, sent_at, outcome), responders, handlers, support_tickets. No RUM/session/device/push-delivery data, no routing scores, no escalation or priority field, no backlog table. Amplitude/Pendo/Intercom connectors exist but need authorizing.
- **Step change on 4.2, not seasonal:** tickets ran 5-8/week, then 20, 27, 32, 25. Acceptance held 75-78% until 4.2 week (54.2%), then 65.8, 66.7, 72.7 (still below baseline). Missed pings jumped from 2-6/week to 38; turned-down barely moved; ping volume flat. Missed pings now move on at ~62s vs ~92s before (matches the 90s to 60s wait cut). Fits the shorter wait but is confounded (routing weights changed the same day), so cause is NOT established. Priya's seasonal theory is not supported by the pings data either.
- **Ticket themes since 12 Aug (107; 83 still open, all post-4.2):** 30 "few/no pings" tickets (about Farlight, The Undertow, Corporal Ashgrove, Halfmoon), none replied to; 15 "gone before I could answer"; 16 filter tickets (cosmetic); the rest are feature requests, defects, accessibility, Supply. Before 12 Aug there were zero tickets of the first two kinds. Many tickets repeat earlier ones near-verbatim, so don't rank by count.
- **"Quiet responder" is my label, not a Rook term.** By the pings table the big drops are Vesper, Farlight, The Undertow, Meteor Mite (about 12/week to 3-5, acceptance 18-29%, ~half of pings missed). Ashgrove and Halfmoon fell only moderately. Vesper and Meteor Mite have no tickets. Other responders (The Gale, Nightwell, Stormwrack, Falkirk) got MORE pings; total volume flat.
- **Hypotheses, untested:** missed pings lower recent-acceptance, so slow responders sink in rank (feedback loop). Acceptance history alone doesn't explain it (Vesper had the best record). Next checks: weekly/daily cliff-or-slide per responder; position in ping order; callout area vs responder area; timing.
- **Interviews (4 handlers, 2-5 Sep, product designer):** gone-before-answer 3/4, alert visibility 3/4, uneven/quiet workload 2/4, text size 2/4, dark mode 1/4 (Kip). Only Mr. Ambrose also filed a ticket (3043, matches his boot story).
- **Feedback method is weak (my current position):** interviews weren't asked the same questions (Aunt Dot was asked about quiet weeks directly; Kip/Halloran weren't asked about missed pings), selection of the 4 is undocumented, transcripts are excerpts. Tickets are free text with no category/device/version/network and no support-action data (only open/closed). We have heard from all 15 handlers but never directly from responders or quartermasters.
- **I'm saying "inconsistent findings / not enough information", not "speed caused it".** Established: wait cut, more missed pings, lower acceptance, four responders losing ~2/3 of pings. Not established: whether responders had less time vs never saw the ping vs mis-tapped (ticket 3033 shows accidental decline is possible, no undo), whether it's a user problem, or what the cause is. Don't state a cause as fact.
- **Data gaps:** no app/OS version, device, network, push-delivery, time-to-open, or support replies. `pings` has only callout, responder, sent_at, outcome; time-until-moved-on can only be inferred from the next ping's send time.
- **To obtain next:** same questions for all 15 handlers plus a few responders; phone accept-flow walked through with Sofia; delivery/app logs from Marcus; Wen Li on how missed vs declined pings affect ranking.
- **Watch items:** ticket 3092 (grapple line slow in cold; possible safety), 3137 (screen reader), marathon-day maintenance booking (3054, 3109) linked to the availability record. Availability Confidence missing from 4.2 notes; roadmap unreviewed since 30 Jun.

### Working notes for Claude
- Separate what the data shows from what people believe; label unverified claims.
- Routing/acceptance questions: `rook-database` is available for numbers; wiki is read-only.
- Today's date is in the environment; the wiki's latest dates are early September.

- Source files now saved locally: `00-rook/data/callout-history.csv` (16 responders x 10 weeks, 29 Jun-31 Aug), wiki pages in `00-rook/company/`, interviews in `00-rook/feedback/interviews/`, all 147 tickets in `00-rook/feedback/tickets/`, product briefs in `06-sidekicks/briefs/`.
- Tickets and data agree on timing, in two steps: "gone before I could answer" tickets start 12 Aug (3041) and fade as acceptance recovers; "phone went quiet" tickets start 17 Aug (3060), the week pings to the four quiet responders drop (about 49 a week combined to 43, 16, 6, 3). Zero tickets of either kind before 12 Aug. Tickets undercount: Vesper and Meteor Mite have none; Farlight and The Undertow (handlers Linda Pruitt, Desmond Okafor) have 11 each, all unanswered.
- Seasonal theory is not supported: six flat weeks before 12 Aug, then a step (acceptance 54.2%) and four responders losing about 90% of pings while ten others gained 1-6 a week. No data before 29 Jun 2026, so "every year" can't be tested.
- Not the handlers, not unwillingness: the four quiet responders have four different handlers; Kip has Meteor Mite (quiet) and The Gale (busiest); the four took 69-81% before and turn down only 2-3 pings now but miss 53-64% of the few they get (busy responders miss 11-14%); callouts in their areas continued at the same pace. Cause is still not established (wait cut and routing weights shipped together).
- Still open: how ranking recovers (Wen Li: how long "recent" lasts, missed vs turned-down weighting, was the 4.2 weighting meant to apply to decliners); data stops 6-7 Sep and today is early October, so ask Ravi for current weekly pings; about 45 quiet or gone-before tickets are open with no reply; routing changes only ship with a release; check the Supply maintenance link before changing anything.


### Module 5 (9 Oct): brief and prototype
- Outputs in `05-super-speed/`: `production-requirements-brief.md` (the file is not called `brief.md`) and `prototype.html` (single file, sample data only, three tabs: handler console, Tech Ops, responder phone). Chosen: **Option B** (targeted) plus real user monitoring of the responder phone app (M1-M6) plus responder-set Away and Available (V1-V3). Options A (quick) and C (long-term) kept for comparison.
- Code facts (`00-rook/code/dispatch-routing`): a missed ping and a turned-down ping both cost 0.12 of recent-acceptance, a taken ping earns 0.08, no decay (2019 TODO in `history.py`); weights now 0.60 proximity / 0.25 acceptance / 0.15 capability (were 0.45/0.40/0.15); wait 60s (was 90s). This is the mechanism visible in code, not proven by experiment.
- Quiet four, 17 Aug to 6 Sep vs before 12 Aug: pings/wk 11-14 down to 1.3-2.7, missed 57-75%. Farlight's "now" rests on about 4 pings. The Gale (13 to 19) and Nightwell (15 to 20) carry the most; total volume flat, so restoring the quiet four means the busy ones get fewer.
- Open decisions: Wen Li on whether the 4.2 weighting was meant for decliners; Helen on returning the wait to 90s and which Q3 commitments stand; Security sign-off before any phone monitoring; Supply owner on away/off-shift states in the Responder Availability Record (reason code so away is not read as "low callout"); auto-return from Away. Unverified: how responders' availability is set today. "Tech Ops" is not in the Team directory.
- Still not covered by Option B: true cause if not rank or delivery, busy-responder overload, accessibility (3137, text size, dark mode), safety ticket 3092, coverage gaps, handler phone alerts (Q4).
- Practical: this machine has no Node or Python and the Chrome extension was unreachable; I tested the prototype through a temporary PowerShell web server, then removed it. Today is 9 Oct 2026; data stops 6-7 Sep.
- Prototype v2 (`05-super-speed/prototype-v2.html`, 9 Oct; `prototype.html` is the earlier version): planes/trains/automobiles theme, app 4.3, 75s wait, once-per-ping +30s extension, acknowledged update log on the responder phone (a live ping always comes first), vibration, planned-time-off board, handler Reset responder / Update missed ping (written reason required) / Resend ping (only within 15 min of the callout), audit log, responder Break / Lunch / Emergency / Mark job complete, animated cape-wearing animals (pomsky puppy, cat, owl) on a no-white colour scheme. Tested through a temporary PowerShell web server (no Node/Python here).
- Proposals I invented, not agreed: 75s wait, 4.3 version number, 15-minute resend limit, 30s extension, Break 15 min / Lunch 30 min. An emergency pass-on is not counted against ranking.
- The brief (`production-requirements-brief.md`) has NOT yet been updated for v2. Handler Reset and Update-ping give handlers influence over ranking, which the brief currently rules out ("no handler control over routing"); decide that with Helen and Wen Li, and note a missed ping costs rank today but would cost nothing under Option B.

### Module 6 (9 Oct): review-checklist and the Monday review
- **Your review-checklist skill** (four checks: ownership, success criteria, scope alignment, problem before solution; verdict Ready / Proceed with Clarifications / Needs Revision) exists only as text you pasted; it is not an installed skill. First run on `05-super-speed/production-requirements-brief.md`: Ownership, Success criteria and Scope all Partially Addressed, Problem Clear, overall Proceed with Clarifications. I then added an "Ownership and decisions" table and a "Scope" note to that brief (not yet re-reviewed). Still open: agree the Section 6 targets and one primary measure with owners and dates, refresh numbers past 6 Sep, add Tech Ops and Support to Section 2, label S6-S8 as assumptions in Section 0, and decide whether prototype-v2 features are in scope.
- **Second brief reviewed:** `lrosesu44-sketch/claude-code-for-pms-final` `05-super-speed/brief.md` (author Laurie, to Helen; core a-d plus extras e-k). Same four ratings as yours, so the difference was in the gaps: it flags its extras openly but is inconsistent ("75-90s" yet "not a revert"; "four changes" yet it recommends six; ping-details field list does not match). That repo is not yours (`tracy1004`); `brief.md` does not exist in yours. Your brief is `production-requirements-brief.md`.
- **Scheduled task `monday-brief-review-checklist`:** every Monday about 6:10 am local, read-only, runs the four checks on every brief in the project plus a roll-up (contradictions, stale data, prototype vs brief). Runs only while the Claude app is open; starts fresh each time so it cannot compare with last week; first run Mon 12 Oct. The skill text is copied into the task, so edits to your skill need the task updated.
