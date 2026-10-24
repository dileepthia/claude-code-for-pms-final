# PRD — Rook Dispatch 4.2 recovery

- **Status:** Draft for review
- **Author:** PM, Rook Dispatch
- **Date:** 24 October 2026
- **Reviewers:** Helen (Director of Product), Marcus (Eng Manager), Wen (Staff Engineer, routing), Nadia (Support Lead), Sofia (Product Design), Ravi (Data)
- **Related:** `00-rook/feedback/customer-interviews-summary.md`; Releases 4.2; Q3 roadmap

## 1. Summary
Release 4.2 (12 Aug 2026) coincided with a sharp drop in callout acceptance
and a 3–4x rise in handler complaints. Two independent 4.2 changes each
produced one of two distinct symptoms. This PRD scopes the recovery: ship the
high-confidence, low-risk fix immediately, confirm and then partially correct
the higher-impact routing change, and clear one small 4.2-related UX gap.
Console-redesign wishes and Supply issues are explicitly out of scope and
routed elsewhere.

## 2. Problem & evidence
Acceptance was flat at 75–78% for six weeks, then fell to 54% the week of
10 Aug — a one-week step change at the release, not a seasonal slope. The drop
is driven mostly by **missed** pings rising from ~2% to ~21%. Complaints split
into two themes, confirmed independently across tickets and interviews:

| Symptom | Tickets | Interviews | Suspected 4.2 cause | Confidence |
|---|---|---|---|---|
| "Too quiet / phone not going off" | 30 | 2 of 4 | Proximity re-weight (0.45→0.60; acceptance 0.40→0.25) concentrates pings on nearest responders | Strong, unproven |
| "Gone before he could answer" | 16 | 3 of 4 | Ping wait cut 90s→60s | High (mechanical) |
| Filters silently reset | 16 | 1 of 4 | 4.2 "filters persist" feature, no reset warning | High |

The two piles disagree on which symptom is #1 — interviews feel "gone before"
most (3 of 4, vivid), tickets show "too quiet" is larger (30 vs 16). The
sequencing below resolves this rather than making them compete.

Business stakes: revenue is priced per **active** responder, so a responder
who stops being pinged is a churn and revenue risk, not only an unhappy user.

## 3. Goals
- Return acceptance rate toward its pre-4.2 baseline (~77%).
- Return missed-ping rate to pre-4.2 levels (< ~5%).
- Return weekly callout-ticket volume toward baseline (~5–8/week).
- Do all of the above **without** recreating the wide-area-responder complaint
  that 4.2 was built to fix.

## 4. Non-goals
- A full revert of 4.2 (trades one aggrieved group for another).
- Console redesign (dark mode, larger text, handler alerting) — backlog.
- Any Rook Supply work.
- Changing *who is eligible* for a callout; this is about ranking order and
  timing only.

## 5. Requirements, in priority order

### P0 — Ease the ping wait back up  *(ship first)*
- **What:** Raise `OFFER_TIMEOUT_SECONDS` from 60s toward 90s; test at an
  intermediate value (e.g. 75s) before committing.
- **Why:** Highest confidence, mechanical, one-line config change, fully
  reversible. Recovers the largest mechanical share of the drop (the missed-ping
  spike). Backed by both piles.
- **Evidence:** "coat and one boot on, when it moved along to someone else
  before he could get to his phone… it felt very fast." (ticket); 3 of 4
  interviews.
- **Acceptance criteria:** missed-ping rate falls back under ~5% within two
  weeks of release; no material rise in time-to-accept that harms coverage.

### P1 — Confirm, then partially re-balance proximity weighting  *(highest impact, gated)*
- **Step 1 — Measure (prerequisite):** Confirm pings are concentrating on the
  nearest responders. Needs the raw availability data (who was available but
  never offered) or a working session with Wen. The `pings` table alone cannot
  show this.
- **Step 2 — Correct (only if confirmed):** Dial the recent-acceptance weight
  partway back up (not to the old 0.40). Target a balance that reaches
  farther-out reliable responders again without starving wide-area responders.
- **Why:** Largest ticket volume (30) and the biggest business risk, but the
  unproven link and the deliberately-made change — so it earns the dial only
  after measurement.
- **Evidence:** "Corporal Ashgrove's phone has not gone off in three days…
  everything is set to available. Could someone check yours?" (ticket); Ambrose:
  "a slower-arriving response of his still landed him the job more often than
  not, and lately that doesn't seem to hold."
- **Acceptance criteria:** "too quiet" ticket theme returns to baseline; no rise
  in complaints from wide-area responders; acceptance rate recovers.

### P1.5 — Filter-reset warning  *(quick win, ship with P0)*
- **What:** When a saved filter reverts to default (e.g. after an update), warn
  the handler rather than silently resetting.
- **Why:** Small, isolated, outside the routing debate; asked for identically in
  both piles.
- **Evidence:** "My saved filters disappeared this morning… I did not notice for
  a while and was looking at the wrong list. Could it warn me when that happens?"
- **Acceptance criteria:** filter-reset tickets stop recurring; reset events
  surface a visible notice.

### P2 — Triage the admin/operations long tail  *(steady-state, pick top few)*
- **What:** Fix the 3–4 highest-volume non-regression items from the ~50 tickets
  in the account/admin and operational-UI groups — candidates: too-frequent
  logout, Monday-morning slowness, accidental turn-down (undo / spacing),
  "how does bulk callout work" (docs/UX).
- **Why:** Not caused by 4.2 and individually small, but collectively a large,
  cheap goodwill opportunity.
- **Acceptance criteria:** chosen items' ticket counts decline.

## 6. Out of scope — routed elsewhere
- **Console redesign** (dark mode ~4 tickets but emphatic in interviews; larger
  status-badge text; alert-the-handler-not-just-the-phone). Loud in interviews,
  rare in tickets. → Sofia's redesign backlog, not this recovery.
- **Supply / gear** (26 tickets: requisition approval queue, gear failures,
  deliveries, maintenance timing, field-failure no-reply). → Rook Supply team.

## 7. Risks & open questions
- P1 depends on data we don't yet have; if it can't be confirmed quickly, P0 +
  P1.5 still deliver real relief on their own.
- Routing config ships with the release (no runtime tuning), so every change
  here waits for a release train — sequence P0/P1.5 into the nearest release.
- Confirm with Helen which 4.2-squeezed roadmap items are still Q3 commitments;
  the roadmap still marks dropped items "Committed" (open thread from handoff).
- "Mostly seasonal" is still the prevailing internal read; this PRD assumes the
  data (step change at release) over that read.

## 8. Success metrics (summary)
| Metric | Baseline | Target |
|---|---|---|
| Acceptance rate | ~77% (pre-4.2) | Recover toward ~77% |
| Missed-ping rate | ~2% (pre-4.2) | < ~5% |
| Weekly callout tickets | ~5–8 | Return toward baseline |
| Wide-area responder complaints | low | No increase vs 4.2 |

## 9. Appendix — source data
- Interviews: Customer interviews database (Sofia, 2–5 Sept 2026), 4 handlers.
- Tickets: `support_tickets`, 147 rows, 29 Jun–7 Sep 2026, grouped: too quiet 30,
  console UI 29, Supply 26, admin/how-to 24, gone-before 16, filters 16,
  notifications 6.
- Pings: `pings`, weekly acceptance/missed trend (step change week of 10 Aug).
- Code: `00-rook/code/dispatch-routing/config.py` (weights, timeout).
