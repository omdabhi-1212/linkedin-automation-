# Om's Weekly LinkedIn Content Pipeline

## How It Works

One post per week. Low-effort, high-quality — not a volume play. Om reviews and approves at
every stage, not just the final text. Nothing publishes automatically: there is no LinkedIn
posting connector, so the pipeline's output is always a finished, ready-to-paste post waiting on
Om to post it himself.

The pipeline runs five stages. Each stage produces something Om looks at before the next stage
starts.

---

## Two Phases: Calibration, Then Automated

**Right now the pipeline is in Calibration phase.** Three posts aren't enough to have actually
nailed Om's voice — see `voice-calibration-log.md` for status. In this phase:

- Every post gets drafted together, not handed off. Expect more than one draft round per post.
- Every post, approved or not, gets logged in `voice-calibration-log.md` — the draft, Om's real
  feedback, and what it changed (or confirmed) in `LinkedIn_SKILL.md`.
- Any external post Om points to as "I like this" gets logged in `external-reference-posts.md`,
  with the specific technique pinned down and checked against Om's actual voice before it goes
  anywhere near `LinkedIn_SKILL.md`. Liking a post doesn't mean copying its voice — see that
  file's intro.
- `LinkedIn_SKILL.md` is expected to change frequently during this phase. That's the point of
  calibration, not a sign something's broken.

**Automated phase** starts once Om says the voice is nailed — see "Graduating to Automated
Phase" below. After that, the pipeline is trusted to draft closer to final on the first pass, and
stage-by-stage review can compress (e.g. reviewing Draft + Guardrail Check together) rather than
five hard stops every time. The five stages themselves don't change — what changes is how much
back-and-forth each one needs.

---

## The Five Stages

### 1. Research

Gather raw material for the week's post: something that happened at Geneverse/Sorus worth
generalizing (Pillar 1), a genomics or business concept worth translating (Pillar 2), or an
event/experience worth reflecting on (Pillar 3).

Two sources feed this stage:
- **What Om supplies that week** — a rough note, a conversation, something he mentions in passing.
  Never invent source material.
- **The research agent's daily digests** (`research-agent.md`, `research/digest-*.md`) — a passive
  two-wing collector (trusted-source substance + LinkedIn virality signals) that builds a pool of
  current, relevant material so this stage isn't starting cold. The digest is a convenience, not a
  bypass: anything drawn from it passes the same guardrails as any other input, and anything tagged
  `INTERNAL` in the digest is forced through mandatory human review at Guardrail Check (see
  `research-agent.md` → Confidentiality handling).

**Output Om reviews:** raw notes and a proposed content pillar, before any drafting starts.

### 2. Angle

Propose the specific take: what's the actual lesson, translation, or reflection, which pillar and
post mode (Technical/Personal/Informative — see `LinkedIn_SKILL.md` → Post Modes) it should use,
and roughly how the post should open (see `LinkedIn_SKILL.md` → Voice Profile → Opening pattern).

For any claim in the angle, do a quick Voice Arc check (`LinkedIn_SKILL.md` → Voice Arc): has Om
actually earned the confidence this angle implies, on this specific topic? Technical-mode claims
grounded in his real scientific training can be direct from day one; operator/business claims
should stay in learning-posture framing until he's genuinely got reps on that specific thing.

If the research touches Geneverse/Sorus, flag right here what needs to be genericized and check
the guardrail question early rather than after a full draft is written:

*"If a competitor, a current Sorus partner's lawyer, or a Sorus exec read this, would it tell
them anything that isn't already public?"*

**Output Om reviews:** the proposed angle, pillar, mode, a Voice Arc confidence check, and a
first-pass confidentiality read, before any full draft is written.

### 3. Draft

Write the full post using `LinkedIn_SKILL.md` — voice, structure, format, post type. This is
where the actual writing skill does its work.

**During Calibration phase:** treat the first draft as a starting point, not a delivery. If Om's
feedback changes it, log the round in `voice-calibration-log.md` before moving to Guardrail
Check — don't wait until the post is fully approved to log it, since a rejected draft with clear
feedback is exactly the data the calibration phase needs.

**Output Om reviews:** the full draft, plus the skill's standard output note (character count,
pillar, post type).

