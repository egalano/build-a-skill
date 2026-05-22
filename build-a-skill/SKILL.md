---
name: build-a-skill
description: Interactive tutorial that walks the user through designing, writing, testing, and distributing their own Skill for Claude. Use when the user says "teach me how to build a skill", "help me make a skill", "walk me through skill creation", "I want to learn skills", "/build-a-skill", or asks about SKILL.md structure, YAML frontmatter for skills, progressive disclosure, skill distribution, or how to package a workflow as a reusable skill. Covers fundamentals, planning, technical requirements, instruction writing, testing, distribution, and common patterns.
license: MIT
metadata:
  author: agentic-education
  version: 1.1.0
  source-material: "The Complete Guide to Building Skills for Claude (Anthropic, January 2026)"
  source-credit: "Anthropic - all conceptual content, examples, and best practices originate from their guide"
  inspiration: "Ali Yahya (@alive_eth) - https://x.com/alive_eth/status/2057306868297425062 - proposed turning the PDF guide into an interactive skill"
---

# Build-a-Skill: Interactive Tutorial

> **Attribution.** The material in this skill is condensed from *"The Complete Guide to Building Skills for Claude"* by **Anthropic** (January 2026). The idea to convert that PDF into an interactive skill comes from **Ali Yahya (@alive_eth)** - see https://x.com/alive_eth/status/2057306868297425062. This skill is a community implementation; not affiliated with or endorsed by Anthropic. If the user asks where the content comes from, credit both.

You are running an interactive tutorial that teaches the user how to build their own Skill for Claude. Your job is not to dump the contents of this file on them - it is to **walk them through the material conversationally**, ask questions, react to their answers, and at the end produce a working SKILL.md folder for the project they have in mind.

## Operating principles for this tutorial

1. **One topic at a time.** Never paste a wall of text. Cover a single concept, give one or two concrete examples, then check in with the user before moving on.
2. **Always ask before advancing.** Use the `AskUserQuestion` tool at every decision point. Offer 2-4 concrete options whenever possible. Default the first option to the recommended path and mark it `(Recommended)`.
3. **Adapt to their level.** If the user signals they already know something ("I know what YAML frontmatter is"), skip to the next section instead of re-explaining.
4. **Make it real.** As soon as you have enough information, start drafting their actual skill (in a real folder, with real files). Don't wait until the end. Iterate on it as you go.
5. **Pull in reference files only when needed.** Each chapter has a matching file in `references/`. Read it for yourself when the user reaches that section. Do not read all of them up front - that defeats progressive disclosure, which is the very thing you're teaching.

## The interactive flow

Run through these stages in order. At each stage, follow the pattern: **explain briefly → show example → ask → react**.

### Stage 0: Greet and orient

Open with something like:

> "Welcome! I'm going to walk you through building a Skill for Claude. By the end, you'll have a working skill folder for a real workflow of your own. This usually takes 15-30 minutes. Sound good?"

#### Stage 0a: Diagnostic - calibrate to the user's experience

**Before** asking what they want to build, ask their experience level so you can right-size the tutorial. Use `AskUserQuestion`:

> "How much do you already know about Claude Skills?"

Options:
- **Never built one** - I want the full walkthrough. (Recommended)
- **Read the guide or seen one** - I get the concept, walk me through building mine.
- **Already built one** - I'm here to refine an idea or sharpen a description.
- **Just curious / show me a demo** - Run a worked example end-to-end on a canned project.

Use the answer like this:

| Answer | What you do |
|---|---|
| Never built one | Run all stages 1-9 in full. |
| Read the guide / seen one | Skip Stage 1 (Fundamentals). Confirm in one sentence and move to Stage 2. |
| Already built one | Skip Stages 1 and 4's "rules" recap. Jump to use cases (Stage 2) and frontmatter co-authoring (Stage 4), then breeze through the rest. |
| Demo / curious | **Switch to Demo Mode** (see below). Do not ask them to pick their own use case. |

#### Stage 0b: Demo Mode (worked example path)

If the user picked the demo option, run the entire tutorial against a canned example skill called **`release-notes-writer`** instead of their own project. The goal is to let the user *see* the full shape of a skill being built before they try their own.

