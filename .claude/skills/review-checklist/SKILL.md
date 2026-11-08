---
name: review-checklist
description: Checks a product brief against a four-point PM rubric — it names an owner, says how we'll know it worked, keeps the same scope from start to finish, and explains the problem before the fix. Use when the user says "review-checklist", "/review-checklist", "check this brief", "review my brief", or points at a brief file or a folder of briefs.
---

# Review checklist

A reusable reviewer for product briefs. Point it at a brief and it reports, for
each of four things a good brief must do, whether the brief does it — and flags
the ones it doesn't, with a reason the author can act on. It is a structural
gate, not an edit: it does not rewrite the brief or critique its wording.

## What to review

- If the user named a file or a folder, use that.
- If they said "this brief" / "my brief" and one brief is clearly in context, use it;
  otherwise ask one short question: which brief?
- A folder means every brief file in it — review each one separately.

## The four checks

Read the whole brief first, then judge each check against the brief as a whole.
A check is **yes** only if the brief clearly does it. When you are genuinely
unsure, **flag it** — a reviewer's job is to catch the gap, not to give the
brief the benefit of the doubt.

1. **Owner named** — Does the brief name a specific person or team who owns the work?
   - *yes:* a named owner, DRI, or team is responsible for delivering it.
   - *flag:* no one is named as owning it. Mentioning people in passing, or
     "flagging it for a future owner," is not the same as naming one.

2. **Success measure** — Does it say how we'll know it worked?
   - *yes:* at least one metric, target, or observable outcome that tells you it succeeded.
   - *flag:* it describes what gets built or shipped but never how anyone would
     know it is working.

3. **Scope stays bounded** — Is the scope the same at the end as at the start?
   - *yes:* what is in scope stays consistent from start to finish.
   - *flag:* it opens as one bounded thing ("that's the whole ask") but picks up
     extra asks along the way — a new feature, a dashboard, a cross-team rule, a
     spreadsheet replacement.

4. **Problem stated before the fix** — Does the problem come before the solution?
   - *yes:* the brief establishes the problem first, then proposes the fix.
   - *flag:* it opens with the solution or the feature, and the actual problem
     only appears later.

## How to report

For each brief, print this block (keep the dotted lines aligned):

```
<brief name>
  owner named ................. yes | NO — flagged
  success measure ............ yes | NO — flagged
  scope stays bounded ........ yes | NO — flagged
  problem stated before fix .. yes | NO — flagged
  -> <N> flag(s): <one short reason per flag, each pointing to where it falls short>
```

If nothing is flagged, end that brief's block with `-> clean: all four checks pass.`
After all briefs, print one summary line:
`<N> briefs checked, <M> flagged, <total> flags.`

## Rules

- **Be specific.** Every flag names where the brief falls short — quote a few
  words or name the section — so the author knows exactly what to fix.
- **Judge only these four things.** Don't rewrite the brief or critique grammar,
  style, or strategy. This is a pass/flag gate, not an editorial review.
- **Mentioning a topic isn't passing it.** The brief has to actually do the
  thing. When genuinely unsure, flag it and say why.
- Rook Industries is fictional course content. Read and write only within this
  course folder.
