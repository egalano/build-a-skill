# build-a-skill

> **All instructional content in this skill comes from Anthropic.** It is a community-built interactive packaging of ["The Complete Guide to Building Skills for Claude"](https://resources.anthropic.com/hubfs/The-Complete-Guide-to-Building-Skill-for-Claude.pdf) (Anthropic, January 2026). Every concept, example, pattern, and best practice originates from that guide. **This repository is not affiliated with, endorsed by, or sponsored by Anthropic.** Please read the original PDF for the canonical, authoritative material.

An interactive Claude Skill that walks you through designing, writing, testing, and distributing your own Skill for Claude - the way most "guides" should work now that you have a world-class educator in your pocket.

Instead of reading a 33-page PDF, you load this skill and your agent teaches you conversationally: it asks what you want to build, helps you nail down use cases, drafts the YAML frontmatter and instructions with you, walks you through testing, and runs a pre-flight checklist before you ship.

## See it in action
<img width="530" height="658" alt="The build-a-skill tutorial mid-conversation, asking the user a multiple-choice question about progressive disclosure" src="https://github.com/user-attachments/assets/8cf1df48-9475-47a4-bb87-39969c00d6f6" />

*Mid-tutorial: the agent walks you through the concepts and checks understanding with multiple-choice prompts before moving on.*

## Credits

- **Source material — owned by Anthropic**: ["The Complete Guide to Building Skills for Claude"](https://resources.anthropic.com/hubfs/The-Complete-Guide-to-Building-Skill-for-Claude.pdf) (January 2026). All conceptual content, examples, patterns, and best practices in this skill come from that guide.
- **Idea to convert the PDF into a skill**: [Ali Yahya (@alive_eth)](https://x.com/alive_eth/status/2057306868297425062) — "Why is this a PDF?? We now all have a genius world class educator in our pocket. This 'guide' should be a skill file that gets your agent to walk you through everything you need to know interactively. That is how most education should work going forward."
- **This packaging**: a transformative derivative work under MIT license. It does not re-license Anthropic's underlying guide, which remains their property.

## What's in the box

```
build-a-skill/
├── SKILL.md                              # The interactive tutorial flow
└── references/
    ├── fundamentals.md
    ├── planning-and-design.md
    ├── technical-requirements.md
    ├── writing-instructions.md
    ├── testing-and-iteration.md
    ├── distribution.md
    ├── patterns.md
    ├── troubleshooting.md
    ├── checklist.md
    └── resources.md
```

The skill itself is a small example of progressive disclosure: `SKILL.md` is the lean entry point, and the chapter-by-chapter material lives in `references/` so Claude only loads what's needed for the user's current stage.

## Install

Two-step install (this is the path the [official guide](https://resources.anthropic.com/hubfs/The-Complete-Guide-to-Building-Skill-for-Claude.pdf) recommends):

### 1. Download the skill

- **From Releases (recommended)**: grab the latest `build-a-skill-vX.Y.Z.zip` from [github.com/egalano/build-a-skill/releases/latest](https://github.com/egalano/build-a-skill/releases/latest). The zip extracts to a `build-a-skill/` folder containing `SKILL.md`.
- Or clone the repo: `git clone https://github.com/egalano/build-a-skill.git`.

### 2. Install in Claude

**For Claude.ai (web or desktop):**

1. Open Claude.ai → **Settings → Capabilities → Skills**.
2. Click **Upload skill**.
3. Select `build-a-skill-vX.Y.Z.zip` (or zip the `build-a-skill/` folder yourself if you cloned).
4. Toggle the skill on.

**For Claude Code:**

```bash
# After cloning or unzipping
cp -r build-a-skill ~/.claude/skills/
```

### 3. Test it

Start a new conversation and say one of:

- "Walk me through building a skill"
- "Teach me how to make a Claude skill"
- "/build-a-skill"

The skill should load automatically and the tutorial will begin.

### Or just read it

If you'd rather skim the source material than be tutored through it, the chapter files under `build-a-skill/references/` are condensed, human-readable summaries of the original guide.

## What to expect

Total time: about 15-30 minutes for a working first skill. The agent will:

1. Ask what you want to build.
2. Cover fundamentals (what a skill is, progressive disclosure, how it composes with MCP) - briefly, and only the parts you don't already know.
3. Help you write 1-3 concrete use cases.
4. Pick a success signal you actually care about.
5. Draft your YAML frontmatter with you (especially the `description` field, which is the single most important thing).
6. Co-write the body of `SKILL.md`.
7. Pick the workflow pattern that fits your category.
8. Walk you through triggering tests, functional tests, and a quick before/after comparison.
9. Show you how to distribute it (just yourself, your team, the world, or via API).
10. Run a pre-flight checklist before declaring you done.

At the end you'll have a real folder on disk with a real `SKILL.md` you can upload immediately.

## License

MIT. See [LICENSE](./LICENSE). Adapt freely.

## Contributing

If you find a chapter that's missing, a pattern that's wrong, or a question the tutorial doesn't handle well, open an issue or a PR.
