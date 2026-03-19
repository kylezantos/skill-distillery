# Simple Skill Template

Use this template for single-workflow skills under 200 lines. Copy and customize.

```markdown
---
name: {{SKILL_NAME}}
description: "{{What it does}}. Use when {{trigger conditions}}. Triggers on: {{keyword1}}, {{keyword2}}, {{keyword3}}."
argument-hint: "[{{expected input}}]"
allowed-tools: Read, Bash
---

# {{Skill Display Name}}

{{1-2 sentence philosophy or core insight. What mental model does this skill operate from?}}

## Quick Start

**Example invocations:**
> "{{example user input 1}}"
> "{{example user input 2}}"

---

## How This Skill Thinks

{{3-5 core principles or mental filters. These should be operational — things you actually check while doing the work, not abstract values.}}

1. **{{Principle 1}}** — {{Brief explanation}}
2. **{{Principle 2}}** — {{Brief explanation}}
3. **{{Principle 3}}** — {{Brief explanation}}

---

## Choose Your Mode

{{If the skill has modes, present them. If not, delete this section.}}

If the user specifies a mode, go directly to it. If not, ask:

> I can help two ways:
> 1. **{{Mode A}}** — {{Description}}
> 2. **{{Mode B}}** — {{Description}}
>
> Which one?

---

## Mode 1: {{Mode A Name}}

### Phase 1: {{Understanding / Discovery}}

{{Ask 2-3 questions to understand context. Don't proceed without context.}}

### Phase 2: {{Generate / Execute}}

{{Core workflow steps.}}

### Phase 3: {{Review / Stress-Test}}

{{Validate output. Present to user for feedback.}}

---

## Mode 2: {{Mode B Name}}

### Phase 1: {{Intake}}

{{Mode-specific intake.}}

### Phase 2: {{Diagnose / Analyze}}

{{Core workflow steps.}}

### Phase 3: {{Prioritize / Deliver}}

{{Structured output.}}

---

## Gotchas

Common failure points — where Claude gets this wrong:

- **Don't {{X}}** because {{Y}}. Instead, {{correct approach}}.
- **Don't {{X}}** because {{Y}}. Instead, {{correct approach}}.
- **Don't {{X}}** because {{Y}}. Instead, {{correct approach}}.

---

## Working With Incomplete Information

{{How the skill adapts when user doesn't have everything.}}

For the complete {{reference material name}}, see [reference.md](reference.md).
```

## Customization Notes

- **Delete unused sections** — If no modes needed, remove the mode selection and collapse into a single workflow
- **Add wait gates** at discovery and architecture decision points
- **Keep under 200 lines** — If it grows beyond that, consider the router template instead
- **Description** should be under 1024 chars with specific trigger keywords
