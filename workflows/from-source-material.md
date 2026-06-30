# Workflow: From Source Material

Turn an article, talk, framework, or advice into a strategic, interactive skill.

## Required Reading

Read these reference files before proceeding:
1. `references/synthesis-patterns.md`
2. `references/architecture-decisions.md`
3. `references/evaluation-patterns.md`

---

## Step 1: Ingest Source Material

Detect what the user provided and ingest it:

- **URL** — Fetch with WebFetch, extract the content
- **File path** — Read the file (supports .md, .pdf, .txt, .html)
- **Pasted content** — Work with what's in the conversation

After reading, present a brief summary:

> Here's what I'm working with: **[title/topic]** — [1-2 sentence summary of what the source covers].
> Is this the right source? Anything I should know about your intent before I start extracting?

**Wait gate:** Confirm the source is correct before proceeding.

---

## Step 2: Bottom-Up Extraction

Extract strategy from the source material. Do NOT restructure it — distill it.

For each of these categories, identify what the source provides:

### Core Principles (3-7)
What are the fundamental ideas that make this approach work? Each should be actionable, non-obvious, and testable.

### Mental Models
What frameworks does this teach for thinking about the problem? Decision trees, taxonomies, spectrums, matrices, or process loops.

### Decision Heuristics
What rules-of-thumb does it provide? "If X, do Y. If Z, do A instead."

### Context Separation
Which advice always applies (universal) vs. which depends on the situation (context-dependent)?

### Gotchas & Anti-Patterns
What does it say NOT to do? What common mistakes does it call out? Where would Claude specifically be likely to fail when applying this knowledge? Structure each as "don't do X because Y."

### Success Criteria
How would you know you're doing this well? What does good output look like?

### Present Synthesis

Format as a structured summary:

```
SYNTHESIS: [Source Title]

Core Principles:
1. [Principle] — [One-line explanation]
2. [Principle] — [One-line explanation]
3. ...

Mental Model: [Brief description of the framework]

Key Heuristics:
- If [X], then [Y]
- If [Z], then [A]

Gotchas (where Claude will fail):
- Don't [X] because [Y]
- Don't [X] because [Y]

Context-Dependent (not universal):
- [Advice] applies when [condition]
```

> Here's what I extracted from the source. Does this capture the essence? Anything missing, wrong, or that I'm weighting too heavily?

**Wait gate:** Do not proceed until the user validates the synthesis. Adjust if they push back.

---

## Step 3: Skill Strategy

Interactive decisions about how the skill should work. Use AskUserQuestion where available, plain-text questions as fallback.

### Question 1: Rigor Lane

"What kind of skill are we making?"

If AskUserQuestion is available:
- **Quick personal** — Fast capture for one high-context user; concise `SKILL.md`; 1-3 smoke prompts; no README/publication by default
- **Team internal** — Shared skill; clear enough for teammates; eval-light pack; README optional
- **Public polished** — Publishable skill; README, eval pack, fresh-agent test, and compatibility review required

Otherwise ask: "Should this be quick personal, team internal, or public polished?"

### Question 2: Invocation

"What should trigger this skill?"

- **Manual only** — User invokes via /command (set `disable-model-invocation: true`)
- **Auto-detect** — Claude loads when relevant (description-driven)
- **Both** — User can invoke manually, Claude can also auto-detect (default)

### Question 3: Interactivity

"How interactive should this skill be?"

- **Guided** — Discovery phase, multiple questions, wait gates (like design-portfolio-assistant)
- **Direct** — User provides input, skill executes immediately
- **Adaptive** — Ask 1-2 clarifying questions, then execute (like language-market-fit)

### Question 4: Modes

"Does this skill need multiple modes?"

- **Yes** — [Ask user to describe the modes]
- **No** — Single workflow
- **Not sure** — "Based on the source material, I'd suggest [recommendation]. Does that feel right?"

### Question 5: Complexity

"How complex is the knowledge?" (determines file structure)

- **Simple** — Everything fits in one file (<200 lines of instruction)
- **Medium** — Needs 1-3 reference files for depth
- **Complex** — Multiple workflows + references + maybe templates

