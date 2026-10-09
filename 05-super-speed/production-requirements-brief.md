# Production requirements brief: quiet responders after 4.2

Rook Dispatch · Draft for Helen Achebe · 9 Oct 2026 · Owner: Dispatch PM
Status: **Option B selected** (9 Oct), now including real user monitoring (RUM) of the responder phone app for handlers and Tech Ops. Options A and C are kept for comparison. Nothing here is committed or scheduled; Wen Li, Marcus, Security and Helen still need to agree.

## 0. The problem in one paragraph

Since 4.2 (12 Aug), four responders (Vesper, Farlight, The Undertow, Meteor Mite) receive about a fifth of the pings they used to, and they miss over half of the few they get. Ten others get more. Total ping volume is flat. Separately, every responder now has 60s, not 90s, to answer. Handlers see the result on a card but cannot tell what their responder was actually shown, or why.

**What the data and code show.**
- Missed pings jumped from 2-6 a week to 38, and acceptance fell from 75-78% to 54% in 4.2 week, then partly recovered to 72.7%.
- In code, a missed ping and a turned-down ping both cost the responder 0.12 of recent-acceptance score, while a taken ping earns only 0.08. The score never recovers on its own (a 2019 TODO in `history.py` asks whether it should).
- Proximity weight went from 0.45 to 0.60 and recent-acceptance weight from 0.40 to 0.25 in the same release.

**What is not established.**
- Whether the cause is the shorter wait, the new weights, or both.
- Whether responders had less time, never saw the ping, or mis-tapped.
- Whether the 4.2 weighting was meant to apply to decliners. Wen Li has not answered Marcus's question of 14 Aug.

The three options below are built so each works whichever of those turns out to be true. Where one depends on the answer, I say so.

## 1. Current state and future state: every responder and handler

**How to read these tables.**
- *Before* is 29 Jun to 9 Aug (6 weeks). *Now* is 17 Aug to 6 Sep (3 weeks; the data stops there, so refresh with Ravi, because today is early October).
- Pings/wk is pings offered to that responder. Taken % and Missed % are shares of those pings.
- **Future state** is what I propose for **Option B (recommended)**. Targets are proposals for you and Wen Li to adjust, not agreed figures.
- The group names ("went quiet", "squeezed", "carrying more") are my labels, not Rook terms.

### 1a. Responders (16)

