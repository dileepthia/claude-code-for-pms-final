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
- Answer only the question the student asks, from the sources they
  name. Don't investigate beyond it or offer a theory about what
  Release 4.2 changed unless they ask for one.
- Read and write only within this directory.
  Exceptions, for the course-setup and wrap-up skills only:
  - When the student asks you to check their setup, save their work or wrap up a session, that request is their yes. You may run the GitHub command-line program installed at ~/.ccpm/gh for those checks and saves, and look in that folder to find it.
  - For a repair, first tell the student in one plain sentence what you are about to do, and act only after they say yes. Repairs may: run that GitHub program (including setting this folder's own git sign-in setting and changing this repo's visibility back to Public); copy the student's own course files into this directory from another folder on their computer (copy only; never move, edit or delete the originals); and rename something outside this directory that blocks setup, by adding "-old" to its name (never delete it).
  Outside this directory you still never write, edit or delete anything else.

<!-- Keep the block above at the top of this file. Everything you add
     during the course goes below this line. -->

---

## Working context

I'm the new PM for **Rook Dispatch**, started the week of 25 August 2026. This section is my orientation — who, what, and where things stand.

### The company
Rook Industries builds coordination and provisioning software for the protective-response sector. Customers are independently operating masked responders and the handlers/quartermasters who support them — Rook does not employ responders, it sells to them. Subscription revenue, priced per active responder. ~241 staff, mostly remote (HQ is Site Aleph, a remote ice-shelf station).

**Confidentiality — hard rule.** Responder cover identities are never stored in production. Rook holds capability tags, availability windows, and callout history, never the mapping to a legal identity. Never design features that assume we can identify anyone; never try to work out who a responder is. (Read Security Policy 4.1 before designing anything touching responder records.)

### The two products
- **Rook Dispatch** (mine) — responder coordination: availability, proximity, who gets pinged for a callout, and whether they take it. Users: handlers (web console) and responders (phone app). Current release 4.2.
- **Rook Supply** — gear provisioning: requisitions, maintenance, failure reports. Users: handlers and quartermasters. Supply *reads* the Responder Availability Record that Dispatch writes (to schedule maintenance into low-callout windows); it never writes to it.

Monthly release train; point releases are 4.x. Routing config ships *with* the release — handlers can't tune it at runtime.

### Vocabulary (Dispatch)
- **Callout** — a request for a responder to attend an incident; the unit of work.
- **Ping** — a callout offered to one responder on their phone. **Taken** (yes) / **Turned down** (no) / **Missed** (nobody answered in time). Turned-down and missed both pass the callout to the next responder; tracked separately.
- **Ping wait** — how long a ping stays before it's missed and moves on (set per release). Cut from 90s → 60s in 4.2.
- **Routing priority** — the score ranking available responders for a callout. Inputs: proximity (travel-time estimate), availability, capability match, recent acceptance history. Turning down/missing lowers your recent-acceptance component → lower place next time.
- **Acceptance rate** — share of pings taken. The headline metric, reported weekly in aggregate.
- **Time-to-accept** — median seconds from ping sent to taken.
- **Coverage gap** — incident where no available responder had the required capability tags (nobody *could* go — distinct from low acceptance).
- **Capability tag** — a competency on a responder record, matched to incident needs (flight, structural-entry, hazmat-tolerant, cold-weather, aquatic, crowd-management, de-escalation).
- **Responder Availability Record** — shared record of availability; written by Dispatch, read by Supply.
- **Mutual aid** — responders covering for each other across areas. Not built; Q4 exploration.
- Supply terms heard on shared calls: **requisition**, **field failure report**, **service interval**, **quartermaster**.

### The team (Dispatch) — and who's who in 4.2 comments
Comments on the 4.2 release page are signed by role; mapped to the Team directory:
- **Marcus Oyelaran** — Engineering Manager, Dispatch (Site Aleph). "Engineering manager" in comments; owns the eng team. Good first stop for anything uncertain; can pull rough numbers (not the official weekly ones).
- **Wen Li** — Staff Engineer, Dispatch (Berlin). "Staff engineer" in comments; built the routing/"who gets pinged" logic. Was away 14–24 Aug. To really understand ranking, talk to her — there's no written doc.
- **Sofia Marino** — Product Designer (Site Aleph). "Product designer" in comments; owns the console and phone app; ran the September interviews.
- **Nadia Hoffmann** — Support Lead (Berlin). "Support lead" in comments; owns tickets; worth a standing 15 min. Tracking the 4.2 ticket split.
- **Ravi Menon** — Data Analyst (Singapore). Owns the real numbers; reports weekly on acceptance. (The "pull the trend" ask in comments is really his data.)
- **Helen Achebe** — Director of Product (Site Aleph). My director; owns roadmap and commitments.
- **Priya Raghunathan** — the previous Dispatch PM; left 21 Aug 2026 after 14 months. Her handover doc is `00-rook/company/notes/handoff-from-priya.docx`.

### Releases (recent)
- **4.0** (7 Apr 2026) — new console nav; responder profile redesign; routing-override audit log.
- **4.1** (16 Jun 2026) — proximity via travel-time estimate; bulk callout; phone push reliability. (Mobile considered stable since here.)
- **4.2** (12 Aug 2026, current) — **change to who gets pinged (proximity weighted up vs. recent acceptance)**; ping wait 90s→60s; console filters persist; 3 defect fixes.