### Question 6: Target Agents

"Where will this skill be used?"

- **Claude Code only** — Personal/team use, can use all Claude Code features (AskUserQuestion, Agent tool, allowed-tools, etc.) without fallbacks
- **Claude Code primary, others welcome** (recommended for public skills) — Build for Claude Code but include plain-text fallbacks so other agents (Cursor, Codex, Kimi CLI, etc.) can use it
- **Universal** — Must work equally well across all agents. No Claude Code-specific features — use action-oriented language throughout, no AskUserQuestion, no Agent tool references

This answer shapes the build:
- **Claude Code only:** Use AskUserQuestion freely, reference specific tools, use `allowed-tools` and `context: fork` without concern
- **Claude Code primary:** Use AskUserQuestion with plain-text fallbacks, action-oriented language for instructions, `allowed-tools` is fine (ignored by other agents)
- **Universal:** Plain-text questions only, no tool-name references, no agent-specific features

For Quick Personal skills, ask this only when the user mentions multiple agents or publication. Otherwise default to Claude Code only and keep moving.

### Research Gate

If the skill involves external APIs, libraries, or domains where best practices shift fast:

> This involves [X]. Want me to research current best practices before building, or do you have enough context?

If research requested: spawn 2-3 parallel Sonnet agents with scoped questions (one per topic). Present findings before proceeding.

---

## Step 4: Evaluation Plan

Read `references/evaluation-patterns.md`, then create the right-sized test plan for the chosen lane.

### Quick Personal

Capture 1-3 smoke prompts. These can live in your working notes unless the user asks for an `evals.md`.

Include:
- 1 positive trigger prompt
- 1 representative task prompt
- Optional incomplete-context prompt

### Team Internal

Create or plan an `evals.md` with:
- 3 positive trigger prompts
- 1-2 negative/non-trigger prompts
- 2 representative task prompts
- 1 incomplete-context prompt
- Expected behavior bullets

### Public Polished

Create or plan an `evals.md` with:
- 5 positive trigger prompts
- 3 negative/non-trigger prompts
- 3 representative task prompts
- 2 edge-case or adversarial prompts
- 1 incomplete-context prompt
- Fresh-agent test checklist
- Expected behavior, success signals, and failure signals

Present:

```
EVALUATION PLAN: [skill-name]

Lane: [Quick Personal / Team Internal / Public Polished]
Artifacts: [SKILL.md only / evals.md / README / references]
Test prompts:
- [Prompt] -> [Expected behavior]
- ...
Fresh-agent test: [Required / Optional / Skipped]
```

For Team Internal and Public Polished skills, this is a wait gate. For Quick Personal skills, proceed after presenting the smoke prompts unless the user objects.

---

## Step 5: Architecture Proposal

Based on the strategy decisions, propose the exact file structure:

```
PROPOSED ARCHITECTURE: [skill-name]

Structure: [Simple / Simple + References / Router]
Rigor lane: [Quick Personal / Team Internal / Public Polished]

[skill-name]/
├── SKILL.md                    # [What goes here]
├── evals.md                    # [If Team Internal or Public Polished]
├── README.md                   # [If Public Polished or requested]
├── references/                 # [If needed]
│   ├── [file-a].md            # [What it contains]
│   └── [file-b].md            # [What it contains]
└── workflows/                  # [If router pattern]
    ├── [workflow-a].md         # [What it does]
    └── [workflow-b].md         # [What it does]

Rationale:
- [Why this structure vs. alternatives]
- [What goes in SKILL.md vs. references]
- [Why X reference files, not more/fewer]
- [How the lane affected artifact count and validation depth]
```

> Here's my proposed structure. Shall I proceed, or would you adjust anything?

**Wait gate:** Do not build until user approves the architecture.

---

## Step 6: Build

Create all files following the official spec. Order:
1. Reference files first (foundation)
2. Workflow files (if router pattern)
3. `evals.md` when required by lane, using `templates/eval-pack.md`
4. SKILL.md last (references everything else)

