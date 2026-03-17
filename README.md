# Skill Distillery

An agent skill that turns raw material — articles, talks, frameworks, recent AI work, or just an idea — into strategic, interactive skills. Bottom-up synthesis, not content reformatting.

## What it does

Skill-distillery is a skill that creates skills. It works from four entry points:

**From source material** (`/skill-distillery https://example.com/article`) — Give it a URL, file, or pasted content. It reads the source, extracts principles and mental models bottom-up, then collaboratively designs the skill architecture with you.

**From a description** (`/skill-distillery`) — Describe what you want. If your description is detailed, it proposes an approach directly. If it's vague, it brainstorms with you until the idea is clear.

**From recent work** (`/skill-distillery capture`) — Just did something with AI and want to save the process? It extracts the repeatable workflow, decision points, and gotchas, then packages them as a reusable skill.

**Audit an existing skill** (`/skill-distillery audit ~/.claude/skills/my-skill`) — Runs any skill through a five-lens evaluation framework: synthesis quality, architecture, interactive design, spec compliance, and cross-agent compatibility. Produces a rated assessment with prioritized improvements.

## The approach

Most skill creators assume you already know what you want to build. Skill-distillery starts earlier — from raw material — and works bottom-up.

A 5,000-word article doesn't become a skill by restructuring paragraphs into SKILL.md format. It becomes a skill when you extract the 5 principles that make the approach work, the mental model that organizes the domain, and the decision heuristics someone needs while doing the work. Then you design an interactive architecture around those extracted elements.

The workflow is collaborative: extract → validate → strategize → propose architecture → build. Every phase has a checkpoint where you confirm before proceeding. The skill suggests interactive patterns from proven skills (discovery phases, mode detection, conditional loading, wait gates) as opinionated defaults you can override.

## Best experience on Claude Code

Skill-distillery works with any agent that supports skills, but it's designed to shine on **Claude Code** thanks to:

- **AskUserQuestion** — Tappable multiple-choice options for strategy decisions
- **Parallel research agents** — Spawns Sonnet agents for fast, scoped research when needed
- **Agent tool** — Launches specialized subagents for deep research

On other agents, these features degrade gracefully — AskUserQuestion becomes a plain-text question, parallel research becomes sequential, and the core workflow works the same way.

## Install

### One command (all agents)

```bash
npx skills add kylezantos/skill-distillery
```

Auto-detects your installed agents and installs to each. Works with Claude Code, Codex, OpenCode, Cursor, Gemini CLI, Windsurf, and [35+ more](https://github.com/vercel-labs/skills).

### Target specific agents

```bash
npx skills add kylezantos/skill-distillery -a claude-code
npx skills add kylezantos/skill-distillery -a codex -a opencode
```

### Manual install

Copy the entire `skill-distillery/` directory into your agent's skills path:

| Agent | Path |
|-------|------|
| Claude Code | `~/.claude/skills/skill-distillery/` |
| Codex | `~/.codex/skills/skill-distillery/` |
| OpenCode | `~/.config/opencode/skills/skill-distillery/` |
| Cursor | `~/.cursor/skills/skill-distillery/` |
| Gemini CLI | `~/.gemini/skills/skill-distillery/` |
| Windsurf | `~/.codeium/windsurf/skills/skill-distillery/` |

Or clone:

```bash
git clone https://github.com/kylezantos/skill-distillery.git ~/.claude/skills/skill-distillery
```

## Usage

```
/skill-distillery https://example.com/great-article
/skill-distillery ~/Documents/talk-transcript.md
/skill-distillery capture
/skill-distillery audit ~/.claude/skills/my-skill
/skill-distillery add reference
/skill-distillery
```

## What's inside

```
skill-distillery/
├── SKILL.md                              # Router + principles + research protocol
├── workflows/
│   ├── from-source-material.md           # Article → skill (flagship)
│   ├── from-description.md               # Idea → skill
│   ├── from-recent-work.md               # Process → skill
│   ├── audit-existing-skill.md           # 5-lens audit framework
│   └── add-component.md                  # Extend existing skills
├── references/
│   ├── official-spec.md                  # 2026 skill specification
│   ├── synthesis-patterns.md             # Bottom-up extraction methodology
│   ├── architecture-decisions.md         # When to use simple vs. router
│   ├── interactive-design-patterns.md    # Discovery, modes, wait gates
│   ├── quality-checklist.md              # Validation checklist
│   └── cross-agent-compatibility.md      # Cross-agent portability guide
└── templates/
    ├── simple-skill.md                   # Single-file template
    └── router-skill.md                   # Multi-file template
```

## Attribution

Built by [Kyle Zantos](https://x.com/KyleZantos). Synthesizes knowledge from [Anthropic's official skill specification](https://code.claude.com/docs/en/skills), the [create-agent-skills](https://github.com/everyinc/compound-engineering-plugin) and [skill-creator](https://github.com/everyinc/compound-engineering-plugin) skills from Compound Engineering, and interactive design patterns from several other skills we've created and iterated on.
