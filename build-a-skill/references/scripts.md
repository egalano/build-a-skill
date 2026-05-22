# Scripts: when and how to bundle code in a skill

## The decision rule

Use **instructions** when the task requires judgment, prose, or branching that benefits from Claude's reasoning.

Use a **script** when the task is deterministic, repetitive, or numerically sensitive - and getting it wrong is worse than getting it slow.

| Use instructions | Use a script |
|---|---|
| "Rewrite this in the team's voice" | "Validate that the JSON matches this schema" |
| "Group these PRs by category" | "Compute the diff between two release tags" |
| "Pick the right MCP tool for the user's intent" | "Run a checksum against the bundled file" |
| Anything requiring taste, audience awareness, or trade-offs | Anything you'd write a unit test for |

A simple heuristic: if you'd be uncomfortable having Claude do the math, write a script. Code is deterministic; language interpretation isn't.

## Folder shape

```
my-skill/
├── SKILL.md
└── scripts/
    ├── validate_input.py
    └── compute_diff.sh
```

Scripts live in `scripts/`. They're loaded **only when SKILL.md tells Claude to run them**, which makes them part of progressive disclosure level 3 - they cost zero tokens until invoked.

## Invoking a script from SKILL.md

Be explicit. Don't write "validate the input" - write the exact command:

```markdown
## Step 2: Validate the input

Run the validation script:

\`\`\`bash
python scripts/validate_input.py --input "{user_file}"
\`\`\`

Expected exit codes:
- 0: input is valid, continue to Step 3
- 1: schema mismatch - show the error message to the user and stop
- 2: file not found - ask the user to re-attach
\`\`\`
```

Three things this gets right:
1. The exact command, ready to run.
2. The expected exit codes, so Claude knows how to branch.
3. The error-handling behavior for each failure mode.

## What makes a good skill script

- **Single purpose.** One script, one job. `validate_input.py` not `do_everything.py`.
- **Self-documenting `--help`.** When Claude is unsure how to invoke it, `--help` should tell the whole story.
- **Stable, machine-readable exit codes.** 0 = success, non-zero = failure. Differentiate failure modes if Claude needs to branch on them.
- **Stdout for data, stderr for diagnostics.** Standard Unix discipline; Claude can grep either one.
- **No interactivity.** Scripts must run non-interactively. No `input()` prompts - take everything via arguments or stdin.

## Languages

- **Python** is the default. Almost always available; great for data, validation, file munging.
- **Bash** is fine for shelling out to existing tools (`git`, `jq`, `gh`, `ffmpeg`).
- **Node** if your skill is JS-ecosystem-native (linting, prettier-style transformations).

Avoid compiled languages unless you ship a binary - and even then, prefer a script that wraps it.

## Security considerations

Scripts inside a skill run with the same privileges as the host process. That's powerful and dangerous.

**Things to be careful about:**

- **Do not bundle secrets.** API keys, tokens, credentials of any kind. If the script needs them, take them from environment variables and document that in SKILL.md.
- **Validate inputs before passing them to shell.** A naive `os.system(f"git log {user_input}")` is a command injection waiting to happen. Use `subprocess.run([...], shell=False)` with an argument list.
- **Read-only by default.** If a script writes to disk, name it loudly (`apply_migration.py`, not `process.py`) and document it in SKILL.md so Claude doesn't run it speculatively.
- **No network calls without saying so.** If a script hits an external API, mention it in SKILL.md. Users have a right to know what runs in their environment.
- **Don't paste untrusted MCP output into a shell command.** Treat it as untrusted input.

## Example: a validation script

`scripts/validate_release_notes.py`:

```python
#!/usr/bin/env python3
"""Validate that release notes match the team's template.

Exit codes:
  0  Valid
  1  Schema mismatch (missing required section)
  2  File not found

Usage:
  validate_release_notes.py --input PATH
"""
import argparse
import sys
from pathlib import Path

REQUIRED_SECTIONS = ["## Features", "## Fixes", "## Breaking changes"]

def main() -> int:
    p = argparse.ArgumentParser()
    p.add_argument("--input", required=True)
    args = p.parse_args()

    path = Path(args.input)
    if not path.exists():
        print(f"error: file not found: {path}", file=sys.stderr)
        return 2

    body = path.read_text()
    missing = [s for s in REQUIRED_SECTIONS if s not in body]
    if missing:
        print(f"error: missing required sections: {missing}", file=sys.stderr)
        return 1

    print("valid")
    return 0

if __name__ == "__main__":
    sys.exit(main())
```

And the matching line in SKILL.md:

```markdown
Run `python scripts/validate_release_notes.py --input {output_path}` before
declaring the release notes done. If it returns non-zero, fix the missing
section and re-run.
```

## When *not* to use a script

- The task is one-off. Don't write a script you'll never re-run; just put the logic in SKILL.md.
- The logic depends on context Claude has but you don't (the user's voice, the audience, the current goal). Scripts can't read the room.
- You're tempted to write a script "just in case." Resist. Add it the second time you need it, not the first.
