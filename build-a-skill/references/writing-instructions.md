# Writing the body of SKILL.md

## The description field is everything

This is the first level of progressive disclosure. Claude reads it to decide whether to load the rest of your skill.

Formula:

> `[What it does] + [When to use it] + [Key capabilities or file types]`

### Good descriptions

```
description: Analyzes Figma design files and generates developer
handoff documentation. Use when user uploads .fig files, asks for
"design specs", "component documentation", or "design-to-code
handoff".
```

```
description: Manages Linear project workflows including sprint
planning, task creation, and status tracking. Use when user
mentions "sprint", "Linear tasks", "project planning", or asks
to "create tickets".
```

```
description: End-to-end customer onboarding workflow for PayFlow.
Handles account creation, payment setup, and subscription
management. Use when user says "onboard new customer", "set up
subscription", or "create PayFlow account".
```

### Bad descriptions

```
# Too vague
description: Helps with projects.

# Missing triggers - Claude won't know when to load it
description: Creates sophisticated multi-page documentation systems.

# Too technical, no user-facing triggers
description: Implements the Project entity model with hierarchical
relationships.
```

## Recommended body structure

After the frontmatter, write the actual instructions in Markdown. Adapt this template:

```markdown
---
name: your-skill
description: ...
---

# Your Skill Name

## Instructions

### Step 1: [First major step]
Clear explanation of what happens.

Example:
\`\`\`bash
python scripts/fetch_data.py --project-id PROJECT_ID
\`\`\`
Expected output: [describe what success looks like]

### Step 2: ...

## Examples

### Example 1: [Common scenario]
User says: "Set up a new marketing campaign"
Actions:
1. Fetch existing campaigns via MCP
2. Create new campaign with provided parameters
Result: Campaign created with confirmation link

## Troubleshooting

### Error: [Common error message]
Cause: [Why it happens]
Solution: [How to fix]
```

## Best practices for instructions

### Be specific and actionable

✅ Good:
```
Run `python scripts/validate.py --input {filename}` to check
data format. If validation fails, common issues include:
- Missing required fields (add them to the CSV)
- Invalid date formats (use YYYY-MM-DD)
```

❌ Bad:
```
Validate the data before proceeding.
```

### Include error handling

```
## Common Issues

### MCP Connection Failed
If you see "Connection refused":
1. Verify MCP server is running: Settings > Extensions
2. Confirm API key is valid
3. Try reconnecting: Settings > Extensions > [Service] > Reconnect
```

### Reference bundled resources clearly

```
Before writing queries, consult `references/api-patterns.md` for:
- Rate limiting guidance
- Pagination patterns
- Error codes and handling
```

### Use progressive disclosure

Keep `SKILL.md` focused on core instructions. Move detailed documentation into `references/` files and link to them. Aim to keep `SKILL.md` under ~5,000 words.

### Put critical instructions at the top

If something is mandatory, put it near the top with a `## Important` or `## Critical` heading. Repeat key points if needed.

### Prefer unambiguous language

❌ Bad: "Make sure to validate things properly"

✅ Good:
```
CRITICAL: Before calling create_project, verify:
- Project name is non-empty
- At least one team member assigned
- Start date is not in the past
```

### When determinism matters, use a script

For critical validations, bundle a script in `scripts/` and call it from `SKILL.md`. Code is deterministic; language interpretation isn't. See Anthropic's Office skills for examples of this pattern.

### Counter "laziness"

Adding explicit encouragement helps:

```
## Performance Notes
- Take your time to do this thoroughly
- Quality is more important than speed
- Do not skip validation steps
```

(Side note: this tends to be even more effective when added to user prompts than to SKILL.md.)