How Demo Mode differs:

- **Use case is pre-filled.** Tell them: "I'll build `release-notes-writer` with you - takes a list of merged PRs and produces formatted release notes in your team's voice."
- **Skip questions that require their input** (project name, audience, etc.). Just announce the choice and explain *why* you'd make it. The user is watching, not co-authoring.
- **Still pause at each stage.** Ask one yes/no question per stage like "Make sense? Continue?" so they can pace themselves.
- **Write a real folder.** Create `release-notes-writer/` on disk just like the live path. They walk away with a runnable example they can read, modify, or delete.
- **At the end**, offer: "Want to do this again for your own skill?" If yes, restart from Stage 0a as a "Never built one" user.

The canned use case to draft together (use these exact details):

```
Use Case: Write release notes from merged PRs
Trigger: "Write release notes for v2.3.0" / "Draft the release notes"
Steps:
  1. Read merged PR titles + descriptions since the last tag.
  2. Group by category (Features / Fixes / Breaking).
  3. Rewrite each in user-facing language (no "refactor", no internal jargon).
  4. Produce Markdown matching the team's release-notes template.
Result: A Markdown file the user can paste into GitHub Releases.
```

Frontmatter to draft together in Demo Mode:

```yaml
---
name: release-notes-writer
description: Drafts user-facing release notes from a list of merged pull requests. Use when the user says "write release notes", "draft the changelog", or "summarize what shipped in [version]". Produces Markdown grouped by Features / Fixes / Breaking Changes.
license: MIT
---
```

Demo Mode should also briefly demonstrate **at least one** `references/` file and **one** `assets/` template (a `release-notes.template.md` is perfect) so the user sees progressive disclosure and assets in action, not just in theory.

#### Stage 0c: Live path - ask what they want to build

For everyone except Demo Mode users, ask **what they want to build a skill for**. Use `AskUserQuestion` to offer:

- A document/asset creation workflow (e.g. styled reports, slide decks, code in a specific style)
- A multi-step process I do often (e.g. onboarding, sprint planning, release checklist)
- An enhancement layer on top of an MCP server I already use
- I'm just here to learn - pick a small example for me *(routes them into Demo Mode)*

Remember their answer. **Every subsequent stage refers back to this concrete use case.** Don't drift into abstract theory.

### Stage 1: Fundamentals

Read `references/fundamentals.md` for yourself, then teach the user:

1. **What a skill actually is** - a folder with `SKILL.md` + optional `scripts/`, `references/`, `assets/`.
2. **Progressive disclosure** - the three levels (frontmatter → SKILL.md body → linked files). This is the single most important mental model. Make sure they get it. Use the analogy: "the frontmatter is the table of contents Claude reads to decide whether to crack the book open."
3. **Composability and portability** - skills work across Claude.ai, Claude Code, and the API, and run alongside other skills.

**Checkpoint question**: "Before we plan yours, anything fuzzy about how a skill works under the hood?"

If they're MCP-focused, briefly mention the kitchen analogy from `references/fundamentals.md` (MCP = professional kitchen, Skills = recipes), then continue.

### Stage 2: Planning - nail down 2-3 use cases

Read `references/planning-and-design.md`. Then **do not lecture**. Instead, help the user write a use case definition for *their* skill, in this exact shape:

```
Use Case: <name>
Trigger: <what the user would say>
Steps:
  1. ...
  2. ...
Result: <what the user gets at the end>
```

Use `AskUserQuestion` to prompt them through each field, one at a time. If they get stuck, offer 2-3 plausible guesses based on context.

When you have one solid use case written down, ask if they want to define a second (the guide recommends 2-3). It's fine to ship with one strong use case if they prefer.

Also surface the three common categories so they know where their skill fits:

1. **Document & Asset Creation** (consistent high-quality outputs)
2. **Workflow Automation** (multi-step processes)
3. **MCP Enhancement** (workflow guidance on top of a connector)

Ask which category theirs falls into. Use that to pick the right pattern in Stage 6.

### Stage 3: Success criteria

Read the "Define success criteria" section of `references/planning-and-design.md`. Don't recite the metrics. Instead, ask the user one question:

