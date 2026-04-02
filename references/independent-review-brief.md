# Independent Review Brief

Instructions for a review sub-agent evaluating a freshly built skill. This agent has NOT been involved in the building process — that's the point. Fresh eyes catch what the builder rationalizes past.

## Setup

The orchestrator spawns you as an independent reviewer. You receive:
1. The path to the skill directory
2. Access to skill-distillery's reference files

## Your Job

Read the skill cold. Then evaluate it from these angles — each targets a specific blind spot that builder agents commonly have.

## Review Angles

### 1. Distillation Integrity

**Builder blind spot:** The builder spent time with the source material and thinks they distilled it. Often they just reorganized it.

**What to check:**
- Read the core principles. Are they genuinely extracted strategy, or reworded summaries of the source?
- Test: Could you explain each principle WITHOUT referencing the source material? If not, it's still a summary, not a distillation.
- Are principles actionable and testable, or are they descriptive statements?
- Is there a coherent mental model, or just a list of good ideas?

**Red flags:**
- Principles that sound like section headers from the source
- "Do good X" statements with no operational definition
- 7+ principles (likely under-consolidated)

### 2. Cold Start Test

**Builder blind spot:** The builder has full context from the conversation. They unconsciously assume the skill reader will too.

**What to check:**
- Read only SKILL.md. Could you invoke this skill and know exactly what to do without reading the builder's conversation?
- Are there implicit assumptions that only make sense if you watched the skill get built?
- Does the description contain enough trigger keywords that Claude would auto-load it in the right situations?
- If the skill has modes: is it obvious from the description which mode to pick?

**Red flags:**
- References to context that doesn't exist in the skill files ("as discussed", implicit audience assumptions)
- Description that's too generic to trigger correctly
- Missing instructions for the no-arguments case

### 3. Gotchas Authenticity

**Builder blind spot:** Builders generate plausible-sounding gotchas to check a box. Real gotchas come from knowing where Claude actually fails.

**What to check:**
- For each gotcha: Is this a REAL Claude failure mode for this specific domain, or a generic warning?
- Test: Has Claude actually demonstrated this failure? Would it naturally make this mistake without the warning?
- Are gotchas specific enough to be checkable? ("Don't do X because Y" not "Be careful with X")
- Are there obvious Claude failure modes for this domain that AREN'T listed?

**Red flags:**
- Gotchas that apply to any skill ("don't make assumptions")
- Warnings that describe human mistakes, not Claude mistakes
- Fewer than 3 gotchas for a non-trivial skill

### 4. Architecture Fit

**Builder blind spot:** Builders sometimes over-engineer during the build (adding files that weren't in the plan) or under-split (cramming too much into SKILL.md to avoid creating files).

**What to check:**
- Does the file count match the knowledge diversity? (Many files = many distinct knowledge domains, not just "it's complex")
- Are there reference files under 30 lines that should be merged?
- Is SKILL.md over 300 lines with content that should be in references?
- Does every file get loaded by at least one workflow? (No orphans)
- Is progressive disclosure actually working — or does every invocation load everything?

**Red flags:**
- Reference files that are only read by one workflow and are short (inline them)
- SKILL.md over 350 lines
- Files organized by document type (quotes.md, examples.md) instead of domain concern

### 5. Interactivity Quality

**Builder blind spot:** Builders focus on content quality and treat interactivity as an afterthought. Or they add interactivity mechanically without considering whether it improves the experience.

**What to check:**
- Do interactive patterns serve the user, or do they just slow things down?
- Are there wait gates at genuine decision points (not just "does this look right?" after every step)?
- If AskUserQuestion is used: are options meaningfully different, with clear labels and implications?
- Are there shortcuts for when the user already provided context via $ARGUMENTS?
- Would a user enjoy interacting with this, or would it feel bureaucratic?

**Red flags:**
- Wait gates after every step (over-cautious)
- No wait gates at all for a complex skill (under-cautious)
- AskUserQuestion options that are vague or overlap
- No shortcut detection (always asks full question battery even when context was provided)

### 6. Action Test

**Builder blind spot:** Skills that inform without changing behavior. The builder thinks "this is useful knowledge" when the real question is "does this change what the agent does?"

**What to check:**
- If you removed this skill entirely, would the agent's behavior visibly change?
- Does the skill produce different outputs for different user contexts, or is it one-size-fits-all?
- Are instructions operational (do X, check Y, if Z then A) or descriptive (X is important, Y matters)?

**Red flags:**
- Long explanatory sections with no operational instructions
- Principles stated as beliefs rather than checks
- No conditional logic based on user context

## Output Format

Return your findings as a structured list. For each finding:

```
FINDING: [Short title]
Angle: [Which review angle — 1-6]
Severity: [Critical / Important / Nit]
What I found: [Specific observation with file:line references]
Suggested fix: [Concrete recommendation]
```

**Severity guide:**
- **Critical** — Skill won't work correctly or will mislead users
- **Important** — Skill works but misses an opportunity or has a notable weakness
- **Nit** — Polish item, take it or leave it

Only report findings you're genuinely confident about. If something seems fine, don't force a critique. An empty findings list is a valid outcome.

## What NOT to Review

- Spec compliance (frontmatter, file naming) — the builder already ran the quality checklist for this
- Cross-agent compatibility — already reviewed in a separate step
- Writing style/formatting — unless it actively hurts usability
