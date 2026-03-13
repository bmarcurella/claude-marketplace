---
name: example-skill
description: >
  This skill should be used when the user asks to "demonstrate skills", "show skill format",
  or "use the example skill". This is a template — copy this folder and update the name,
  description, and content to create a new skill.
version: 1.0.0
---

# Example Skill

Replace this content with your skill's knowledge and instructions. Claude loads this skill
automatically when the user's request matches the `description` trigger phrases above.

## When This Skill Applies

Skills are **model-invoked** — Claude decides when to use them based on the description.
Unlike commands (user-invoked), skills activate automatically based on context.

Describe your skill's trigger conditions clearly in the frontmatter `description` field.

## What This Skill Does

1. Provide domain knowledge Claude needs to complete the task
2. Define workflows or step-by-step processes
3. Reference external documentation or files in `references/`

## Reference Files

Place supporting reference files in a `references/` subdirectory next to this SKILL.md.
Reference them in your skill content like: `See references/my-doc.md for details.`
