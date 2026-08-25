# Interest Profile — the research agent's scope

This is the ontology that defines what the research agent (`research-agent.md`) treats as relevant.
It is deliberately a **list, not a vibe** — without it, "relevant" gets decided fresh every run and
drifts. Tiered by how directly each cluster serves Om's pillars, because the agent scores relevance
and a daily budget can't chase everything equally.

This is a **living document**, seeded here from Om's portfolio, goals, and history. Om cuts and adds
as some keywords prove noisy and new areas open up. Everything below is a proposed starting point.

---

## How the tiers map to the pillars

- **Tier 1 — Core.** Directly about consumer/wellness genomics — the Geneverse space. Highest
  relevance weight. Feeds Pillar 1 (building-in-public) and Pillar 2 (translation) most.
- **Tier 2 — Adjacent.** The broader biotech / health-tech / preventive-health world that Om's work
  connects to. Medium weight. Mostly Pillar 2 fuel, some Pillar 1.
- **Tier 3 — Personal-journey.** Om's specific positioning — India biotech, the science-to-business
  and US-to-India transitions, building in public. Lower volume, but high *differentiation* value.
  Feeds Pillar 3 and gives every pillar its distinctly-Om angle.
- **Cross-cutting — Business-of-genomics.** Not a tier but a lens that overlays Tiers 1-2: the
  money, models, regulation, and data questions. This is the core of Pillar 2 and what makes Om's
  dual fluency the actual value proposition.

---

## Tier 1 — Core (consumer & wellness genomics)

**Keywords / phrases**
- consumer genomics · direct-to-consumer (DTC) genetic testing · at-home genetic testing
- genomic wellness · wellness genomics · precision wellness
- polygenic risk scores (PRS) · genetic risk scoring
- nutrigenomics · personalized nutrition · nutrigenetics
- pharmacogenomics (wellness/consumer context)
- microbiome testing · gut health genomics
- epigenetics + lifestyle · epigenetic aging / biological age tests
- preventive genomics · predictive genomics · genetic predisposition
- consumer genetic counseling · returning genetic results to consumers
- health & wellness genetic panels · trait testing

**Entities to track**
- Companies: 23andMe, Ancestry, Nucleus Genomics, Function Health, Viome, Zoe, Thorne, GalleriGRAIL
  (screening), Tata 1mg / MapmyGenome / Nimble (India DTC), Orig3n
- Bodies/regulatory: FDA (DTC genomics guidance), ICMR & DBT (India), the DPDP Act (India data)

---

## Tier 2 — Adjacent (biotech, health-tech, preventive health)

**Keywords / phrases**
- AI in diagnostics · AI drug discovery · machine learning genomics
- preventive medicine · predictive medicine · early disease detection
- digital twins in health · in-silico patient models
- wearables + health data · continuous health monitoring · biosensors
- longevity · healthspan · aging biology
- precision medicine · biomarkers · liquid biopsy
- **alternative protein · cultivated / cultured meat · precision fermentation · cell-based foods**
- synthetic biology · engineered biology
- **biomanufacturing · bioprocess scale-up · microbial fermentation · biopharma manufacturing ·
  CDMO / CMO**
