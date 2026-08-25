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
**scheduled Claude agent** — a cron-triggered remote session that runs once a day and uses:

- `WebSearch` + `WebFetch` for public web, news, and open-access journal content (Wing A)
- The `Consensus` MCP for real peer-reviewed papers with citations (Wing A, Technical-mode fuel)
- Manual paste, or a paid third-party API (e.g. Apify), for LinkedIn engagement data (Wing B)

**Two infrastructure realities this design has to respect:**

1. **The container is ephemeral.** A daily agent can't keep state in memory between runs. So the
   digest it produces is **committed to this repo** each day (`research/digest-YYYY-MM-DD.md`),
   and a small `research/seen.md` index prevents re-surfacing the same item. The repo *is* the
   durable store.
2. **Wing B can't be automated through Claude alone.** LinkedIn blocks automated reading of posts
   and engagement metrics behind auth; `WebFetch` cannot get behind that wall, and scraping it
   with Om's own account risks his account and violates ToS. So Wing B defaults to **manual
   paste** (Om drops in posts he notices doing well; the existing `linkedin-hook-extractor` +
   compatibility gate process them) and only becomes automated if Om funds a third-party API that
   never touches his own account.

---

## The Interest Profile — what the agent needs to know

This is the answer to "what do you need to boil down the areas of interest?" The agent is only as
good as this profile. Broad instructions like "anything relevant" produce noise; a tight profile
with explicit *exclusions* is what keeps a daily passive collector from piling up junk and wasting
tokens. Om fills this in (a starter is proposed; refine it like the voice profile).

### 1. Topic axes (what counts as relevant)
Ranked, because the agent scores relevance and a daily budget means it can't chase everything.

1. **Genomic wellness / consumer genomics** — the space Geneverse is in. Direct-to-consumer
   genetic testing, polygenic risk scores for wellness, nutrigenomics, the science and the
   business model both.
2. **AI + healthcare / AI + genomics** — predictive diagnostics, digital twins, wearables,
   AI in diagnostics and drug discovery. (Om's HAI Conclave and Mumbai Tech Week posts live here.)
3. **The business of genomics/biotech** — funding rounds, partnership models, unit economics,
   go-to-market, regulation. This is Pillar 2 fuel — the genomics↔business translation.
4. **Biomanufacturing / bioprocess** — fermentation, scale-up, India's bio-manufacturing push
   (the BiOZEEN post's territory).
5. **India biotech / India health-tech ecosystem** — because Om's journey is specifically an
   India-based one, and that's a differentiator.
6. **Adjacent-personal** — building in public, career pivots, learning in public, the
   science-to-entrepreneurship transition. Lower volume, Pillar 3.

### 2. Explicit exclusions (what to drop even if it matches a keyword)
Just as important as the includes. Starter list — Om extends it:
- Generic "AI will change everything" think-pieces with no specific finding or mechanism
- Pure clinical genetics with no wellness or business angle (unless Technical-mode material)
- Crypto/web3, generic startup hustle content, motivational-quote content
- Anything paywalled beyond an abstract (not worth the token cost to half-read)
- US/EU-only regulatory minutiae with no India relevance

### 3. Keywords / search phrases per axis
The agent searches these. Om seeds them; they get refined as some prove noisy. (Starter set lives
in `research/keywords.md` once Om approves the axes above.)

### 4. Trusted dense sources (Wing A allowlist)
The key feature Om asked for: prefer information-dense, trusted sources so tokens aren't wasted on
blind searching. Starter candidates to confirm/cut:
- **Journals / preprints:** Nature Genetics, Nature Biotechnology, bioRxiv (via Consensus MCP for
  anything Technical-mode)
- **Industry news:** Endpoints News, STAT News, Fierce Biotech, GenomeWeb
- **India-specific:** Inc42, YourStory (biotech/health verticals), The Ken (health)
- **People to follow** (named accounts whose posts are dense signal): Om supplies these — the
  people already in his network whose content he trusts. This is where his real network beats any
  generic source list.

### 5. Confidentiality tags (see next section)
Every source is tagged `PUBLIC` or `INTERNAL` at ingestion. This governs how the material can be
used downstream.

### 6. What Om is actually working on right now
A short, frequently-updated note ("this month I'm on fermentation scale-up / the data-ownership
problem / a specific AI build") so the agent can weight fresh material toward what Om can write
about with real, current authority — the Voice Arc's "earned reps" made concrete.

---

## Confidentiality handling (Om chose: internal sources allowed)

Om opted to let the agent ingest internal Sorus/Geneverse material too, auditing at the end. That
speeds research, and it also removes several layers of defense — so internal material is
**quarantined**, not treated like public material:

- **Tagged at the source.** Every ingested item carries `PUBLIC` or `INTERNAL`. Internal =
  anything from Sorus/Geneverse systems, internal docs, private conversations, or Om's own
  non-public work notes.
- **Kept separate in the digest.** `INTERNAL` items live in their own clearly-marked section, never
  interleaved with public material, so it's never ambiguous what's safe.
- **Never auto-merged into a draft.** An `INTERNAL` item can *inform* an angle, but any draft that
  draws on one is flagged and **forced through mandatory human review at the Guardrail Check
  stage** — the model does not get to auto-clear it, because the model cannot reliably tell what's
  already public vs. what's internal for Sorus. Only Om can. (This is flaw #3 in
  `pipeline-critique.md` — the confidentiality gate has no ground truth without Om.)
- **Om's end-audit is the last line, not the only line.** The quarantine above means a leak has to
  get past the tag, the separation, the forced review, *and* the audit — instead of the audit
  alone.

The one honest caveat, recorded so it's a deliberate choice not an oversight: ingesting internal
material at all creates a standing store of confidential text in the digest history. If that ever
feels like too much exposure, the fallback is public-sources-only + Om manually supplying internal
context per-post, which keeps the agent off the confidential path entirely.

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

**The standing risk, named:** Wing B pulls toward the mean of LinkedIn content — the precise thing
the voice profile fights. So the hard rule (also flaw #1 in the critique): **Wing B may shape a
post's structure, never its voice or content.** Virality insight tells you a topic is hot or a
hook shape works; it never tells you what Om thinks or how Om sounds.

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
- **Public substance (Wing A):** scored, deduped items with source/date/why-it-matters/pillar.
- **Internal (quarantined):** clearly separate, tagged, forced-review-on-use.
- **Virality signals (Wing B):** technique + compatibility verdict + the topic it surfaced.
- **Cross-wing links:** any A↔B loops fired this run.
- **Carry-over:** un-aged items still live from prior digests.

When a post gets drafted, the Research stage reads the recent digests instead of starting cold.
Nothing in a digest is ever used without passing the same guardrails as any other input — the
digest is a convenience, not a bypass.

---

## Build status

- **Designed, not yet built.** This file is the spec.
- **Wing A** is buildable now on the scheduled-agent + WebSearch/WebFetch/Consensus path, once Om
  confirms the Interest Profile (topic axes, exclusions, sources, keywords).
- **Wing B** works today in manual-paste mode through the existing hook-extractor; automation waits
  on an API decision.
- **Blocked on Om:** the Interest Profile above. The agent can't be pointed at anything until the
  axes, exclusions, and trusted sources are confirmed.
