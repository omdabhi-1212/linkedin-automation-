# Research Agent — Two-Wing Passive Research

Feeds the **Research** stage of `linkedin-weekly-system.md`. Runs once a day, passively building a
pool of relevant material so that when a post gets drafted, there's real, current substance to
draw on instead of starting cold.

The agent has two wings that feed each other (see "How the Wings Feed Each Other" below):

- **Wing A — Substance.** Information-dense, trusted sources. What's actually happening in
  genomics, genomic wellness, AI + healthcare, biomanufacturing, and the business of all of it.
- **Wing B — Virality.** High-engagement LinkedIn posts on those same topics, mined for *why*
  they worked, so Om can build an organic version of the technique (never a copy).

---

## What runs it (the honest infrastructure picture)

There is no generic "scraper" wired up, and there doesn't need to be a custom one. The engine is a
**Claude agent Om triggers** — for now a session he kicks off himself, later a scheduled one if it's
worth automating — that uses:

- `WebSearch` + `WebFetch` for public web, news, and open-access journal content (Wing A)
- The `Consensus` MCP for real peer-reviewed papers with citations (Wing A, Technical-mode fuel)
- Manual paste for LinkedIn posts (Wing B) — see reality #3 below

**Three infrastructure realities this design has to respect:**

1. **Runs locally, triggered manually (for now).** Om's current call: keep this a local project he
   runs by triggering it himself, not a hosted always-on service. "Daily" means "each day Om kicks
   it off," until there's a reason to automate the schedule — at which point the host (a scheduled
   GitHub Action, a cron box, etc.) becomes its own decision. Nothing here assumes a long-running
   server that doesn't exist yet.
2. **State lives in the repo.** A manually-triggered agent still can't keep state in memory between
   runs. So each run commits its digest to the repo (`research/digest-YYYY-MM-DD.md`), and a small
   `research/seen.md` index prevents re-surfacing the same item. The repo *is* the durable store.