### 4. Guardrail Check

A dedicated pass over the finished draft against both hard gates in `LinkedIn_SKILL.md`:

- **Confidentiality** — no partnership terms, no named partners without clearance, no unreleased
  pack names, no pricing, no competitor intel, no internal metrics, no unauthorized colleague
  names.
- **Technical accuracy** — no overclaimed causal genomics language, no invented statistics,
  dates, or study claims.

This stage runs even if the Angle stage already looked safe — a draft can drift during writing.
Anything that fails gets cut or genericized further, not softened with a caveat.

**Output Om reviews:** a pass/fail note per gate, with the specific sentence(s) flagged and how
they were fixed (or a question back to Om if the fix isn't obvious, e.g. "is this colleague OK
to name?").

### 5. Final

Run the AI Detection Layer and Pre-Publish Checklist from `LinkedIn_SKILL.md` (vocabulary,
structure, tone, formatting, specificity, read-aloud). Deliver the finished post ready to
copy-paste, plus the **visual companion recommendation** (`LinkedIn_SKILL.md` → Visual Companion
Recommendation) — whether a visual would help, what type, a concrete description, source-vs-generate,
and any confidentiality note — and the **topic tag(s)** for the Topic Reps Tally.

**Output Om reviews:** the final post text, the full checklist result, the visual recommendation,
and the topic tags. This is what Om actually copies into LinkedIn; the visual is his to act on or
skip.

---

## Quality Gates, In Order

Confidentiality and technical accuracy are checked before the anti-AI layer, not after — a post
that sounds perfectly human but leaks something or states something wrong is a worse outcome
than a post that sounds slightly AI-ish. All three risks are weighted equally per Om's own
priorities, but confidentiality and accuracy are irreversible once posted; voice is not.

1. Confidentiality guardrail (`LinkedIn_SKILL.md` → Confidentiality Guardrails)
2. Technical accuracy guardrail (`LinkedIn_SKILL.md` → Technical Accuracy Guardrails)
3. Anti-AI detection layer (`anti-ai-writing-guide.md` + `LinkedIn_SKILL.md` → AI Detection
   Layer)

---

## Content Mix (Priority Order)

Reflects Om's own ranking for why this account exists, not equal weighting:

1. Building audience/network in genomics-entrepreneurship — most posts should serve this
2. Documenting the Geneverse/Sorus journey publicly — Pillar 1, the most common post type
3. Credibility for MBA/career opportunities — a natural byproduct, not a separate content type
4. Pure thought leadership on genomics + AI — lowest priority; a post with no build-in-public or
   translation angle at all should be rare

At one post a week (roughly 4 a month), a reasonable mix looks like:
- 2-3 Pillar 1 posts (building-in-public, genericized)
- 1 Pillar 2 post (genomics ↔ business translation)
- 0-1 Pillar 3 post (personal/curiosity)

This isn't a strict quota — it's a check to run monthly if the mix starts drifting toward only
one pillar.

---

## Trigger Phrases

| Om says | Pipeline does |
|---|---|
| "Let's do this week's post" / gives a topic or raw note | Starts at Research with that input |
| "Here's what happened this week" | Starts at Research, pillar TBD until angle stage |
| Approves a stage's output | Pipeline moves to the next stage |
| Rejects or edits a stage's output | Pipeline reworks that stage before moving on — never skips ahead on an unapproved stage |
| "Is this safe to post?" | Runs the Guardrail Check stage standalone against provided text |
| "What's my content mix looked like this month?" | Reviews recent posts against the pillar mix above |
| "I like this post" / Om shares an external post with notes | Optionally runs `linkedin-hook-extractor` to name the technique, checks the Hook Formula Compatibility Table in `external-reference-posts.md` first — a Banned formula never gets logged as a candidate — then logs a passing one, extracted as a prose principle, never a fillable template |
| "That's not me" / "keep that, that's exactly right" on a draft | Logs the feedback in `voice-calibration-log.md` for the current post, and updates `LinkedIn_SKILL.md` if it's a durable rule, not a one-off |
| "Are we ready to automate?" | Checks the Graduating to Automated Phase criteria below against `voice-calibration-log.md` |

---

## Tracking

Two logs, both in `files/`:

- **`voice-calibration-log.md`** — every post drafted together: the input, the draft(s), Om's
  real feedback, the final approved text, and what changed in `LinkedIn_SKILL.md` as a result.
  This is the primary calibration record.
- **`external-reference-posts.md`** — external posts Om likes, with the specific technique
  extracted and checked against his voice before anything gets folded into `LinkedIn_SKILL.md`.

Together these are what makes the Voice Arc (`LinkedIn_SKILL.md`) usable over time — the record
of which topics Om has actually built real reps on, which is what future Angle stages should
check before deciding whether a claim has earned confident framing or still belongs in
learning-posture framing.

---

## Graduating to Automated Phase

Move from Calibration to Automated when, looking at `voice-calibration-log.md`:

- Recent drafts are landing close to right on the first pass — Om's feedback has shifted from
  "this doesn't sound like me" to minor line edits.
- The banned/allowed phrase lists and "What GOOD Looks Like" examples in `LinkedIn_SKILL.md`
  haven't needed a real update in the last several posts.
- All three post modes (Technical/Personal/Informative) have at least one logged, approved
  example — not just Personal, which is what the original three calibration posts happened to
  be.
- The Hook Formula Compatibility Table (`external-reference-posts.md`) has stopped changing —
  no verdict has needed reclassifying in the last several reference posts checked against it.
  Until then it stays a living table, revised whenever a verdict turns out to be conflating two
  different mechanisms (see that file's F12/F13 reclassifications for what that looks like).
- Om says so. This is ultimately his call, not a checklist the pipeline can tick off on its own.

When that happens, update the Status block at the top of `voice-calibration-log.md` to
`Phase: Automated` and note the date, and mark the compatibility table fixed in
`external-reference-posts.md` (see that file's header for the exact note). The log doesn't stop —
new posts still get added, and a post that clearly misses the voice still gets logged and folded
back into `LinkedIn_SKILL.md` — but stage-by-stage review can compress per "Two Phases" above.

**One distinction worth being precise about:** "Automated" here means automated *drafting* —
the pipeline trusted to produce a near-final post with lighter review. It does not mean automated
*posting*. There's still no LinkedIn posting connector wired up, so every post, in either phase,
ends at "ready-to-paste text" and Om pastes it in himself. If auto-posting ever becomes part of
this system, that's a separate, later decision — not something Automated phase implies on its
own.

---

## Project Files Reference

| File | Purpose |
|---|---|
| `LinkedIn_SKILL.md` | Voice profile, content pillars, confidentiality guardrails, technical accuracy guardrails, anti-AI checklist, post format, pre-publish checklist. The core writing skill used at the Draft and Guardrail Check stages. Updated directly whenever calibration feedback implies a durable rule. |
| `anti-ai-writing-guide.md` | Voice-agnostic guide for detecting and eliminating AI-sounding patterns. Used at the Final stage. |
| `linkedin-weekly-system.md` | This file. Describes the pipeline phases, five stages, review points, and content mix. |
| `voice-calibration-log.md` | Every post drafted during Calibration phase, with feedback and what it changed in the skill file. The record that decides when to graduate to Automated phase. |
| `external-reference-posts.md` | External posts Om likes, with the technique extracted and checked against his voice before it touches the skill file. Holds the Hook Formula Compatibility Table. |
| `linkedin-hook-extractor/` | Adapted downloaded tool that classifies an external post's structure into one of 16 named formulas. Feeds `external-reference-posts.md` only — gated by the compatibility table, never produces output used directly in a draft. |
| `research-agent.md` | Spec for the two-wing research agent (trusted-source substance + LinkedIn virality signals) that feeds the Research stage. Public-sources-only, manual-trigger, repo-as-store; Wing B manual-paste for now. |
| `interest-profile.md` | The tiered keyword/entity ontology + source allowlist + exclusions + scoring/decay rules that define the research agent's scope. Pending Om's cuts/adds and public-identifier URLs. |
| `pipeline-critique.md` | Adversarial flaw analysis of the whole system with proposed fixes, severity-ranked. Living document — revisit as flaws get resolved or new ones surface. |
