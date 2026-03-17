# Workflow: Audit Existing Skill

Run an existing skill through the full skill-distillery framework. Five evaluation lenses, rated assessment, prioritized improvements.

## Required Reading

Read ALL reference files before proceeding:
1. `references/official-spec.md`
2. `references/synthesis-patterns.md`
3. `references/architecture-decisions.md`
4. `references/interactive-design-patterns.md`
5. `references/quality-checklist.md`
6. `references/cross-agent-compatibility.md`

---

## Step 1: Ingest the Skill

### Locate and Read

- If user provides a path to a skill directory → read everything in it
- If user provides a SKILL.md path → read it and glob for siblings
- If user provides a skill name → search `~/.claude/skills/`, `.claude/skills/`, and plugin caches

Glob for all files in the skill directory:
```
[skill-name]/
├── SKILL.md
├── **/*.md
├── **/*.py
└── **/*
```

Read SKILL.md and every supporting file (references, workflows, templates, scripts).

### Present Inventory

```
SKILL INVENTORY: [skill-name]

Files: [count]
Total lines: [count]
Structure: [Simple / Simple + References / Router]

[Directory tree showing all files]
```

### Understand Context

Ask: "What's the context for this audit?"

If AskUserQuestion is available:
- **I built this** — Give me improvement suggestions I can implement
- **Plugin/marketplace skill** — Tell me what I'd change if it were mine
- **Evaluating before installing** — Help me decide if it's worth using

Otherwise ask: "Did you build this, install it from a plugin, or are you evaluating it?"

This calibrates the audit tone: your own skill gets constructive suggestions, an external skill gets an honest assessment.

**Wait gate:** Confirm context before proceeding with evaluation.

---

## Step 2: Synthesis Quality Review

Evaluate the skill's intellectual foundation.

**Questions to assess:**
- Are core principles clearly stated upfront in SKILL.md?
- Is there a coherent mental model, or is it just a list of instructions?
- Does it separate universal principles from context-dependent advice?
- Is the source material's distinctive perspective preserved, or was it flattened?
- Are anti-patterns documented?
- Would someone understand WHY the skill works, not just HOW to use it?

**Rating:**
- **Strong** — Clear principles, coherent mental model, distinctive perspective
- **Adequate** — Has some principles but could be more structured
- **Needs work** — Just instructions without underlying framework

---

## Step 3: Architecture Review

Evaluate the file structure.

**Questions to assess:**
- Is the file structure right-sized? (too many tiny files? everything in one file?)
- Simple vs. router: is the choice appropriate for the skill's complexity?
- Are references organized by domain concern (not arbitrary category)?
- Is progressive disclosure working? (SKILL.md lean, references loaded conditionally?)
- Are there files that should be merged? Split? Moved?
- Does every file serve a clear purpose?

**Rating:**
- **Well-architected** — Right-sized, organized by concern, progressive disclosure works
- **Functional** — Works but could be better organized
- **Needs restructuring** — Over-engineered, under-structured, or poorly organized

---

## Step 4: Interactive Design Review

Evaluate how the skill handles user interaction.

**Questions to assess:**
- Does the skill have a discovery phase? (should it, given its complexity?)
- Are there wait gates at critical decision points?
- Does it support multiple modes? (should it?)
- How does it handle $ARGUMENTS? (detection + fallback?)
- Does it use AskUserQuestion? (or should it?)
- Does it provide shortcuts when context is already available?
- Is it transparent about confidence levels?
- Does it ask 2-3 questions at a time (not a wall)?

**Rating:**
- **Highly interactive** — Discovery, modes, wait gates, conditional loading
- **Moderately interactive** — Some interactivity but could be more responsive
- **Static** — No adaptation to user context
- **Needs interactivity** — Would significantly benefit from interactive patterns

---

## Step 5: Spec Compliance Review

Evaluate against the official specification.

**Checklist:**
- [ ] Valid YAML frontmatter between `---` delimiters
- [ ] `name` is lowercase-with-hyphens, matches directory
- [ ] `description` specific with trigger keywords, under 1024 chars
- [ ] `disable-model-invocation: true` if skill has side effects
- [ ] SKILL.md under 500 lines
- [ ] Standard markdown headings (not XML tags)
- [ ] All referenced files exist and are properly linked
- [ ] References one level deep (no nested chains)
- [ ] Concrete examples present (not abstract)
- [ ] Consistent terminology

**Rating:**
- **Fully compliant** — All checklist items pass
- **Minor issues** — 1-3 items need fixes (cosmetic or low-impact)
- **Needs fixes** — 4+ items fail or critical issues present

---

## Step 6: Cross-Agent Compatibility Review

Evaluate portability across AI coding agents.

**Checklist:**
- [ ] Claude Code-specific features identified
- [ ] AskUserQuestion calls have plain-text fallbacks
- [ ] Agent/subagent references describe tasks inline
- [ ] Instructions use action-oriented language (not tool names)
- [ ] Frontmatter-only features are safe to ignore
- [ ] Core workflow functions without Claude Code features

**Rating:**
- **Full compatibility** — Works identically across agents
- **Degrades gracefully** — Core works everywhere, enhanced on Claude Code
- **Claude Code-dependent** — Won't function on other agents

---

## Step 7: Present Full Assessment

Format the results:

```
SKILL AUDIT: [skill-name]

Files: [count] | Lines: [total] | Structure: [type]

┌─────────────────────────────┬────────────────────┐
│ Lens                        │ Rating             │
├─────────────────────────────┼────────────────────┤
│ Synthesis Quality           │ [rating]           │
│ Architecture                │ [rating]           │
│ Interactive Design          │ [rating]           │
│ Spec Compliance             │ [rating]           │
│ Cross-Agent Compatibility   │ [rating]           │
└─────────────────────────────┴────────────────────┘

PRIORITY IMPROVEMENTS:

1. [Critical] [Title]
   What: [what's wrong]
   Why: [why it matters]
   Fix: [what the fix looks like]

2. [Important] [Title]
   What: [what's wrong]
   Why: [why it matters]
   Fix: [what the fix looks like]

3. [Opportunity] [Title]
   What: [could be better]
   Why: [the benefit]
   Fix: [what to change]
```

**Wait gate:** Present the full assessment. Ask before implementing anything.

---

## Step 8: Implement Improvements

Present improvements as a selectable list.

If AskUserQuestion is available (multi-select):
- Each improvement as an option with description
- User taps which ones to implement

Otherwise: "Which improvements would you like me to implement? List the numbers."

### For each selected improvement:
1. Read the relevant file(s)
2. Make the changes
3. Re-run the relevant review step to verify the fix

### If restructuring is needed:
- Propose the new structure first
- Wait for approval
- Then implement (create new files, move content, update references)

---

## Step 9: Final Report

After implementing selected improvements:

```
AUDIT COMPLETE: [skill-name]

Changes Made:
- [Improvement 1] — [what changed]
- [Improvement 2] — [what changed]

Rating Changes:
│ Lens                    │ Before    │ After     │
│ [lens]                  │ [rating]  │ [rating]  │

Remaining Recommendations:
- [Anything not implemented that's worth doing later]
```

---

## Success Criteria

This workflow is complete when:
- [ ] All skill files read and inventoried
- [ ] Audit context understood (user built it, plugin, evaluating)
- [ ] All 5 lenses evaluated with ratings
- [ ] Prioritized improvements presented
- [ ] User selected which improvements to implement
- [ ] Selected improvements implemented and verified
- [ ] Before/after comparison presented
