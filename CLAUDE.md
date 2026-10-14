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

Sources: rook-wiki (About Rook, Glossary, Team directory, Releases, Q3 roadmap,
product briefs, Dispatch one-pager, 4.2 page comments) and Priya's handover doc
(00-rook/company/notes/handoff-from-priya.docx). Wiki is the record; re-read it
before relying on anything below. Today is 2026-10-13.

### My job
I'm the new PM for **Rook Dispatch** (started Mon 24 Aug 2026), reporting to
Helen Achebe. I inherited it from Priya, who left 21 Aug with no overlap.

### Company and products
Rook Industries (founded 2014, HQ Site Aleph, 241 staff) sells subscription
software (priced per active responder) to masked, independent **responders** and
the **handlers** and **quartermasters** who support them. Monthly release train;
point releases are 4.x.
- **Rook Dispatch** (mine): responder coordination. Handlers use a web console;
  responders use a phone app. Flow: incident enters console, Dispatch ranks
  available responders (routing priority), pings the top one, they take it or
  turn it down; a turned-down or missed ping moves to the next. Routing config
  ships with the release, not as a runtime setting.
- **Rook Supply**: gear requisitions, maintenance, failure reports. Handlers and
  quartermasters. Reads Dispatch's Responder Availability Record.
- **Hard rule:** Rook never stores or can reconstruct responder cover-to-legal
  identity (Security Policy 4.1). Never design for it or try to work out who anyone is.

### People (4.2 comments use roles, not names)
| Role in comments | Person | Notes |
| --- | --- | --- |
| Engineering manager | Marcus Oyelaran, Site Aleph | Runs Dispatch eng; Priya's "start here" |
| Staff engineer | Wen Li, Berlin | Built the who-gets-pinged logic; no written doc exists. Back from leave 24 Aug |
| Product designer | Sofia Marino, Site Aleph | Console and phone app; ran the September interviews |
| Support lead | Nadia Hoffmann, Berlin | Owns the tickets; hears handler complaints first |
| Director of Product | Helen Achebe, Site Aleph | My boss; owns roadmap and commitments |
| Data analyst | Ravi Menon, Singapore | Owns the weekly numbers on ping-taking. Not mentioned in 4.2 thread |
| Priya | Priya Raghunathan | Former Dispatch PM, gone 21 Aug |
Note: Priya's handover says to ask the engineering manager for numbers; Ravi
actually owns the weekly acceptance reporting. Go to Ravi.

### Vocabulary
- **Callout**: request for a responder at an incident. **Ping**: one callout offered
  to one responder's phone. **Taken / Turned down / Missed** (no answer in time);
  turned down and missed both move the ping on but are recorded separately.
- **Ping wait**: seconds before a ping counts as missed. Same for everyone.
- **Acceptance rate**: pings taken / all pings. Headline metric. Also
  **time-to-accept** (median seconds ping to taken) and **coverage gap** (no
  available responder had the required capability tags; different from low acceptance).
- **Routing priority**: score ranking responders: proximity (travel-time), availability,
  capability match, recent acceptance history. Turning down or missing pings lowers
  the acceptance component and so future rank.
- **Capability tags**: flight, structural-entry, hazmat-tolerant, cold-weather, aquatic,
  crowd-management, de-escalation. **Mutual aid**: cross-area cover, unsupported, Q4 list.
- Supply: **requisition**, **field failure report**, **service interval**.

### Release history
- **4.0** (7 Apr): new console nav, responder profile redesign, routing-override audit log.
- **4.1** (16 Jun): proximity uses travel-time estimate; bulk callout; push reliability.
- **4.2** (12 Aug): proximity weighted up vs acceptance history; **ping wait 90s to 60s**;
  console filters persist; three defect fixes.

### Where things stand: the 4.2 problem
- Since 4.2, callout tickets ~3x normal (Nadia, 18 Aug), still elevated 26 Aug, a handler
  emailed her directly. Split about 2/3 "phone never goes off" and 1/3 "gone before I
  could answer". Nadia can explain the second (60s wait) but not the first.
- Marcus asked 14 Aug whether the new weighting was meant to apply to responders who
  turn jobs down too. Wen never answered (away, then buried). **The code answers it:**
  nothing in config.py/history.py distinguishes them, so it fell out that way. Weights
  went proximity 0.45 to 0.60, recent acceptance 0.40 to 0.25.
- **Likely mechanism (my reading of 00-rook/code/dispatch-routing, not yet tested on data):**
  history.py treats *missed* the same as *turned down* (-0.12; an accept is only +0.08;
  glossary says they're recorded separately, the score doesn't). Scores never decay
  (2019 TODO, never done). So someone who slips down the order gets fewer pings, fewer
  chances to recover, and the 60s wait makes misses likelier. Fits Kip's pair: Meteor
  Mite silent, The Gale nonstop, same city, same week.