- biological research (broad — gated hard by the relevance score so it doesn't flood the queue)

**Entities to track**
- Alt-protein / ferment: Good Food Institute (GFI), Perfect Day, Solar Foods, Believer Meats
- Bioprocess: BiOZEEN (Om's own recent context), Sartorius, Cytiva (as industry signal)
- AI-health: Tempus, Isomorphic Labs, nference, Verily

---

## Tier 3 — Personal-journey (positioning & differentiation)

**Keywords / phrases**
- India biotech · India biotech manufacturing · Make-in-India biotech · India bioeconomy
- India health-tech · India deep-tech startups
- science-to-business transition · scientist-turned-founder · researcher to entrepreneur
- US-to-India transition (biotech / healthcare) · reverse brain drain · returnee founders
- building in public · learning in public
- solo building · no-code / AI tools for non-technical builders
- MBA + science · deep-tech entrepreneurship

**Entities to track**
- India ecosystem: DBT-BIRAC, C-CAMP, IndieBio, Inc42/YourStory startup coverage
- People: Om supplies these — the founders/scientists/operators already in his network whose posts
  are dense signal. This is where his real network beats any generic list, per `research-agent.md`.

---

## Cross-cutting — Business-of-genomics (Pillar 2 lens)

**Keywords / phrases**
- genomics funding rounds · biotech VC (India + global) · health-tech funding
- DTC health unit economics · consumer health CAC/LTV
- **data ownership / data privacy in genomics** · genomic data monetization
- genomic data regulation · consent models · GDPR / DPDP for health data
- partnership models in genomics · pharma-genomics deals · data licensing
- go-to-market for consumer health · retention in wellness products
- biotech business models · lab-to-market

*(Note: the "data ownership kills most genomics partnerships" angle from the brief lives here — a
strong Pillar 2 topic, genericized per the Confidentiality Guardrails.)*

---

## Trusted, information-dense sources (Wing A allowlist)

Grouped by which area they best serve, so the agent knows where to look for what — and doesn't burn
tokens searching a genomics journal for alt-protein news. Confirm/cut per source.

| Area | Sources |
|---|---|
| **Genomics / core science** | Nature Genetics, Nature Biotechnology, npj Genomic Medicine, Genome Biology, AJHG, bioRxiv (preprints), Cell (selective) — Technical-mode claims routed through the **Consensus MCP** for real citations |
| **Biotech / pharma industry news** | Endpoints News, STAT News, Fierce Biotech, GenomeWeb, BioSpace |
| **Preventive / AI health** | Nature Medicine, MedCity News, STAT (health-tech desk) |
| **Alternative protein / fermentation** | Good Food Institute (GFI) research, AgFunderNews, Green Queen |
| **Biomanufacturing / bioprocess** | BioProcess International, Fierce Pharma Manufacturing |
| **India biotech / health-tech** | Inc42, YourStory, The Ken, Entrackr, BioSpectrum India, Mint (health/biotech desk) |
| **Funding / business signal** | Endpoints (deals), Crunchbase News (free tier) |

**Deliberately excluded as low-signal** (the exclusion list matters as much as the includes):
- Generic "AI will change everything" think-pieces with no specific finding or mechanism
- Pure clinical genetics with no wellness or business angle (unless explicitly Technical-mode fuel)
- Longevity/biohacking content that's supplement-marketing dressed as science
- Crypto/web3, generic startup-hustle, motivational-quote content
- Anything paywalled beyond the abstract (not worth the token cost to half-read)
- US/EU-only regulatory minutiae with no India relevance

---

## Relevance scoring & decay (keeps the queue from rotting)

- **Score each candidate 1–5** against the tiers: Tier 1 hit = high; Tier 3 or cross-cutting = mid;
  Tier 2 broad-biology = low unless it also hits a more specific keyword. Drop below a threshold.
- **Prefer dense sources.** Only fall back to open web search when the allowlist is thin on a live
  topic.
- **Dedup** against `research/seen.md` — never re-summarize an item already logged.
- **Daily cap** (~5–8 items) so the digest stays skimmable.
- **Decay / TTL:** an item unused for ~2–3 weeks ages out of the live pool. Relevance is
  perishable; a three-week-old "funding round" is no longer a hook.

---

## Om's public identifiers (for the public-vs-internal line)

The agent needs these so it can tell "already public" from "internal," and so it recognizes Om's own
footprint. Om fills in:
- Om's LinkedIn profile URL: _______
- Sorus public presence (site, public announcements, press): _______
- Geneverse public presence (site, public launches, press): _______

Anything **not** on a public channel is internal and off-limits to the scraper — it stays Om's
manual input, genericized by him (see `research-agent.md` → Confidentiality: public sources only).

---

## Still needs Om

Before Wing A can run, confirm:
1. Which Tier 1–3 keywords to cut or add.
2. Which sources to cut, and — most valuable — the **named people** whose posts are dense signal.
3. The three public-identifier URLs above.
