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
| `files/LinkedIn_SKILL.md` | Core writing skill. Om's voice profile (built from his own posts), a Voice Arc for how confidence should grow over the journey, Technical/Personal/Informative post modes, the three content pillars, confidentiality guardrails, technical accuracy guardrails, post format, and the pre-publish checklist. |
| `files/anti-ai-writing-guide.md` | Voice-agnostic guide for not sounding like AI — banned vocabulary, structure rules, tone rules, formatting tells, and an editing checklist. |
| `files/linkedin-weekly-system.md` | The weekly pipeline: Research → Angle → Draft → Guardrail Check → Final, plus the Calibration → Automated phase model. Describes what Om reviews at each stage. |
| `files/voice-calibration-log.md` | Every post drafted together during calibration, with Om's real feedback and what it changed in the skill file. Determines when the pipeline graduates from Calibration to Automated. |
| `files/external-reference-posts.md` | Other people's posts Om likes, with the specific technique extracted and checked against his own voice before anything is adopted. Holds the Hook Formula Compatibility Table. |
| `files/linkedin-hook-extractor/` | A downloaded post-structure classifier, adapted here: 8 of its 16 "viral hook formulas" are banned outright as engagement-bait or guru-voice mechanics that conflict with the voice profile. Feeds `external-reference-posts.md` only, and never hands over a fill-in-the-blank template — see that file for why. |

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

## Calibration Phase

The voice profile in `files/LinkedIn_SKILL.md` was built from three of Om's real posts — not
enough to call the voice settled. The system is currently in **Calibration phase**: posts get
drafted together, every draft and every piece of feedback is logged in
`files/voice-calibration-log.md`, and durable patterns get folded directly into
`LinkedIn_SKILL.md` in the same pass. External posts Om likes go through
`files/external-reference-posts.md` first — the specific technique gets pinned down and checked
against Om's actual voice before it's allowed near the skill file.

Once drafts are consistently landing close to right on the first pass across all three post
modes, the pipeline graduates to **Automated phase** — see `files/linkedin-weekly-system.md` →
"Graduating to Automated Phase" for the exact criteria.

## Origin

Built by adapting an open-source LinkedIn ghostwriter skill (voice calibration, anti-AI
detection, algorithm strategy) originally released by Marian Kamenistak
(https://www.kamenistak.com) under MIT license. The anti-AI writing guide's core framework is
kept close to the original as it's genuinely voice-agnostic; the voice profile, content pillars,
guardrails, and weekly system are rewritten for Om's context.