| Responder | Handler | Area | Group | Pings/wk before → now | Taken % before → now | Missed % before → now | Current state, from the responder's side | Future state (Option B) |
|---|---|---|---|---|---|---|---|---|
| Vesper | Aunt Dot | Old Town | Went quiet | 13.8 → 2.7 | 82 → 13 | 4 → 63 | Best record before 4.2, now mostly silence. No tickets; Aunt Dot describes both "gone before he's down the stairs" and weeks where "the phone just sits". | Rank recovers; back in the first-offered group for nearby callouts; can undo a decline; sees "no pings for N days" in-app. Target 8+ pings/wk, missed 25% or lower. |
| Farlight | Linda Pruitt | Uptown | Went quiet | 12.0 → 1.3 | 74 → 0 | 4 → 75 | About 4 pings in 3 weeks, none taken (small numbers, read with care). 16 tickets since 4.2, 14 still open, none answered. | As Vesper. A support reply goes out now, before any release. |
| The Undertow | Desmond Okafor | Harborside | Went quiet | 12.0 → 2.0 | 78 → 17 | 1 → 67 | 17 tickets since 4.2, 15 open, none answered. | As Vesper. A support reply goes out now. |
| Meteor Mite | Kip | Eastgate | Went quiet | 11.2 → 2.3 | 69 → 14 | 3 → 57 | Texts Kip all week asking whether something is broken. Kip: "Mite thinks Mite's been forgotten." No tickets. | As Vesper, plus Kip sees why on the console. |
| Corporal Ashgrove | Yusuf Demir | Northfield | Squeezed | 10.0 → 7.3 | 78 → 68 | 2 → 14 | Fewer pings and more misses, but still working. 10 tickets since 4.2, 9 open. | Misses stop costing rank; undo and repeat alert cut the misses. No volume change expected. |
| Halfmoon | Simone Fischer | Westbury | Squeezed | 11.0 → 8.3 | 73 → 68 | 0 → 12 | Same pattern, 12 tickets since 4.2, 9 open. | As Ashgrove. |
| The Drift | Beatrice Calloway | Greenway | Carrying more | 6.2 → 7.7 | 84 → 70 | 3 → 13 | Slightly more pings, slightly more misses. | Misses stop costing rank; volume about the same. |
| The Longcast | Graham Petrov | Lakeshore | Carrying more | 7.2 → 8.7 | 74 → 69 | 2 → 15 | As above. | As above. |
| Sgt. Bulwark | Halloran | Foundry Row | Carrying more | 9.0 → 10.7 | 76 → 72 | 0 → 16 | Steady. Halloran mentions a callout that went to someone else "before he'd even got his boots on". | Misses stop costing rank; undo and repeat alert help. |
| Ironvale | Teresa Alvarez | Riverside | Carrying more | 8.0 → 11.0 | 77 → 70 | 2 → 18 | More pings, more misses. 8 tickets since 4.2, 7 open. | As above. |
| Cindermark | Farid Haddad | Mill District | Carrying more | 9.0 → 11.7 | 80 → 71 | 2 → 11 | More pings. | As above. |
| Sgt. Falkirk | Owen Bramwell | Kingsbridge | Carrying more | 10.0 → 14.7 | 80 → 70 | 3 → 14 | About 5 more pings a week than before. | Volume may ease back as quiet responders return. |
| Captain Vantage | Mr. Ambrose | Hillcrest | Carrying more | 12.0 → 16.7 | 74 → 74 | 3 → 10 | Mr. Ambrose reports a callout lost "halfway into the suit, coat on, one boot on". | Volume may ease back; undo and repeat alert help. |
| Stormwrack | Renata Kovač | Southport | Carrying more | 13.0 → 17.7 | 79 → 75 | 0 → 11 | More pings. | Volume may ease back. |
| The Gale | Kip | Eastgate | Carrying more | 13.0 → 19.3 | 78 → 72 | 4 → 10 | Busiest in the data. Kip: "Gale's exhausted." Same city and same handler as Meteor Mite. | Volume eases back as Meteor Mite and the others return. |
| Nightwell | Marjorie Sung | Midtown | Carrying more | 15.0 → 19.7 | 76 → 71 | 3 → 10 | Busiest after The Gale. | Volume eases back. |

**Trade-off to state plainly.** Total ping volume is flat, so every ping the quiet four get back is one the "carrying more" group does not get. The five who gained 4-6 a week (Falkirk, Vantage, Stormwrack, The Gale, Nightwell) will notice fewer pings. For them this is a return toward where they were, but I should not assume they'll welcome it.

### 1b. Handlers (15) and what they can tell about their responder

Tickets are all topics since 12 Aug (107 in total, 83 open), not only ping problems. The three interviewed handlers without tickets are marked.

