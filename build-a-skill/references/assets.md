# Assets: bundling templates and reference files

## What `assets/` is for

`assets/` is where you put **files that get embedded in or referenced by the skill's output** - templates, boilerplate, fonts, icons, snippets of structured content. Anything that should look identical every time the skill runs.

Think of it as your skill's "brand kit": the parts of the output that shouldn't vary based on Claude's judgment because they're already correct.

| Goes in `references/` | Goes in `assets/` |
|---|---|
| Documentation Claude reads to understand the task | Files the skill copies, fills in, or hands back to the user |
| `voice-guide.md`, `patterns.md` | `release-notes.template.md`, `report.pptx`, `logo.svg` |
| "How to write release notes" | "The release notes template itself" |

## Folder shape

```
release-notes-writer/
├── SKILL.md
├── references/
│   └── voice-guide.md
└── assets/
    ├── release-notes.template.md
    └── changelog-section.snippet.md
```

## Templates: the most common asset

A template is a file with placeholders Claude fills in. The discipline:

1. **Concrete, ready-to-use file.** A working example a user could send out as-is if they only filled in the blanks.
2. **Explicit, named placeholders.** Use `{version}`, `{date}`, `{features_list}` - not "fill in the version here".
3. **One template per output shape.** Don't create one mega-template with conditionals. Split by use case.

Example `assets/release-notes.template.md`:

```markdown
# Release {version} - {date}

{tagline_one_sentence}

## Features

{features_list}

## Fixes

{fixes_list}

## Breaking changes

{breaking_changes_or_none}

---

_Generated from {n_prs} merged pull requests since {previous_version}._
```

And in SKILL.md:

```markdown
## Step 4: Fill the template

Read `assets/release-notes.template.md` and replace each `{placeholder}`
with the corresponding value. Preserve the section order. If
`breaking_changes_or_none` has no entries, write the literal text
"None in this release."
```

## Beyond templates

- **Snippets.** Short reusable chunks - a standard footer, a legal disclaimer, a code preamble.
- **Style assets.** A logo (`.svg`, `.png`), a font file, a brand-colors palette. Slide-deck and document-generation skills lean on these.
- **Reference data.** A CSV of supported currencies, a JSON of feature flags, a list of approved acronyms. Static facts the skill needs to look up.
- **Schemas.** A JSON Schema file the skill validates output against (often paired with a validation script in `scripts/`).

## What does NOT belong in `assets/`

- **Documentation for Claude.** That's `references/`.
- **Executable code.** That's `scripts/`.
- **Secrets, credentials, tokens.** Anywhere. Ever.
- **User-specific data.** Assets are static and shipped with the skill. If it varies per user, ask for it at runtime.
- **Large binaries.** Skills should stay lean. If you need 50 MB of fonts, host them and have a script fetch on first use.

## Progressive disclosure still applies

Assets cost zero tokens until Claude reads them. That means you can ship multiple templates without bloating the skill - Claude only loads the one that matches the current job.

In SKILL.md, link them explicitly:

```markdown
## Choosing the right template

- For patch releases (x.y.Z): use `assets/release-notes.patch.template.md`
- For minor releases (x.Y.z): use `assets/release-notes.minor.template.md`
- For major releases (X.y.z): use `assets/release-notes.major.template.md`
```

## Versioning assets

If users depend on the *exact shape* of your template (e.g. they have downstream tooling parsing it), bump `metadata.version` in SKILL.md when you change the template. Treat asset changes the same way you'd treat API changes.

## A working example: a slide-deck skill

```
weekly-status-deck/
├── SKILL.md
├── references/
│   └── slide-design.md
├── scripts/
│   └── render_deck.py            # turns markdown sections into .pptx
└── assets/
    ├── deck.template.pptx        # the master template with branding
    ├── section-header.snippet.md
    ├── kpi-table.snippet.md
    └── footer.snippet.md
```

This is the canonical pattern: `references/` teaches Claude *how* to think about the output, `assets/` provides the exact shapes, `scripts/` does the deterministic conversion. SKILL.md orchestrates.
