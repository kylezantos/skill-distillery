# Router Skill Template

Use this template for multi-workflow skills with shared principles. Copy and customize.

## SKILL.md Template

```markdown
---
name: {{SKILL_NAME}}
description: "{{What it does across all modes}}. Use when {{trigger conditions}}. Triggers on: {{keyword1}}, {{keyword2}}, {{keyword3}}, {{keyword4}}."
argument-hint: "[{{expected input}}]"
allowed-tools: Read, Write, Edit, Glob, Grep, Bash
---

# {{Skill Display Name}}

{{1-3 sentence description. What is this skill's core insight or philosophy?}}

## Quick Start

**{{Workflow A}}:**
> "{{example invocation}}"

**{{Workflow B}}:**
> "{{example invocation}}"

**{{Workflow C}}:**
> "{{example invocation}}"

---

## Core Principles

These apply to ALL workflows:

1. **{{Principle 1}}** — {{Brief explanation}}
2. **{{Principle 2}}** — {{Brief explanation}}
3. **{{Principle 3}}** — {{Brief explanation}}
4. **{{Principle 4}}** — {{Brief explanation}}
5. **{{Principle 5}}** — {{Brief explanation}}

---

## Entry Point Detection

Detect what the user provided and route accordingly:

- {{Signal A}} → `workflows/{{workflow-a}}.md`
- {{Signal B}} → `workflows/{{workflow-b}}.md`
- {{Signal C}} → `workflows/{{workflow-c}}.md`
- Ambiguous → Ask for clarification

| User Input | Route To |
|-----------|----------|
| {{keyword}}, {{keyword}} | `workflows/{{workflow-a}}.md` |
| {{keyword}}, {{keyword}} | `workflows/{{workflow-b}}.md` |
| {{keyword}}, {{keyword}} | `workflows/{{workflow-c}}.md` |
| Other | Clarify, then route |

**After selecting a workflow, read it and follow it exactly.**

---

## Reference Index

All domain knowledge in `references/`:

| File | Contents | Load When |
|------|----------|-----------|
| `{{reference-a}}.md` | {{What it contains}} | {{When to load it}} |
| `{{reference-b}}.md` | {{What it contains}} | {{When to load it}} |
| `{{reference-c}}.md` | {{What it contains}} | Always |

## Workflow Index

| Workflow | Purpose |
|----------|---------|
| `{{workflow-a}}.md` | {{One-line description}} |
| `{{workflow-b}}.md` | {{One-line description}} |
| `{{workflow-c}}.md` | {{One-line description}} |
```

## Workflow Template

```markdown
# Workflow: {{Workflow Name}}

## Required Reading

Read these reference files before proceeding:
1. `references/{{relevant-file}}.md`
2. `references/{{another-file}}.md`

## Step 1: {{First Action}}

{{What to do. Be specific — actual implementation steps, not "read the references."}}

## Step 2: {{Second Action}}

{{What to do.}}

If AskUserQuestion is available, present decision as tappable options.
Otherwise, ask: "{{plain-text version of the question}}"

## Step 3: {{Third Action}}

{{What to do.}}

**Wait gate:** Present results and ask before proceeding.

## Step 4: {{Validate}}

{{How to verify the output is correct.}}

## Success Criteria

This workflow is complete when:
- [ ] {{Criterion 1}}
- [ ] {{Criterion 2}}
- [ ] {{Criterion 3}}
```

## Reference Template

```markdown
# {{Reference Topic}}

{{Brief overview — what this reference covers and when to use it.}}

## {{Section 1}}

{{Dense, operational content. Tables, decision trees, examples.}}

| Option | When to Use | Trade-off |
|--------|------------|-----------|
| {{A}} | {{scenario}} | {{trade-off}} |
| {{B}} | {{scenario}} | {{trade-off}} |

## {{Section 2}}

{{More operational content.}}

## Anti-Patterns

- **{{Common mistake}}** — {{Why it fails and what to do instead}}
- **{{Another mistake}}** — {{Why it fails and what to do instead}}
```

## Customization Notes

- **SKILL.md target:** 200-300 lines. Push detail into workflows and references.
- **Workflows:** Each should be self-contained with Required Reading + Steps + Success Criteria
- **References:** Organized by domain concern, not by document type
- **Wait gates:** At discovery completion and before building/implementing
- **AskUserQuestion:** Always include plain-text fallback for cross-agent compatibility
