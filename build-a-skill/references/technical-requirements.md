# Technical requirements

## File structure

```
your-skill-name/
├── SKILL.md              # Required - main skill file
├── scripts/              # Optional - executable code
│   ├── process_data.py
│   └── validate.sh
├── references/           # Optional - documentation loaded on demand
│   ├── api-guide.md
│   └── examples/
└── assets/               # Optional - templates, fonts, icons
    └── report-template.md
```

## Critical rules

### `SKILL.md` naming

- Must be exactly `SKILL.md` (case-sensitive). No `SKILL.MD`, no `skill.md`.

### Skill folder naming

- Use kebab-case: `notion-project-setup` ✅
- No spaces: `Notion Project Setup` ❌
- No underscores: `notion_project_setup` ❌
- No capitals: `NotionProjectSetup` ❌

### No `README.md` inside the skill folder

All documentation belongs in `SKILL.md` or under `references/`. A repo-level README *outside* the skill folder is fine and recommended when you distribute on GitHub.

## YAML frontmatter

This is the single most important part. It's what Claude reads to decide whether to load your skill.

### Minimal required format

```yaml
---
name: your-skill-name
description: What it does. Use when user asks to [specific phrases].
---
```

### Field requirements

**`name`** (required)
- kebab-case only
- No spaces, no capitals, no underscores
- Should match the folder name

**`description`** (required)
- MUST include BOTH what the skill does AND when to use it (trigger conditions)
- Under 1024 characters
- No XML tags (no `<` or `>`)
- Mention specific phrases users might say
- Mention file types if relevant

**`license`** (optional)
- Use if open-sourcing. Common values: `MIT`, `Apache-2.0`.

**`compatibility`** (optional)
- 1-500 characters
- Indicates environment requirements (intended product, required system packages, network access, etc.)

**`metadata`** (optional)
- Any custom key-value pairs you want
- Common suggestions: `author`, `version`, `mcp-server`, `category`, `tags`

```yaml
metadata:
  author: ProjectHub
  version: 1.0.0
  mcp-server: projecthub
```

**`allowed-tools`** (optional)
- Restrict which tools the skill can invoke, e.g. `"Bash(python:*) Bash(npm:*) WebFetch"`

## Security restrictions

### Forbidden in frontmatter

- XML angle brackets `<` `>` (frontmatter goes into Claude's system prompt - angle brackets risk prompt injection)
- Skill names containing `claude` or `anthropic` (reserved)

### Allowed

- Any standard YAML type (strings, numbers, booleans, lists, objects)
- Custom metadata fields
- Long descriptions up to 1024 characters
