---
description: |
  Analyse every new or updated pull request, detect the primary programming
  language(s) involved (C#, Angular/TypeScript, SQL), and create targeted
  agent tasks assigned to the specialist agent for each language found.

# Trigger - when should this workflow run?
on:
  workflow_dispatch:  # Manual trigger (also covers existing open PRs)
  pull_request:
    types: [opened, synchronize, reopened]

# Permissions
permissions:
  contents: read
  issues: read
  pull-requests: read

# Tools
tools:
  github:
    toolsets: [default, pull_requests]

# Network access
network: defaults

# Outputs
safe-outputs:
  add-comment:           # Post a triage summary on the PR
  create-agent-task:     # Delegate specialist review tasks to agents
    max: 1               # Up to one task per language found

---

# Pull Request Code Reviewer

Analyse the pull request and dispatch specialist agent tasks based on the
languages detected in the diff.

## Agent roster

| Language / scope | Agent to assign |
|---|---|
| C# / .NET | `c#-expert` |
| Angular / TypeScript / frontend | `ai-team-dev` |
| SQL / database | `ai-team-dev` |
| General / mixed / unclear | `code-reviewer` |

## Instructions

When this workflow is triggered on a new or updated pull request, follow these
steps **in order**:

### 1. Read the pull request

- Fetch the PR title, description, and the full diff (all changed files).
- Note each file extension and the technologies referenced in the diff:
  - `.cs`, `.csproj`, `.sln`, `Program.cs`, `Startup.cs`, etc. → **C#**
  - `.ts`, `.html`, `.scss`, `.css`, `angular.json`, `package.json`, component/service/module files → **Angular / TypeScript**
  - `.sql`, migration files, EF Core `DbContext`/`DbSet` changes, raw SQL strings → **SQL**
  - Anything that does not fit the above → **General**

### 2. Identify languages present

Build a list of **distinct** languages found in the diff. A PR may contain more
than one language (e.g., a feature that adds a C# API endpoint *and* an Angular
component *and* a SQL migration).

### 3. Create one agent task per language

For each language identified, create a separate agent task with:

- **Title**: `[<Language>] Code review: <PR title>`
- **Body**: Include the PR URL, a short summary of the changes relevant to that
  language, and specific review goals listed below.
- **Assigned agent**: use the mapping in the *Agent roster* table above.

#### Review goals per language

**C# (`c#-expert`)**
- Check for correctness, null safety, and proper async/await usage.
- Verify SOLID principles and design patterns are applied correctly.
- Identify missing or weak unit tests; flag untested public APIs.
- Flag security risks (unvalidated inputs, secrets in code, SQL injection via
  string concatenation).

**Angular / TypeScript (`ai-team-dev` — Nova frontend role)**
- Check component architecture, state management, and template bindings.
- Identify type-safety issues (`any`, missing types, unsafe casts).
- Flag accessibility problems (missing ARIA, non-semantic HTML).
- Review module structure, lazy loading, and unnecessary re-renders.

**SQL (`ai-team-dev` — Sage backend role)**
- Review query correctness, joins, and filtering logic.
- Identify missing indexes, N+1 risks, or cartesian products.
- Flag data-loss risks (missing WHERE clauses on UPDATE/DELETE, unsafe
  migrations).
- Check that parameterised queries are used (no raw string interpolation).

**General / mixed (`code-reviewer`)**
- Review for correctness bugs, regressions, and security risks.
- Check for missing tests and unsafe assumptions.
- Provide an overall maintainability assessment.

### 4. Post a triage summary comment on the PR

After creating all agent tasks, add a single PR comment with:

- A one-paragraph summary of what was found in the diff.
- A table listing each language detected, the agent task created, and the
  assigned agent.
- Any cross-cutting concerns (e.g., missing tests across languages, security
  risks that span C# and SQL).
