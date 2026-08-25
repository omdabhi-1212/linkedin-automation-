---
name: linkedin-hook-extractor
description: Analyze the structure of an external LinkedIn post Om likes — which hook formula it uses, why it landed, and the underlying structural principle. Feeds into files/external-reference-posts.md, never directly into a draft. Every formula it identifies is checked against that file's Hook Formula Compatibility Table before anything gets logged as a candidate technique. Use only when Om points to an external post and wants to understand what makes it work.
---

# LinkedIn Hook Extractor — adapted for Om's system

This is a downloaded tool for reverse-engineering viral LinkedIn posts, adapted to fit a system
built around one person's honest voice rather than growth-hacking mechanics. Read this whole
file before using it — the adaptation notes below aren't optional context, they're the reason
half of the original tool's output is no longer used as-is.

**What changed from the original:** the original tool's job was "extract a proven viral
structure, hand over a fill-in-the-blank template, use it to seed a draft." About half of its 16
formulas are growth-hacking or engagement-bait mechanics (comment-gate, curiosity-gap,
manufactured urgency, guru-voice identity reframes) that directly conflict with rules already in
`../LinkedIn_SKILL.md` — see the Hook Formula Compatibility Table in
`../external-reference-posts.md` for the full verdict on all 16. This version of the skill exists
to analyze, not to seed drafts.

## When to use

- Om points to an external post and says "I like this" or "what's this doing"
- Before logging an entry in `../external-reference-posts.md`, to help name the specific
  technique instead of describing it vaguely

**Not for:** generating a template to hand to the Draft stage. See "Output" below — that's a
different, smaller thing than what this tool originally did.

## Input

The pasted text of a LinkedIn post. This tool is **paste-only** — there is no URL parser, no
scraper, and no Apify integration in this setup (the original download referenced `lib.url_parser`
and `lib.ApifyClient`; those don't exist here and have been stripped, not stubbed). If Wing B is
ever automated via a third-party API, that's a separate later build (see `../research-agent.md`);
until then, Om pastes the text.

## Steps

1. **Get the post text.** Om pastes it.
2. **Classify.** Match against the 16 formulas using the features in
   `references/classification-rules.md`.
3. **Check compatibility before doing anything else with the result.** Look up the classified
   formula(s) in the Hook Formula Compatibility Table (`../external-reference-posts.md`). If the
   verdict is **Banned**, stop here — report the formula name and why it's banned, and do not
   produce a structural breakdown or template for it. This is the gate the original tool didn't
   have.
4. **Extract structure** (for anything not banned): hook lines, body architecture, close pattern
   — described in prose, not slot markers. What is the post actually doing, structurally, and
   why did it work?
5. **Hand off a principle, not a template.** See "Output" below.

## Output

- **Formula identified**, with the compatibility verdict from
  `../external-reference-posts.md` (Banned / Already-native / Needs adaptation / Marginal)
- **What it's doing, in prose** — a sentence or two describing the structural move, not a
  fill-in-the-blank skeleton. Example: not "{time period} ago, {event}. {reflection}." but "opens
  by anchoring the reader in a specific moment before saying anything about the topic."
- **Why it worked**, briefly
- **No blank template.** The original tool's step 6 ("generate blank template with {slot}
  markers") is not reproduced here. A slot-filled template is exactly the kind of scaffolding
  that produces AI-smell — see `../external-reference-posts.md` for the full reasoning. What
  gets handed to Om (and logged) is the prose principle above; from there, `../LinkedIn_SKILL.md`
  writes an original post shaped by that principle, never one built by filling in someone else's
  blanks.

## Where this feeds

Everything this skill produces goes into an entry in `../external-reference-posts.md` — never
directly into a draft, and never bypassing the compatibility table. That file's log is the
record of what's been checked and adopted (or checked and rejected).

## Files

- `SKILL.md` — this file
- `references/classification-rules.md` — feature extraction + scoring heuristics (mostly
  unchanged from the original; this part is a genuinely reusable classification mechanism)

## Related

- `../LinkedIn_SKILL.md` — the actual writing skill. Never receives a template from this tool
  directly, only the prose principle logged in `external-reference-posts.md`.
- `../external-reference-posts.md` — the Hook Formula Compatibility Table lives here, along with
  the log of every reference post checked.
- `../anti-ai-writing-guide.md` — covers what the original tool called an "audit for AI tells";
  no separate audit skill is needed, this file already does that job for Om's voice specifically.
