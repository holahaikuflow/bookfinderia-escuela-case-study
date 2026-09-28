# BookFindería Escuela — Technical Case Study

**BookFindería Escuela is a production B2B EdTech platform for Chilean schools, currently running pilots with two schools in Chile.**

The platform helps students discover books aligned with their interests while giving teachers visibility into reading choices, progress, and abandonment patterns.

I am the founder and sole product engineer behind the product, and I built it end-to-end across product strategy, UX, frontend, backend, databases, privacy, recommendation logic, infrastructure, testing, and deployment.

[View the live product](https://bookfinderia.cl/escuela)

> This repository contains a public technical case study. The production source code remains private because it includes commercial code, internal documentation, and shared infrastructure.

## My role

I am the founder, CEO, and sole product engineer behind BookFindería Escuela.

I have worked across the complete product lifecycle:

- Product discovery and scope definition
- UX and interface design
- Frontend development
- Backend and database architecture
- Authentication and authorization
- Security and privacy
- Recommendation logic
- AI-assisted workflows
- Infrastructure and deployment
- Automated and manual testing
- Production verification
- Catalog engineering
- Technical documentation

The goal has been to take the product from an initial problem hypothesis to a deployed system being tested in real school environments.

## The problem

Traditional school reading programs often have limited visibility into what students actually want to read, whether they found a book, whether they started it, and why they abandon it.

BookFindería Escuela was designed around a practical goal defined with a working language teacher:

> “No student should be left without choosing a book.”

Instead of beginning with literary knowledge, the product begins by understanding student interests and using those signals to help them choose books that are both relevant and accessible.

## What I built

### Student experience

- Anonymous access through QR codes
- Alias + PIN access without requiring personal accounts
- No email, national ID, or real name required
- Seven-question interest-based quiz
- Three personalized book recommendations
- Explanations connected to actual student preferences
- Book details and availability guidance
- Reading-status tracking
- Persistent sessions
- Explicit confirmation before changing books
- Reading-abandonment tracking

### Teacher experience

- Registration and authentication
- Password recovery
- Google authentication
- Course creation and management
- QR projection mode for classroom onboarding
- Classroom progress dashboard
- Student reading-status visibility
- Exception-based alerts
- Student alias management
- Book-level teacher guides
- Reading-abandonment reasons
- Assessment creation
- Printable tests
- Course-level access controls validated on the server

### Administration

- School and teacher visibility
- Single-use invitation codes
- Session recovery
- Operational metrics
- Anomaly detection
- Retry logic with backoff for failed writes
- Administrative controls for school-level workflows

## Product walkthrough

### Public landing

The public landing explains the product to school leaders and teachers, including the reading problem, the recommendation experience, the curated catalog, and the privacy model.

![BookFindería Escuela public landing](screenshots/01-landing.png)

### Student quiz

Students answer seven questions about their interests and preferences.

The quiz is designed to understand what they may want to read rather than test literary knowledge.

![BookFindería Escuela student quiz](screenshots/02-student-quiz.png)

### Personalized recommendations

The recommendation engine returns three books aligned with the student's answers and explains why each title may be relevant.

![BookFindería Escuela student recommendations](screenshots/03-student-recommendations.png)

### Teacher dashboard

Teachers can review classroom progress, identify students who may need attention, and follow reading status without manually asking every student.

![BookFindería Escuela teacher dashboard](screenshots/04-teacher-dashboard.png)

## Architecture

BookFindería Escuela shares a repository, database, and selected infrastructure with the consumer BookFindería product.

This reduced infrastructure duplication and allowed both products to reuse catalog and data capabilities, but it also introduced an important architectural constraint:

> A student or teacher inside `/escuela` must not accidentally reach consumer-facing functionality.

To maintain product isolation:

- School-specific components were created instead of directly reusing consumer interfaces
- Routes and navigation were audited
- Shared dependencies were reviewed before deployment
- Compiled JavaScript bundles were inspected for unintended imports
- Complete production flows were tested with browser automation
- Cross-product changes were reviewed before deployment
- Separate Git worktrees were used to coordinate development across the two product surfaces

## Technology stack

### Application

- React
- TypeScript
- Vite
- Tailwind CSS

### Backend and data

- Supabase
- PostgreSQL
- Row Level Security
- Authentication and authorization
- REST-based data access

### Infrastructure

- Cloudflare Pages
- Git
- GitHub
- Git worktrees

### Engineering and verification

- Automated browser testing
- Production route verification
- Database validation
- Deployment verification
- AI-assisted engineering workflows

## Technical documentation

- [System architecture](docs/architecture.md)
- [AI-assisted engineering workflow](docs/ai-assisted-workflow.md)

## Privacy by design

Student privacy is an architectural constraint rather than an afterthought.

The platform does not require students to provide:

- Email addresses
- National identification numbers
- Real names
- Personal student accounts

Students participate using aliases and PINs.

Teachers can manage inappropriate aliases without breaking the relationship between the student's reading activity and the course.

This approach minimizes the amount of personal information handled by the platform while preserving the functionality required for classroom use.

## Selected engineering results

The project has involved significant work beyond the core recommendation interface.

Selected results include:

- Expanded the active school catalog from 14 to **405 books**
- Cross-referenced the catalog with official education sources and reading plans from **23 Chilean schools**
- Generated and verified **162 teacher guides**
- Corrected **131 production book covers**
- Identified and fixed poor descriptions affecting **44% of the initial school catalog**
- Reduced image-loading latency from approximately **0.52–0.63 seconds to 0.025 seconds** for locally available covers
- Detected a shared image-path problem affecting **100% of the school catalog grid**
- Audited interactive elements across more than 10 product screens
- Found and corrected **20 accessibility failures across 12 files**
- Reduced a 44-student mobile dashboard from approximately **3,800px to 1,470px** in collapsed height

## Engineering case studies

Several production issues helped shape the engineering practices used in the project.

### A data point that was almost published incorrectly

A reading statistic was nearly published using a mathematics result from the same assessment.

The mistake led to a stricter evidence rule:

> No public statistic is published without a primary source verified in extractable text.

This became part of the product's research and data-validation workflow.

### The bug that was not a bug

A reported “broken scroll” could not be reproduced technically.

Browser instrumentation confirmed that scrolling worked correctly.

The actual problem was affordance: the interface did not clearly communicate that more content existed below.

The final solution was visual rather than behavioral.

### A cosmetic loading issue that revealed a performance problem

A blank book-cover placeholder led to the discovery that the school catalog grid was routing images through an external proxy even when optimized local files were available.

Correcting the image path reduced loading time for locally available covers from approximately 0.52–0.63 seconds to 0.025 seconds.

### A repeated contrast issue that became a system audit

Three visually weak interactive elements appeared during product review.

Instead of applying another isolated fix, the complete interface was audited using WCAG contrast calculations.

The audit identified 20 failures across 12 files, which were corrected under a shared accessibility standard.

## AI-assisted engineering workflow

AI coding tools are part of the development workflow, but they do not make autonomous production decisions.

The product has been developed using Claude Code sessions operating across separate Git worktrees for:

- Consumer BookFindería
- BookFindería Escuela

The environments share infrastructure and data, so development is coordinated through a documented protocol.

AI tools are used for:

- Repository analysis
- Implementation support
- Alternative evaluation
- Debugging
- Test generation
- Documentation
- Catalog-quality workflows

Human approval remains required for:

- Architecture
- Database changes
- Product scope
- Security decisions
- Data interpretation
- Deployment
- Production verification

The engineering principle is simple:

> AI accelerates implementation. Technical judgment remains human-controlled.

## Verification practices

A change is not considered complete only because it exists in the repository.

Production verification includes:

- Comparing local and deployed bundle hashes
- Testing routes individually
- Running browser automation against the production domain
- Reviewing screenshots visually
- Querying the live database
- Validating data mutations
- Removing test data
- Confirming cleanup independently

This is particularly important because the product combines frontend behavior, database access, authorization, recommendation logic, and shared infrastructure.

## Current status

BookFindería Escuela is deployed and currently running pilots with **two schools in Chile**.

The product is being validated in real educational settings while I continue iterating on student, teacher, and administration workflows based on pilot feedback.

The current stage is product validation, not product-market fit.

## Repository scope

This repository intentionally does not include:

- Production source code
- Database credentials
- Internal SQL
- School or student data
- Commercial documents
- Private operational documentation
- Complete catalog datasets
- Private prompts or environment configuration

The purpose of this repository is to document the engineering decisions, architecture, product workflows, and lessons behind the production system without exposing proprietary or sensitive material.

## Links

- [BookFindería Escuela](https://bookfinderia.cl/escuela)
- [BookFindería](https://bookfinderia.cl)
- [HaikuFlow](https://haikuflow.com)
- [LinkedIn](https://www.linkedin.com/in/victorurrutiacisterna/)
- [Email](mailto:hola@haikuflow.com)