| Handler | Responder(s) | Tickets since 4.2 (open) | Current state: what they see and can't tell | Future state (Option B) |
|---|---|---|---|---|
| Aunt Dot | Vesper | 0 (interviewed 3 Sep) | Hears the phone buzz through the ceiling; the console does not tell her a ping came in. Cannot tell why Vesper has quiet weeks. | Console alert when a ping goes out for Vesper and when it is missed. Vesper's card explains the quiet spell. Larger text on the card, as she asked. |
| Linda Pruitt | Farlight | 16 (14) | Responder has gone almost silent; her tickets have had no reply. | Support reply now. Responder's-eye view shows pings offered, outcome, and a plain "no pings for N days" line. |
| Desmond Okafor | The Undertow | 17 (15) | As above. | As above. |
| Kip | Meteor Mite and The Gale | 0 (interviewed 4 Sep) | Two cards on one screen "might as well be two different products". Cannot tell either responder why. | Each card shows its own ping history against its own last 8 weeks. A distinct alert sound per responder (his own request). |
| Yusuf Demir | Corporal Ashgrove | 10 (9) | Falling pings, rising misses; cannot see why. | Responder's-eye view; reply to open tickets. |
| Simone Fischer | Halfmoon | 12 (9) | As above. | As above. |
| Beatrice Calloway | The Drift | 7 (4) | Little change visible. | Responder's-eye view available; no action needed. |
| Graham Petrov | The Longcast | 7 (4) | As above. | As above. |
| Halloran | Sgt. Bulwark | 0 (interviewed 5 Sep) | Opens the console about twice a shift; only heard of the lost callout from Bulwark. His main issues are Supply (requisition queue). | Alert on a missed ping for Bulwark; the profile shows the timeline. His Supply asks are out of scope here. |
| Teresa Alvarez | Ironvale | 8 (7) | More misses; cannot see why. | Responder's-eye view. |
| Farid Haddad | Cindermark | 6 (4) | Little change visible. | As The Drift. |
| Owen Bramwell | Sgt. Falkirk | 8 (6) | More pings; heavier load. | Load shown in the card; volume may ease. |
| Mr. Ambrose | Captain Vantage | 1 (1) (interviewed 2 Sep) | Learned of the lost callout "from him, not from the console". Wants whatever says a callout is live to be "harder to miss". Would also prefer a warning when a filter resets. | Live-ping alert; larger status badge as he asked. Filter-reset warning is out of scope for this brief and is logged as a separate ask. |
| Renata Kovač | Stormwrack | 7 (5) | Heavier load. | Load shown on the card. |
| Marjorie Sung | Nightwell | 8 (5) | Heavy load. | Load shown on the card. |

**Handler gap this brief addresses directly.** Several handlers cannot tell what their responder is seeing: Ambrose and Aunt Dot hear about a lost callout after the fact, Kip cannot explain a dead card next to a busy one, and Pruitt and Okafor have tickets nobody has answered. Section 3 adds requirements for this: the handler view (H1 to H4) and phone monitoring for handlers and Tech Ops (M1 to M6). Evidence is from four interviews and the ticket table; I have not yet confirmed with Sofia Marino exactly what the console shows today, so treat "can't tell" as the handlers' report, not a verified gap in the product.

## 2. Who this is for, and the scenarios

**Primary user: the impacted responder.** An independent masked responder using the Rook phone app. Rook does not employ them. They are identified only by cover identity and we hold no mapping to a legal identity. Nothing below assumes one.

**Anchor persona: Vesper** (handler Aunt Dot). Best record before 4.2, no tickets (so the ticket count undercounts them), and Aunt Dot's interview describes both halves of the problem.

**Secondary users.**
- **Handlers.** They see the result and cannot explain it (Section 1b).
- **Busy responders.** Any fix must not push their workload up.
- **Quartermasters (Supply).** They read the Responder Availability Record to schedule maintenance into low-callout periods. Changing who gets pinged changes callout load, so it changes Supply's schedule.

**Scenarios, from the user's side.**

| # | Scenario | Who feels it | Evidence |
|---|---|---|---|
| S1 | **Silence.** My phone never goes off, and nothing tells me why. | Quiet four | 30 tickets; Kip, Aunt Dot |
| S2 | **Too late.** The phone buzzes, I reach for it, and the ping has already moved on. It now counts against me. | Everyone, quiet four most | 15 tickets; Aunt Dot, Ambrose, Halloran |
| S3 | **Wrong tap.** I declined by accident and can't undo it. | Any responder | Ticket 3033 |
| S4 | **Stuck.** A few bad pings dropped my rank and there is no way back. | Quiet four | `history.py`: no decay |
| S5 | **Handler can't tell.** My handler sees an empty card, or hears of a lost callout after the fact, and can't explain or act. | All handlers | Kip, Aunt Dot, Ambrose |
| S6 | **Handler can't see what I was shown.** There's no way to check whether my phone received, showed or timed out a ping. | All handlers, plus Support | Same; no delivery data exists |
| S7 | **Planned time away looks like a problem.** I'm taking five days off. There's no way to tell my handler, so my silence reads as "quiet", and if pings still go out they count as missed and cost me rank. | Any responder, and their handler | Not in the data; follows from "recent acceptance" counting every miss (`history.py`). I have not verified how availability is set today. |
| S8 | **Stuck unavailable.** I'm back, or I forgot to switch my status, and I can't put myself back to available, so I get nothing and don't know why. | Any responder | Plausible contributor to "my phone never goes off". Not tested: we don't know how many of the 30 tickets had this cause. |

