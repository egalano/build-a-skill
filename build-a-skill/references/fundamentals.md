# Fundamentals

## What is a skill?

A skill is a folder containing:

- **`SKILL.md`** (required) - Instructions in Markdown with YAML frontmatter.
- **`scripts/`** (optional) - Executable code (Python, Bash, etc.).
- **`references/`** (optional) - Documentation loaded only as needed.
- **`assets/`** (optional) - Templates, fonts, icons, anything bundled into output.

Skills are how you teach Claude a workflow once and reuse it across every conversation, instead of re-explaining your process each time.

## Core design principles

### Progressive disclosure (the most important idea)

Skills use a three-level system:

1. **Frontmatter** - always loaded into Claude's system prompt. Just enough for Claude to decide whether the skill is relevant.
2. **SKILL.md body** - loaded when Claude decides the skill applies to the current task. Contains the full instructions.
3. **Linked files** (everything in `references/`, `scripts/`, `assets/`) - loaded only when Claude navigates to them.

This keeps token usage low while still letting you bundle deep expertise.

### Composability

Claude can have many skills loaded simultaneously. Yours should play nicely with others, not assume it's the only capability available.

### Portability

The same skill works on Claude.ai, Claude Code, and the API. Skill authors can note environment-specific requirements via the optional `compatibility` field.

## Skills + MCP (the kitchen analogy)

- **MCP** is the professional kitchen: tools, ingredients, equipment, connectivity.
- **Skills** are the recipes: step-by-step instructions for producing something valuable.

| MCP (Connectivity) | Skills (Knowledge) |
|---|---|
| Connects Claude to a service (Notion, Linear, Asana, ...) | Teaches Claude how to use that service effectively |
| Real-time data access, tool invocation | Workflows and best practices |
| What Claude *can* do | How Claude *should* do it |

If the user already has an MCP server, a skill is the workflow layer on top - it turns raw tool access into reliable, repeatable processes.

### Why this matters for MCP integrations

Without a skill, users connect the MCP and don't know what to do next. They get inconsistent results, file support tickets asking "how do I do X", and blame the connector when the gap is actually workflow guidance.

With a skill, the workflow activates automatically, results are consistent, and best practices are embedded in every interaction.
