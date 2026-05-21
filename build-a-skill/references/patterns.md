# Patterns and approaches

These patterns emerged from skills created by early adopters and internal teams. They're common, not prescriptive.

## Problem-first vs tool-first framing

Think Home Depot. You can walk in with a problem ("I need to fix a kitchen cabinet") and an employee hands you the right tools. Or you can pick a new drill and ask how to use it.

Skills work the same way:

- **Problem-first**: "I need to set up a project workspace." Your skill orchestrates the right MCP calls in the right sequence. Users describe outcomes; the skill handles the tools.
- **Tool-first**: "I have Notion MCP connected." Your skill teaches Claude the optimal workflows. Users have access; the skill provides expertise.

Most skills lean one direction. Knowing which fits your use case helps you pick a pattern below.

## Pattern 1: Sequential workflow orchestration

Use when users need multi-step processes in a specific order.

```
## Workflow: Onboard New Customer

### Step 1: Create Account
Call MCP tool: `create_customer`
Parameters: name, email, company

### Step 2: Setup Payment
Call MCP tool: `setup_payment_method`
Wait for: payment method verification

### Step 3: Create Subscription
Call MCP tool: `create_subscription`
Parameters: plan_id, customer_id (from Step 1)

### Step 4: Send Welcome Email
Call MCP tool: `send_email`
Template: welcome_email_template
```

Key techniques: explicit step ordering, dependencies between steps, validation at each stage, rollback instructions for failures.

## Pattern 2: Multi-MCP coordination

Use when a workflow spans multiple services.

```
### Phase 1: Design Export (Figma MCP)
1. Export design assets from Figma
2. Generate design specifications
3. Create asset manifest

### Phase 2: Asset Storage (Drive MCP)
1. Create project folder in Drive
2. Upload all assets
3. Generate shareable links

### Phase 3: Task Creation (Linear MCP)
1. Create development tasks
2. Attach asset links
3. Assign to engineering team

### Phase 4: Notification (Slack MCP)
1. Post handoff summary to #engineering
2. Include asset links and task references
```

Key techniques: clear phase separation, data passing between MCPs, validation before moving to next phase, centralized error handling.

## Pattern 3: Iterative refinement

Use when output quality improves with iteration (reports, documents, designs).

```
## Iterative Report Creation

### Initial Draft
1. Fetch data via MCP
2. Generate first draft report
3. Save to temporary file

### Quality Check
1. Run validation script: `scripts/check_report.py`
2. Identify issues:
   - Missing sections
   - Inconsistent formatting
   - Data validation errors

### Refinement Loop
1. Address each identified issue
2. Regenerate affected sections
3. Re-validate
4. Repeat until quality threshold met

### Finalization
1. Apply final formatting
2. Generate summary
3. Save final version
```

Key techniques: explicit quality criteria, iterative improvement, validation scripts, and - critically - **know when to stop iterating**.

## Pattern 4: Context-aware tool selection

Use when the same outcome requires different tools depending on context.

```
## Smart File Storage

### Decision Tree
1. Check file type and size
2. Determine best storage location:
   - Large files (>10MB): Use cloud storage MCP
   - Collaborative docs: Use Notion/Docs MCP
   - Code files: Use GitHub MCP
   - Temporary files: Use local storage

### Execute Storage
Based on decision:
- Call appropriate MCP tool
- Apply service-specific metadata
- Generate access link

### Provide Context to User
Explain why that storage was chosen
```

Key techniques: clear decision criteria, fallback options, transparency about choices.

## Pattern 5: Domain-specific intelligence

Use when your skill adds specialized knowledge beyond raw tool access.

```
## Payment Processing with Compliance

### Before Processing (Compliance Check)
1. Fetch transaction details via MCP
2. Apply compliance rules:
   - Check sanctions lists
   - Verify jurisdiction allowances
   - Assess risk level
3. Document compliance decision

### Processing
IF compliance passed:
  - Call payment processing MCP tool
  - Apply appropriate fraud checks
  - Process transaction
ELSE:
  - Flag for review
  - Create compliance case

### Audit Trail
- Log all compliance checks
- Record processing decisions
- Generate audit report
```

Key techniques: domain expertise embedded in logic, compliance before action, comprehensive documentation, clear governance.
