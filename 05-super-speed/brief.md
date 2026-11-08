# Back in rotation — bringing quiet responders back into the callout flow

**Director brief · Product, Dispatch · 8 November 2026 · for Helen Achebe**
*v2 — evidence-backed. Supersedes the 7 Nov one-pager; adds the full investigation behind the recommendation.*

---

## TL;DR

Release 4.2 (12 Aug) didn't have a soft August — it had a routing regression that shows up on the release day itself. Two things changed together: callouts now time out faster (60s), and the ranking concentrates each area's callouts onto its single top local. The result is two symptoms from one cause: reliable responders **miss** more, and a handful get **frozen out entirely** — and because of how scoring works, the frozen-out ones *can't recover on their own*. Aggregate acceptance has drifted back toward normal, but that number is hiding the four people it buried, who were still getting nothing when the data ends (7 Sep).

**The ask:** ship the config fixes now, **and** build **"Back in rotation"** — the part a person can actually see and act on: a handler flag that names a passed-over responder, automatic re-insertion, and the scoring change that finally answers the note Wen left years ago. One decision needed from you: approve the scope and the "automatic, not a handler button" call.

---

## 1. What happened

4.2 shipped 12 Aug, the week before I arrived. Since then acceptance is down and handler complaints are up. The received wisdom in the handover was "mostly seasonal — wait for September, don't revert." The data does not support that.

## 2. What we found

**It was the release, not the season.** Acceptance sat flat at 75–78% for six straight weeks, then broke the week of 4.2. Splitting that week *at the release moment* removes any doubt:

| Week of 10 Aug | Acceptance | Missed |
|---|---|---|
| 10–11 Aug (pre-release) | 72.5% | 2.0% |
| 12–16 Aug (post-release) | 46.8% | 29.4% |

Same week, same people, same season — acceptance halves and misses go ~15× the instant you cross 12 Aug. The raw rows show the first missed pings appearing the morning of the 12th.

**Two symptoms, one release.** (a) *"Gone before I could answer"* — the 90s→60s ping-wait cut makes callouts time out; the entire acceptance drop is **missed** pings (turn-downs never moved). (b) *"My phone never rings"* — the proximity re-weight concentrates callouts on each area's nearest/top responder, leaving others unoffered.

**Why "quiet" becomes permanent — the score trap.** In the routing code, a responder's score only goes **up** by *taking* a job, and only goes **down** when they decline — and a **miss is scored the same as a refusal**. There's no time-decay (a `TODO` from 2019 that was never built) and the floor is zero. So a responder who gets starved can't climb back: no pings → no takes → no way to earn the score back. Quiet is self-reinforcing.

**Who it hit, and why it isn't who you'd guess.** It is *not* "homebodies vs travelers." Compare two responders who are *both* home-only (neither ever takes an out-of-area job):

| Per week | **Farlight** (Uptown) | **The Gale** (Eastgate) |
|---|---|---|
| Own-area pings, before → after | **9.4 → 1.6 (−90%)** | **12.1 → 14.6 (+20%)** |
| Total pings/wk | 12 → ~0 | 13 → 21 |
| Fate | starved to zero | busier than ever |

The difference is entirely what happened to each one's **own-area** pings. 4.2 turned routing into "the top local takes almost all," so identical responders ended at opposite extremes — The Gale won Eastgate, Farlight got frozen out of Uptown. That's why one handler (Kip) sees "two cards, two different products" on one screen.

**The recovery is a mirage.** Aggregate acceptance has climbed back from a 17 Aug trough (53.8%) to ~75% by early September — but:
- The **missed** rate is still ~12.7% vs a ~2% baseline (**~6× high**). The headline recovered largely because *turn-downs fell below normal*, masking the still-broken misses.
- It's the tail of the data (ends 6 Sep) — not a confirmed steady state.
- Most important: **the average healed while the distribution didn't.** The four quiet responders were *still* getting nothing in early September — their tickets run right to 7 Sep ("hasn't gone off in six days," "nothing coming through").

**Two independent sources agree.** The field interviews (4 handlers) and the 147 support tickets both fingerprint 12 Aug and both carry the same two symptoms — gone-before (3/4 interviews; 16 tickets) and too-quiet (2/4 interviews; 30 tickets, the single largest product block). They disagree only on *timing*: pings move on release day, tickets lag and peak ~2 weeks later — so **don't judge the recovery by ticket volume.**

