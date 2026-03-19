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
| `hooks` | — | Hooks scoped to this skill's lifecycle. See Hooks in Skills below. |

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
| `${CLAUDE_SKILL_DIR}` | Directory containing the skill's SKILL.md. Use to reference bundled scripts/files regardless of working directory. |
| `${CLAUDE_PLUGIN_DATA}` | Stable folder for data that persists beyond skill upgrades. Use for logs, config, cached results, or any data the skill accumulates over time. |

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

## Hooks in Skills

Skills can define lifecycle hooks via the `hooks` frontmatter field. These run shell commands at specific points during skill execution.

**Use cases:**
- **Safety guardrails** — Block destructive commands (`rm -rf`, `DROP TABLE`, `force-push`) while the skill is active
- **Directory constraints** — Restrict edits to specific paths during the skill's execution
- **Logging** — Track skill invocations for usage measurement
- **Setup/teardown** — Run setup scripts when the skill starts, cleanup when it ends

Hooks defined in a skill are scoped to that skill's lifecycle only — they don't affect the rest of the session.

## Persistent Data Storage

Skills that need to store data across invocations can use `${CLAUDE_PLUGIN_DATA}` — a stable folder that persists beyond skill upgrades. Store:
- Text logs (gotchas captured during usage, run history)
- JSON config (user preferences, cached API responses)
- SQLite databases (for structured data accumulation)

This is distinct from the skill directory itself, which may be overwritten on updates.

## Writing Effective Descriptions

**Frame descriptions as trigger conditions, not summaries.** Claude scans descriptions to decide when to invoke a skill. Write them as "when should Claude reach for this?" not "what does this skill contain?"

```yaml
# Good — trigger-oriented, tells Claude WHEN to activate
description: Extract text and tables from PDF files, fill forms, merge documents. Use when working with PDF files or when the user mentions PDFs, forms, or document extraction.

# Bad — summary that doesn't tell Claude when to activate
description: Helps with documents

# Bad — describes contents instead of triggers
description: A comprehensive guide to PDF manipulation with examples and best practices
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
