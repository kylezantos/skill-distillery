# Skill Evals Template

Use for Team Internal and Public Polished skills. Delete sections that exceed the chosen lane.

```markdown
# Skill Evals

Skill: {{SKILL_NAME}}
Lane: {{Quick Personal / Team Internal / Public Polished}}

## Positive Triggers

- Prompt: "{{Prompt that should activate the skill}}"
  Expected: {{What the agent should do}}

- Prompt: "{{Prompt that should activate the skill}}"
  Expected: {{What the agent should do}}

## Non-Triggers

- Prompt: "{{Adjacent request that should not activate the skill}}"
  Expected: {{What should happen instead}}

## Representative Tasks

- Prompt: "{{Realistic task}}"
  Expected: {{Workflow or output shape}}
  Success signals:
  - {{Signal}}
  - {{Signal}}

## Incomplete Context

- Prompt: "{{Request missing an important input}}"
  Expected: {{Question, fallback, or safe behavior}}

## Edge Cases

Use for Public Polished skills.

- Prompt: "{{Messy, adversarial, or ambiguous task}}"
  Expected: {{Correct boundary or behavior}}
  Failure signals:
  - {{Signal}}
  - {{Signal}}

## Fresh-Agent Test

Use for Public Polished skills.

- Install/load steps:
- Positive trigger result:
- Representative task result:
- Non-trigger result:
- Limitations:
```
