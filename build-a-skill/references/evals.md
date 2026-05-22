# Evals: going beyond the three lightweight tests

> **About this chapter.** Anthropic's [Complete Guide to Building Skills for Claude](https://resources.anthropic.com/hubfs/The-Complete-Guide-to-Building-Skill-for-Claude.pdf) (January 2026) introduces three lightweight tests every skill should pass: triggering, functional, and performance comparison (covered in `testing-and-iteration.md`). Those three are the *floor*. This chapter is a community contribution that adds a **regression-protection layer** on top - a small, durable eval set you re-run on every change. The "5-prompt starter", the Markdown file format, and the runner options are practitioner conventions, not verbatim from the source guide.

The three lightweight tests from `references/testing-and-iteration.md` (triggering, functional, performance comparison) are enough to ship. But once your skill has users, you'll want **regression protection** - a way to know that today's tweak to the description didn't break yesterday's behavior.

That's what an eval set is for. It's a tiny, version-controlled set of prompts paired with expected outcomes. Run it before every change. If it goes red, you know what broke and where.

## The 5-prompt starter eval set

Don't build a 100-case harness. Start with 5 prompts that exercise the boundaries:

- **3 should-trigger.** Cover the prototypical request, a paraphrased version, and an edge case at the edge of the description's scope.
- **2 should-not-trigger.** Cover an adjacent topic you specifically want to avoid loading on, and a hostile-trigger case (a prompt that uses skill-related keywords but is actually about something else).

For each, write down the expected behavior. That's the eval.

## File format

Save it as `evals/eval-set.md` inside your skill folder. Markdown is fine - you don't need a fancy framework yet. The point is to make the file scannable so you can run it manually in 2 minutes.

```markdown
# eval-set for release-notes-writer

## Should trigger

### T1: Direct request
Prompt: "Write release notes for v2.3.0"
Expected: skill loads; asks for PR list or tag range; produces Markdown with Features / Fixes / Breaking sections.
Pass criteria: all three sections present; no internal jargon ("refactor", "wip", "lgtm").

### T2: Paraphrase
Prompt: "Draft the changelog for the release we're cutting today"
Expected: skill loads; produces same shape as T1.
Pass criteria: section structure matches.

### T3: Edge - patch release with no breaking changes
Prompt: "Release notes for v2.3.1, just two bug fixes"
Expected: skill loads; "Breaking changes" section says "None in this release."
Pass criteria: literal string "None in this release." appears.

## Should NOT trigger

### N1: Adjacent topic
Prompt: "Summarize this design doc"
Expected: skill does NOT load. Claude summarizes without invoking release-notes structure.
Pass criteria: no "Features / Fixes / Breaking" headers appear in output.

### N2: Hostile keyword overlap
Prompt: "What does it mean to 'release' a process in Linux?"
Expected: skill does NOT load. Claude explains the OS concept.
Pass criteria: response is about processes/OS, not about software releases.
```

## How to run it

**Manual (good enough for most skills):**

1. Open a fresh chat in Claude.ai or Claude Code with the skill enabled.
2. Run each prompt in a separate conversation (so state doesn't leak).
3. Mark each row pass / fail / weird.
4. Commit the results. The next time you change the description, run it again.

**Semi-automated (Claude Code):**

A Bash script in `scripts/run_evals.sh` can shell out to `claude -p "<prompt>"` for each line, capture the output, and grep for the pass criteria. This is enough to wire into a pre-push hook.

**Programmatic (API):**

For skills shipped to teams or via the API, use the Messages API with the skill enabled, the same prompt-per-conversation discipline, and `re.search` for pass criteria. Wire it into CI. Fail the build on regressions.

## What changes to test against

After **every** edit to:

- The `description` field (changes triggering)
- The body of SKILL.md (changes behavior)
- Any `assets/` template (changes output shape)
- Any `scripts/` script (changes determinism guarantees)

If you only have 5 minutes, run the 5 evals. They catch 80% of regressions.

## Growing the eval set

Add a row every time you:

- Find a bug. Reproduce it as a failing eval first, then fix it.
- Get user feedback. "It didn't trigger when I said X" → that's a new should-trigger row.
- Add a feature. New behavior gets at least one eval to lock it in.

Cap the set at ~20 cases. If it's getting bigger, your skill is probably trying to do too many things - consider splitting it.

## What an eval set is *not*

- It is not a substitute for using the skill yourself. Quantitative passes don't catch the awkward-to-read sentence.
- It is not a substitute for `references/testing-and-iteration.md`. The three lightweight tests are about *whether the skill works*; the eval set is about *whether it still works tomorrow*.
- It is not a benchmark for comparing models. The signal you care about is your skill's behavior, holding everything else constant.

## Connecting back to your success signal

In Stage 3 the user picked one signal they cared about. Make sure at least one eval row tests that signal directly. If they said "Triggers automatically on obvious prompts", T1 above is exactly that. If they said "Output is consistent across sessions", T2 is exactly that. The eval set is how that promise stays kept.
