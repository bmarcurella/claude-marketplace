---
description: A template slash command — copy and rename this file to create your own commands.
argument-hint: "[optional-argument]"
---

# Example Command

Replace this content with your command's instructions. Claude will follow these instructions
when the user invokes `/example-command [optional-argument]`.

The argument value (if provided) is available as `$ARGUMENTS` anywhere in this file.

User requested: $ARGUMENTS

## What to do

1. Step one — describe what Claude should do first
2. Step two — describe the next action
3. Step three — describe how to finish and respond

## Tips

- Commands are user-invoked via `/command-name` in the chat
- The filename (without .md) becomes the command name
- Use `argument-hint` to show users what arguments are accepted
- Reference skills with the Skill tool if needed