## 3. Options

### Option A: Quick fix (time-saving). "Stop punishing the miss."

**What ships.** One release (a 4.2.x or an early 4.3 inclusion). Engineering says the change itself is small.
1. A missed ping no longer lowers recent-acceptance. It is still recorded as missed, but costs 0 instead of 0.12. Turned down keeps its penalty.
2. Ping wait goes back to 90s. The release notes name the weighting, not the wait, as what responders asked for, so this does not undo the requested change. Decision for Helen and Wen Li.
3. Weights stay at 0.60 / 0.25 / 0.15.
4. **Outside the product:** Nadia replies to the roughly 45 open "quiet" or "gone before" tickets with a plain message while the fix is built.

**What changes for the responder.**
- S2: a late ping no longer costs rank; at 90s it's also less likely.
- S1/S4: no new damage accrues. The four already sunk do not climb back quickly, because there is still no recovery. Expect slow recovery at best.
- S3, S5, S6: unchanged.

**What changes for the handler.** Nothing in the console. Support replies are the only improvement.

**What it deliberately doesn't do.**
- Does not rescue the four already sunk.
- Adds no responder-facing or handler-facing feature.
- Does not touch the 0.60 / 0.25 weights.
- Does not tell us whether the weights or the wait caused the drop. Doing both at once confounds it again; ship the penalty change first and the wait second if we want a clean read.

**Risks.** Cheap and reversible. It restores load to the quiet four with no matching change on the busy ones, so Supply's low-callout picture shifts; check with the Supply owner first.

---

### Option B: Targeted approach, with real user monitoring. "Let quiet responders back in, let them answer, and let handlers and Tech Ops see what the phone actually did." **Selected for build (9 Oct).**

**What ships.** One release, built on Option A, plus recovery logic, a small phone-app change, a handler view, and real user monitoring (RUM) of the responder phone app. RUM means measuring what real responders' phones do with real pings, as it happens.

*Ranking*
- **R1.** Everything in A (Wen Li confirms the exact missed-versus-declined weighting).
- **R2. Recovery:** recent-acceptance eases back toward neutral over time (proposed start: half-way over 14 days). This closes the 2019 TODO in favour of "a bad month shouldn't follow you into spring".
- **R3. Floor for the unpinged:** a responder with no pings for N days (proposed 7) is ranked at neutral for the next suitable callout in their area.

*Responder phone app*
- **P1. Undo a decline** for 10 seconds after tapping (S3).
- **P2. Ping stays reachable after a miss,** with one-tap access if the callout is still open (S2). The wait stays at the product value.
- **P3. Repeat alert** on pings the app has not seen opened (S2). Now buildable, because M1 supplies the "not opened" signal.
- **P4. Plain-language status line:** "You haven't been offered a callout for N days. Your handler can see this." (S1)
- **P5. "What we measure" screen,** in plain language, listing exactly what the app reports about the phone, with a statement that it contains no location trail and no real-world identity.

*Handler console: the responder's-eye view* (S5 and S6)
- **H1. Ping timeline** on each responder's card: every ping in the last 7 and 30 days with time offered, outcome (taken, turned down, missed) and seconds from sent to outcome. All of this exists in the `pings` table today.
- **H2. "Quiet this week" indicator,** neutral wording, comparing the responder with their own last 8 weeks, with a one-line plain explanation.
- **H3. Live alert** when a ping goes out for your responder and when it is missed, named by responder, with a distinct sound per responder.
- **H4. One-tap "Report a problem with this ping"** creates a ticket with the ping and its phone trail (M2) attached, instead of free text.

