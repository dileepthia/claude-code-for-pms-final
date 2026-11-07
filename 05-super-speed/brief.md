# Back in rotation — bringing quiet responders back into the callout flow

One-pager · Product, Dispatch · 7 November 2026 · draft for Helen

## Problem
Since 4.2, a handful of reliable responders stopped getting pings. Not because
they turn work down — in their own area they still take 85–95% of what reaches
them. The ranking quietly dropped them, and the way scoring works they can't
climb back: the only way your score goes up is to *take* a job, and they aren't
being offered any. So "quiet" becomes permanent on its own.

The fix in the code is small. But to the people it happens to, the whole thing
is invisible. Nobody at Rook can see a responder going quiet — it only shows up
later as a handler writing in. And the responder just experiences a phone that
stopped ringing, with no idea why and no way back. Helen's steer: don't just
change the number. Build the part a person would actually notice.

## What we'd build — from two points of view
**The handler (someone like Kip).** A "gone quiet" flag on the coverage card.
When one of his responders has had **no callouts offered for 3 days**, the card
says so — and says why: *"Being passed over — not turning jobs down."* That
distinction is the whole point; today Kip can't tell the two apart. The card
also tells him it's already being handled: *"Back in rotation automatically."*
Kip goes from fielding a "my phone never rings" message he can do nothing about,
to seeing it before the responder even notices — and knowing the system is
already fixing it. He doesn't have to do anything; he just isn't in the dark.

**The quiet responder (someone like Meteor Mite).** After 3 quiet days the
system eases them back on its own — no one has to flag them first. The next ping
gives them a little more time to answer; a 60-second fuse isn't fair to someone
who hasn't been pinged in a week and isn't watching the phone. And if they do
miss it, it doesn't count against them the way saying "no" would. Running
quietly in a corner of their app is a small status indicator — *"Actively
receiving pings"* — that's simply on when they're in rotation. No "you went
quiet," no event, nothing that reads as pity or surveillance: just steady
reassurance that they're in the flow. The feeling we're after is "I'm in the
mix," not "I got lucky once."

## Why this instead of a setting
A config change (longer timeout, re-weighted score) would lift the numbers, and
we should ship it. But it's invisible — it fixes the symptom without anyone ever
seeing the responder who was dropped or being able to help the next one. This
makes the quiet responder a *thing a person can see and act on*, which is what
Helen asked for and what keeps it from silently happening again.

## Success measure
- The four currently-quiet responders (Vesper, Farlight, The Undertow, Meteor
  Mite) get offered callouts again and their weekly takes recover.
- Handler "phone never rings" tickets fall back toward their pre-4.2 level.
- No responder sits quiet longer than ~2 weeks without either being offered work
  or a handler being shown it.
- Watch acceptance rate and missed-ping rate — not ticket volume, which lags the
  real picture by about two weeks.

## Scope
Dispatch only. The quiet-responder flag lives in the console's existing coverage
view (it informs the handler; there's no button to press — re-insertion is
automatic); the longer re-entry window + the ambient indicator live in the
responder app. The underlying scoring fixes (let a quiet score heal over
time; stop counting a miss the same as a refusal) ship alongside — but those are
the plumbing. This brief is the part people see.

Not in scope: a full rewrite of how ranking works, a handler phone app (separate
Q4 exploration), anything on the Supply side. Confidentiality stands — none of
this needs or touches a responder's real identity.

## Decisions so far
- **Automatic, not a handler action.** The system brings a quiet responder back
  on its own; the handler's card is there to *inform*, not to require a tap.
- **Trigger: 3 days with no callouts offered** (keyed on pings *sent*, not takes,
  so someone who is merely refusing doesn't get flagged).
- **Responder cue is ambient, not an event** — a small "Actively receiving
  pings" indicator, never a "you went quiet" message.

## Clickable version
A rough prototype of the handler's coverage card (the "gone quiet · back in
rotation automatically" flag) and the responder's app corner (the "Actively
receiving pings" indicator) to follow, so Helen can click it rather than read it.