For each file:
- Follow the appropriate template from `templates/` as a starting point
- Adapt based on the synthesis from Step 2 and strategy from Step 3
- Use standard markdown headings
- Keep SKILL.md lean: under 150 lines for Quick Personal, 200 for simple shared skills, 300 for router skills
- Include concrete examples from the source material
- Keep research notes, rationale, and eval prompts out of SKILL.md unless needed at runtime

If decisions arise during build, use AskUserQuestion or ask in plain text. Don't guess on important choices.

---

## Step 7: Gotchas Review

Before validation, explicitly review the skill's gotchas content. This is the highest-signal content in any skill.

**Check:**
- Does the skill have a dedicated `## Gotchas` section (in SKILL.md or a reference file)?
- Are there at least 3-5 specific gotchas for non-trivial skills?
- Does each gotcha use "don't do X because Y" structure?
- Are gotchas based on real Claude failure modes (not theoretical warnings)?

**If gotchas are thin or missing**, think through:
1. What would Claude get wrong on first attempt applying this knowledge?
2. What are the most common misinterpretations of this domain?
3. What looks correct but is subtly wrong?
4. What does the source material explicitly warn against?

Add or strengthen gotchas before proceeding to validation.

---

## Step 8: Validate

Read `references/quality-checklist.md` and run through every item (note: gotchas are now the first section):

- Rigor lane selected and followed
- Eval depth matches lane
- Frontmatter valid and complete
- Description includes trigger keywords
- Line counts within limits
- All files cross-referenced
- Standard markdown headings
- Concrete examples present
- Interactivity patterns in place (if applicable)

Present the validation report:

> Skill created at [path]. Validation report:
> - [X] items pass
> - [Y] items need attention: [list them]

---

## Step 9: Eval & Smoke Test Review

Run or simulate the evaluation plan from Step 4.

### Quick Personal
- Run the 1-3 smoke prompts mentally or in the current agent session
- Fix obvious trigger, workflow, or verbosity problems
- Report any prompt you could not test

### Team Internal
- Check positive and negative trigger prompts
- Check representative task prompts against expected behavior
- Confirm `evals.md` records expected behavior clearly

### Public Polished
- Check the full eval pack
- Run at least one positive trigger, one representative task, and one non-trigger in the current environment
- Perform the fresh-agent test if available
- If a true fresh-agent test is unavailable, mark that limitation in the validation report and keep the checklist in `evals.md`

Present:

> Eval review:
> - [X] prompts pass
> - [Y] prompts need fixes
> - Fresh-agent test: [passed / unavailable / skipped by lane]

Fix any failed Public Polished eval before moving on.

---

## Step 10: Cross-Agent Compatibility Review

Skip this for Quick Personal skills unless the user requested multiple-agent support. For Team Internal and Public Polished skills, read `references/cross-agent-compatibility.md` and evaluate:

- Identify every Claude Code-specific feature used
- Check for plain-text fallbacks
- Verify tool references are action-oriented (not tool-name-specific)
- Assign degradation rating

Present:

> Cross-agent compatibility: **[Degrades Gracefully / Full / Claude Code-Dependent]**
> - [X] features are Claude Code-enhanced with fallbacks
> - [Y] features are universal
> - [Specific fixes needed, if any]

---

## Step 11: Generate README

Generate a README only when the lane calls for it:

- **Quick Personal:** skip unless requested
- **Team Internal:** ask whether a short README would help installation or handoff
- **Public Polished:** required

Create `README.md` alongside SKILL.md with:

1. **Skill name** as heading
2. **Install command** prominently displayed:
   ```
   npx skills add <owner/repo>
   ```
   Use a placeholder (`owner/repo-name`) if not yet published. Update later if the user publishes.
3. **What it does** — 2-3 sentences explaining the skill's purpose
4. **Who it's for** — target audience and common use cases
5. **Usage examples** — invocation commands (`/skill-name`, `/skill-name argument`)
6. **What's inside** — file tree showing SKILL.md + references
7. **Compatibility** — based on the target agent answer from Step 3:
   - Claude Code only: "Built for Claude Code."
   - Claude Code primary: "Built for Claude Code. Degrades gracefully on other agents (Cursor, Codex, Kimi CLI, etc.)."
   - Universal: "Works across all coding agents."
