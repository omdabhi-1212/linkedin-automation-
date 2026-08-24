# External Reference Posts

A swipe file of other people's posts Om likes, with his notes on exactly what he likes about
each one.

**This is a technique reference, not a voice source.** Nothing here gets copied into
`LinkedIn_SKILL.md`'s "What GOOD Looks Like" section, which is reserved for Om's own approved
posts (see `voice-calibration-log.md`) — that section is voice calibration and needs to be
100% Om. What belongs here is different: a structural device, an opening move, a way of landing
an ending — extracted as a principle, then checked against Om's actual voice before it's allowed
anywhere near `LinkedIn_SKILL.md`. Liking a post's craft doesn't mean the underlying voice fits
Om — plenty of posts work because of a voice (confrontational, punchy, guru-ish) the Narrative
Positioning section explicitly rules out for him.

---

## How to Log a Reference

Append a new entry using the template below. When Om points out something he likes, don't just
note the compliment — pin down the mechanism, then test it against his voice before it goes
anywhere near the skill file.

```
### Reference #[N] — [author/source, if known] — [date added]

**Post text or link:**
[the post, or a link/description if it's long]

**What Om likes about it (his words):**
[verbatim or close to it]

**Principle extracted:**
[the actual transferable technique — e.g. "opens with a number before any context," "ends the
post on a single-word line," "names the tension in the first sentence, not the topic"]

**Fits Om's voice?** [Yes / No / Partially] — [why, checked against Voice Profile /
Narrative Positioning in LinkedIn_SKILL.md]

**Folded into LinkedIn_SKILL.md?** [where it was added — section and edit — or "not yet, needs
Om's read on the extracted principle first" / "no, doesn't fit — see reasoning above"]
```

**A principle that doesn't fit isn't wasted.** Log it anyway with "No" and the reasoning. That's
still useful — it sharpens what Om's voice is *not*, the same way the banned-phrases list in
`LinkedIn_SKILL.md` does.

---

## Using the Hook Extractor

`linkedin-hook-extractor/` (in this same `files/` directory) is a downloaded tool, adapted for
this system, that classifies a post's structure against 16 named "hook formulas." When Om points
to an external post, it's useful for naming precisely what technique is at work instead of
describing it vaguely — but its output never skips the steps below.

**The gate happens before logging, not after:**

1. Run the classifier (or eyeball it against the table below for something obvious, like a
   comment-gate CTA).
2. Look up the formula's verdict in the Hook Formula Compatibility Table.
3. **If the verdict is Banned, do not log it as a candidate at all.** Not "logged as No" —
   simply don't create an entry. A banned formula (comment-gate, curiosity-gap, manufactured
   urgency, guru-voice identity reframes, and the others below) is incompatible with
   `LinkedIn_SKILL.md`'s Narrative Positioning and Post Format on principle, not on a
   case-by-case judgment call — there's nothing to weigh. If Om still wants a note on why he
   liked the post's effect, that's a conversation, not a log entry.
4. **If the verdict is Already-native, Needs Adaptation, or Marginal,** proceed to a normal log
   entry — the "Principle extracted" field is where the classifier's structural read goes,
   rewritten in plain prose (see "Never Hand Off a Blank Template" below).

**Never hand off a blank template.** The hook extractor's original design generates a
fill-in-the-blank template with `{slot}` markers, meant to be handed straight to the Draft stage.
This system doesn't use that output. Here's why, and how the boundary actually works:

