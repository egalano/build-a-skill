# Troubleshooting

## Skill won't upload

### "Could not find SKILL.md in uploaded folder"

**Cause**: File isn't named exactly `SKILL.md`.

**Fix**: Rename to `SKILL.md` (case-sensitive). Verify with `ls -la`.

### "Invalid frontmatter"

**Cause**: YAML formatting issue.

Common mistakes:

```yaml
# Wrong - missing delimiters
name: my-skill
description: Does things
```

```yaml
# Wrong - unclosed quotes
name: my-skill
description: "Does things
```

```yaml
# Correct
---
name: my-skill
description: Does things
---
```

### "Invalid skill name"

**Cause**: Name has spaces or capitals.

```yaml
# Wrong
name: My Cool Skill

# Correct
name: my-cool-skill
```

## Skill doesn't trigger

**Symptom**: Skill never loads automatically.

**Fix**: Revise the description field. Check:

- Is it too generic? ("Helps with projects" won't work.)
- Does it include trigger phrases users would actually say?
- Does it mention relevant file types if applicable?

**Debugging technique**: Ask Claude **"When would you use the [skill name] skill?"** Claude will quote the description back. Adjust based on what's missing.

## Skill triggers too often

**Symptom**: Skill loads for unrelated queries.

### Fixes

**1. Add negative triggers**

```yaml
description: Advanced data analysis for CSV files. Use for
statistical modeling, regression, clustering. Do NOT use for
simple data exploration (use data-viz skill instead).
```

**2. Be more specific**

```yaml
# Too broad
description: Processes documents

# More specific
description: Processes PDF legal documents for contract review
```

**3. Clarify scope**

```yaml
description: PayFlow payment processing for e-commerce. Use
specifically for online payment workflows, not for general
financial queries.
```

## MCP connection issues

**Symptom**: Skill loads but MCP calls fail.

### Checklist

1. **Verify MCP server is connected.** Claude.ai > Settings > Extensions > [Your Service]. Should show "Connected".
2. **Check authentication.** API keys valid and not expired. Proper scopes granted. OAuth tokens refreshed.
3. **Test MCP independently.** Ask Claude to call the MCP directly (without the skill): "Use [Service] MCP to fetch my projects". If that fails, the issue is the MCP, not the skill.
4. **Verify tool names.** Skill references the correct MCP tool names (case-sensitive).

## Instructions not followed

**Symptom**: Skill loads but Claude doesn't follow the instructions.

### Common causes

**1. Instructions too verbose**

- Keep instructions concise.
- Use bullet points and numbered lists.
- Move detailed reference to separate files in `references/`.

**2. Instructions buried**

- Put critical instructions at the top.
- Use `## Important` or `## Critical` headers.
- Repeat key points if needed.

**3. Ambiguous language**

```markdown
# Bad
Make sure to validate things properly

# Good
CRITICAL: Before calling create_project, verify:
- Project name is non-empty
- At least one team member assigned
- Start date is not in the past
```

**Advanced technique**: For critical validations, bundle a script that performs the checks programmatically rather than relying on language instructions. Code is deterministic; language interpretation isn't. See the Anthropic Office skills for examples.

**4. Model "laziness"**

Add explicit encouragement:

```
## Performance Notes
- Take your time to do this thoroughly
- Quality is more important than speed
- Do not skip validation steps
```

Note: this tends to be more effective when added to user prompts than inside SKILL.md.

## Large-context issues

**Symptom**: Skill seems slow or responses degrade.

**Causes**:

- SKILL.md too large
- Too many skills enabled simultaneously
- All content loaded instead of using progressive disclosure

**Fixes**:

1. **Optimize SKILL.md size.** Move detailed docs into `references/`. Link to them instead of inlining. Keep SKILL.md under ~5,000 words.
2. **Reduce enabled skills.** If you have 20-50+ skills enabled simultaneously, recommend selective enablement. Consider grouping related skills into "packs".