8. **License** — MIT unless user specifies otherwise

Keep the README concise — under 100 lines. It's a landing page, not documentation.

---

## Step 12: Independent Review (Optional)

For Public Polished skills, offer a fresh-eyes review from an independent sub-agent. For Team Internal skills, offer it when the skill is complex or high-impact. Skip by default for Quick Personal skills.

> Skill looks good from my end. Want me to get a second opinion? I can spawn an independent review agent that reads the skill cold — no context from our building conversation — and flags anything it catches.

If AskUserQuestion is available:
- **Yes, get a second opinion** — Spawn review agent for fresh-eyes feedback
- **No, ship it** — Skill is ready as-is

If the user opts in:

### Spawn the Review Agent

Read `references/independent-review-brief.md` for the full review instructions.

Spawn one sub-agent (Sonnet — fast and sharp enough for review work) with this brief:

> You are an independent reviewer evaluating a freshly built skill. You were NOT involved in building it — that's the point.
>
> **Skill to review:** [full path to skill directory]
>
> **Your instructions are in:** [path to skill-distillery]/references/independent-review-brief.md — read that first.
>
> **Also read these skill-distillery references for evaluation criteria:**
> - references/synthesis-patterns.md
> - references/interactive-design-patterns.md
> - references/architecture-decisions.md
>
> Then read the entire skill (all files) and evaluate it according to the review brief. Return structured findings.

### Filter and Present

When the review agent returns findings, **don't just pass them through**. Act as a filter:

For each finding, give your honest assessment:

```
REVIEW FINDING: [Title]
Severity: [Critical / Important / Nit]
Reviewer says: [Their observation]
My take: [AGREE / DISAGREE / PARTIALLY AGREE] — [Brief rationale from your building context]
```

Be honest. If the reviewer caught something you missed, say so. If you disagree, explain why — you have context the reviewer doesn't. The user benefits from seeing both perspectives.

After presenting all findings with your assessments:

> These are the reviewer's findings with my take on each. Which ones would you like me to fix?

If AskUserQuestion is available, present fixable items as selectable options. Otherwise ask for numbers.

Implement selected fixes, then confirm what changed.

---

## Step 13: Publication (Optional)

For Public Polished skills, offer publication for `npx skills` installation. Skip this step for Quick Personal and Team Internal skills unless the user asks to publish.

> Want to publish this skill to GitHub so anyone can install it with `npx skills add`?

If AskUserQuestion is available:
- **Yes, create a GitHub repo** — Create a public repo, push the skill files, and update the README with the real install command
- **No, keep it local** — Skill stays in the local skills directory

If the user opts in:

1. Create a public GitHub repo via `gh repo create <skill-name> --public`
2. Copy all skill files (SKILL.md, references/, README.md) into a temp directory
3. Initialize git, commit, and push
4. Update the README.md install command from placeholder to the real `owner/repo`
5. Present the repo URL and install command:

> Published at: https://github.com/<owner>/<repo>
>
> Anyone can install with:
> ```
> npx skills add <owner>/<repo>
> ```
>
> It will auto-appear on skills.sh once people start installing it.

---

## Success Criteria

This workflow is complete when:
- [ ] Source material read and understood
- [ ] Synthesis validated by user
- [ ] Rigor lane selected
- [ ] Strategy decisions made interactively
- [ ] Evaluation plan matches the lane
- [ ] Architecture proposed and approved
- [ ] All files created following official spec
- [ ] Gotchas section reviewed and populated with real failure modes
- [ ] Quality checklist passed
- [ ] Eval or smoke test review completed at the chosen depth
- [ ] Cross-agent compatibility reviewed when lane/target agents require it
- [ ] README.md generated when lane requires it
- [ ] Independent review offered when lane requires or warrants it
- [ ] Publication offered only for public/publishable skills
- [ ] User can invoke the skill and it works as designed