## 3. The recommendation — "Back in rotation"

A config change (ease the ping wait, re-weight the score) would lift the numbers and **we should ship it**. But it's invisible: nobody at Rook can *see* a responder going quiet — it only surfaces later as a handler writing in, and the responder just experiences a phone that stopped ringing with no idea why. Your steer was: don't just change the number — build the part a person would notice. So:

**The handler (someone like Kip).** A *"gone quiet"* flag on the coverage card. When a responder has had **no callouts offered for 3 days**, the card says so — and says *why*: **"Being passed over — not turning jobs down."** That distinction is the whole point; today Kip can't tell the two apart. It also reassures him it's handled: **"Back in rotation automatically."** He goes from fielding a complaint he can do nothing about to seeing it before the responder even notices.

**The quiet responder (someone like Meteor Mite or Farlight).** After 3 quiet days the system eases them back **on its own**. The next ping gives them **a little more time** to answer — a 60-second fuse isn't fair to someone who hasn't been pinged in a week. And if they miss it, it **doesn't count against them** the way a refusal would. Running quietly in a corner of the app: an ambient **"Actively receiving pings"** indicator — never a "you went quiet" event. The feeling we're after is *"I'm in the mix,"* not *"I got lucky once."*

**The plumbing underneath** (the config fixes, shipped alongside): let a quiet score **ease back toward neutral over time** — finally answering Wen's 2019 note — and **stop scoring a miss as a refusal.** These are what make the visible part durable instead of cosmetic.

## 4. Decisions made

- **Automatic, not a handler action.** The system brings a quiet responder back on its own; the handler's card *informs*, it doesn't require a tap.
- **Trigger: 3 days with no callouts *offered*** (keyed on pings *sent*, not takes — so someone who is merely refusing doesn't get flagged).
- **Responder cue is ambient, not an event** — the "Actively receiving pings" indicator, never a "you went quiet" message.

## 5. Success measures

- The four currently-quiet responders (**Vesper, Farlight, The Undertow, Meteor Mite**) get offered callouts again and their weekly **takes** recover.
- Handler "phone never rings" tickets fall back toward pre-4.2 levels.
- No responder sits quiet longer than ~2 weeks without either being offered work or a handler being shown it.
- **Watch acceptance and missed-ping rate — not ticket volume** (it lags the real picture ~2 weeks). Specifically, drive the missed rate back toward its ~2% baseline, not just acceptance.

## 6. Scope

**In:** Dispatch only. The quiet-responder flag in the console's existing coverage view; the longer re-entry window + ambient indicator in the responder app; the scoring fixes (score decay + miss≠refusal) underneath.

**Not in:** a full rewrite of ranking, a handler phone app (separate Q4 exploration), anything on the Supply side. **Confidentiality stands** — none of this needs or touches a responder's real identity.

## 7. Open question for engineering (Wen)

The data shows the *pattern* cleanly, but *why* Uptown dropped Farlight while Eastgate kept The Gale — the exact ranking math that decides each area's "winner" — lives in the routing internals that aren't in the database. One conversation with Wen closes it, and it's also where the score-decay fix gets designed.

---

## Appendix — the evidence at a glance

**Weekly acceptance (pings table):** 76.7 · 77.1 · 76.4 · 75.0 · 77.6 · 77.9 → **54.2** (wk of 10 Aug, 4.2) → 65.8 · 66.7 · 72.7. Missed over the same weeks: ~1–3% → **21.5%** → 17.7 · 14.8 · 12.7.

**Near vs far (whole system, after 4.2):** out-of-area ping share rose **17% → 39%**; far pings miss **30%** vs near **10%**; **65% of all post-4.2 misses are far pings.**

**Ticket grouping (147 total):** console UI 37 · **too-quiet 30** · supply 26 · **gone-before 16** · filters 16 · admin 16 · notifications 6. The two 4.2 symptoms (46) are the largest product block; before 4.2 they were 0 and 1.

**Clickable version (built):** `prototype.html` in this folder — open in a browser, no setup. "Simulate: 3 days" shows one event across both screens: Kip's card flags a responder **by name** ("being passed over — not turning jobs down… back in rotation automatically"), and the responder app shows the ambient "Actively receiving pings" indicator plus an incoming callout on a longer window. Whatever the responder does — take, decline, miss — flows back onto Kip's card with a timestamp. *(Next pass: align the prototype to this v2, then build the executive deck.)*
