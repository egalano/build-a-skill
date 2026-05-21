# Distribution and sharing

Skills make any MCP integration feel more complete. They give users a fast path to value and an edge over MCP-only alternatives.

## Distribution model (as of January 2026)

### Individual users

1. Download the skill folder.
2. Zip the folder if needed.
3. Upload to Claude.ai via **Settings > Capabilities > Skills**, OR drop into the Claude Code skills directory.

### Organization-level

- Admins can deploy skills workspace-wide (shipped December 18, 2025).
- Automatic updates.
- Centralized management.

### An open standard

Anthropic has published Agent Skills as an open standard. Like MCP, skills are intended to be portable across tools and platforms. Some skills are designed to take advantage of platform-specific capabilities; authors can note this in the optional `compatibility` field.

## Using skills via the API

For programmatic use cases (applications, agents, automated workflows):

- `/v1/skills` endpoint for listing and managing skills.
- Add skills to Messages API requests via the `container.skills` parameter.
- Version control through the Claude Console.
- Works with the Claude Agent SDK.

Skills in the API require the **Code Execution Tool beta**, which provides the secure environment skills need to run.

### When to use which surface

| Use case | Best surface |
|---|---|
| End users interacting with skills directly | Claude.ai / Claude Code |
| Manual testing and iteration during development | Claude.ai / Claude Code |
| Individual, ad-hoc workflows | Claude.ai / Claude Code |
| Applications using skills programmatically | API |
| Production deployments at scale | API |
| Automated pipelines and agent systems | API |

## Recommended distribution path

The most common path today:

### 1. Host on GitHub

- Public repo for open-source skills.
- Clear README with installation instructions (the repo-level README is for human visitors - this is separate from your skill folder, which should NOT contain a README.md).
- Example usage and screenshots.

### 2. Document in your MCP repo

- Link to the skill from MCP documentation.
- Explain the value of using MCP + skill together.
- Provide a quick-start guide.

### 3. Provide an installation guide

```
## Installing the [Your Service] skill

1. Download the skill:
   - Clone: `git clone https://github.com/yourcompany/skills`
   - Or download ZIP from Releases

2. Install in Claude:
   - Open Claude.ai > Settings > Skills
   - Click "Upload skill"
   - Select the skill folder (zipped)

3. Enable the skill:
   - Toggle on the [Your Service] skill
   - Ensure your MCP server is connected

4. Test:
   - Ask Claude: "Set up a new project in [Your Service]"
```

## Positioning your skill

How you describe your skill determines whether users understand its value.

### Focus on outcomes, not features

✅ Good:
> "The ProjectHub skill enables teams to set up complete project workspaces in seconds - including pages, databases, and templates - instead of spending 30 minutes on manual setup."

❌ Bad:
> "The ProjectHub skill is a folder containing YAML frontmatter and Markdown instructions that calls our MCP server tools."

### Highlight the MCP + skills story

> "Our MCP server gives Claude access to your Linear projects. Our skills teach Claude your team's sprint planning workflow. Together, they enable AI-powered project management."
