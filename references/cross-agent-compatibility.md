# Cross-Agent Compatibility

Guide for ensuring skills work across multiple AI coding agents. Skills published on skills.sh see installs from Claude Code, Codex, OpenCode, Gemini CLI, Cursor, GitHub Copilot, and Kimi CLI.

## What Works Everywhere

These features are universally supported or gracefully ignored:

| Feature | Support | Notes |
|---------|---------|-------|
| YAML frontmatter (`name`, `description`) | Universal | All agents parse these |
| Standard markdown body | Universal | Headings, lists, tables, code blocks |
| `$ARGUMENTS` | Widely supported | Most agents pass arguments to skills |
| Reference files linked from SKILL.md | Universal | All agents can read linked files |
| Directory structure (workflows/, references/) | Universal | Standard file system |

## Claude Code-Specific Features

These only work in Claude Code. Each needs a fallback strategy:

### `AskUserQuestion`

**What it does:** Presents tappable multiple-choice options in the terminal.

**Fallback:** Add a plain-text question nearby. Other agents will use the text version.

```markdown
## Step 3: Choose Approach

If AskUserQuestion is available, present as tappable options:
- "Guided" — Full discovery phase with multiple questions
- "Direct" — Execute immediately from provided input
- "Adaptive" — Light clarification, then execute

Otherwise, ask: "How interactive should this skill be?
(1) Guided with discovery, (2) Direct execution, (3) Adaptive"
```

### `Agent` Tool / Subagent Spawning

**What it does:** Spawns parallel research agents with specific models.

**Fallback:** Describe the research task inline. Other agents will do it sequentially.

```markdown
## Research Protocol

In Claude Code: Spawn 2-3 parallel Sonnet agents with scoped questions.

In other agents: Perform the following research sequentially:
1. Search for current [library] documentation
2. Find 2026 best practices for [topic]
3. Check for deprecated patterns
```

### `context: fork`

**What it does:** Runs the skill in an isolated subagent context.

**Fallback:** None — this is a Claude Code execution model feature. Safe to include in frontmatter (other agents ignore unknown fields).

### `allowed-tools`

**What it does:** Grants automatic tool permissions without user prompts.

**Fallback:** Other agents ignore this field (permissive by default or use their own permission model). Safe to include.

### Dynamic Context Injection (`` !`command` ``)

**What it does:** Runs shell commands at load time, injects output into skill content.

**Fallback:** Provide equivalent instructions to run the command manually.

```markdown
## Context
- Current branch: !`git branch --show-current`

If dynamic injection is not supported, run: `git branch --show-current`
```

### `disable-model-invocation` / `user-invocable`

**What it does:** Controls whether the agent can auto-load the skill.

**Fallback:** Other agents ignore these fields. Safe to include — worst case, the skill is always available.

### `model` Field

**What it does:** Forces a specific model (haiku, sonnet, opus).

**Fallback:** Other agents ignore this. The skill runs on whatever model is configured.

## Writing Portable Instructions

### Use action-oriented language, not tool-specific language

```markdown
# Bad — Claude Code specific
Use the Read tool to read the file at path/to/file.md.
Then use the Glob tool to find all *.md files.

# Good — portable
Read the file at path/to/file.md.
Find all *.md files in the directory.
```

### Reference tools by intent, not by name

```markdown
# Bad
Use WebFetch to get the URL content.
Use Grep to search for the pattern.

# Good
Fetch the content at [URL].
Search the codebase for [pattern].
```

### Exception: `allowed-tools` frontmatter

Tool names in `allowed-tools` are fine — this field is Claude Code-specific anyway and other agents ignore it.

## Graceful Degradation Patterns

### Pattern: Conditional Feature

```markdown
If AskUserQuestion is available, use it for the following decision.
Otherwise, ask the question in plain text and wait for a response.
```

### Pattern: Research Agent with Sequential Fallback

```markdown
## Research Phase

**Parallel approach (Claude Code):** Spawn research agents for each topic.
**Sequential approach (other agents):** Research each topic in order:
1. [Topic A] — search for [specific query]
2. [Topic B] — fetch docs for [specific library]
```

### Pattern: Progressive Enhancement

Structure the skill so the core workflow works everywhere, with Claude Code features as enhancements:

```
Core workflow (universal):
  Discovery questions → Synthesis → Build → Validate

Enhanced with Claude Code:
  AskUserQuestion for discovery → Agent-based research → Build → Validate
```

## Degradation Rating System

After reviewing a skill, rate its cross-agent compatibility:

### Full Compatibility
Works identically across all agents. No Claude Code-specific features, or all features have equivalent behavior everywhere.

### Degrades Gracefully
Core functionality works on all agents. Claude Code users get enhanced experience (tappable options, parallel research, auto-permissions). Other agents get plain-text fallbacks that achieve the same outcome.

**This is the target for most skills.** Full compatibility sacrifices too much; Claude Code-dependent limits your audience.

### Claude Code-Dependent
The skill relies on Claude Code-specific features with no fallback. Won't function meaningfully on other agents.

**Acceptable for:** Personal skills, team-internal skills, or skills that fundamentally require Claude Code features (subagent orchestration, isolated execution).

## Review Checklist

- [ ] All AskUserQuestion calls have plain-text fallback questions nearby
- [ ] All Agent/subagent references describe the task inline for sequential execution
- [ ] Instructions use action-oriented language ("read the file") not tool names ("use Read tool")
- [ ] Frontmatter-only features (`allowed-tools`, `context`, `model`) are safe to ignore
- [ ] Dynamic context injection has manual command fallbacks
- [ ] Core workflow functions without any Claude Code-specific features
- [ ] Degradation rating assigned: Full / Graceful / Dependent
