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

**Fix [partly done].** The hook-extractor already outputs a prose *principle*, never a template,
and the compatibility gate already bans 6 of the most manipulative formulas. The missing hard rule,
now written into `research-agent.md`: **Wing B may shape a post's structure, never its voice or
content.** Virality tells you a topic is hot or a hook-shape works; it never tells you what Om
thinks or how he sounds. A stronger version worth considering: cap how often a Wing-B-influenced
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

**Fix [done in research-agent.md].** Treat Om as the only reliable oracle for public-vs-internal.
Internal-sourced material is tagged, quarantined, and any draft touching it is **forced through
mandatory human review** at Guardrail Check — the model never auto-clears internal-sourced content.
The gate's job shifts from "decide if it's safe" (which it can't) to "detect that it *might* not be
and escalate to Om" (which it can). Om's end-audit becomes the last line behind the quarantine, not
the only line.

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

**Fix [design in research-agent.md].** For Technical-mode posts, require a **real citation** from
Wing A / the Consensus MCP, not a model assertion. If there's no source, the claim gets qualified
down to what Om can personally stand behind or cut. The accuracy gate should demand provenance, not
just plausibility.

---

## 🟠 6. The system optimizes "sounds like Om" but never learns what actually built audience

**Mechanism.** Goal #1 is building a genomics-entrepreneurship audience. But nothing in the pipeline
observes whether posts *actually did that*. It optimizes purely for voice fidelity and guardrail
safety — a post can be perfectly Om-voiced, perfectly safe, and land flat, and the system learns
nothing. Voice calibration and audience outcomes are different things, and only the first has a loop.

**Fix.** A lightweight **post-performance log**, separate from voice calibration: after a post is up
a week or two, Om notes how it did (reach, meaningful comments, who engaged) in one line. That feeds
*topic/angle selection* at the Research and Angle stages — not the voice. Keeps the two loops
separate (a flat post isn't a voice failure) while finally closing the loop on the actual goal.

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

**Fix.** Give the tracking log a **per-topic rep counter** (how many posts Om has actually written
on fermentation, on partnerships, on AI-in-diagnostics), and make the Angle-stage Voice Arc check
*consult* it rather than guess. Confidence framing then keys off a real count, not the model's
impression. Inert until flaw #2/#7 give it data to count.

---

## 🟡 9. "Anything and everything relevant" is an unbounded research scope

**Mechanism.** Passive daily collection with a broad net is how a research queue rots — noise piles
up, tokens get spent on marginal items, and the signal Om actually wanted drowns. It's the opposite
of the "information-dense sources" instinct that motivated the whole thing.

**Fix [design in research-agent.md].** A tight Interest Profile with **ranked axes, explicit
exclusions, relevance scoring, a hard daily item cap, and aggressive TTL** on unused items. The
exclusion list matters as much as the include list. Scope discipline is what keeps a passive
collector useful past week two.

---

## 🟡 10. Ephemeral environment vs. a "daily" agent

**Mechanism.** The container is reclaimed on inactivity; there's no persistent process to "run
daily." A naive daily scraper simply wouldn't exist between sessions, and its collected state would
evaporate.

**Fix [design in research-agent.md].** A **scheduled trigger** (cron) wakes a fresh session daily;
durable state lives in the **git repo** (committed digests + a `seen.md` dedup index), not in
memory. The repo is the database. Worth stating plainly so nobody assumes a long-running server that
isn't there.

---

## 🟡 11. Wing B's automation path is a real account/legal risk

**Mechanism.** Automated LinkedIn scraping violates LinkedIn ToS and can get an account restricted.
Doing it with Om's own account risks the very asset he's building the audience on.

**Fix [design in research-agent.md].** Default to **manual paste** (zero risk, works today). If
automated, only ever a **third-party API that never uses Om's account/cookies** — and even then it's
a gray-area cost Om accepts knowingly, not a default.

---

## The through-line

Most of these rhyme. The system is well-specified but **completely unexercised** — 0 posts, 0
research runs, every learning loop still theoretical. The single highest-value move isn't another
feature; it's **getting real posts through the pipeline** (flaws #2, #7, #8 all resolve the moment
real data exists). And the single biggest standing risk is the interaction of #3 and #4: automating
drafting *and* letting internal material in *while* one model grades its own confidentiality
homework. As long as Om is the human gate (Calibration phase), that's contained. It becomes
dangerous exactly at the automation boundary — which is the right place to demand the adversarial
check (#4) and the internal-material quarantine (#3) are actually solid before graduating.
