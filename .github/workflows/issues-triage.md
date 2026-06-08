---
# Trigger - run whenever a new issue is opened or reopened
on:
  issues:
    types: [opened, reopened]

# Permissions
permissions:
  contents: read
  issues: read

# Tools
tools:
  github:
    toolsets: [context, issues]

# Network access
network: defaults

# Outputs
safe-outputs:
  add-comment:
  add-labels:
  assign-to-user:

---

# issues-triage

Triage newly opened issues: apply area and kind labels, set priority, detect duplicates, ask clarifying questions, and assign to the right team member.

## Labels

**Area labels** (pick all that apply based on issue content):
- `area:aspnetcore` – ASP.NET Core, web APIs, middleware, Razor
- `area:angular` – Angular framework, components, TypeScript frontend
- `area:csharp` – C# language, .NET, console apps, libraries
- `area:cplusplus` – C++ language, native code, memory management
- `area:java` – Java language, Spring, Maven/Gradle
- `area:ansible` – Ansible playbooks, roles, automation
- `area:sql` – SQL queries, databases, schema design

**Kind labels** (pick exactly one):
- `kind:bug` – something is broken or behaves incorrectly
- `kind:question` – the author is asking for help or clarification
- `kind:learning-task` – a structured learning exercise or challenge
- `kind:docs` – documentation improvement or correction
- `kind:exercise` – a coding exercise or kata

**Priority labels** (pick exactly one):
- `priority:p1` – critical: blocks learning progress or is a security/data issue
- `priority:p2` – important: significant impact but has a workaround
- `priority:p3` – low: minor issue, cosmetic, or nice-to-have

## Instructions

1. **Read the issue** – carefully read the title, body, and any existing comments.

2. **Apply area labels** – identify which technology areas are involved and apply all matching `area:*` labels. If the area cannot be determined, skip area labels and ask (see step 5).

3. **Apply a kind label** – determine the nature of the issue and apply exactly one `kind:*` label:
   - If the author is stuck or asking how to do something → `kind:question`
   - If something in the repo content (exercises, docs, code samples) is wrong → `kind:bug`
   - If it is a request to improve or add documentation → `kind:docs`
   - If it is a new exercise or learning task to be created → `kind:learning-task` or `kind:exercise`

4. **Apply a priority label** – assign exactly one `priority:*` label:
   - `priority:p1` if the issue blocks a learner from making any progress, or involves incorrect/misleading core content
   - `priority:p2` if the issue is a meaningful improvement but learners can work around it
   - `priority:p3` for typos, cosmetic issues, minor suggestions, or nice-to-haves

5. **Check for duplicates** – search open issues for similar titles or descriptions. If a likely duplicate is found, add a comment mentioning the existing issue (e.g. "This looks similar to #123 – closing as duplicate if confirmed") but do **not** close it automatically.

6. **Ask clarifying questions** – if the issue is missing key information needed to act on it, post a friendly comment requesting the details. Examples:
   - For a `kind:bug`: ask for steps to reproduce, expected vs actual behaviour, and environment details.
   - For a `kind:question`: ask which specific part they are stuck on and what they have tried.
   - For a `kind:learning-task` / `kind:exercise`: ask for the target skill level and any constraints.
   - If the area cannot be determined: ask which language or technology the issue relates to.

7. **Assign to a team member** – based on the area label(s), assign the issue to the appropriate owner:
   - `area:aspnetcore` or `area:csharp` → assign to the C#/.NET maintainer
   - `area:angular` → assign to the frontend maintainer
   - `area:cplusplus` → assign to the C++ maintainer
   - `area:java` → assign to the Java maintainer
   - `area:ansible` → assign to the DevOps maintainer
   - `area:sql` → assign to the database maintainer
   - Multiple areas → assign to all relevant maintainers
   - Area unknown → skip assignment until area is clarified

8. **Post a triage summary comment** – after completing all steps, add a brief comment summarising the triage decision (labels applied, any duplicate found, questions asked, assignee). Keep it concise and friendly.

## Notes

- Run `gh aw compile` to generate the GitHub Actions workflow
- See https://github.github.com/gh-aw/ for complete configuration options and tools documentation