### Where things stand — the 4.2 situation
Since 4.2, **acceptance is down and handler complaints are up.** Support sees two ticket themes: (1) "my phone never goes off anymore" (~2/3 of tickets) and (2) "it buzzes but it's gone before I can answer" (~1/3 — explained by the 60s ping wait). Theme (1) is unexplained so far. Three things moved at once (routing change, ping-wait cut, plus a normal seasonal August dip), so cause isn't isolated. Priya's read was "mostly seasonal, wait for September numbers, don't revert." An open eng question (Marcus, 14 Aug): the routing change doesn't distinguish responders who've been turning jobs down — unclear if that was intended. The team deliberately held off concluding so I could look fresh. Nadia has a ticket breakdown ready; Ravi has the real weekly numbers.

### Q3 roadmap status (owner was Priya)
- **Committed:** Change to who gets pinged (shipped in 4.2), Availability Confidence (4.2), Ping timeout tuning (4.2), Requisition approval chains (4.3, Supply).
- **Exploring (Q4):** Handler phone app, Shared cover between responders.
- Open thread from the handoff: confirm with Helen which squeezed-out 4.2 items are still real Q3 commitments vs. quietly dropped — that conversation hasn't happened yet.

### Proposed work (product briefs)
- **Routing Override Audit Log** (Dispatch) — log manual routing overrides (who/when/who-instead/why) in the console activity view. Small, contained, design mocked.
- **Bulk Callout** (Dispatch) — send the same callout to several responders at once (each still accepts individually). *Note: release notes list bulk callout as shipped in 4.1 — check current status.*
- **Handler Phone App** (Dispatch, Q4 exploration) — notifications, live status view, one-tap contact for handlers. Scoped tight to those three things.
- **Requisition Approval Chains** (Supply, 4.3) — second approval step for requisitions over $2,000. Core ask is small; the brief has a lot of scope-creep tacked on (reroute slow approvers, cross-site dashboard, handler status visibility, replacing a spreadsheet).

### Notes to self
- Priya flagged: no written description of how routing decides who gets pinged — she meant to write it; it needs writing.
- Filter-persistence tickets in 4.2 are cosmetic noise — don't let them eat the month.

### Session findings — Module 4 (X-ray vision, 24 Oct 2026)
- The 4.2 drop is a one-week cliff, not a seasonal slope: pings data (database) show acceptance flat at 75–78% late Jun–3 Aug, then 54% the week of 10 Aug (4.2 shipped 12 Aug). Missed pings jumped ~2% → ~21%. Contradicts the "mostly seasonal" read in Priya's handoff and the 4.2 comments.
- Two separate causes, both from 4.2: (a) ping wait 90s→60s drives the missed-ping spike / "gone before I could answer" (~1/3 of tickets); (b) proximity re-weight (0.45→0.60, acceptance 0.40→0.25 in config.py) concentrates pings on the nearest responders, leaving others unoffered / "phone never goes off" (~2/3). Likely two dials to turn, not a revert.
- Cause (b) is strong but unproven: pings table only records pings sent, not who was available-but-never-offered. To confirm, need Wen (built ranking) or raw availability data.
- Tickets (database): ~5–8/week baseline → 20 / 27 / 32 in the weeks after 4.2, still climbing for weeks (Support lead's "3x" understated it).
- Bulk Callout contradiction confirmed across sources: Releases DB + 4.1 page + repo CHANGELOG say shipped 4.1; the Bulk Callout brief says "nobody's picked it up yet." Also no baseline/data field exists for its success metric (time-to-full-coverage).
- Open EM question (did routing change intend to hit decliners too?) is answerable from repo: history.py treats "turned down" and "missed" identically. Q3 roadmap DB still marks dropped 4.2 items "Committed" — the Helen conversation about what slipped still hasn't happened.

### Session findings — Module 4, part 2 (interviews + full tickets, 24 Oct 2026)
- Four customer interviews (Sofia, 2–5 Sept; wiki Customer interviews DB): "gone before answer" 3/4, "too quiet" 2/4, small text 2/4, alert-the-handler-too 2/4, dark mode 1/4 (Kip, emphatic), filter-reset-warning 1/4 (Ambrose), Supply/requisitions 1/4 (Halloran).
- All 147 support_tickets grouped (counts sum to 147): too quiet 30, console UI 29, Supply/gear 26, admin/how-to 24, gone-before 16, filters 16, notifications 6. The two 4.2 symptoms (quiet 30 + gone-before 16 = 46) are the largest product-related block.
- The two piles DISAGREE on the #1 symptom: interviews feel "gone before" most (3/4, vivid); tickets show "too quiet" is bigger (30 vs 16). Resolve by sequencing, not by picking one.
- Interview-loud / ticket-rare: dark mode, bigger text, alert-the-handler. Ticket-heavy / interview-absent: admin/how-to (24), operational UI bugs (logouts, slowness), specific gear failures, notification reliability.
- Agreed priority order (my rec): P0 ease ping wait back up (high-confidence, 1-line, reversible); P1 confirm-then-partially-rebalance proximity (highest impact, gated on availability data/Wen); P1.5 filter-reset warning (quick win); P2 triage admin/ops long tail; redesign asks → Sofia; Supply → Supply team.
- Deliverables written this session: 00-rook/feedback/customer-interviews-summary.md and 00-rook/company/prd-4.2-recovery.md (4.2 recovery PRD, draft — student may refine in later modules).
- Still open: "Lab A sentence" — student referenced it but it's not in the repo; need the actual sentence to check it.
