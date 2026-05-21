# Pre-flight checklist

Walk through this list with the user before they ship. For each item, either confirm it's done or fix the gap.

## Before you start

- [ ] Identified 2-3 concrete use cases
- [ ] Tools identified (built-in or MCP)
- [ ] Reviewed the guide and at least one example skill
- [ ] Planned folder structure

## During development

- [ ] Folder named in kebab-case
- [ ] `SKILL.md` file exists with exact spelling (capitalized `SKILL`, lowercase `.md`)
- [ ] YAML frontmatter has `---` delimiters at top and bottom
- [ ] `name` field: kebab-case, no spaces, no capitals
- [ ] `description` field includes WHAT and WHEN, plus literal trigger phrases
- [ ] No XML tags (`<` or `>`) anywhere in frontmatter
- [ ] Skill name doesn't contain `claude` or `anthropic` (reserved)
- [ ] No `README.md` inside the skill folder
- [ ] Instructions are clear and actionable (no "validate the data properly")
- [ ] Error handling included
- [ ] At least one worked example
- [ ] References clearly linked from SKILL.md

## Before upload

- [ ] Tested triggering on obvious tasks
- [ ] Tested triggering on paraphrased requests
- [ ] Verified it does NOT trigger on unrelated topics
- [ ] Functional tests pass
- [ ] Tool integration works (if applicable)
- [ ] Compressed as `.zip` file (if uploading to Claude.ai)

## After upload

- [ ] Tested in real conversations
- [ ] Monitor for under/over-triggering
- [ ] Collect user feedback
- [ ] Iterate on description and instructions
- [ ] Update `metadata.version` when shipping changes
