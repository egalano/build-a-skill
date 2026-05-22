# Changelog

All notable changes to **build-a-skill** are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

Each release is built and published by `.github/workflows/release.yml`, triggered by pushing a `vX.Y.Z` tag. The workflow extracts the matching section below into the GitHub Release body, so keep entries reader-friendly: this is what end users see when they download the skill.

## [Unreleased]

_Nothing yet._

## [1.1.0] - 2026-05-22

### Added
- **Experience-level diagnostic** at Stage 0a so the tutorial right-sizes itself - skips Fundamentals for users who already know them, jumps straight to refinement for users who've already shipped one.
- **Demo Mode** (Stage 0b): runs the full tutorial flow against a canned `release-notes-writer` example so users can watch a skill being built end-to-end before tackling their own.
- **End-of-tutorial deliverable** (Stage 10): the agent now builds a real installable `.zip` and prints absolute install paths for both Claude.ai upload and `cp -r ~/.claude/skills/`.
- New reference chapter `references/scripts.md`: when to bundle code, invocation patterns from `SKILL.md`, exit-code discipline, security considerations, and a worked Python example.
- New reference chapter `references/assets.md`: the `references/` vs `assets/` boundary, template discipline with named placeholders, what does *not* belong in `assets/`, and a canonical multi-file skill layout.
- New reference chapter `references/evals.md`: a 5-prompt starter eval set (3 should-trigger, 2 should-not) for regression protection on top of the three lightweight tests, with manual / semi-automated / programmatic runner options.
- Optional Stage 7b for co-authoring an eval set with the user before they ship.
- GitHub Actions release workflow (`.github/workflows/release.yml`) that validates the skill structure, builds the installable zip, generates release notes from this changelog, and publishes a GitHub Release on every `v*` tag.
- This `CHANGELOG.md`, in Keep a Changelog format.

### Changed
- `SKILL.md` operating flow expanded from 9 stages to 10 (Stage 10 ships the deliverable).
- README `What's in the box` annotates each chapter with a one-line scope, and a new "What's new" section calls out v1.1 additions.
- README License section clarifies that the MIT license covers this packaging only - the underlying conceptual content remains Anthropic's.
- Pre-flight checklist (`references/checklist.md`) gained items for `scripts/`, `assets/`, `evals/`, and a zip-listing sanity check.
- Skill `metadata.version` bumped to `1.1.0`.

### Documented
- Each new reference chapter opens with a scope note distinguishing source-derived material from community-added practitioner convention.
- The Python example in `scripts.md` is now line-commented to teach the design choices it embodies (exit-code contract, argparse vs sys.argv, stderr/stdout separation, `Path()` normalization, `sys.exit` propagation).

## [1.0.0] - 2026-05-20

### Added
- Initial release: interactive Skill packaging of *The Complete Guide to Building Skills for Claude* (Anthropic, January 2026).
- `SKILL.md` with the 9-stage interactive tutorial flow.
- Ten chapter files in `references/`: fundamentals, planning-and-design, technical-requirements, writing-instructions, patterns, testing-and-iteration, distribution, troubleshooting, checklist, resources.
- Repo-level README, LICENSE (MIT), and attribution to Anthropic's source guide and to Ali Yahya (the originator of the "guide-as-skill" idea).

[Unreleased]: https://github.com/egalano/build-a-skill/compare/v1.1.0...HEAD
[1.1.0]: https://github.com/egalano/build-a-skill/compare/v1.0.0...v1.1.0
[1.0.0]: https://github.com/egalano/build-a-skill/releases/tag/v1.0.0