3. **Wing B can't be automated through Claude alone.** LinkedIn blocks automated reading of posts
   and engagement metrics behind auth; `WebFetch` cannot get behind that wall, and scraping it
   with Om's own account risks his account and violates ToS. **Current call: manual paste only.**
   Om drops in posts he notices doing well; the existing `linkedin-hook-extractor` + compatibility
   gate process them. A third-party API (never touching Om's own account) stays a later option if
   he ever wants Wing B automated — that's a separate session's work.

---

## The Interest Profile — what the agent needs to know

This is the answer to "what do you need to boil down the areas of interest?" The full ontology —
tiered keywords, entities, source allowlist, exclusions, scoring and decay rules — now lives in its
own file, **`interest-profile.md`**, seeded from Om's portfolio and goals. That file is the agent's
scope. This section just names the six inputs it captures and why each matters; edit the profile
itself in `interest-profile.md`.

1. **Tiered topic ontology (Core / Adjacent / Personal-journey + Business-of-genomics lens).**
   Ranked keywords and entities, because the agent scores relevance and a daily budget can't chase
   everything equally. This replaces "anything relevant" with an actual list.
2. **Explicit exclusions.** As important as the includes — the exclusion list is what keeps a
   passive collector from piling up junk and burning tokens.
3. **Keywords / search phrases per tier.** What the agent actually searches.
4. **Trusted dense sources, mapped to areas.** Prefer information-dense sources so tokens aren't
   wasted on blind searching, and route each source to the area it best serves. The highest-value
   entries are the **named people** in Om's network whose posts are dense signal — his real network
   beats any generic list.
5. **Om's public identifiers** (his LinkedIn, Sorus/Geneverse public presence). These draw the
   public-vs-internal line — see Confidentiality below.
6. **What Om is working on right now.** A short, updated note so the agent weights fresh material
   toward what Om can write about with real, current authority — the Voice Arc's "earned reps" made
   concrete.

---

## Confidentiality handling (public sources only)

**The scraper ingests only public, external sources — never anything internal, never Om's work
files, never Sorus/Geneverse internal systems.** This is a hard boundary, decided deliberately:
an automated agent that ingests internal material creates a standing store of confidential text in
the repo and makes Om's end-audit the *only* line of defense. Keeping the scraper on public sources
only means it can never be the thing that leaks something.

The division of labor that this sets up:
- **The scraper's job is the outside world** — news, journals, public LinkedIn, public
  announcements. If it isn't on a public channel, the scraper doesn't touch it.
- **Om stays the only source of internal material.** Anything from his actual Sorus/Geneverse work
  enters the pipeline the way it always has: Om supplies it manually and genericizes it himself,
  under the Confidentiality Guardrails in `LinkedIn_SKILL.md`. The scraper never reaches for it.

This also resolves the "no ground truth for public-vs-internal" problem (flaw #1 in
`pipeline-critique.md`) at the source: the scraper can't misjudge what's internal, because it's
structurally barred from internal material in the first place. The public-identifier list in
`interest-profile.md` is how it recognizes Om's own public footprint and stays on the right side of
that line.

---

## Wing A — Substance

**Job:** every day, search the keyword set against the trusted-source allowlist, pull the genuinely
relevant items, and summarize each into: source, date, one-paragraph what-happened, why-it-matters
for Om's pillars, and which pillar/mode it could feed.

**Anti-noise rules** (this is what makes it worth the tokens):
- Score each candidate 1-5 on relevance to the ranked topic axes; drop anything below a threshold.
- Prefer the dense sources; only fall back to open web search when the allowlist is thin on a topic.
- Dedup against `research/seen.md` — never re-summarize an item already logged.
- Hard daily item cap (e.g. 5-8 items) so the digest stays skimmable, not a firehose.
- Aggressive TTL: an item that sits in the pool unused for N weeks ages out. Relevance decays.

**Technical-mode fuel:** anything that could support a Technical-mode post gets a real citation via
Consensus, so the Technical Accuracy Guardrail has an actual source, not a model assertion (flaw #5).

---

## Wing B — Virality

**Job:** find high-engagement LinkedIn posts on the same topic axes, and extract *why* they
worked, so Om can build an organic version.

**How it runs** (given the LinkedIn constraint above):
- **Default — manual paste.** Om drops in posts he sees doing well. Each goes through the existing
  `linkedin-hook-extractor` → Hook Formula Compatibility Table (`external-reference-posts.md`).
- **Optional — third-party API.** If funded, an Apify-style actor pulls high-engagement posts by
  keyword. Never uses Om's own account/cookies.

**What it extracts** (not the post — the technique):
- The hook formula (F1-F16) and, critically, its **compatibility verdict**. A Banned formula is
  reported as "this went viral *because* of a mechanic we don't use" and stops there — it does not
  become something to imitate.
- The structural principle, in prose, for anything not Banned — exactly the pipeline already built
  in `external-reference-posts.md`. Never a fill-in-the-blank template.
- The underlying *topic* that resonated — which feeds back into Wing A (see below).

**Informed decision, not blind copy-paste.** The point of Wing B is not to make a post go viral by
reusing content that already did. It's to *understand* what kind of content is resonating and how
it's structured, pull whatever genuine insight is there, and let that inform Om's own writing while
keeping the whole thing organic. It's a more-informed authoring decision, never a paste. That's why
the output is always the prose principle through the gate, never the post or a template.

**The standing risk, named:** even so, Wing B pulls toward the mean of LinkedIn content — the
precise thing the voice profile fights. So the hard rule (also flaw #1 in the critique): **Wing B
may shape a post's structure, never its voice or content.** Virality insight tells you a topic is
hot or a hook shape works; it never tells you what Om thinks or how Om sounds.

---

## How the Wings Feed Each Other

This is the part Om specifically asked for — the two wings aren't parallel, they're a loop:

- **B → A:** When Wing B finds that a topic is going viral (say, "digital twins for preventive
  health"), it tells Wing A to go get the *substance* on that topic — the real papers, the real
  findings — so Om can write something true and specific about a hot topic, not just ride the
  wave with an empty take. Virality identifies demand; substance supplies credibility.
- **A → B:** When Wing A surfaces a genuinely important development (a real finding, a funding
  event), it tells Wing B to go see whether/how anyone has posted about it successfully — is there
  a proven way this lands on LinkedIn, or is this an open lane Om could be early on? Substance
  identifies what matters; virality shows whether there's an audience shape for it.

The output of the loop is a research item that carries both: *this topic matters (A) and here's
what makes content about it land (B)* — which is exactly what the Angle stage needs to pick a post
that is both true and likely to build audience.

---

## Output: the daily digest

Each run commits `research/digest-YYYY-MM-DD.md` with:
- **Substance (Wing A):** scored, deduped public items with source/date/why-it-matters/pillar.
- **Virality signals (Wing B):** technique + compatibility verdict + the topic it surfaced.
- **Cross-wing links:** any A↔B loops fired this run.
- **Carry-over:** un-aged items still live from prior digests.

When a post gets drafted, the Research stage reads the recent digests instead of starting cold.
Nothing in a digest is ever used without passing the same guardrails as any other input — the
digest is a convenience, not a bypass.

---

## Build status

- **Designed, not yet built.** This file is the spec; the ontology is in `interest-profile.md`.
- **Wing A** is buildable now on the manual-trigger agent + WebSearch/WebFetch/Consensus path, once
  Om confirms `interest-profile.md` (keywords to cut/add, sources, named people, the three
  public-identifier URLs).
- **Wing B** is manual-paste only for now, through the existing `linkedin-hook-extractor`. Its
  automation (a third-party API) is explicitly deferred to a later session.
- **Blocked on Om:** `interest-profile.md`. The agent can't be pointed at anything until the
  keywords, sources, and public identifiers are confirmed.
