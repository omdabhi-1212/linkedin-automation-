# Hook Formula Classification Rules

Features extracted from a post and how they map to formulas. This is the classification
mechanism from the original downloaded tool, mostly unchanged — feature extraction and scoring
are voice-agnostic, the same way `anti-ai-writing-guide.md`'s structural rules are. What changed
is what happens *after* classification: see `../SKILL.md` step 3 and the Hook Formula
Compatibility Table in `../../external-reference-posts.md`. A formula classified here as F2, F6,
F8, F9, F10, F12, or F13 gets stopped at that gate, not turned into a template.

## Feature extraction

### Hook features (first 2 lines)
- `anaphora_count`: number of parallel "X can Y" style lines at the top
- `leads_with_number`: does line 1 start with a dollar figure or stat?
- `question_hook`: is line 1 a question?
- `confession_phrase`: "I stopped", "I was wrong", "for years I"
- `obituary_phrase`: "R.I.P.", "dying since", "cause of death"
- `time_anchor`: "{N} {days|months|years} ago"
- `year_over_year`: "In {2024|2025}, I ... In {2025|2026}, I'm"
- `curiosity_gap`: short incomplete tease (<8 words, no noun specified)
- `free_reversal`: "I charge X. Today it's free."
- `public_commitment`: "For the next 24 hours, I will"

### Body features
- `has_numbered_list`: 1., 2., 3., ... with ≥4 items
- `has_dated_receipts`: multiple "{Month Year} — {event}" lines
- `has_ledger`: line-item dollar amounts (non-rounded)
- `has_teardown`: screenshot references or annotations
- `has_checklist`: named steps with instructions

### Close features
- `mirror_question`: "What's your {last→this} pivot?"
- `identity_reframe`: "If you're X, you already lost"
- `commitment_close`: "If I'm wrong, I owe you a post"
- `soft_offer`: "Connect + DM me for X"
- `comment_gate`: "Comment KEYWORD below"

## Mapping features → formulas

```python
FORMULA_RULES = {
    "F1_anaphora": {
        "required": ["anaphora_count >= 3"],
        "boost": ["has_numbered_list", "metaphor_close"],
    },
    "F2_rip_obituary": {
        "required": ["obituary_phrase"],
        "boost": ["has_numbered_list", "identity_reframe"],
    },
    "F3_year_over_year": {
        "required": ["year_over_year"],
        "boost": ["mirror_question"],
    },
    "F4_time_anchor_confession": {
        "required": ["time_anchor OR confession_phrase"],
        "boost": ["mirror_question"],
    },
    "F5_self_proving_meta": {
        "required": ["public_commitment"],
        "boost": ["commitment_close", "has_numbered_list"],
    },
    "F6_comment_gate": {
        "required": ["comment_gate"],
        "boost": ["has_numbered_list"],
    },
    "F7_odd_precision_money": {
        "required": ["leads_with_number", "has_ledger"],
        "boost": ["identity_reframe"],
    },
    "F8_paid_vs_free_reversal": {
        "required": ["free_reversal"],
        "boost": ["has_checklist", "soft_offer"],
    },
    "F9_curiosity_gap": {
        "required": ["curiosity_gap"],
        "boost": [],
    },
    "F10_contrarian_historical": {
        "required": ["has_dated_receipts"],
        "boost": ["identity_reframe"],
    },
}
```

F11-F16 (Emotional Cold-Open, Permission Slip, Bait-and-Switch, Named Gratitude,
Explain-to-Kids, Status-Strip) are identified by the qualitative cues listed in `../SKILL.md`
rather than this feature table — the original tool didn't reduce them to boolean features either.

## Confidence scoring

```python
def score_formula(post_features: dict, rules: dict) -> float:
    required_met = sum(1 for r in rules["required"] if eval_feature(post_features, r))
    if required_met < len(rules["required"]):
        return 0.0
    boost = sum(1 for b in rules["boost"] if post_features.get(b))
    return 1.0 + 0.15 * boost  # cap at 1.6
```

Return top 2 formulas with score > 0.8.

**Read these scores as a rough ranking aid, not a real probability.** The weighting
(`0.15 * boost`, cap `1.6`) isn't derived from any measured data — it's a heuristic, same as
picking 3 as "the right number of bullet points" would be. Treat it the way
`../../LinkedIn_SKILL.md` treats any unconfirmed statistic: useful for ordering candidates,
not something to quote as a fact.

## Edge cases

- **Hybrid hooks:** when a post mixes two formulas (e.g., F4 confession + F3 year-over-year),
  return both with split confidence. If either is Banned per the compatibility table, treat the
  whole post as Banned for template purposes — don't launder a banned mechanic through a hybrid
  classification.
- **Narrative-only posts:** if no structural hook fires, classify as "free-form narrative" and
  skip formula assignment. This is, incidentally, close to Om's own default mode — most of his
  posts won't cleanly match a named formula, and that's fine.
- **Non-English:** skip classification, return structural breakdown only.
