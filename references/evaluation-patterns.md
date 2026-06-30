# Evaluation Patterns

Use evaluation depth to match the skill's audience. Investigation can be thorough without making the runtime skill verbose: keep research notes, rationale, and test packs outside `SKILL.md` unless the agent needs them while executing.

## Rigor Lanes

### Quick Personal
For one-person, high-context skills where speed matters more than polish.

Default shape:
- One concise `SKILL.md`
- No README unless requested
- No publication prep
- 1-3 smoke prompts captured in notes or a short `evals.md`
- Minimal references only when they prevent real runtime mistakes

Good enough when:
- Trigger description is clear
- Main workflow is executable
- Known gotchas are captured
- A fresh prompt can invoke the intended behavior once

### Team Internal
For shared workflows where other people or agents need predictable behavior.

Default shape:
- `SKILL.md` plus references for longer procedures
- Short usage notes or README when the skill will be installed by others
- Eval-light pack
- Cross-agent notes when Claude/Codex/Gemini behavior differs

Minimum eval pack:
- 3 positive trigger prompts
- 1-2 negative/non-trigger prompts
- 2 representative task prompts
- 1 incomplete-context prompt
- Expected behavior bullets for each prompt

### Public Polished
For published, reusable skills that need to earn trust without private context.

Default shape:
- Concise `SKILL.md`
- Progressive disclosure references
- README required
- Eval pack required
- Fresh-agent install/use test required
- Cross-agent compatibility review required when the skill claims broad support

Minimum eval pack:
- 5 positive trigger prompts
- 3 negative/non-trigger prompts
- 3 representative task prompts
- 2 edge-case or adversarial prompts
- 1 incomplete-context prompt
- Fresh-agent test script or checklist
- Expected behavior, success signals, and failure signals for each prompt

## Eval Pack Format

Create `evals.md` for Team Internal and Public Polished skills unless the user asks for another format.

Recommended sections:

```markdown
# Skill Evals

## Positive Triggers
- Prompt:
  Expected:

## Non-Triggers
- Prompt:
  Expected:

## Representative Tasks
- Prompt:
  Expected:
  Success signals:

## Edge Cases
- Prompt:
  Expected:
  Failure signals:

## Fresh-Agent Test
- Install/load steps:
- Prompt:
- Expected:
```

## Writing Good Evals

Prefer prompts that resemble how a real user would ask. Include messy phrasing, missing context, and adjacent-but-wrong requests. The point is not to prove the skill can answer one golden path; it is to prove the trigger, workflow, and boundaries survive normal use.

Strong evals test:
- Whether the skill activates when it should
- Whether it stays quiet when it should not activate
- Whether the workflow handles missing inputs
- Whether artifacts are scoped to the requested audience
- Whether references are loaded only when needed
- Whether the output stays compact enough for runtime use

## Fresh-Agent Test

For Public Polished skills, validate from a clean perspective:

1. Install or copy the skill into a temporary skill directory.
2. Start a fresh agent session with no prior conversation context.
3. Run one positive trigger prompt.
4. Run one representative task prompt.
5. Run one non-trigger prompt.
6. Confirm the agent loads only the necessary files and follows the intended workflow.
7. Record failures in `evals.md` or the README before publishing.

If a true fresh-agent run is unavailable, create the checklist and run the prompts in the current environment, marking the limitation honestly.
