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
- **Step change on 4.2, not seasonal:** tickets ran 5-8/week, then 20, 27, 32, 25. Acceptance held 75-78% until 4.2 week (54.2%), then 65.8, 66.7, 72.7 (still below baseline). Missed pings jumped from 2-6/week to 38; turned-down barely moved; ping volume flat. Points at the 60s ping wait. Priya's seasonal theory is not supported.
- **Ticket themes since 12 Aug (107; 83 still open, all post-4.2):** ~30 "few/no pings" tickets concentrated in four responders (Farlight, The Undertow, Corporal Ashgrove, Halfmoon), unanswered; ~15 "gone before I could answer"; 14 filter tickets (cosmetic); rest are feature requests, defects, accessibility, Supply.
- **Open hypothesis, untested:** the four quiet responders are being down-ranked via the recent-acceptance score. Next step: per-responder cut of `pings` for them (history of turned-down/missed, week by week).
- **Watch items:** ticket 3092 (grapple line slow in cold; possible safety), 3137 (screen reader), marathon-day maintenance booking (3054, 3109) linked to the availability record. Availability Confidence missing from 4.2 notes; roadmap unreviewed since 30 Jun.

### Working notes for Claude
- Separate what the data shows from what people believe; label unverified claims.
- Routing/acceptance questions: `rook-database` is available for numbers; wiki is read-only.
- Today's date is in the environment; the wiki's latest dates are early September.

