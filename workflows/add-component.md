# Workflow: Add Component

Add a reference file, workflow, script, or template to an existing skill.

## Required Reading

Read this reference file before proceeding:
1. `references/architecture-decisions.md`

---

## Step 1: Understand the Existing Skill

Read the target skill:
- Glob for all files in the skill directory
- Read SKILL.md to understand structure and current architecture
- Note existing references, workflows, templates, scripts

Present:

> Current structure of [skill-name]:
> [Directory tree]
>
> What would you like to add?

If AskUserQuestion is available:
- **Reference file** — Domain knowledge that workflows can load
- **Workflow** — Step-by-step procedure for a new user intent
- **Script** — Executable code for deterministic tasks
- **Template** — Output structure for consistent results

Otherwise ask: "What type of component? (reference, workflow, script, or template)"

---

## Step 2: Design the Component

Based on the component type:

### Reference File
- What domain knowledge does it contain?
- Which workflows will load it?
- Is it organized by domain concern?
- Does it overlap with existing references? (merge instead of creating new)

### Workflow
- What user intent does it serve?
- Which references does it need?
- Does it share principles with existing workflows?
- Does SKILL.md's routing table need updating?

### Script
- What task does it automate?
- What are the inputs/outputs?
- What error handling is needed?

### Template
- What output structure does it define?
- What parts are customizable vs. fixed?

Propose the component:

> I'd add `[type]/[filename].md` with:
> - [What it contains]
> - [Which existing files reference it]
> - [Changes needed to SKILL.md]
>
> Does this fit, or should I adjust?

**Wait gate:** Confirm before creating.

---

## Step 3: Build

1. Create the new file following the appropriate template pattern
2. Update SKILL.md:
   - Add to reference/workflow index
   - Update routing table (if new workflow)
   - Add to any relevant loading conditions
3. Update any workflows that should reference the new component

---

## Step 4: Validate

Quick check:
- [ ] New file is linked from SKILL.md
- [ ] File name is lowercase-with-hyphens
- [ ] Content is one level deep from SKILL.md
- [ ] SKILL.md still under 500 lines after updates
- [ ] No duplicate content with existing files

---

## Success Criteria

- [ ] Component type and content confirmed with user
- [ ] File created in correct location
- [ ] SKILL.md updated with references and routing
- [ ] Existing workflows updated if needed
- [ ] Validation passed
