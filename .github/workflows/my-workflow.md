---
description: |
  Triage new issues: label them by area, kind and priority, identify duplicates,
  ask clarifying questions when the description is unclear, and assign them to
  the right team members.

# Trigger - when should this workflow run?
on:
  workflow_dispatch:  # Manual trigger
  issues:
    types: [opened, reopened]

# Permissions - what can this workflow access?
# Write operations (creating issues, PRs, comments, etc.) are handled
# automatically by the safe-outputs job with its own scoped permissions.
permissions:
  contents: read
  issues: read
  pull-requests: read

# Tools - GitHub API access via toolsets (context, repos, issues, pull_requests)
tools:
  github:
    toolsets: [default, issues]

# Network access
network: defaults

# Outputs - what APIs and tools can the AI use?
safe-outputs:
  add-comment:           # Post clarifying questions or duplicate notices
  add-labels:            # Apply area, kind and priority labels
  assign-to-user:        # Assign issue to the right team member
  # close-issue:
  # create-issue:
  # remove-labels:
  # update-issue:

---

# Issue Triage Workflow

Automatically triage every newly opened or reopened issue in this repository.

## Instructions

When this workflow is triggered on a new or reopened issue, perform the following steps **in order**:

### 1. Read and understand the issue

- Read the issue title and full description carefully.
- Note the programming language, framework, or technology mentioned.

### 2. Apply an `area:*` label

Pick **exactly one** area label that best matches the issue topic:

| Label | When to use |
|---|---|
| `area:aspnetcore` | ASP.NET Core, .NET, C# web/API |
| `area:angular` | Angular, TypeScript, frontend |
| `area:csharp` | C# topics not related to ASP.NET Core |
| `area:cplusplus` | C++ topics |
| `area:java` | Java topics |
| `area:ansible` | Ansible, infrastructure-as-code |
| `area:sql` | SQL, databases |

If the area cannot be determined from the description, skip this label and ask a clarifying question (see step 5).

### 3. Apply a `kind:*` label

Pick **exactly one** kind label:

| Label | When to use |
|---|---|
| `kind:bug` | Something is broken or not working as expected |
| `kind:question` | Author is asking for help or an explanation |
| `kind:learning-task` | A new study topic or hands-on exercise request |
| `kind:docs` | Documentation improvement or correction |
| `kind:exercise` | A coding challenge or practice exercise |

### 4. Apply a `priority:*` label

Assign a priority based on urgency and impact:

| Label | Criteria |
|---|---|
| `priority:p1` | Blocks progress, critical bug, or time-sensitive |
| `priority:p2` | Important but not blocking; should be addressed soon |
| `priority:p3` | Nice to have, low urgency, or exploratory |

Default to `priority:p3` when there is no clear urgency signal.

### 5. Check for duplicate issues

- Search open issues for similar titles or descriptions.
- If a likely duplicate is found, add a comment mentioning the existing issue (e.g. "This looks similar to #123. Closing as duplicate if confirmed.") and apply any relevant labels. Do **not** close the issue automatically.

### 6. Ask clarifying questions if needed

If the issue description is too vague to triage confidently (missing steps to reproduce, unclear goal, no code sample), post a single comment asking for the missing information. Be concise and friendly. Examples:
- "Could you share the error message or stack trace?"
- "Which version of [technology] are you using?"
- "What is the expected vs. actual behaviour?"

### 7. Assign to the right team member

Use the area label applied in step 2 to assign the issue:

| Area | Assignee |
|---|---|
| `area:aspnetcore` | @ametekBenHur78 |
| `area:angular` | @ametekBenHur78 |
| `area:csharp` | @ametekBenHur78 |
| `area:cplusplus` | @ametekBenHur78 |
| `area:java` | @ametekBenHur78 |
| `area:ansible` | @ametekBenHur78 |
| `area:sql` | @ametekBenHur78 |

If the area is still unknown (pending clarification), skip assignment until the area is determined.