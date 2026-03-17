# Quality Checklist

Run through this checklist after building or auditing a skill. Items are grouped by category and ordered by impact.

## Frontmatter

- [ ] Valid YAML between `---` delimiters
- [ ] `name` is lowercase-with-hyphens, matches directory name, max 64 chars
- [ ] `description` is specific, includes trigger keywords, max 1024 chars
- [ ] `description` says what it does AND when to use it
- [ ] `description` written in third person ("This skill..." or "Use when...")
- [ ] `disable-model-invocation: true` if skill has side effects (deploy, commit, send)
- [ ] `allowed-tools` set if specific tools are needed
- [ ] `argument-hint` set if skill accepts arguments

## Structure

- [ ] SKILL.md under 500 lines (target: 200-300 for router skills)
- [ ] All supporting files one level deep from SKILL.md
- [ ] No nested reference chains (SKILL.md → ref.md → detail.md)
- [ ] Every file referenced in SKILL.md actually exists
- [ ] Every file in the directory is referenced somewhere
- [ ] Reference files organized by domain concern, not arbitrary category
- [ ] File names are lowercase-with-hyphens and descriptive

## Content

- [ ] Standard markdown headings (`##`, `###`) — not XML tags
- [ ] Core principles stated upfront in SKILL.md (always loaded)
- [ ] Concrete examples present (not abstract descriptions)
- [ ] No explanation of concepts Claude already knows
- [ ] Consistent terminology throughout (not mixing synonyms)
- [ ] No time-sensitive information without versioning
- [ ] Default recommendation with escape hatch (not a menu of equivalent options)
- [ ] Anti-patterns documented (what NOT to do)

## Interactivity

- [ ] Discovery phase present (if skill adapts based on user context)
- [ ] Questions asked 2-3 at a time (not a wall of questions)
- [ ] Wait gates at genuine decision points (not after every step)
- [ ] Mode detection via $ARGUMENTS with fallback question
- [ ] Reference loading is conditional (not all files loaded every time)
- [ ] Shortcuts for when user has already provided context
- [ ] AskUserQuestion has plain-text fallbacks for non-Claude agents

## Invocation

- [ ] Invocable via `/skill-name` and produces correct behavior
- [ ] Auto-triggering works when description keywords match user input
- [ ] $ARGUMENTS are handled correctly (or appended if not referenced)
- [ ] Skill works when invoked with no arguments (handles gracefully)

## Writing Quality

- [ ] Imperative/instructional tone ("To do X, do Y" not "You should do X")
- [ ] Dense and operational (not explanatory prose)
- [ ] Tables used for comparisons, routing, and decision mapping
- [ ] Parenthetical design rationale where helpful (explains WHY)
- [ ] No filler, hedging, or unnecessary transitions

## Router-Specific (if applicable)

- [ ] Entry point detection logic covers all common inputs
- [ ] Routing table maps user signals to specific workflows
- [ ] Each workflow has `## Required Reading` header
- [ ] Each workflow has success criteria
- [ ] Reference index lists all files with when to load each
- [ ] Workflow index lists all workflows with one-line purpose
