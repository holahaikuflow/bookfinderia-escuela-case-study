# AI-assisted engineering workflow

## Purpose

BookFindería Escuela was developed with an AI-assisted workflow designed to increase implementation speed without giving AI agents authority over product, architecture, data, or production decisions.

The workflow combines:

- Claude Code
- Cursor
- ChatGPT
- Git worktrees
- Written coordination protocols
- Browser automation
- Manual review
- Production verification

AI tools support the engineering process, but the founder remains responsible for every production decision.

## Working configuration

Two Claude Code sessions operate in parallel on separate Git worktrees from the same repository.

- **APP session** — consumer BookFindería product
- **School session** — BookFindería Escuela B2B product

Both products share:

- The same PostgreSQL database
- The `books` catalog table
- Low-level infrastructure
- Deployment dependencies
- Some internal utilities

Because the two sessions can affect shared infrastructure, coordination is treated as an engineering problem rather than an informal conversation.

## Coordination protocol

The sessions coordinate through a shared state document:

```text
SINCRONIA.md
```

Each session must:

1. Read the document before starting a task
2. Check whether another session has changed shared infrastructure
3. Record relevant changes after completing work
4. State whether a change is planned or already applied
5. Include the related commit and date when appropriate

The founder does not manually relay every message between sessions. The protocol provides a durable shared source of truth.

A separate planning session is used to:

- Define product direction
- Review technical reports
- Compare implementation alternatives
- Approve or reject changes
- Authorize production deployments

## Responsibility boundaries

AI agents may assist with:

- Repository analysis
- Implementation proposals
- Code generation
- Refactoring suggestions
- Debugging
- Test generation
- Documentation
- Catalog-quality analysis
- Browser automation
- Alternative comparison

AI agents may not independently decide:

- Product scope
- Architecture
- Database migrations
- Production deployment
- Legal interpretation
- Public data claims
- Destructive operations
- Final validation

Human approval remains required before any production release.

## Deployment rules

### Explicit approval

No deployment is allowed without explicit human approval.

This rule applies even when the stated purpose is only to test, inspect, or measure something.

The rule was strengthened after an AI session ran a Cloudflare deployment command believing it would create a harmless preview. Because the local Git branch was already `main`, the command deployed directly to production.

The deployment caused no damage, but it demonstrated that an agent's interpretation of a command is not sufficient evidence that the operation is safe.

### Sequential deployments

Deployments are executed one at a time.

Parallel deployments are prohibited because both products share infrastructure and deployment artifacts.

This rule was introduced after a corrupted deployment manifest incident.

### Applied versus planned changes

Every coordination message involving shared infrastructure must state whether the change is:

- **APPLIED** — already active, with commit and date
- **PLANNED** — not yet active, with the conditions required before execution

This distinction prevents one session from treating an already-applied database change as a future proposal, or treating a proposal as production reality.

## Git safety

The project uses separate branches and worktrees for parallel work.

A Git anti-divergence protocol was introduced after discovering that 39 commits from the school branch were not reaching production.

The protocol requires:

- Confirming the active branch before implementation
- Checking branch divergence before deployment
- Reviewing the commit range included in a release
- Confirming that expected school commits are present
- Avoiding assumptions based only on the working directory
- Verifying the deployed bundle rather than trusting branch state

## Database safety

The database is shared by the consumer and school products.

AI sessions are not allowed to reconcile migration history autonomously.

The project rules include:

1. Do not run `supabase db push`
2. Do not run migration repair automatically
3. Apply reviewed DDL through controlled SQL execution
4. Preserve the SQL file in version control
5. Treat migration-history reconciliation as a human decision

These controls exist because a technically valid migration command can still be unsafe when multiple products share infrastructure but have different migration histories.

## Verification model

A task is not considered complete because:

- The code was written
- Tests passed locally
- The change exists in `main`
- An agent reported success
- A document says the work was completed

Verification must be based on the real system.

The production verification process includes:

- Comparing the local build and production bundle hashes
- Testing routes individually
- Running browser automation against the live domain
- Reviewing screenshots visually
- Querying the production database
- Removing test records
- Confirming cleanup through a separate query

## Human reports override automated confidence

If a real user reports a visible problem, the report is treated as valid until evidence from the user's actual environment disproves it.

Automated browser tests may scroll programmatically, use different viewport behavior, or fail to reproduce a real interaction problem.

This principle led to the correct diagnosis of a reported broken-scroll issue.

Automated measurements showed that the interface could scroll correctly. Direct browser instrumentation later demonstrated that the technical scroll behavior was working.

The real issue was visual affordance: the interface did not communicate that more content existed below.

The final solution was a visual indicator and additional spacing, not another scroll implementation.

## Documented decisions are not proof of implementation

The workflow distinguishes between:

- A decision that was documented
- Code that was committed
- A change that was deployed
- Behavior verified in production

These states are not interchangeable.

Several project tasks had previously been described as completed in reports even though the corresponding change was not present in the live database.

For that reason, verification is performed against production rather than against memory or documentation alone.

## AI output verification

Generated content is not accepted through self-review by the same model alone.

For example, teacher guides were subjected to a second independent factual-verification pass.

When repeated factual errors remained in a guide for *The Count of Monte Cristo*, automated generation was abandoned for that title and the guide was written manually.

This produced a permanent rule for future assessment generation:

> If AI-generated questions affect a graded evaluation, a teacher must approve every question before use.

## Cost controls

No paid API batch is executed without:

- Estimating the expected cost
- Explaining the purpose
- Receiving explicit approval

This prevents experimental AI workflows from creating uncontrolled operational costs.

## Operating principles

The workflow is governed by several principles:

- AI accelerates work but does not own decisions
- Verify against the live system
- Distinguish estimated results from verified results
- Diagnose patterns before applying isolated patches
- Treat shared infrastructure as a coordination risk
- Require approval before destructive or production operations
- Record rules created from incidents
- Re-read evidence instead of trusting memory
- Prefer explicit state over informal assumptions

## What this workflow demonstrates

This project does not use AI only as an autocomplete tool.

It demonstrates the ability to:

- Coordinate multiple coding agents
- Define operational boundaries
- Detect incorrect agent assumptions
- Design human approval gates
- Manage shared infrastructure risk
- Verify generated work independently
- Convert incidents into durable engineering rules
- Preserve human accountability for production systems

The objective is not maximum automation.

The objective is reliable delivery with AI-assisted leverage.
