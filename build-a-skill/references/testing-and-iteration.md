# Testing and iteration

Skills can be tested at varying levels of rigor:

- **Manual testing in Claude.ai** - run queries, observe behavior. Fast, no setup.
- **Scripted testing in Claude Code** - automate test cases for repeatable validation.
- **Programmatic testing via the Skills API** - build evaluation suites that run systematically.

Pick the level that matches your skill's audience. A small internal skill needs less rigor than one deployed to thousands of enterprise users.

### Pro tip: iterate on a single task before expanding

The most effective skill creators iterate on **one** challenging task until Claude succeeds, then extract the winning approach into a skill. This leverages in-context learning and gives faster signal than broad testing. Expand to multi-case coverage only after you have a working foundation.

## Three areas to cover

### 1. Triggering tests

Goal: ensure the skill loads at the right times.

- ✅ Triggers on obvious tasks
- ✅ Triggers on paraphrased requests
- ❌ Does NOT trigger on unrelated topics

Example suite:

```
Should trigger:
  - "Help me set up a new ProjectHub workspace"
  - "I need to create a project in ProjectHub"
  - "Initialize a ProjectHub project for Q4 planning"

Should NOT trigger:
  - "What's the weather in San Francisco?"
  - "Help me write Python code"
  - "Create a spreadsheet" (unless this skill handles sheets)
```

Debug technique: ask Claude **"When would you use the [skill name] skill?"** Claude will quote the description back, and you can adjust based on what's missing.

### 2. Functional tests

Goal: verify the skill produces correct outputs.

Cover:
- Valid outputs generated
- API calls succeed
- Error handling works
- Edge cases handled

Example:

```
Test: Create project with 5 tasks
Given: Project name "Q4 Planning", 5 task descriptions
When: Skill executes workflow
Then:
  - Project created in ProjectHub
  - 5 tasks created with correct properties
  - All tasks linked to project
  - No API errors
```

### 3. Performance comparison

Goal: prove the skill improves results vs baseline.

```
Without skill:
  - User provides instructions each time
  - 15 back-and-forth messages
  - 3 failed API calls requiring retry
  - 12,000 tokens consumed

With skill:
  - Automatic workflow execution
  - 2 clarifying questions only
  - 0 failed API calls
  - 6,000 tokens consumed
```

## Iteration based on real-world signals

Skills are living documents. Plan to iterate based on:

### Undertriggering

- Skill doesn't load when it should
- Users manually enable it
- Support questions about when to use it

→ Add more detail and nuance to the description, including technical-term keywords.

### Overtriggering

- Skill loads for irrelevant queries
- Users disable it
- Confusion about its purpose

→ Add negative triggers, be more specific, narrow the scope phrase ("for X, not for Y").

### Execution issues

- Inconsistent results
- API call failures
- Users having to correct Claude

→ Improve instructions, add error handling, consider extracting deterministic checks into a script.

## Using the `skill-creator` skill

Anthropic's `skill-creator` skill (available in Claude.ai via the plugin directory, and downloadable for Claude Code) can help you generate, review, and improve skills.

It can:
- Generate skills from natural language descriptions
- Produce properly formatted SKILL.md with frontmatter
- Suggest trigger phrases and structure
- Flag common issues (vague descriptions, missing triggers, structural problems)
- Suggest test cases based on the skill's stated purpose

Use it iteratively. After hitting an edge case, bring it back:

> "Use the issues and solutions from this chat to improve how the skill handles [specific edge case]."

Note: `skill-creator` helps you design and refine skills - it does not execute automated test suites or produce quantitative evaluation results.
