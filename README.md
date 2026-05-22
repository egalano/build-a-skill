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
├── SKILL.md                              # Interactive tutorial flow (entry point)
└── references/
    ├── fundamentals.md                   # What a skill is, progressive disclosure, MCP + skills
    ├── planning-and-design.md            # Use cases, success criteria, the three categories
    ├── technical-requirements.md         # Folder/file rules, frontmatter fields
    ├── writing-instructions.md           # SKILL.md body: structure, specificity, examples
    ├── scripts.md                        # When to bundle code, invocation, security  (community-expanded)
    ├── assets.md                         # Templates and static reference files       (community-expanded)
    ├── patterns.md                       # Five workflow patterns to choose from
    ├── testing-and-iteration.md          # The three lightweight tests every skill should pass
    ├── evals.md                          # Optional 5-prompt eval set for regressions (community addition)
    ├── distribution.md                   # Individual / team / world / API
    ├── troubleshooting.md                # Common failure modes and fixes
    ├── checklist.md                      # Pre-flight checklist before shipping
    └── resources.md                      # Links to the original guide and further reading
```

The skill itself is a small example of progressive disclosure: `SKILL.md` is the lean entry point, and the chapter-by-chapter material lives in `references/` so Claude only loads what's needed for the user's current stage. Files marked "community-expanded" or "community addition" go beyond the source guide; see the per-file scope notes at the top of each one for details on what's source-derived vs. practitioner convention.

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

1. **Calibrate to your level.** A quick diagnostic up front - never built one? Already shipped one? - lets the tutorial skip what you already know.
2. Ask what you want to build, or run **Demo Mode** on a canned `release-notes-writer` example if you're just here to see the shape.
3. Cover fundamentals (what a skill is, progressive disclosure, how it composes with MCP) - briefly, and only the parts you don't already know.
4. Help you write 1-3 concrete use cases.
5. Pick a success signal you actually care about.
6. Draft your YAML frontmatter with you (especially the `description` field, which is the single most important thing).
7. Co-write the body of `SKILL.md`. Add `scripts/`, `assets/`, or an `evals/` set if your skill needs them.
8. Pick the workflow pattern that fits your category.
9. Walk you through triggering tests, functional tests, a quick before/after comparison, and optionally build a tiny eval set for regression protection.
10. Show you how to distribute it (just yourself, your team, the world, or via API).
11. Run a pre-flight checklist.
12. **Build a real `.zip` and print the absolute install paths**, ready to paste into Claude.ai or `cp` into `~/.claude/skills/`.

At the end you'll have a real folder *and* a real zip on disk - both immediately installable.

## What's new

### v1.1 (community packaging additions)

These are improvements to the *tutorial experience*, not changes to the underlying skills format taught by the source guide.

- **Experience-level diagnostic** up front so the tutorial right-sizes itself (skips Fundamentals if you already know them, etc.).
- **Demo Mode** that runs the whole flow against a canned `release-notes-writer` example, so you can watch a skill being built before doing your own.
- **End-of-tutorial deliverable**: the agent builds a real `.zip` and prints absolute install paths for both Claude.ai and Claude Code.
- New reference chapters: `scripts.md`, `assets.md`, and `evals.md` (the first two expand on optional folders the source guide mentions; the third adds a regression-protection layer on top of the source guide's three lightweight tests).
- Pre-flight checklist updated with scripts/assets/evals checks and a zip-listing sanity check.

### v1.0

Initial release - condensed, conversational packaging of *The Complete Guide to Building Skills for Claude* (Anthropic, January 2026) as an interactive skill.

## License

MIT. See [LICENSE](./LICENSE). Adapt freely.

The MIT license covers this packaging only. The underlying conceptual content - examples, patterns, and best practices - is owned by Anthropic and quoted/summarized here under fair use for educational purposes. If you redistribute or adapt this skill, preserve attribution to both the original guide and this packaging.

## Contributing

If you find a chapter that's missing, a pattern that's wrong, or a question the tutorial doesn't handle well, open an issue or a PR. When adding material that goes beyond the source guide, please flag it with a "community contribution" scope note at the top of the file (as `scripts.md`, `assets.md`, and `evals.md` do) so readers can tell source-derived material from practitioner convention.