*Real user monitoring of handheld devices* (new; closes S6)
- **M1. Collection from the responder app.** For each ping, the app reports timestamps for: push received on the device, notification shown, app opened, screen rendered, and the responder's tap (take or decline). Per device it reports app version, OS version, device model, network type (wifi, mobile, none), notification permission on or off, battery-saver on or off, and crashes or errors. It also records each status change (Available, Off shift, Away) with its time. Nothing else.
- **M2. Per-ping phone trail, visible to the handler** on the timeline: "Sent 22:41:03 · Reached phone 22:41:04 · Shown 22:41:04 · Opened 22:41:19 · Declined 22:41:21". It replaces the earlier H5 and answers "what was my responder shown, and when?" in terms of events, not screens.
- **M3. Phone-health line on the responder card** in plain words, for example "Notifications are switched off on this phone" or "App is two versions behind". The handler can pass it to their responder. It's a fact about the phone, not a score about the person.
- **M4. Tech Ops monitoring view,** across all responders, filterable by app version, OS, device model and network. It shows: pings that reached the phone (delivery rate); median and 90th-percentile time from sent to shown and from shown to opened; crash and error counts; and which responders have notifications off. It uses neutral cover-identity names only.
- **M5. Incident reporting from either role.** A handler or a Tech Ops user can raise an incident from a ping, a responder card or a Tech Ops chart. The incident carries a category (ping never arrived, arrived late, shown but too late to answer, wrong tap, app error, other), the affected pings and their phone trails (M2), the device facts (M1), and who raised it. Handler-raised incidents land in Support; Tech Ops can see all of them and take over the technical ones.
- **M6. Threshold alerts to Tech Ops,** for example when the delivery rate for an app version or network type falls below an agreed level, or time-to-shown goes above an agreed level. The thresholds are set after two weeks of baseline data; they are not guessed now.

*Responder-controlled availability* (new; S7 and S8)
- **V1. "Plan time away," set by the responder on the phone.** The responder picks a number of days (for example 5, up to a maximum to be agreed) starting now or on a chosen date. No reason is asked for. The handler is told immediately: a console alert (H3 style) and a visible "Away until Wed 14 Oct" badge on the card.
  - While away: no pings are sent, so nothing is recorded as missed or turned down. The recent-acceptance score is **held** (neither penalised nor decayed by R2). The "quiet this week" indicator (H2) switches off and says "Away, not quiet". Support and Tech Ops see the state so tickets and incidents are not raised for a planned absence.
  - On the return date the responder gets a reminder the day before and is set back to Available by their own confirmation (see open question 8 on auto-return). On return the score is restored to where it was before they left.
  - The responder can end it early, with V2.
- **V2. "Set me available," one tap by the responder.** Works from Away, Off shift or any state where they are not receiving pings. The change reaches the Responder Availability Record and the handler's card straight away, with a console alert ("Meteor Mite is available"). It also covers S8: a responder who finds they have been unavailable can fix it themselves.
- **V3. Status history for the handler.** The card lists the last status changes, who made them (the responder) and when, so a handler can explain a quiet period ("set Away on Mon, back Fri") without asking Support.
- **Record and Supply.** The new states are written to the Responder Availability Record with a reason code (Away, Off shift, Available), so Supply can tell a planned absence from a genuinely low-callout period before it schedules maintenance.

**What changes for the responder.** S1: the quiet spell has an end, and the app says so. S2: a miss is not punished, the ping stays reachable for a moment, and an unseen ping is repeated. S3: undo exists. S4: ranking recovers by design. New: they can see exactly what their phone reports about itself (P5). S7: they can tell their handler they are away, with nothing counted against them. S8: they can put themselves back to available.

**What changes for the handler.** S5: they see what their responder was offered, when, and what happened, and are told when a ping goes out or is missed. S6: they see the phone trail on each ping, a plain-language phone-health line, and can raise an incident with that evidence attached. S7/S8: they are told when a responder goes away or comes back, the card says "Away, not quiet", and the status history explains the gap.

**What changes for Tech Ops.** For the first time they can answer "did the push arrive, and how late?" across all responders, by version, OS, device and network, and can receive and act on incidents with the evidence already attached.

