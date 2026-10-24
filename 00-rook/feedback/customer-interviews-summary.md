# Customer interviews — research summary

_Four console handlers, interviewed by Sofia (Product Design) 2–5 Sept 2026._
_Source: Customer interviews database (Rook wiki). Mapped against 4.2 release findings._

## The headline
Three of the four handlers described the "gone before he could answer" moment
**unprompted** — and two separately described unexplained quiet stretches.
These are the exact two symptoms Support has logged since 4.2, now confirmed
independently from the field. The interviews didn't set out to study 4.2; the
pattern surfaced on its own, which makes it stronger evidence.

## Themes, counts, and likely 4.2 cause

| # | What handlers said | Raised | Maps to | Confidence |
|---|---|---|---|---|
| 1 | Callouts vanish before the responder can grab them | 3/4 (Dot, Ambrose, Halloran) | Ping wait cut 90s → 60s | High — mechanical; matches missed-ping spike 2%→21% |
| 2 | Unexplained quiet / lumpy volume — "phone just sits there" | 2/4 (Dot, Kip) | Proximity re-weight (0.45→0.60; acceptance 0.40→0.25) concentrating pings on nearest responders | Strong but unproven (can't see available-but-unoffered) |
| 6 | Filter persistence silently reverts without warning | 1/4 (Ambrose) | 4.2 "console filters persist" feature — works, but no reset warning | High — direct line to a 4.2 change |
| 3 | Text too small to read at a glance | 2/4 (Dot, Ambrose) | Pre-existing console UX, not 4.2 | n/a |
| 4 | Console doesn't alert the handler, only the responder's phone | 2/4 (Dot, Kip) | Pre-existing gap; relevant to Handler Phone App idea | n/a |
| 5 | No dark mode / screen too bright | 1/4 (Kip, emphatic) | Pre-existing UX | n/a |
| 7 | Supply: slow requisition queue, priority field does nothing, reports "go into a void" | 1/4 (Halloran) | Rook Supply, out of Dispatch scope | n/a |

## Quotes
- **1 — Dot:** "by the time he's actually got a thumb on the screen — it's gone. Gone to somebody else already."
- **2 — Kip:** "Meteor Mite's been texting me all week asking if something's broken because it's so quiet… And The Gale hasn't stopped, like, at all, same week, same city."
- **3 — Ambrose:** "at a glance I sometimes cannot tell engaged from available without leaning in."
- **4 — Dot:** "something that tells me too, not just him, when something's come in, so I'm not relying on hearing a buzz through a ceiling."
- **5 — Kip:** "the console is just this wall of white light in the middle of my face… I am begging. Dark mode."
- **6 — Ambrose:** "Twice now it's reverted to the default view… I'd rather it warned me the filter had reset than simply reset it."
- **7 — Halloran:** "One queue, everything in it, doesn't matter if it's laces or plate."

## What this adds to the 4.2 picture
- Validates both symptoms with real voices, not just ticket counts.
- Dot is the key witness — the only one who put both sides together in one call
  ("the quiet weeks and the fast-vanishing ones… I hadn't really put those two
  things next to each other"), which is the two-dials story.
- Ambrose adds a telling detail for Theme 2: "there was a period where a
  slower-arriving response of his still landed him the job more often than not,
  and lately that doesn't seem to hold." A handler feeling the acceptance-weight
  drop from the outside.
- Theme 6 is a quick, safe win — small, clearly tied to a 4.2 change, unrelated
  to the routing debate.

## Open questions
- Theme 2 still needs the availability data (or Wen) to confirm pings are
  concentrating — the interviews strengthen the hypothesis but can't prove it.
- Themes 3/4/5 are console-redesign input, separate from the 4.2 fix — keep in
  Sofia's redesign backlog, not the 4.2 response.