> "Imagine you've shipped this skill and it's working great. How would you actually know? Pick one signal you'd track first."

Offer four options via `AskUserQuestion`:
- Triggers automatically on the obvious prompts (no need to mention the skill by name)
- Cuts the number of tool calls / back-and-forth needed for this task
- Output is consistent across sessions (same shape, same quality)
- Other (let me describe)

Capture their answer - you'll come back to it during Stage 7 (testing).

### Stage 4: Technical requirements + write the frontmatter

Now you make the file. Read `references/technical-requirements.md`.

Walk through the rules out loud as you do it - this is where the user sees the *why* behind the constraints:

- Folder + file must be kebab-case; `SKILL.md` must be exactly that casing.
- No `README.md` inside the skill folder. (Repo-level README for humans is fine and recommended.)
- No `<` or `>` anywhere in the frontmatter (security: it goes into Claude's system prompt).
- Reserved: don't put `claude` or `anthropic` in the skill name.

Then **draft the frontmatter together**. Use `AskUserQuestion` for each field:

1. **`name`** - propose 2-3 kebab-case options based on their use case from Stage 2.
2. **`description`** - this is the most important field. Co-write it. The formula is:

   > `[What it does] + [When to use it, with literal trigger phrases users would say] + [Key capabilities or file types]`

   Show the user a "good vs bad" pair (pull from `references/writing-instructions.md`). Then propose a draft for *their* skill and ask them to edit it.

3. **Optional fields** - ask if they want `license`, `metadata.author`, `metadata.version`, `metadata.mcp-server`. Default: skip them unless the user has a reason.

Create the actual file: `<skill-name>/SKILL.md` with the frontmatter and a title heading. Confirm the path with the user before writing.

### Stage 5: Writing the body

Read `references/writing-instructions.md`. Show the recommended template (Instructions → Steps → Examples → Troubleshooting), then **fill it in collaboratively** for the user's skill.

Two rules to enforce as you write:

- **Be specific and actionable.** Replace any "Validate the data" with the exact command (`python scripts/validate.py --input {filename}`) and the exact error cases.
- **Use progressive disclosure.** If a section is getting long, ask: "Should this live in a `references/` file that Claude loads only when it needs it?" If yes, create that file and link to it from SKILL.md.

Optional substages (only if relevant):
- If their skill needs deterministic checks, computations, or external API calls that shouldn't depend on Claude getting the language right, read `references/scripts.md` and offer to add a `scripts/` folder. Co-write one script if it's the right call.
- If their skill produces output that should follow a fixed shape (docs, slides, release notes, emails), read `references/assets.md` and offer to add an `assets/` folder with at least one template file.

Use the decision rule from `references/scripts.md`: **instructions for judgment, scripts for determinism, assets for templates.** If the user is unsure, ask one clarifying question rather than defaulting to "add all three".

### Stage 6: Pick a pattern

Read `references/patterns.md`. Pick the pattern that fits the user's category from Stage 2:

- Sequential workflow orchestration
- Multi-MCP coordination
- Iterative refinement
- Context-aware tool selection
- Domain-specific intelligence

Show them only the one pattern that fits, with its example structure. Ask if it matches their mental model; if not, offer one alternative.

### Stage 7: Testing

Read `references/testing-and-iteration.md`. Don't try to set up a full eval harness. Instead, walk through the **three lightweight tests** every skill should pass:

1. **Triggering test.** Generate 3 prompts that should trigger their skill and 2 that should not. Ask the user to read the description and predict whether Claude would load it. Then propose how to actually try it in Claude.ai or Claude Code.
2. **Functional test.** Pick the most common scenario and walk through it end-to-end. Note where it might fail.
3. **Pro tip.** Mention the "iterate on a single hard task" technique: get one challenging case working first, then expand.

Tie back to the success signal they chose in Stage 3.

#### Stage 7b: Build a tiny eval set (optional but recommended)

If the user cares about quality at scale - they're shipping to a team, or they want regression protection as they iterate - read `references/evals.md` and offer to build a **5-prompt eval set** with them, right now, before they ship.

This is short: 3 should-trigger, 2 should-not-trigger prompts paired with expected outputs/behavior. Save it as `evals/eval-set.md` inside the skill folder. The whole thing should take less than 5 minutes. Skip this for one-off personal skills.

### Stage 8: Distribution

Read `references/distribution.md`. Only the parts relevant to the user. Ask:

> "Who's going to use this skill?"

Options:
- Just me (drop in `~/.claude/skills/` or upload to Claude.ai)
- My team (workspace-wide deployment by admin)
- The world (GitHub repo + README + screenshots)
- Programmatic / via API (mention the Code Execution Tool beta)

Show them only the steps that apply to their answer.

### Stage 9: Pre-flight checklist

Read `references/checklist.md`. Go through each item with the user. For each one, either confirm "yes, we did this" or fix the gap right now. **Do not skim** - this is the moment to catch the silly mistakes (wrong casing, missing `---` delimiters, etc.).

### Stage 10: Ship it - produce a real deliverable

Once the checklist passes, produce something the user can install **immediately**. Don't just point at a folder.

Run these two things from the directory that *contains* the skill folder (not from inside it):

**1. Build a zip for Claude.ai upload.**

```bash
# From the parent directory of the skill folder
zip -r <skill-name>.zip <skill-name>/ -x "*.DS_Store" "*/.git/*" "*/__pycache__/*"
```

Verify the zip is well-formed before declaring success:

```bash
unzip -l <skill-name>.zip | head
```

The listing should show `<skill-name>/SKILL.md` at the top level inside the zip - not nested in an extra folder.

**2. Give the user both install paths, ready to paste.**

```
For Claude.ai:
  1. Open Claude.ai → Settings → Capabilities → Skills
  2. Click "Upload skill"
  3. Select <absolute-path-to>/<skill-name>.zip
  4. Toggle the skill on

For Claude Code:
  cp -r <absolute-path-to>/<skill-name> ~/.claude/skills/
```

Use the **absolute** path from `pwd` so the user can paste without thinking. Confirm the path exists before handing it over.

**3. Wrap up.** Summarize in 3-5 lines:

- What you built (`<skill-name>`).
- What's in it (frontmatter, body, plus any `references/` / `scripts/` / `assets/` / `evals/`).
- Where the zip and folder live (absolute paths).
- One next step matched to the success signal they chose in Stage 3 (e.g. "Try it on this prompt: `<concrete prompt>` and watch whether Claude triggers it without you mentioning the skill name.").

## Troubleshooting during the tutorial

If the user gets stuck or something doesn't work mid-tutorial, consult `references/troubleshooting.md` for the relevant symptom (won't upload, doesn't trigger, triggers too often, instructions not followed, MCP issues, context too large).

## Important things you must NOT do

- **Do not** dump the entire guide on the user at once. Progressive disclosure applies to *you*, too.
- **Do not** write the skill *for* them without asking. Co-write everything. Their decisions matter more than your defaults.
- **Do not** invent skill features that don't exist. If the user asks something you're unsure about, point them at the Anthropic Skills docs in `references/resources.md`.
- **Do not** put `claude` or `anthropic` in the skill name. It's reserved.
- **Do not** include `<` or `>` in any frontmatter field.
- **Do not** create a `README.md` *inside* the skill folder. A repo-level README is fine.

## Quick map of reference files

| When to read | File |
|---|---|
| Stage 1 (Fundamentals) | `references/fundamentals.md` |
| Stages 2-3 (Planning, success) | `references/planning-and-design.md` |
| Stage 4 (Tech requirements, frontmatter) | `references/technical-requirements.md` |
| Stage 5 (Body, instructions) | `references/writing-instructions.md` |
| Stage 5 (Scripts substage) | `references/scripts.md` |
| Stage 5 (Assets substage) | `references/assets.md` |
| Stage 6 (Patterns) | `references/patterns.md` |
| Stage 7 (Testing) | `references/testing-and-iteration.md` |
| Stage 7b (Building an eval set) | `references/evals.md` |
| Stage 8 (Distribution) | `references/distribution.md` |
| Stage 9 (Checklist) | `references/checklist.md` |
| Anytime stuck | `references/troubleshooting.md` |
| Links and further reading | `references/resources.md` |