**What it deliberately doesn't do.**
- No new weight on the three ranking inputs and no machine-learned scoring.
- No handler control over routing (config still ships with the release).
- **No screen recording, no mirror of the responder's phone screen, no keystroke capture.** RUM reports events and timings, not what was on screen.
- **No location trail.** Proximity already uses a travel-time estimate; RUM adds no tracking of where the responder is or has been.
- **No linking of device data to a legal identity,** and no device identifiers that could be matched to one. Data is keyed to the cover identity the app already holds.
- RUM is not a performance score for responders. Handlers see the phone trail and phone health, not a ranking of responders by device quality.
- No handler phone app (still Q4 Exploring); H3 alerts go to the console.
- Away is not a leave-approval system: no handler approval, no reason, no calendar sync. The responder decides, the handler is informed. A handler cannot set a responder Away or Available on their behalf in this release.
- Away does not arrange cover. Whether the handler or another responder covers the days is outside this release (mutual aid is still Q4 Exploring).
- No mutual aid or cross-area cover, no Availability Confidence score, no per-responder ping wait.
- Does not decide whether proximity should be 0.60.

**Dependencies and risks.**
- Wen Li (routing owner) must confirm intent before build.
- Supply dependency, larger than in A, because B also changes who is offered what.
- Phone app work: Sofia Marino to walk the accept flow; the app team to add and ship the M1 collection.
- **RUM tooling choice.** Amplitude, Pendo and Intercom connectors exist but need authorizing, and none is confirmed to cover push delivery on a phone. Marcus to say whether to use one of them or build the collection in-house.
- **Privacy and Security review before build,** because M1 is new collection from responders' phones and the cover-identity rule (Security Policy 4.1) is a contractual constraint. Needs Security sign-off on what M1 collects, how long it is kept, and who can see it.
- **No history.** M1 starts collecting on release, so there is no "before" for 4.2 to compare against. The first two weeks after release are a baseline, not a verdict.
- R2 and R3 need a simulation on the 10-week history so busy responders' load does not swing the other way.
- **Supply and the availability record:** V1/V2 change what Dispatch writes to the Responder Availability Record. Supply reads it to put maintenance into low-callout periods, so an away period could look like a quiet one and get maintenance booked. The reason code is meant to prevent that, but the Supply owner must agree before build.
- **Coverage:** Away shrinks the available pool. Coverage gaps are counted separately and should not be inflated or hidden by it; to be checked with Ravi.
- **How availability is set today is unverified.** I have not confirmed with Wen Li or Sofia who can change it and from where. V1/V2 assume the responder cannot do it themselves; if they can, the scope shrinks.
- Console changes need Sofia's design; H1 to H4 and M2 to M5 can be shown to Helen as a click-through.

---

### Option C: Long-term, sustainable. "Make fairness and availability something the system knows, not an accident of a score."

**What ships.** A staged programme across two to three releases, run with Helen as a roadmap item, not a release fix.
1. Everything in B, including RUM (M1 to M6).
2. **Separate "willing" from "able".** Recent acceptance splits into a *response* signal (did they see it and act in time?) and a *preference* signal (did they decline?).
3. **Availability Confidence,** the 4.2-committed item that was squeezed out: a confidence score beside stated availability.
4. **Per-responder ping wait,** chosen by the responder within limits. This changes the glossary rule that ping wait is the same for everyone.
5. **Fairness guard-rail:** a monitored ceiling on how concentrated pings can get, with an alert to the PM if any responder's share drops sharply week-on-week.
6. **Handler explanation, "why was this responder asked first / not asked":** a plain-language reason per callout, with the audit log extended to cover it.
7. **Written routing documentation** (the gap Priya left).
8. **Extend RUM** (already in B) to the callout acceptance flow end to end and to the handler console, so the next 4.2 is diagnosable in days.
9. **Groundwork for mutual aid** (Q4 Exploring).

**What changes for the responder.** All of B, and in addition: S1 the system can say *why*; S2 the responder sets their own wait within limits; S4 the system distinguishes "did not see it" from "does not want it".

**What changes for the handler.** All of B, plus S5 and S6 closed: they see the reason a responder was or was not asked, and a fairness view across their responders.

