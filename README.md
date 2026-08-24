# LinkedIn Personal Brand — Agentic Pipeline

A skill-based system for writing LinkedIn posts that sound like a real person, adapted for Om's
genomics-entrepreneurship personal brand.

## What This Is

Om is an intern in the Genomics, Cytogenetics, and Molecular Genetics department at Sorus
Laboratories (Surat, Gujarat), working on Geneverse (a consumer wellness genomics brand under
Sorus) and internal Sorus projects. This system markets Om as an individual — researcher,
aspiring entrepreneur, curious generalist — separate from any Geneverse/Sorus brand marketing.

It produces one finished, ready-to-paste LinkedIn post per week. There is no auto-publish (no
LinkedIn posting connector is wired up) — Om reviews and approves at every stage of the pipeline,
then posts it himself.

This system started from an open-source LinkedIn ghostwriter skill built for a different voice
(an engineering leadership coach) and has been rewritten end to end: voice profile, content
pillars, and guardrails are all specific to Om's context.

## The Files

| File | What It Does |
|---|---|
| `files/LinkedIn_SKILL.md` | Core writing skill. Om's voice profile (built from his own posts), the three content pillars, confidentiality guardrails, technical accuracy guardrails, post format, and the pre-publish checklist. |
| `files/anti-ai-writing-guide.md` | Voice-agnostic guide for not sounding like AI — banned vocabulary, structure rules, tone rules, formatting tells, and an editing checklist. |
| `files/linkedin-weekly-system.md` | The weekly pipeline: Research → Angle → Draft → Guardrail Check → Final. Describes what Om reviews at each stage. |

## Content Pillars

1. **Building-in-public lessons from Geneverse** — genericized so they never leak
   Sorus-confidential specifics (partnership terms, competitor intel, unreleased pack names,
   pricing).
2. **Genomics ↔ business translation** — explaining genomics to a business audience, or
   business/ops concepts to a genomics audience.
3. **Occasional personal/curiosity posts** — keeps it human, not just a content machine.

## Guardrails

Two hard gates run before anything is finalized, checked in this order:

1. **Confidentiality** — nothing that could leak Sorus/Geneverse-confidential information, even
   generalized. See `files/LinkedIn_SKILL.md` → Confidentiality Guardrails for the full rule set
   and the guardrail question used on every draft.
2. **Technical accuracy** — genomics is a technical field; claims must be checkable, causal
   language must not be overclaimed, and no statistic or credential is ever invented.

Only after both gates pass does the post go through the anti-AI detection layer.

## Pipeline

The five-stage pipeline (Research, Angle, Draft, Guardrail Check, Final) is described in
`files/linkedin-weekly-system.md`. Om reviews and approves output at every stage — full pipeline
visibility, no stage is skipped.

## Adapting Further

The voice profile in `files/LinkedIn_SKILL.md` was built from three of Om's real posts. It will
get sharper with use — after each batch of new posts, feed back what felt right and what felt
off, and update the banned-phrase list and the "What GOOD Looks Like" examples accordingly.

## Origin

Built by adapting an open-source LinkedIn ghostwriter skill (voice calibration, anti-AI
detection, algorithm strategy) originally released by Marian Kamenistak
(https://www.kamenistak.com) under MIT license. The anti-AI writing guide's core framework is
kept close to the original as it's genuinely voice-agnostic; the voice profile, content pillars,
guardrails, and weekly system are rewritten for Om's context.
