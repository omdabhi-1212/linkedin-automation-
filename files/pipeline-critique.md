# Pipeline Critique — Flaws and Fixes

A deliberately adversarial read of the whole system as it stands. The point isn't to praise what
works — it's to find where this breaks, especially the failure modes that are invisible until
they've already done damage. Each flaw has a severity, the actual mechanism of failure, and a
proposed fix. Fixes marked **[done]** are already reflected in the files; the rest are proposals
for Om to accept or reject.

Severity: 🔴 could sink the whole goal · 🟠 real quality/risk cost · 🟡 friction or slow decay.

---

## 🔴 1. The viral-content wing pulls toward the exact mean the voice profile fights

**Mechanism.** The entire system exists to sound like Om, not like generic LinkedIn. Wing B of the
research agent mines viral LinkedIn content — which is, definitionally, content that succeeds at
being generic-LinkedIn-optimal. Feed "what makes posts go viral" into an LLM drafter and it drifts
toward that mean, undoing the anti-AI work. This is the deepest tension in the design.

**Fix [done].** The hook-extractor outputs a prose *principle*, never a template; the compatibility
gate bans the manipulative formulas; and `research-agent.md` now carries both the hard rule —
**Wing B may shape a post's structure, never its voice or content** — and Om's framing that Wing B
is an *informed authoring decision, never a blind copy-paste* (understand why content resonates,
keep the process organic). Still worth considering later: cap how often a Wing-B-influenced
structure can be used (e.g. never two weeks running), so the account never becomes a viral-format
tribute act.

---

## 🔴 2. The calibration flywheel has never actually turned

**Mechanism.** `voice-calibration-log.md` has **0 entries**. The voice profile is built from 3
posts, all Personal-mode. The whole "learns over time" apparatus — the log, the feedback loop, the
Voice Arc rep-tracking — is untested. Technical-mode and Informative-mode have *zero* real
examples, so the system is currently guessing at two of its three registers. Automation criteria
that depend on this data are, right now, criteria against an empty dataset.