**What it deliberately doesn't do.**
- No handler-tunable routing at runtime (a separate decision).
- No attempt to link or infer a legal identity from any of the new data.
- No Supply changes, though Supply is told in advance of the load effects.
- Does not ship in the first month.

**Risks.** Large. It touches the ranking inputs Supply reads, the glossary, and the app. It depends on data we do not yet collect and on the Q3 commitments conversation with Helen.

## 4. Comparison

| | A: quick | B: targeted | C: sustainable |
|---|---|---|---|
| S1 silence | stops getting worse | ends, and is explained | explained and monitored |
| S2 too late | less likely | less likely and softened | responder sets their wait |
| S3 wrong tap | no | undo | undo |
| S4 stuck | no | recovery rule | separate signals |
| S5 handler can't tell | support replies only | timeline, alert, indicator | plus reasons and fairness view |
| S6 what responder was shown | no | per-ping phone trail, phone-health line, incident reporting (RUM) | same, plus the end-to-end acceptance flow |
| S7 planned time away | no | responder sets Away; held rank; handler told | same, plus cover suggestions (future) |
| S8 stuck unavailable | no | responder sets Available | same |
| Tech Ops can see delivery and device problems | no | yes (M4 to M6) | yes, extended |
| Size | days | one release plus app, console and monitoring work | 2-3 releases |
| Reversible | yes | mostly | partly |
| Supply impact | yes (small) | yes | yes, planned |

## 5. Recommendation

You asked for one fix that addresses the whole end-user experience, and you have chosen **Option B**, with RUM added. I agree with that choice.
- It is the smallest option that covers S1 to S8 and gives handlers and Tech Ops a real answer to "what did the phone do with this ping".
- It contains A as its first step, so if Wen Li or Marcus want the penalty change out early, nothing is wasted.
- C depends on data we do not have and decisions Helen has not made.
- A alone leaves the four already-quiet responders where they are and the handlers in the dark.

The main risk of B is the RUM part: it is new data collection from responders' phones, so it needs Security review and an agreed tooling choice first. If that review runs long, A can ship first and the rest of B (with RUM) follow.

### Does Option B fix every negative experience? No.

Left unsolved or only partly solved, with where each is picked up:

| Negative experience | Covered by B? | Where it goes |
|---|---|---|
| Quiet because of rank, late pings, accidental declines, handler in the dark, silent planned absence, stuck status | Yes (S1 to S8) | This brief |
| The real cause is something other than rank (for example the new weights themselves, or a delivery problem we can't see yet) | Partly. B treats the symptoms and adds the data (M1) to find the cause | Review after the 2-week baseline; Wen Li's answer on weighting intent |
| The 60s wait itself is too short for some responders | Only if Helen approves going back to 90s (Option A step); per-responder wait is Option C | Open question 6 |
| Busy responders being worn out (Kip: "Gale's exhausted") | Only indirectly, as quiet responders return and share the load; there's no rest or load cap | New item for the roadmap |
| Accessibility: text size (2 of 4 interviews), screen reader (ticket 3137), dark mode (Kip) | No | Separate Sofia Marino backlog; not in this brief |
| Safety report: grapple line slow in cold (ticket 3092) | No | Needs triage now, outside this release |
| Maintenance booked on marathon days (tickets 3054, 3109) | Partly: V1's reason code helps, but the cause is in Supply | Supply owner |
| No one is available with the right capability (coverage gap) | No | Existing coverage-gap reporting; mutual aid is Q4 Exploring |
| Handlers cannot be alerted away from the desk (Aunt Dot, Ambrose) | Only on the console | Handler phone app, Q4 Exploring |
| We have not heard from responders or quartermasters directly | No | Research plan: same questions for all handlers plus a few responders |

## 6. Proposed success measures

Proposed, not agreed. "Now" figures are to 6 Sep and need refreshing with Ravi.

| Measure | Before 4.2 | Now | Proposed target |
|---|---|---|---|
| Overall acceptance | 75-78% | 72.7% (last full week in data) | 75% or higher |
| Pings per week, quiet four | 11-14 each | 1-3 each | 8 or more each within 6 weeks of release |
| Miss rate, quiet four | 1-4% | 57-75% | 25% or lower |
| Miss rate, carrying-more group | 0-4% | 10-18% | no worse than now, ideally lower |
| Busiest responders' pings per week | 13-15 (Gale, Nightwell) | 19-20 | back to 17 or lower |
| Open "quiet" and "gone before" tickets with no reply | 0 before 12 Aug | about 45 | none unanswered |
| Handlers who can see ping timeline for their responder | 0 | 0 | all 15 |
| Pings with a recorded phone trail (reached, shown, opened) | none collected | none collected | 95% or more after release; set the rest after a 2-week baseline |
| Push delivery rate and time-to-shown, by app version and network | unknown | unknown | measure for 2 weeks, then agree thresholds (M6) |
| Incidents raised with the phone trail attached | 0 (tickets are free text) | 0 | all incidents raised from a ping carry it |
| Pings sent to a responder who is Away or Off shift | not measured | not measured | 0 |
| Away periods that cost the responder rank | unknown | unknown | 0 (rank on return equals rank on leaving) |
| Tickets about "no pings" where the responder was Away or Off shift | unknown | unknown | trend down, since Support can see the state |

## 7. Open questions, in order

1. **Wen Li:** was the 4.2 weighting meant to apply to responders who decline? How long does "recent" last? Should missed count the same as turned down? (Marcus asked on 14 Aug; unanswered.)
2. **Ravi:** current weekly pings per responder (data stops 7 Sep). Did any of the four recover since?
3. **Marcus and the app team:** what push-delivery, app-version and device logs already exist, how M1 would be collected, whether to use a connector (Amplitude, Pendo, Intercom) or build in-house, and effort for A and B.
3a. **Security (Security Policy 4.1 owner):** sign-off on exactly what M1 collects, how long it is kept, who sees it, and confirmation that device data is keyed to cover identity only.
3b. **Tech Ops lead (not yet identified in the Team directory):** who they are, what they need from M4 to M6, and whether they want incidents in Support's queue or their own.
4. **Sofia Marino:** walk the phone accept flow (ticket 3033, alert visibility in 3 of 4 interviews) and confirm what the console shows a handler today.
5. **Supply owner:** what the availability record and callout load change means for maintenance scheduling (tickets 3054, 3109).
6. **Helen:** which Q3 commitments still stand (Availability Confidence, who-gets-pinged, ping wait tuning), and whether the wait goes back to 90s.
7. **Nadia:** reply to the roughly 45 open tickets now, without waiting for a release.
8. **Sofia Marino and Helen:** should return from Away be automatic on the end date, or only when the responder confirms? Automatic risks pinging someone who isn't ready; confirm-only risks someone staying silent after they're back (S8 again). What is the maximum length of an away period?
9. **Wen Li and Sofia:** how is availability set today, by whom, and from where? Does the Responder Availability Record already have an away state? (Needed before V1/V2 are sized.)

## 8. Constraints that apply to every option

- No design may assume a mapping from cover identity to legal identity (Security Policy 4.1). The handler view shows cover-identity names only.
- Phone monitoring data (M1) is keyed to the cover identity the app already holds, contains no location trail, no screen content and no legal identity, and is visible only to the responder's handler, Tech Ops and Support.
- Routing changes ship only with a release.
- Do not state a cause as fact until it has been tested on the data or by a staged rollout.

## 9. Caveats on the evidence

- Our evidence comes from handlers (tickets from all 15, four interviews). We have not heard directly from responders or quartermasters.
- The four interviews did not use the same questions, and the selection is undocumented.
- Tickets are free text with no category, device, or version.
- We hold no phone-side data today, so every claim about delivery, "never saw it" or "mis-tapped" remains untested until M1 has run. The phone trail in the prototype is invented sample data.
- "Tech Ops" is not named in the Team directory I have; I've assumed a team that owns app and push health. Confirm who that is.
- Farlight's "now" figures rest on about 4 pings; The Undertow, Meteor Mite and Vesper on 6-8. Read those percentages as direction, not precision.
- The tickets, interviews, and ping data agree on timing and on the four responders involved. They do not say why.