- **"Seasonal" claim:** Priya (19 Aug, handover) and Marcus (26 Aug) said August is
  always soft and it would settle in September. No prior-year or weekly data was shown.
  It can't explain "gone before I could answer" (the 60s wait does), and a quieter month
  shouldn't lower acceptance rate (a share). The 2-5 Sep interviews (Aunt Dot, Ambrose,
  Halloran, Kip) describe both patterns continuing into September. Treat as unproven.
- Priya also said don't revert 4.2 (three-quarter-old ask from wide-area responders). A
  fix to the *scoring* (decay, miss vs decline) is not a revert, but is not decided.
- **Helen's note (05-super-speed/director-request.txt):** she says the team traced the
  quiet-responder problem into the routing code, a small fix is available, and she wants
  a one-pager (ideally clickable) on what handlers and quiet responders would *experience*
  before anyone touches code. Helen, Marcus and Wen's TODO all appear to know already;
  the wiki thread doesn't. Marcus's "regroup after a week" (28 Aug) has no record.
- Still needed before concluding: Ravi's weekly acceptance data (maybe rook-database),
  split by responders who turn down/miss vs not, and before/after each 4.2 change.
- Supply risk, unconfirmed: Supply books maintenance into low-callout windows from the
  Availability Record. Starved responders could look "free". Tell Supply before changing
  that record's shape (availability.py).

### Roadmap (Q3 table, last reviewed 30 Jun, all owned by Priya, stale)
| Item | Target | Status | Reality |
| --- | --- | --- | --- |
| Change to who gets pinged | 4.2 | Committed | Shipped |
| Ping timeout tuning | 4.2 | Committed | Shipped as 90s to 60s |
| Availability Confidence | 4.2 | Committed | **Not in 4.2 notes; apparently slipped** |
| Requisition approval chains | 4.3 | Committed | Supply; brief is bloated |
| Handler phone app | Q4 | Exploring | Sofia exploring |
| Shared cover between responders | Q4 | Exploring | No brief |
Priya flagged that squeezed-out items need a conversation with Helen on which are
still Q3 commitments. **That conversation hasn't happened.** Do it early; committed
items are changed through Product, not directly. Q3 ended 30 Sep, so 4.3 and
Availability Confidence are overdue or at risk; verify current status.

### Briefs (all stale; check before trusting)
- **Bulk Callout** (Jan): says unowned, but bulk callout shipped in 4.1.
- **Routing Override Audit Log** (Mar): "ready to build", but it shipped in 4.0.
- **Requisition Approval Chains** (Jul, Supply): real ask is a second approval over
  $2,000; the brief then piles on bypass-after-48h, a cross-site dashboard, handler
  status view, spreadsheet replacement. Phasing TBD; scope needs cutting.
- **Handler Phone App** (8 Sep, Sofia): push notifications, status view, one-tap contact;
  prompted by Aunt Dot's interview, but her real pain is her responder losing the race
  to the ping, which a handler app doesn't fix. No baseline. Not staffed. Brief and
  roadmap name different owners.
- Requisition brief vs Halloran's interview: he wants *faster* (11 days for a cracked
  plate, one queue, priority field ignored), the brief adds a second sign-off.
- Interviews (Sofia, 2-5 Sep, console redesign research, 4 handlers): Aunt Dot (Vesper),
  Mr. Ambrose (Captain Vantage), Kip (Meteor Mite and The Gale), Halloran (Sgt. Bulwark,
  also runs the gear cage). Other asks: bigger text, dark mode (Kip), per-responder
  alert sounds, filter-reset warning.
- 06-sidekicks/scheduled-run-output.txt (13 Oct) flags all 4 briefs but reads a
  briefs/ folder not in this repo and misses that two already shipped.

### Other open items
- Priya asked me to **write down how who-gets-pinged works**. Wen is the source.
- Filter persistence: Priya called it noise, but Ambrose says it silently reset twice.
  Small reliability bug, not nothing.
- "Shared cover between responders" has no brief and is ambiguous (cover identity vs
  mutual aid); check against the confidentiality rule before scoping.
- Supply isn't in the Team directory; Supply PM/owner unknown.
- Priya's first-month advice: use the fresh-eyes window to question unexamined decisions.

### Learned in session 1 (13 Oct)
- Interviewees are **handlers**; "quiet" or "fast-vanishing" describes their *responders*
  (e.g. Kip is a handler; Meteor Mite is the quiet responder).
- The routing code lives in `00-rook/code/dispatch-routing/` (tuning values in config.py;
  its header says tell the engineering manager before changing anything there).
  Routing order, not eligibility: everyone available is on the list.
- Where to look: wiki search matches titles only (read pages for text and comments);
  wiki items, roadmap and briefs lag reality, so cross-check against release notes and code.
- Helen's open ask is in `05-super-speed/director-request.txt`; the 4.2 diagnosis is
  already known to leadership and engineering, so don't present it as news.
- Nothing here is verified against data yet. Anything marked hypothesis stays that way
  until Ravi's numbers say otherwise.