**Fix.** Two moves. (a) **Decouple calibration volume from publishing cadence** (see flaw #7) —
draft more posts for calibration than get published, especially early, so the dataset grows faster
than 1/week. (b) **Deliberately front-load a Technical and an Informative post** in the first few,
instead of letting all early posts be Personal (the easy default) — the graduation criteria already
require one approved example per mode, so target them on purpose rather than waiting for them to
happen.

---

## 🔴 3. The confidentiality gate has no ground truth — it can't know what's already public

**Mechanism.** The gate asks "would this tell a competitor something not already public?" But the
*model* doesn't know what's public vs. internal for Sorus/Geneverse. A funding number, a partner
name, a pack detail — the model can't reliably tell which of these Sorus has already announced and
which is confidential. It will confidently clear something that's actually internal. Om's decision
to let the agent ingest internal material sharpens this: now confidential text is *in the pipeline*,
not just in Om's head.

**Fix [done — resolved at the source].** Om's decision: the scraper ingests **public sources
only** (`research-agent.md` → Confidentiality). Internal material never enters via the scraper —
it stays Om's manual input, genericized by him. This resolves the flaw structurally rather than by
mitigation: the scraper can't misjudge public-vs-internal because it's barred from internal
material in the first place. The residual judgment call (is *this specific generalized lesson* safe)
stays with Om at Guardrail Check, which is where it always was and where it belongs — he doesn't
approve any post he isn't sure of.

---

## 🟠 4. One model drafts and grades its own homework

**Mechanism.** The same model writes the draft, runs the anti-AI check, runs the guardrail check,
and runs the accuracy check. A model is bad at catching its own tells — the phrasing it generated
is the phrasing it finds natural, so its self-review has a blind spot exactly where the errors are.

**Fix.** Make the checks genuinely adversarial rather than same-pass self-review. Cheapest version:
run Guardrail Check and the anti-AI pass as a *separate* step with a fresh, skeptical framing
("assume this draft is leaking something and sounds AI-generated; prove it"), not as a
continuation of the drafting context. Stronger version once it matters: a separate agent/session
does the checks so it never sees the drafting rationale. Om's stage-by-stage review is currently
the real adversary — which is fine in Calibration, but this gap is what makes full automation risky
(see flaw #3's interaction with #4).

---

## 🟠 5. The technical-accuracy gate has no external check

**Mechanism.** "Checkable against published science" — but nothing checks it. The model asserts the
genomics is right. In a technical field, confident-but-wrong is the worst outcome (it's Risk 3 in
the brief, credibility-costly), and it's exactly what LLMs do.

**Fix [done in LinkedIn_SKILL.md → Technical Accuracy Guardrails].** For any hard scientific/numeric
claim, a **real citation** (Wing A / Consensus MCP) is required before it clears — not a model
assertion. No source → the claim is either qualified down to exactly what Om can personally stand
behind, or flagged "Om must verify" in the output note. The gate now demands provenance, not
plausibility.

---

## 🟠 6. The system optimizes "sounds like Om" but never learns what actually built audience

**Mechanism.** Goal #1 is building a genomics-entrepreneurship audience. But nothing in the pipeline
observes whether posts *actually did that*. It optimizes purely for voice fidelity and guardrail
safety — a post can be perfectly Om-voiced, perfectly safe, and land flat, and the system learns
nothing. Voice calibration and audience outcomes are different things, and only the first has a loop.

**Fix [done in voice-calibration-log.md → Post Performance Log].** A lightweight post-performance
log, separate from voice calibration: ~1-2 weeks after a post goes up, Om notes how it did (reach,
meaningful comments, who engaged, a one-word verdict). It feeds *topic/angle selection* at the
Research and Angle stages — not the voice. The two loops stay separate (a flat post isn't a voice
failure) while the loop on the actual goal finally closes.

---

## 🟠 7. Cadence and calibration are in direct tension

**Mechanism.** 1 post/week is the right *publishing* rhythm for a credible low-volume account. But
it's a terrible *learning* rate — reaching even 20 logged posts across 3 modes takes most of a year,
so the voice profile sharpens agonizingly slowly and the Voice Arc's "earned reps per topic" barely
accumulates.

**Fix.** Separate the two rates explicitly. **Publish** 1/week. **Draft for calibration** as often
as Om has material and appetite — extra drafts get logged and improve the profile without being
posted. The calibration log doesn't care whether a post shipped; a great drafted-but-unpublished
post is just as much signal as a published one.

---

## 🟡 8. The Voice Arc has no actual trigger — it's aspiration without a meter

**Mechanism.** The Voice Arc says confidence should grow "as Om earns reps on a topic." But nothing
counts reps. The calibration log is supposed to be the evidence, but with 0 entries it's inert, so
in practice the Arc is a vibe, not a mechanism — the model will guess when Om has "earned" more
authority.

**Fix [done in voice-calibration-log.md → Topic Reps Tally].** A per-topic rep counter (how many
posts Om has actually written on fermentation, on partnerships, on AI-in-diagnostics), which the
Angle-stage Voice Arc check now *consults* rather than guesses. Confidence framing keys off a real
count. Related to but distinct from the performance log (flaw #6): reps = how *many* posts on a
topic; performance = how *well* they did. Both are needed, and both stay inert until flaw #2/#7 give
them data to count.

---

## 🟡 9. "Anything and everything relevant" is an unbounded research scope

**Mechanism.** Passive daily collection with a broad net is how a research queue rots — noise piles
up, tokens get spent on marginal items, and the signal Om actually wanted drowns. It's the opposite
of the "information-dense sources" instinct that motivated the whole thing.

**Fix [done in interest-profile.md].** A tight, tiered Interest Profile (Core / Adjacent /
Personal-journey + a Business-of-genomics lens) with **ranked keywords, explicit exclusions,
relevance scoring, a hard daily item cap, and aggressive TTL** on unused items. The exclusion list
is written out, not implied. Still needs Om to confirm cuts/adds and supply the named-people
sources, but the scope discipline is now a real document, not "anything relevant."

---

## 🟡 10. Ephemeral environment vs. a "daily" agent

**Mechanism.** The container is reclaimed on inactivity; there's no persistent process to "run
daily." A naive daily scraper simply wouldn't exist between sessions, and its collected state would
evaporate.

**Fix [Om's call: local + manual trigger for now].** The agent runs as a local project Om triggers
himself; durable state lives in the **git repo** (committed digests + a `seen.md` dedup index), not
in memory. "Daily" means "each day Om runs it" until there's a reason to automate the schedule — at
which point the host (a scheduled GitHub Action, a cron box) becomes its own decision. No
long-running server is assumed. See `research-agent.md` → infrastructure realities.

---

## 🟡 11. Wing B's automation path is a real account/legal risk

**Mechanism.** Automated LinkedIn scraping violates LinkedIn ToS and can get an account restricted.
Doing it with Om's own account risks the very asset he's building the audience on.

**Fix [done — Om's call: manual paste only for now].** Wing B is manual-paste, zero-risk, working
today through the hook-extractor. Any automated path (a third-party API that never uses Om's
account/cookies) is explicitly deferred to a later session, and would be a gray-area cost Om accepts
knowingly, not a default.

---

## Resolution status (this pass)

Decisions applied in this session, so the doc reflects the live system, not just the original
critique:
- **#1 viral-vs-authentic** — resolved: hard "structure not voice" rule + "informed decision, not
  copy-paste" framing in `research-agent.md`.
- **#3 confidentiality ground-truth** — resolved at the source: scraper is **public-only**;
  internal stays Om's manual input. (Reversed last session's "internal too.")
- **#5 accuracy citation** — resolved: citation-or-flag requirement in `LinkedIn_SKILL.md`.
- **#6 performance loop** — resolved: Post Performance Log in `voice-calibration-log.md`.
- **#8 Voice Arc trigger** — resolved: Topic Reps Tally in `voice-calibration-log.md`.
- **#9 research scope** — resolved: `interest-profile.md` (pending Om's cuts/adds).
- **#10 infra** — Om's call: local + manual trigger, repo as store.
- **#11 Wing B risk** — Om's call: manual paste only.
- **Compatibility-table churn vs. graduation** (was flaw #9 in the first chat pass) — resolved:
  Wing B is decoupled from the table (new viral examples don't reopen settled verdicts), with Om as
  the one-way mediator who can still hardwire a verdict change when a viral insight convinces him.
  See `external-reference-posts.md`.
- **Still open by choice:** #2/#4/#7 (need real posts / adversarial check at the automation
  boundary), and the genericization judgment — Om relies on not approving anything he isn't sure of,
  rather than a formal cooling-off rule.

## The through-line

Most of these rhyme. The system is well-specified but **still barely exercised** — 0 posts, 0
research runs, every learning loop still theoretical. The single highest-value move isn't another
feature; it's **getting real posts through the pipeline** (flaws #2, #7, #8 all resolve the moment
real data exists). And the single biggest standing risk is now #4 alone: one model grading its own
homework. With the scraper on public-only and Om as the human gate through Calibration, the
confidentiality exposure (#3) is contained. #4 becomes the thing to harden at the automation
boundary — the right place to demand a genuinely adversarial check before graduating.
