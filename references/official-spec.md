# Official Skill Specification (2026)

Source: [code.claude.com/docs/en/skills](https://code.claude.com/docs/en/skills)

## Commands and Skills Are Merged

Custom slash commands and skills are now the same thing. `.claude/commands/review.md` and `.claude/skills/review/SKILL.md` both create `/review`. If both exist, the skill takes precedence.

## SKILL.md Structure

YAML frontmatter + standard markdown body:

```markdown
---
name: my-skill
description: What it does and when to use it
---

# My Skill

## Quick Start
[Immediate actionable guidance]

## Instructions
[Step-by-step procedures]

## Examples
[Concrete usage examples]
```

## Frontmatter Reference

All fields are optional. Only `description` is recommended.

| Field | Default | Description |
|-------|---------|-------------|
| `name` | directory name | Lowercase letters, numbers, hyphens. Max 64 chars. |
| `description` | — | What it does AND when to use it. Max 1024 chars. Claude uses this for auto-discovery. |
| `argument-hint` | — | Autocomplete hint. Example: `[issue-number]` |
| `disable-model-invocation` | `false` | Prevents Claude from auto-loading. For manual workflows with side effects. |
| `user-invocable` | `true` | Set `false` to hide from `/` menu. For background knowledge. |
| `allowed-tools` | — | Tools Claude can use without permission prompts. Example: `Read, Bash(git *)` |
| `model` | inherited | Force a specific model: `haiku`, `sonnet`, `opus` |
| `context` | — | Set `fork` to run in isolated subagent context. |
| `agent` | — | Subagent type when `context: fork`: `Explore`, `Plan`, `general-purpose`, or custom agent name. |

## Invocation Control

| Frontmatter | User invokes | Claude invokes | When loaded |
|-------------|-------------|----------------|-------------|
| (default) | Yes | Yes | Description always in context; full content on invocation |
| `disable-model-invocation: true` | Yes | No | Description NOT in context; loads only when user invokes |
| `user-invocable: false` | No | Yes | Description always in context; loads when relevant |

**Use `disable-model-invocation: true`** for workflows with side effects: deploy, commit, triage, send messages. Prevents Claude from deciding to deploy because your code looks ready.

**Use `user-invocable: false`** for background knowledge: coding conventions, domain context, legacy system docs.

## Skill Locations & Priority

```
Enterprise (highest) → Personal → Project → Plugin (lowest)
```

| Type | Path |
|------|------|
| Personal | `~/.claude/skills/<name>/SKILL.md` |
| Project | `.claude/skills/<name>/SKILL.md` |
| Plugin | `<plugin>/skills/<name>/SKILL.md` (namespaced as `plugin:skill`) |

## String Substitutions

| Variable | Description |
|----------|-------------|
| `$ARGUMENTS` | All arguments passed when invoking |
| `$ARGUMENTS[N]` or `$N` | Specific argument by 0-based index |
| `${CLAUDE_SESSION_ID}` | Current session ID |

If `$ARGUMENTS` is not present in the skill content, arguments are appended automatically.

## Dynamic Context Injection

The `` !`command` `` syntax runs shell commands before content reaches Claude:

```markdown
## Context
- Current branch: !`git branch --show-current`
- Changed files: !`gh pr diff --name-only`
```

Commands execute at load time. Claude only sees the output.

## Progressive Disclosure

Three-level loading system:

1. **Metadata** (name + description) — Always in context (~100 words, ~2% context budget)
2. **SKILL.md body** — When skill triggers (<500 lines)
3. **Supporting files** — As needed by Claude (unlimited)

```
my-skill/
├── SKILL.md           # Entry point (required, under 500 lines)
├── reference.md       # Detailed docs (loaded when needed)
├── examples.md        # Usage examples (loaded when needed)
└── scripts/
    └── helper.py      # Utility script (executed, not loaded)
```

Link from SKILL.md: `For API details, see [reference.md](reference.md).`

**Keep references one level deep.** Claude may partially read files referenced from other referenced files.

## Subagent Execution

Add `context: fork` to run in isolation:

```yaml
---
name: deep-research
context: fork
agent: Explore
---
Research $ARGUMENTS thoroughly...
```

The skill content becomes the subagent's prompt. No access to conversation history.

## Writing Effective Descriptions

Write in third person. Include both what it does and when to use it:

```yaml
# Good — specific with trigger keywords
description: Extract text and tables from PDF files, fill forms, merge documents. Use when working with PDF files or when the user mentions PDFs, forms, or document extraction.

# Bad — too vague
description: Helps with documents
```

## Key Rules

- SKILL.md under 500 lines — split detailed content into reference files
- Standard markdown headings — `##`, `###` (not XML tags)
- Concrete examples — not abstract descriptions
- Consistent terminology throughout
- No time-sensitive information (use "current method" / "legacy" pattern)
- Default to one recommended approach with escape hatch, not a menu of options
- `disable-model-invocation: true` for any workflow with side effects
- Test with real usage scenarios, not test cases

## Distribution

- **Project skills**: Commit `.claude/skills/` to version control
- **Plugins**: Add `skills/` directory to plugin
- **Enterprise**: Deploy via managed settings
- **Marketplace**: Package and publish to skills.sh
