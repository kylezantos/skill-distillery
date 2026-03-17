# Workflow: From Description

User describes what they want the skill to accomplish. Brainstorm if vague, propose directly if detailed.

## Required Reading

Read these reference files before proceeding:
1. `references/interactive-design-patterns.md`
2. `references/architecture-decisions.md`

---

## Step 1: Listen & Assess

Read what the user provided. Determine which path to take:

### Path A: Detailed Description

The user gave a clear, specific description of what the skill should do. Signs:
- Named specific functionality
- Described who it's for
- Mentioned trigger conditions or use cases
- Referenced similar tools or approaches

**Action:** Skip heavy brainstorming. Go directly to proposal:

> That's clear. To accomplish that, I'd structure it this way:
>
> **Name:** [suggested-name]
> **Core function:** [what it does]
> **Entry points:** [how users would invoke it]
> **Complexity:** [Simple / Medium / Complex]
>
> Sound right, or should I dig deeper on anything?

If user confirms → proceed to Step 3 (Strategy).

### Path B: Vague or Exploratory

The user has an idea but hasn't fleshed it out. Signs:
- "I want something that helps with..."
- "Can you make a skill for..."
- General domain without specific functionality
- No mention of who it's for or when to use it

**Action:** Brainstorm together. Ask 2-3 questions:

Use AskUserQuestion if available, plain text otherwise:

1. **"What problem does this skill solve?"**
   - What's the frustration or gap today?
   - What triggers someone to need this?

2. **"What does success look like after using it?"**
   - What's different? What did they produce?
   - How would they know it worked?

3. **"Can you give an example of how you'd use it?"**
   - "I'd type /skill-name and then..."
   - Concrete scenario, not abstract description

### Decision Gate

After initial questions:

> Ready to proceed with building, or would you like to explore more?

If AskUserQuestion is available:
- **Proceed** — I have enough context to propose an approach
- **Explore more** — There are more details to clarify
- **Let me add context** — I want to provide additional information

---

## Step 2: Research (if needed)

Follow the Research Protocol from SKILL.md:

- If the skill involves external APIs/libraries, trigger the research gate
- Ask: "This involves [X]. Want me to research current best practices, or do you have enough context?"
- If yes: spawn 2-3 parallel Sonnet agents with scoped questions (Context7 for library docs, WebSearch for patterns)
- Feed findings back into the proposal — don't dump into skill files
- If the user already knows the domain well: skip entirely

---

## Step 3-7: Convergent Process

From here, follow the same process as the source-material workflow:

**Step 3: Skill Strategy** — Interactive decisions about audience, invocation, interactivity, modes, complexity. See `workflows/from-source-material.md` Step 3.

**Step 4: Architecture Proposal** — Propose exact file structure with rationale. Wait for approval. See `workflows/from-source-material.md` Step 4.

**Step 5: Build** — Create all files following official spec. See `workflows/from-source-material.md` Step 5.

**Step 6: Validate** — Run quality checklist. See `workflows/from-source-material.md` Step 6.

**Step 7: Cross-Agent Compatibility Review** — Evaluate cross-agent support. See `workflows/from-source-material.md` Step 7.

---

## Success Criteria

This workflow is complete when:
- [ ] User's intent clearly understood (via detailed description or brainstorming)
- [ ] Research done if external dependencies involved
- [ ] Strategy decisions made interactively
- [ ] Architecture proposed and approved
- [ ] All files created following official spec
- [ ] Quality checklist passed
- [ ] Cross-agent compatibility reviewed
