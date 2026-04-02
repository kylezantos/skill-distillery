# Interactive Design Patterns

Patterns for making skills interactive, strategic, and responsive to user context. Extracted from high-performing skills (design-portfolio-assistant, language-market-fit, soleio-design-hiring).

## Pattern 1: Discovery-First Intake

Start every complex skill with discovery questions before doing anything. Prevents generic advice.

**How it works:**
1. Ask 2-3 questions at a time (not a wall of questions)
2. Present your understanding in a structured summary
3. Wait for user confirmation before proceeding
4. Adapt subsequent questions based on what you've learned

**Implementation:**

```markdown
## Step 1: Discovery

Ask 2-3 questions at a time, covering:
- **Identity** — Who is the user, what's their context?
- **Goal** — What are they trying to accomplish?
- **Current state** — What exists already?

### Present Understanding

```
DISCOVERY COMPLETE

User profile: [summary]
Goal: [what they want]
Current state: [what exists]

Does this feel right? Should I adjust before proceeding?
```

### Wait for Confirmation
**STOP and wait.** Do not proceed until the user confirms or adjusts.
```

**Why it matters:** The same skill gives different advice to different users. A junior designer gets different portfolio advice than a senior design engineer. Discovery makes the skill context-aware.

## Pattern 2: Mode Detection

Support multiple modes of operation with automatic detection and manual fallback.

**Three detection strategies:**

### Via $ARGUMENTS parsing
```markdown
Accepts `$ARGUMENTS` for direct mode selection:
- Contains "audit", "review" → Audit mode
- Contains "compose", "write", "create" → Compose mode
- Ambiguous → Ask: "Would you like me to audit existing copy or compose new copy?"
```

### Via explicit question
```markdown
## Choose Your Mode

> I can help two ways:
> 1. **Compose** — Write new copy together
> 2. **Audit** — Tear apart existing copy and rebuild what's broken
>
> Which one? And what's the product/page?
```

### Via AskUserQuestion (tappable)
```markdown
Use AskUserQuestion with options:
- "Compose" — Write new [thing] from scratch
- "Audit" — Evaluate and improve existing [thing]
```

**Best practice:** Try $ARGUMENTS detection first, fall back to asking.

## Pattern 3: Conditional Reference Loading

Don't load all references for every invocation. Load based on user's context.

**Implementation:**

```markdown
## Step 2: Load Relevant Knowledge

Based on confirmed context:
- **Read `references/craft-purists.md`** if craft quality is primary concern
- **Read `references/hiring-managers.md`** if job search is the goal
- **Always read `references/contradictions.md`** — prevents conflicting advice

**Mode-specific:**
- **Read `audit-checklist.md`** — Audit mode only
- **Read `references/storytelling.md`** — Plan mode only
```

**Why it matters:** Loading everything wastes context window. A user auditing their portfolio doesn't need the project discovery questionnaire. A user planning doesn't need the audit checklist.

## Pattern 4: Wait Gates

Explicit stops that prevent the skill from charging ahead without user alignment.

**When to use:**
- After presenting a synthesis or understanding
- After proposing an architecture or approach
- Before implementing changes
- Before any destructive or irreversible action

**Implementation:**

```markdown
**Wait gate:** Present assessment and ask before implementing anything.

STOP. Do not proceed until the user:
1. Confirms the assessment is accurate
2. Selects which improvements to implement
3. Or redirects to a different approach
```

**Common mistake:** Adding so many wait gates that the skill feels bureaucratic. Use them at genuine decision points, not after every step.

## Pattern 5: AskUserQuestion Patterns

When `AskUserQuestion` is available, use it for tappable multiple-choice decisions.

### When to use AskUserQuestion
- Binary or small-set decisions (2-4 options)
- When options have clear labels and descriptions
- When tapping is faster than typing

### When NOT to use AskUserQuestion
- Open-ended questions ("What problem does this solve?")
- When you need detailed context (use plain text conversation)
- When there are more than 4 distinct options

### Effective option design
```markdown
Use AskUserQuestion:
Question: "How interactive should this skill be?"
Options:
1. "Guided" — Discovery phase, multiple questions, wait gates
2. "Direct" — User provides input, skill executes immediately
3. "Adaptive" — Ask 1-2 clarifying questions, then execute
```

**Each option needs:**
- A short label (1-3 words)
- A description that explains the implication (what happens if you choose this)

### Fallback for non-Claude agents
Always include a plain-text version nearby:

```markdown
If AskUserQuestion is available, present as tappable options. Otherwise, ask:
"How interactive should this skill be? (1) Guided with discovery, (2) Direct execution, (3) Adaptive with light clarification"
```

## Pattern 6: Shortcut Detection

Skip questions the user has already answered.

**Implementation:**

```markdown
### Shortcut: Provided Context

Before asking questions, check if the user provided:
- **URL or file path** — Read it to extract context
- **Detailed description** — Parse for answers to standard questions
- **Mode selection** — Via $ARGUMENTS

If provided, skip redundant questions. Present extracted understanding for
confirmation instead of asking from scratch.
```

**Example from dive-club:** If user provides a resume file, the skill reads it and skips the "What's your role? What's your career stage?" questions — presents extracted understanding for confirmation instead.

## Pattern 7: Stress-Testing

Before finalizing output, challenge your own work from the user's perspective.

**Implementation (from language-market-fit):**

```markdown
### Phase 4: Stress-Test Together

Don't just present final output — walk the user through your stress-test:

> **Playing skeptical [audience] for a moment:**
> - [Quote a specific part] — A [user type] seeing this would think: "___." Does that match your intent?
> - [Quote another part] — This could mean [X] or [Y]. Which interpretation do we want?
> - The biggest risk with this version is ___. Here's how I'd mitigate that: ___.
```

**When to use:** After generating output that the user will present to others (copy, portfolios, plans, documentation).

## Pattern 8: Transparency

Be explicit about what's grounded in evidence vs. what's inferred.

**Implementation (from language-market-fit):**

```markdown
For each recommendation:
- **Grounded** — Based on actual data/source material provided
- **Inferred** — Best inference without direct evidence

Flag: "I'm working from inference here because [reason]. The fastest way to get
real signal would be [specific suggestion]."
```

**When to use:**
- Source material is incomplete
- Recommendations go beyond what the data supports
- User needs to know confidence levels to make decisions

## Combining Patterns

Most effective skills use 3-5 of these patterns together:

| Skill type | Recommended patterns |
|-----------|---------------------|
| Audit/review skill | Discovery + Conditional Loading + Wait Gates + Stress-Testing |
| Creative/generative | Mode Detection + Discovery + Transparency + Stress-Testing |
| Guided workflow | Discovery + AskUserQuestion + Wait Gates + Shortcut Detection |
| Knowledge/reference | Conditional Loading + Shortcut Detection |
