# Synthesis Patterns

How to extract strategy from source material bottom-up. The goal is to distill — not restructure. A 5,000-word article becomes a 300-line skill by extracting the signal, not by reformatting paragraphs.

## The Distillation Mindset

Source material contains three layers:
1. **Principles** — The fundamental ideas that make this approach work (keep these)
2. **Explanation** — Context that helps readers understand the principles (compress or discard)
3. **Filler** — Repetition, hedging, transitions, anecdotes (discard)

The skill needs layer 1 intact, a fraction of layer 2, and none of layer 3.

## Principle Extraction

Read the entire source material, then ask:

**"If someone could only remember 3-7 things from this, what should they be?"**

These are the core principles. They should be:
- **Actionable** — Each one changes what you do, not just what you think
- **Non-obvious** — Claude doesn't already know this (skip "write clean code")
- **Testable** — You can check whether you're following them

### Extraction technique

1. Highlight every claim, rule, or recommendation in the source
2. Group related claims into themes
3. For each theme, write one principle that captures the essence
4. Test: "Could I explain this principle without referencing the source?" If no, it's still too tied to the original phrasing.

### Example: language-market-fit

Source: A long article about writing marketing copy that converts.

Extracted principles (the "5 mental filters"):
1. Would someone Googling their problem recognize this in under 2 seconds?
2. Can I point to a specific customer struggle this line addresses?
3. Does this complete "Now you can ___" or "Our product is ___"?
4. Could a competitor paste this on their site?
5. If I showed this for 5 seconds, could they explain what the product does?

Notice: These aren't summary sentences. They're operational checks — things you actually run through while doing the work.

## Mental Model Identification

Mental models are frameworks for thinking, not instructions. Look for:

- **Decision trees** — "If X, do Y. If Z, do A instead."
- **Taxonomies** — Categories that organize the domain (e.g., dive-club's four advisor lenses)
- **Spectrums** — Scales with endpoints (e.g., "high freedom ↔ low freedom")
- **Matrices** — Two dimensions that create quadrants
- **Process loops** — Repeating cycles (build → test → refine)

### Example: dive-club-portfolio-advisor

Source: 16 podcast episodes with design leaders giving portfolio advice.

Extracted mental model: Four advisory lenses with context-dependent weighting.
- Craft Purists (visual quality, interactions)
- Hiring Managers (what actually gets hired)
- Builder-Launchers (shipping, visibility)
- The Strategist (business impact, narrative)

Plus a role-to-lens mapping table that determines which lens matters most based on the user's situation.

This mental model didn't exist in any single episode — it emerged from synthesis across all 16. That's the value of bottom-up extraction.

## Context Separation

Not all advice applies universally. Separate:

**Universal principles** — Always true regardless of context:
- "Side projects speak louder than professional work" (portfolio advice)
- "Good copy comes from customer language, not clever wordsmithing" (copywriting)

**Context-dependent guidance** — True only in specific situations:
- "Lead with craft" (true for visual designers targeting agencies, not for product designers targeting startups)
- "Use struggle-framed headlines" (depends on product category and audience sophistication)

For context-dependent guidance, always document WHEN it applies. Use tables or conditional statements:

```markdown
| Situation | Primary Approach | Why |
|-----------|-----------------|-----|
| Visual designer → agency | Lead with craft | Agencies hire on visual quality |
| Product designer → startup | Lead with impact | Startups hire on outcomes |
```

## Anti-Pattern Mining

Every "do this" implies a "don't do that." Extract anti-patterns explicitly:

- **Direct negatives** — The source says "never do X" → document it
- **Implied negatives** — The source recommends A over B → document why B fails
- **Common mistakes** — The source describes what most people get wrong → document the patterns

Anti-patterns are often more useful than positive principles because they prevent specific failure modes.

## Voice Preservation

The source material has a distinctive perspective. Preserve it:

- **Keep signature phrases** — If the author coined a term or has a memorable way of saying something, use it (with attribution)
- **Maintain the stance** — If the source is opinionated and direct, the skill should be too. Don't flatten a provocative framework into bland advice.
- **Cite, don't plagiarize** — Use quotes for distinctive phrases. Paraphrase for general ideas.

**Bad:** Flattening "Your copy either 'looks like food' instantly or gets ignored" into "Make sure your copy is immediately understandable."

**Good:** Keeping the "looks like food" metaphor because it's more memorable and actionable than the generic version.

## Compression Strategy

### How much to keep

| Source length | Target SKILL.md | Target references | Total |
|--------------|-----------------|-------------------|-------|
| < 1,000 words | Single file, ~100 lines | None | ~100 lines |
| 1,000-5,000 words | ~200 lines | 1-2 files | ~400 lines |
| 5,000-15,000 words | ~250 lines (router) | 3-5 files | ~800 lines |
| 15,000+ words | ~300 lines (router) | 5-8 files | ~1,200 lines |

### What to put where

| Content type | Location | Why |
|-------------|----------|-----|
| Core principles | SKILL.md inline | Always loaded, cannot be skipped |
| Decision frameworks | SKILL.md or reference | If universal → SKILL.md. If workflow-specific → reference |
| Detailed examples | References | Loaded only when doing that specific task |
| Anti-patterns | References (consolidated) | Loaded when relevant, not cluttering the main file |
| Operational checklists | Workflows | Used during execution, not loaded by default |

### Compression rules

1. **Cut explanation of obvious concepts** — Claude knows what a headline is
2. **Replace paragraphs with tables** — Comparison prose → comparison table
3. **Replace stories with principles** — Anecdote → the lesson it teaches
4. **Merge overlapping advice** — 3 slightly different ways of saying the same thing → 1 clear statement
5. **Keep code examples short** — Minimal working example, not full application
