# Planning and design

## Start with 2-3 concrete use cases

Before writing anything, identify 2-3 use cases the skill should enable. Use this shape:

```
Use Case: Project Sprint Planning
Trigger: User says "help me plan this sprint" or "create sprint tasks"
Steps:
  1. Fetch current project status from Linear (via MCP)
  2. Analyze team velocity and capacity
  3. Suggest task prioritization
  4. Create tasks in Linear with proper labels and estimates
Result: Fully planned sprint with tasks created
```

Ask:

- What does a user want to accomplish?
- What multi-step workflow does this require?
- Which tools are needed (built-in or MCP)?
- What domain knowledge or best practices should be embedded?

## The three common categories

### Category 1: Document & Asset Creation

Consistent, high-quality outputs (docs, slides, sheets, code, designs).

Example: `frontend-design` skill - "Create distinctive, production-grade frontend interfaces..."

Key techniques: embedded style guides, template structures, quality checklists, no external tools required.

### Category 2: Workflow Automation

Multi-step processes that benefit from consistent methodology, often spanning multiple MCP servers.

Example: `skill-creator` skill itself.

Key techniques: step-by-step workflows with validation gates, templates, iterative refinement loops, built-in review steps.

### Category 3: MCP Enhancement

A workflow layer on top of an existing MCP server.

Example: Sentry's `sentry-code-review` skill - automatically analyzes and fixes bugs in GitHub PRs using Sentry's MCP.

Key techniques: coordinates multiple MCP calls in sequence, embeds domain expertise, provides context users would otherwise have to specify, handles common MCP errors.

## Define success criteria

These are rough benchmarks, not hard thresholds. Expect a vibes element.

### Quantitative signals

- **Skill triggers on >90% of relevant queries.** Run 10-20 test prompts that should trigger it. Count how many actually load it without explicit invocation.
- **Completes the workflow in X tool calls.** Compare same task with and without the skill. Count tool calls and tokens.
- **0 failed API calls per workflow.** Monitor MCP server logs during test runs; track retries and error codes.

### Qualitative signals

- Users don't have to prompt Claude about next steps.
- Workflows complete without user correction.
- Results are consistent across sessions and across users.

### Pro tip

Iterate on a single hard task until Claude succeeds with the skill, then extract the winning approach. This gives you faster signal than broad testing and leverages Claude's in-context learning. Expand to multiple tests for coverage once you have a working foundation.