Om's format is flowing paragraphs shaped by what actually happened to him — not a structure with
blanks to fill. A slot-filled template is itself a kind of AI-smell generator: it's the same
failure mode the anti-AI guide warns about (AI's default architecture, generic list patterns) —
except imported from someone else's post instead of generated fresh. Filling in
`"{time period} ago, {event}. {reflection}."` produces a post shaped like the original writer's
voice wearing Om's facts, not a post in Om's voice.

The actual mechanism: whatever the hook extractor identifies gets written into "Principle
extracted" as a **sentence describing the structural move**, never as a fillable skeleton.
"Opens by anchoring the reader in a specific moment before naming the topic" is a principle.
`"{N} weeks ago, I {action}."` is a template. Only the first form is allowed in this file or in
`LinkedIn_SKILL.md`. When a post later gets drafted using that principle, `LinkedIn_SKILL.md`'s
Draft stage writes original sentences shaped by the idea — it does not open the reference post,
copy its skeleton, and swap in Om's words.

---

## Hook Formula Compatibility Table

Verdict for all 16 formulas from `linkedin-hook-extractor`. Checked once, here, so it doesn't
need re-litigating on every reference post.

| # | Formula | Verdict | Why |
|---|---|---|---|
| F1 | Anaphora (parallel "X can Y" lines) | Marginal | Parallel-structure lists read as an AI tell per `anti-ai-writing-guide.md` Part 2.3. Usable only rarely, for a genuinely earned rhetorical moment — never a default. |
| F2 | R.I.P. / obituary | **Banned** | Declarative, hype-driven framing. Conflicts with Narrative Positioning — Om doesn't declare things dead, he reflects on what he's learning. |
| F3 | Year-over-year pivot | Needs adaptation | Fits journey-narrative posts (matches the BiOZEEN post's shape) if written as one grounded contrast, not the rigid "In 2024 I... In 2025 I'm..." parallel template, which is itself an AI tell. |
| F4 | Time-anchor / confession | **Already-native** | This is Om's real Opening pattern, already in `LinkedIn_SKILL.md` — "A few weeks ago, I attended...", "I moved back... a few months ago." Nothing to import; recognize it, don't reformulate it. |
| F5 | Self-proving meta (public 24h commitment) | **Banned** | Performative influencer-challenge mechanic. Doesn't fit a reflective, non-performative voice. |
| F6 | Comment-gate ("Comment KEYWORD below") | **Banned** | Already explicitly banned in `LinkedIn_SKILL.md`'s Pre-Publish Checklist as generic engagement-bait. |
| F7 | Odd-precision money / ledger | Needs adaptation | Fine only when the numbers are real and confirmed (per "Never invent statistics" in `LinkedIn_SKILL.md`). Drop the original rationale — using precise numbers because they psychologically read as more credible — and keep only "use real numbers because they're true and specific." |
| F8 | Paid-vs-free reversal | **Banned** | Sales/lead-gen mechanic. Om isn't selling a service; this doesn't apply to any of the three content pillars. |
| F9 | Curiosity-gap (short incomplete tease) | **Banned** | Textbook clickbait. Directly conflicts with the Opening pattern rule against cold-open "hot take" hooks. |
| F10 | Contrarian-historical + identity reframe ("if you're X, you already lost") | **Banned** | Guru-voice, confrontational persona — the exact register dropped when this system moved off the original coaching-voice skill. |
| F11 | Emotional cold-open (no setup) | Marginal | More abrupt than Om's demonstrated pattern, which is grounded and temporal, not scene-first-no-context. Situational at best, never the default opening. |
| F12 | Permission slip ("I don't know who needs to hear this") | **Banned** | A LinkedIn-influencer cliché functioning as engagement-bait reassurance. Also just not a phrase Om would say. |
| F13 | Bait-and-switch (fake bad news that resolves positive) | **Banned** | Manufactured tension for effect. Conflicts with the honesty this whole voice is built on — see Voice Arc's "real uncertainty stated as uncertainty." |
| F14 | Named gratitude (roll-call of people thanked) | **Already-native** | Matches the HAI Conclave post exactly. Already governed by the Names section and Confidentiality Guardrails in `LinkedIn_SKILL.md` — external people with real credit, never Sorus/Geneverse colleagues without clearance. |
| F15 | Explain-to-kids | Needs adaptation | Could work for Pillar 2 Informative-mode posts if genuinely accessible, not gimmicky or condescending. |
| F16 | Status-strip identity contrast | Marginal | More dramatic than Om's understated tone. Situational, needs the confrontational edge sanded off if used at all. |

**Reading this table:** 8 of 16 formulas are banned outright — not because they "don't sound like
Om" in a soft, negotiable way, but because they collide with rules already locked into
`LinkedIn_SKILL.md` (engagement-bait bans, no hard CTAs, no clickbait hooks, no guru-voice). 2 are
already how Om naturally writes and don't need "adapting" so much as recognizing. The remaining 6
range from usable-with-real-changes to situational-at-best — none of them get used as delivered.

---

## References

*(Entries go below, oldest first.)*
