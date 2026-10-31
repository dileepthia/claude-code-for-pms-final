# Draft reply to Marcus — 4.2 thread

Context: Marcus asked (14 Aug) whether the "change to who gets pinged" was
meant to apply to responders who've been turning jobs down too, noting
"the config doesn't distinguish between them."

Draft reply (for the 4.2 release thread):

---

On your 14 Aug question — you're right that the config doesn't distinguish,
and it doesn't look like a deliberate call. There's no separate handling
anywhere for responders who turn jobs down: the only lever is the
recent-acceptance score in history.py, and 4.2 actually *reduced* how much
that counts (weight 0.40 → 0.25). The catch is that offer.py feeds both
"turned down" and "missed" into the same record_declined penalty, so a ping
that simply wasn't answered in time is scored exactly like a refusal. That's
now landing on our most reliable home-area responders — the four who've gone
quiet since 4.2 (Vesper, Farlight, The Undertow, Meteor Mite) — who are being
docked as if they'd said no. So decliners aren't singled out; the real gap is
that we can't tell "no" from "didn't get the chance."

---

Still open for Wen: *why* those four dropped far enough down the ranking to
stop being pinged in the first place (the travel-time behaviour), which isn't
visible in the pings/config data.