### Learned in session 2 (13 Oct)
- `support_tickets` (rook-database, 147 rows, 29 Jun to 7 Sep) is filed by **handlers** (12 filers),
  never responders, though some quote a responder. Wording is partly templated (same text across
  handlers), so counts show ticket volume, not distinct incidents.
- Since 12 Aug: about 4 tickets a day vs about 0.9 before. 15 "gone before they could answer" and 30
  "quiet" tickets, none before 12 Aug (first on 12 Aug and 17 Aug); 4 silent filter resets. Dark mode,
  badge size, alert sounds, tag legend, requisition delays, failure-report silence and catalog search
  all predate 4.2. All 45 lost/quiet tickets are still open.
- Quiet then lost is rare: only tickets 3110 and 3140 say both; Farlight, Halfmoon and The Undertow
  appear in both groups. Interviews undersell how bad quiet gets (tickets: 3 to 9 days of nothing).
- Interviews vs tickets: they agree on onset and on lost/quiet. Halloran's Supply complaints were loud
  but are only 4/2/2 tickets; tickets show console friction no interviewee raised. Ambrose's interview
  ("two weeks ago") and ticket 3043 (13 Aug) look like the same coat-and-one-boot scene but the dates differ.
- Supply: 26 tickets, 13 issues each filed twice. Marathon-day maintenance bookings (3054, 3109)
  contradict Halloran's "smarter scheduling"; no evidence yet that quiet responders are booked as "free".
- Untested idea: the routing pool is zero-sum, so one change makes some responders quiet and others
  (e.g. The Gale) busy; handlers of busy responders don't file tickets. Ravi's data is still the test.

### Learned in session 3 (14 Oct) — pings data
- `pings` (rook-database, 1,696 rows, 29 Jun to 6 Sep) has only callout, responder, sent_at, outcome: no response time, rank score, or capability tags (not in `callouts` or `responders` either). Missed pings went from 2.3% to 18% of pings on 12 Aug (7 on 12 Aug, 12 on 13 Aug); turn-downs stayed about 20%. Gap after a miss is about 60s.
- Four responders went quiet: Vesper, Farlight, The Undertow, Meteor Mite (about 49 pings a week combined to 3 by week of 31 Aug; 59% of their pings missed vs about 14% for the other 12). The other 12 got more pings (123 to 162 a week). Halfmoon and Corporal Ashgrove are drifting the same way and have quiet tickets.
- Vesper (Old Town) took 70 of 72 Old Town callouts before 4.2 and 5 of 38 after, last accept 17 Aug; Captain Vantage, Nightwell and The Longcast took over. Old Town coverage held (2 unanswered both periods), so the cost is to Vesper, not the area. Only Eastgate has two responders (Meteor Mite quiet, The Gale busy).
- Tickets vs pings: lost tickets start on release day (12 Aug); quiet tickets lag about 5 days, peak week of 24 Aug. Lost tickets fade while misses stay high, so tickets understate. No lost/quiet tickets found for Vesper or Meteor Mite. Interviews, tickets and pings agree on direction and order (miss first, then quiet); none shows the 60s wait or rank decay directly.
- Callouts with no taken ping: 5.5% to 11.1% (48 before, 49 after), mostly one-ping chains that just stop; why is unknown. Callout count also fell (about 140 to 120 a week) after 4.2, unexplained. Open asks: Ravi for time-to-accept and rank history; Wen/Marcus on why chains stop; capability tags to test callout needs.
- Code read-through (14 Oct, dispatch-routing): the only things that change a score are `record_accepted` (+0.08, called at offer.py:28) and `record_declined` (-0.12, called for anything not taken at offer.py:30). No fade, reset, floor protection or quiet-responder alert exists. Nothing tells a ping that never reached a phone from one ignored. `availability.py` and the phone push functions are empty stubs, so travel time and delivery can't be read from here.
- Correction to my earlier lock-in story: with recent acceptance at 0.25 weight, a score of 0 costs about 19 min of travel-time equivalent (vs about 40 at 0.40), and each yes is worth about 1.5 min. So the 60s wait (misses) plus the proximity weight probably do most of the damage. Untested; no rank scores or reply times in the data.
- Pings test of Marcus's 14 Aug question: the code applied the new weights to everyone and no decision is recorded. Turn-down rates (about 13-29%, flat) don't separate quiet from busy responders; misses do (53-64% vs 10-18% after 12 Aug), and the gap to the next ping after a miss is about 22h vs about 13.5h after a take or turn-down. Draft reply to Marcus (cc Wen) written in session, not sent.
- Glossary (last updated 4 Aug, before 4.2) says misses are recorded separately from turn-downs, and that both lower the score; it is silent on whether they should count equally. The 4.2 release page also lists three defect fixes (duplicate push when a ping is re-sent; capability-tag order in the detail panel; time zone on the coverage export). The duplicate-push fix isn't in the routing code, and Availability Confidence isn't in the code at all.
- Supply's wiki page says changes to how Dispatch calculates or updates the Availability Record flow into Supply's maintenance scheduling with no change on their side. The code note only asks to tell Supply before changing the record's shape. No named Supply owner yet.
