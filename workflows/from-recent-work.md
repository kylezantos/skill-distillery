# Workflow: From Recent Work

Capture a process you just did with AI as a reusable skill. Turn "I figured this out" into "anyone can do this."

## Required Reading

Read these reference files before proceeding:
1. `references/synthesis-patterns.md`
2. `references/architecture-decisions.md`

---

## Step 1: Understand What Happened

Figure out what the user just did and what they learned.

### Gather Context

Ask 2-3 questions:

1. **"What did you just do? What was the task?"**
   - What problem were you solving?
   - What was the end result?

2. **"What did you figure out along the way?"**
   - Any non-obvious steps or gotchas?
   - Decisions you had to make?
   - Things that didn't work before you found what did?

3. **"Do you want me to review our recent conversation for context?"**
   - If conversation history is available, scan it for the process
   - If git history is relevant, offer to check recent commits

### Source Material Options

Depending on what's available:

- **Conversation history** — Review recent messages for the process, decisions, and lessons learned
- **Git history** — Scan recent commits for what changed and why
- **User description** — Work from what they tell you
- **Combination** — Use all available sources

---

## Step 2: Extract the Reusable Process

From whatever context you gathered, extract:

### Repeatable Workflow
What steps would apply next time someone faces this problem? Order matters — what has to happen first?

### Decision Points
What choices did you face? What did you decide and why? These become the skill's interactive decision points.

### Pitfalls & Gotchas
What went wrong before it went right? What would you warn someone about?

### Essential Tools & Commands
What specific tools, libraries, commands, or APIs were critical?

### Present Extraction

```
CAPTURED PROCESS: [Task Name]

Workflow (repeatable steps):
1. [Step] — [Why this step matters]
2. [Step] — [Decision point: if X, do Y; if Z, do A]
3. [Step] — [Gotcha: watch out for...]

Key Decisions:
- [Decision] → [What we chose and why]

Pitfalls:
- [Thing that went wrong] → [How to avoid it]

Tools Used:
- [Tool/library] for [purpose]
```

> Here's what I captured from the process. Does this cover the important parts? Anything I'm missing or got wrong?

**Wait gate:** Confirm extraction accuracy before proceeding.

---

## Step 3-9: Convergent Process

From here, follow the same process as the source-material workflow:

**Step 3: Skill Strategy** — Interactive decisions about audience, invocation, interactivity, modes, complexity. See `workflows/from-source-material.md` Step 3.

**Step 4: Architecture Proposal** — Propose exact file structure with rationale. Wait for approval. See `workflows/from-source-material.md` Step 4.

**Step 5: Build** — Create all files following official spec. See `workflows/from-source-material.md` Step 5.

**Step 6: Gotchas Review** — Ensure gotchas section is populated with real Claude failure modes. See `workflows/from-source-material.md` Step 6.

**Step 7: Validate** — Run quality checklist (gotchas first). See `workflows/from-source-material.md` Step 7.

**Step 8: Cross-Agent Compatibility Review** — Evaluate cross-agent support. See `workflows/from-source-material.md` Step 8.

**Step 9: Independent Review (Optional)** — Offer fresh-eyes review from an independent sub-agent. See `workflows/from-source-material.md` Step 9.

---

## Success Criteria

This workflow is complete when:
- [ ] Process understood from conversation history, git history, or user description
- [ ] Reusable workflow extracted with decision points and pitfalls
- [ ] Extraction validated by user
- [ ] Strategy decisions made interactively
- [ ] Architecture proposed and approved
- [ ] All files created following official spec
- [ ] Gotchas section reviewed and populated
- [ ] Quality checklist passed
- [ ] Cross-agent compatibility reviewed
- [ ] Independent review offered (and completed if accepted)
