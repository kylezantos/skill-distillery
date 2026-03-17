# Architecture Decisions

How to choose the right file structure for a skill. The goal is right-sized architecture — not too simple, not over-engineered.

## Simple vs. Router: Decision Tree

```
Does the skill have multiple distinct user intents?
├── No → Does the content exceed 200 lines?
│   ├── No → SIMPLE (single SKILL.md)
│   └── Yes → SIMPLE with references (SKILL.md + 1-3 reference files)
└── Yes → Does each intent share core principles?
    ├── No → Consider separate skills instead of one complex skill
    └── Yes → ROUTER (SKILL.md + workflows/ + references/)
```

**"Distinct user intents"** means fundamentally different tasks:
- Create vs. Audit vs. Improve → distinct intents (router)
- Build a React app vs. Build a Vue app → same intent, different context (simple with references)

## When to Add Each File Type

### `references/` — Domain Knowledge

Add when:
- Multiple workflows need the same knowledge
- Content is too detailed for SKILL.md (> 50 lines of dense material)
- Information is loaded conditionally (not every invocation needs it)
- Knowledge changes independently from procedures

Don't add when:
- Only one workflow uses it (inline it in that workflow)
- It's short enough to live in SKILL.md (< 30 lines)
- It duplicates what's already in the workflow

**Name by domain concern**, not by category:
- `craft-purists.md` not `reference-1.md`
- `supabase-rls-patterns.md` not `database-docs.md`

### `workflows/` — Step-by-Step Procedures

Add when:
- The skill has 2+ distinct user intents that share principles
- Each workflow needs different reference files loaded
- Procedures are sequential (Step 1 → Step 2 → Step 3)

Don't add when:
- There's only one workflow (put it directly in SKILL.md)
- The "workflow" is just "read this reference" (that's not a workflow)

Each workflow should:
- Start with `## Required Reading` listing which references to load
- Contain actual implementation steps (not just "read references")
- End with success criteria
- Be self-contained enough to follow without re-reading SKILL.md

### `templates/` — Output Structures

Add when:
- The skill produces consistent output structures (plans, specs, reports, skill files)
- Structure matters more than creative generation
- Users need a starting point they can customize

Don't add when:
- Output varies too much between invocations
- The template is simple enough to describe inline

### `scripts/` — Executable Code

Add when:
- Same code is rewritten every invocation
- Deterministic reliability is needed
- Operations are error-prone when generated fresh each time

Don't add when:
- The code is simple enough to generate inline
- The code needs to adapt to context every time
- You're the only user (scripts add distribution complexity)

## File Count Guidance

| Skill complexity | Typical file count | Structure |
|-----------------|-------------------|-----------|
| Single task, simple | 1 | SKILL.md only |
| Single task, deep knowledge | 2-3 | SKILL.md + 1-2 references |
| Multi-task, shared principles | 5-10 | SKILL.md + 2-4 workflows + 2-4 references |
| Domain expertise | 10-20 | SKILL.md + 4-6 workflows + 5-10 references |

**Signs you have too many files:**
- Reference files under 30 lines (merge them)
- Workflows that share 80%+ content (consolidate)
- Files that are only read once by one workflow (inline)

**Signs you have too few files:**
- SKILL.md over 400 lines (split into references)
- Workflows loading the same big block of knowledge (extract to reference)
- Distinct user intents handled by conditional logic instead of separate workflows

## Reference Organization

Organize by **domain concern**, not by content type:

**Good — organized by what the user needs:**
```
references/
├── craft-purists.md          # What craft-focused advisors say
├── hiring-managers.md        # What hiring managers look for
├── storytelling-narrative.md # How to structure project stories
└── contradictions.md         # When advisors disagree
```

**Bad — organized by document type:**
```
references/
├── quotes.md
├── checklists.md
├── examples.md
└── principles.md
```

## Architecture Comparison: Kyle's Skills

| Skill | Structure | Files | Why it works |
|-------|-----------|-------|-------------|
| language-market-fit | Simple + 1 ref | 2 | Two modes but shared principles; reference holds operational tools (kill list, interview framework) |
| dive-club-portfolio-advisor | Router-like | 12 | 4 advisory lenses loaded conditionally; 2 modes with shared discovery phase |
| visual-taste | Complex router | 15+ | Multiple designers, nested examples, feedback loops |
| soleio-design-hiring | Simple | 2 | Two modes but simple enough for one file |

**Key insight from this comparison:** File count should match knowledge diversity, not skill complexity. dive-club has many files because it has 4 distinct knowledge bases (lenses). language-market-fit is complex but all the knowledge is one domain, so 2 files suffice.

## SKILL.md Content Rules

SKILL.md is always loaded. Use this guarantee strategically.

**Put in SKILL.md (unavoidable):**
- Core principles (3-7 that apply to ALL workflows)
- Entry point detection / routing logic
- Research protocol (when and how to research)
- Reference index (what files exist and when to load each)

**Keep out of SKILL.md (loaded on demand):**
- Detailed procedures (→ workflows)
- Deep domain knowledge (→ references)
- Templates and boilerplate (→ templates)
- Executable code (→ scripts)

**Target length:** 200-300 lines for router skills, under 200 for simple skills.
