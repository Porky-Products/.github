# Porky GitHub Copilot Organization Instructions
## Purpose

These instructions provide GitHub Copilot and other approved development agents with an actionable version of the organization’s GitHub developer conventions.

Apply these instructions when asked to create/update an issue; suggest issue labels, type, priority, scope, or risk; create or name a branch; draft or improve a commit message; organize commits; create or update a pull request; review a pull request; rebase or merge a branch; create or update GitHub Actions workflows; prepare a deployment, release, version, or hotfix; draft an Architecture Decision Record; plan or evaluate a technical spike; or recommend an exception to the normal workflow.

---

## Instruction Priority

Instructions use the following markers:

- **`[POLICY]`** — Must be followed unless an approved exception applies.
- **`[GUIDELINE]`** — Should normally be followed but may be adapted when the repository or task requires a different approach.
- **`[CONDITIONAL]`** — Applies only when the stated condition is true.

Repository-specific instructions may add stricter requirements.

Do not silently override organization policy with a repository convention. When instructions conflict or are unclear, identify the conflict and request clarification rather than guessing.

---

## Agent Integrity and Evidence

These requirements apply to every task.

- **`[POLICY]`** Never invent issue IDs, test results, approvals, deployment results, screenshots, logs, version numbers, build numbers, or runtime evidence, and never claim that a test, build, review, deployment, migration, or release succeeded unless supporting evidence is available.
- **`[POLICY]`** Never expose or reproduce credentials, passwords, tokens, API keys, cookies, private keys, customer data, order data, or other sensitive values.
- **`[POLICY]`** Do not present unverified assumptions as established facts. Do not push directly to `main`. Do not substitute an AI review for a required human review on High-risk changes.
- **`[GUIDELINE]`** Prefer the smallest safe and complete change that solves the stated problem, and keep work traceable from issue through branch, commit, PR, review, CI, merge, deployment, and issue closure.
- **`[GUIDELINE]`** Use `N/A — <reason>` when a required field does not apply, and state what was not tested, not inspected, or not proven instead of hiding missing evidence.
- **`[GUIDELINE]`** Avoid unnecessary process, but do not remove controls required by the change’s risk.

---

# 1. Repository Setup

A production repository should allow a new developer to clone, run, test, and safely modify the project within minutes.

## Required Files for Production Repositories

When evaluating or creating a production repository, confirm that the following files or folders exist:

| File or Folder | Expected Content |
|---|---|
| `README.md` | Purpose, owner team, local setup, required runtimes, common commands, and deployment/readiness notes |
| `.gitignore` | Build output, local configuration, IDE files, logs, secrets, and other local artifacts |
| `.env.example` | Placeholder-only local configuration; never real secret values |
| `docs/` | Relevant documentation such as `api/`, `adr/`, `deployment/`, `development/`, `maintenance/`, or `user-guides/` |

## Suggested Files

Consider these files when applicable:

| File or Folder | Purpose |
|---|---|
| `.github/dependabot.yml` | Dependency update and security update automation |
| `.github/workflows/*.yml` | CI or other repository automation |
| `.github/copilot-instructions.md` | Per-Project organization-approved Copilot instructions |

- **`[GUIDELINE]`** Scale repository automation to the repository’s importance and production impact.
- **`[GUIDELINE]`** Do not add unnecessary CI or release automation to documentation-only, archival, or non-executable repositories.
- **`[POLICY]`** Do not place real credentials or sensitive values in repository documentation or example configuration.

---

# 2. Standard Developer Workflow

The standard workflow is:

```text
Issue
→ Branch
→ Commit
→ Pull Request
→ Review
→ CI
→ Merge
→ Deploy
→ Close Issue
```

Not every repository or change requires every step, but production-impacting work should follow this flow unless a documented exception applies.

| Step | Expected Action | Purpose |
|---|---|---|
| Issue | Create or confirm the issue or work item | Makes the reason for the change visible and trackable |
| Branch | Create a correctly named working branch | Isolates work from long-lived branches |
| Commit | Create clear, atomic commits | Makes history understandable, reviewable, and revertible |
| Pull Request | Open a PR with useful reviewer information | Provides context and gives CI a place to validate the change |
| Review | Address feedback and obtain required review | Improves correctness and team awareness |
| CI | Ensure required checks pass | Prevents known broken code from merging |
| Merge | Use the approved merge strategy | Integrates reviewed and validated changes into the correct long-lived branch |
| Deploy | Use an organized release process | Keeps production changes intentional and traceable |
| Close Issue | Close or update the linked issue | Keeps work tracking current |

---

# 3. Issues and Work Tracking

Issues define, prioritize, and track work before it becomes code.

A useful issue should explain the problem or requested outcome, why the work matters, expected behavior or acceptance criteria, relevant context and constraints, priority, primary issue type, area/scope when useful, and risk when the work affects security, data, infrastructure, deployment, or production behavior.

Use issues for non-trivial work such as bugs, features, technical debt, spikes/research, refactors, documentation, CI/CD changes, infrastructure changes, dependency upgrades, security changes, and production follow-up work.

## Issue Types

Every non-trivial issue should have one primary type.

| Issue Type | Use For |
|---|---|
| `bug` | Broken, unexpected, or incorrect behavior |
| `feature` | New user-facing or business functionality |
| `chore` | Maintenance that does not directly change product behavior |
| `tech-debt` | Cleanup or improvement that reduces future engineering risk |
| `spike` | Time-boxed research, prototype, or feasibility work |
| `docs` | Documentation-only work |
| `security` | Security hardening, vulnerability remediation, or sensitive access changes |
| `ci` | CI/CD, GitHub Actions, build, or automation work |
| `infra` | Infrastructure, hosting, runners, deployment platform, or environment work |

Use `feature` as the issue type but `feat` as the corresponding branch or commit type.

## Issue Labels

- **`[POLICY]`** Each non-trivial issue should have a type and priority.
- **`[GUIDELINE]`** Add area or scope labels when useful.
- **`[GUIDELINE]`** Add a risk label when work affects security, data, deployment, infrastructure, internal systems, or production behavior.
- **`[GUIDELINE]`** Avoid adding labels that do not improve filtering, routing, planning, or reporting.

## Priority

| Priority | Meaning |
|---|---|
| `p0` | Critical issue actively blocking production, security, or major business operations |
| `p1` | High-priority issue that should be handled soon |
| `p2` | Normal planned work |
| `p3` | Low-priority, backburner, or nice-to-have work |

Do not confuse priority with effort. A small issue can be urgent, and a large issue can be low priority.

---

# 4. Branch Naming

## Format

Use:

```text
<type>/<issue-id>-<short-description>
```

Examples:

```text
feat/MOE-123-catalog-search
fix/MOE-247-order-rounding
security/MOE-330-tighten-session-handling
hotfix/MOE-500-login-outage
```

When no tracked issue exists and the change is genuinely trivial:

```text
docs/update-readme
chore/update-gitignore
```

## Branch Naming Rules

- **`[POLICY]`** Use an existing issue ID when one is available; never invent one.
- **`[GUIDELINE]`** Use lowercase kebab-case, one to three meaningful words in the short description.
- **`[GUIDELINE]`** Do not use a developer’s name as the primary branch identifier, and avoid vague names such as `fix`, `changes`, `stuff`, `final`, `new-work`, `my-branch`, `test123`.

## Allowed Branch and Commit Types

| Type | Use For |
|---|---|
| `feat` | New user-facing or business functionality |
| `fix` | Bug fix or correction to existing behavior |
| `chore` | Maintenance without direct product behavior change |
| `docs` | Documentation-only changes |
| `test` | Test-only changes or test coverage improvements |
| `refactor` | Internal code cleanup with no intended behavior change |
| `perf` | Performance or optimization work |
| `ci` | CI/CD or GitHub Actions workflow changes |
| `spike` | Investigative or prototype work |
| `hotfix` | Urgent production fix |
| `build` | Build tooling, packaging, or dependency pipeline work |
| `revert` | Reverting a prior change |
| `release` | Large release preparation or staging work when needed |
| `style` | Formatting-only changes with no behavior impact |
| `security` | Security hardening or vulnerability fix |

---

# 5. Commit Messages

## Format

Use:

```text
<type>(<scope>): <description>
```

The scope is optional.

Examples:

```text
feat(catalog): add customer availability filtering
fix(auth): preserve the middleware session cookie
test(orders): add duplicate-submit regression coverage
ci(actions): add pull request build checks
docs(onboarding): document local setup
chore(deps): update TypeScript
security(auth): restrict privileged session access
```

## Common Scopes

| Scope | Use For |
|---|---|
| `auth` | Login, sessions, permissions, or user identity |
| `ui` | Shared UI components, layout, or visual behavior |
| `api` | API routes, request/response handling, or backend integration |
| `db` | Database schema, migrations, or queries |
| `ci` / `actions` | CI checks, deployment workflows, or GitHub Actions |
| `deps` | Dependency updates |
| `docs` | Documentation |
| `config` | App configuration, environment templates, or runtime settings |
| `infra` | Infrastructure, hosting, runners, or deployment |
| `tests` | Test setup or coverage |

Repository-specific feature scopes may also be used (e.g. `catalog`, `orders`, `checkout`, `pricing`, `notifications`, `scanner`, `mobile`).

## Commit Quality

A good commit should represent one logical change, use the correct type and a meaningful scope, clearly explain what changed (concise but not vague), leave the application buildable and testable where practical, avoid unrelated formatting/dependency/generated-file/behavior changes, and use a commit body when the reason or tradeoff is not obvious.

## Do and Don’t

| Do Not Use | Prefer |
|---|---|
| `fix` | `fix(orders): correct total rounding` |
| `update` | `chore(deps): update TypeScript` |
| `changes` | `refactor(api): isolate response normalization` |
| `more tests` | `test(auth): add expired-session coverage` |
| `wip` | A descriptive logical commit or squash before merge |
| A paragraph-long subject | A concise subject with context in the body |

## Commit Body

Use a commit body when necessary:

```text
fix(auth): preserve the middleware session cookie

The client previously discarded Set-Cookie responses, causing each
request to create a new middleware session.

Refs #247
```

The subject explains what changed. The body explains why.

## Commit Atomicity

- **`[POLICY]`** Each commit should represent one logical change.
- **`[GUIDELINE]`** Keep commits independently understandable and revertible; leave the application working and tests passing after each commit where practical.
- **`[GUIDELINE]`** Do not mix unrelated feature, dependency, formatting, database, and CI work merely to reduce commit count. Prefer a clear revert commit over silently rewriting shared history.

---

# 6. What Must Not Be Committed

Do not commit generated or local-only artifacts unless the repository explicitly requires them. Common examples: `node_modules/`, `dist/`, `build/`, `coverage/`, `.env`, `.DS_Store`, temporary logs, local IDE state, local build output, personal AI scratch files, and temporary prompt files.

Organization-approved instruction files may be committed (e.g. `.github/copilot-instructions.md`).

## Secrets

- **`[POLICY]`** Never commit credentials, passwords, API keys, tokens, cookies, private keys, or service-account material. `.env.example` must contain placeholders only.
- **`[POLICY]`** Production and shared secrets belong in GitHub Secrets, GitHub Environments, or another approved secrets manager. Do not include sensitive values in issues, commit messages, PR descriptions, logs, screenshots, fixtures, or documentation.
- **`[GUIDELINE]`** Local development secrets may be stored in ignored `.env` files. Before drafting or reviewing a PR, check whether logs, screenshots, fixtures, or examples contain sensitive data.

---

# 7. Branching Strategy

## Long-Lived Branches

The organization uses a develop-branch-first workflow. `main` is the source of truth for production code; `develop` is the staging and integration branch used for production-like validation and additional QA. Other branches should normally be short-lived.

## Branch Rules

- **`[POLICY]`** Do not push directly to `main`, force-push to `main`, or delete `main`. Create ordinary working branches from the latest `develop` branch.
- **`[GUIDELINE]`** Do not start a new short-lived branch from another short-lived branch unless the dependency is intentional and documented.
- **`[GUIDELINE]`** Merge short-lived branches promptly (preferably daily); if a working branch cannot be merged promptly, rebase it onto the latest `develop` branch regularly to keep branch lifetime short and reduce conflicts.
- **`[GUIDELINE]`** If work was mistakenly based on `main`, create the correct branch from `develop` and cherry-pick the relevant commits.

---

# 8. Rebase Instructions

When asked to update a working branch with the latest `develop` branch, use:

```bash
git fetch origin
git switch <working-branch>
git rebase origin/develop
```

When conflicts occur:

```bash
# Resolve the affected files
git add <resolved-files>
git rebase --continue
```

To cancel the rebase, use `git rebase --abort`. After rebasing a branch that was already pushed, use `git push --force-with-lease`.

- **`[POLICY]`** Do not rebase `main`, and do not rebase `develop` merely to update an individual working branch.
- **`[GUIDELINE]`** Rebase only branches you own or branches whose collaborators have coordinated the history rewrite. Use `--force-with-lease`, not plain `--force`, for a previously pushed working branch.

---

# 9. Pull Requests

A PR should be opened for each focused unit of work, including features, bug fixes, refactors, security changes, documentation updates, test changes, spikes, CI/CD changes, infrastructure changes, and hotfixes.

Do not describe every PR as a feature.

## PR Description Requirements

Every PR should include:

```md
## Summary

- <Brief breakdown of what changed>

## Why is this change needed?

<Explain the problem or requirement being addressed. Mention meaningful alternatives when applicable.>

## How was it tested?

- `<Exact command or verification method>` — `<Observed result>`
- `<Exact command or verification method>` — `<Observed result>`

## Risk Level

`Low | Medium | High`

**Risk rationale:** <Explain why>

## Linked Issue

`Closes #___ | Refs #___ | Part of #___ | N/A — <reason>`

## Screenshots / Logs

<Links or `N/A — no visual or log evidence required`>

## Deployment Notes

<Environment variables, configuration, migration, deployment, versioning, or `N/A — no deployment impact`>

## Rollback Notes

<Required for High-risk changes. Otherwise use `N/A — <reason>`.>
```

## PR Authoring Rules

- **`[POLICY]`** Include every required heading; use exact test commands and state the observed result of each check.
- **`[POLICY]`** Do not claim all tests passed without evidence, and never invent links, issue IDs, screenshots, logs, or deployment results.
- **`[POLICY]`** Identify the PR’s risk level and include rollback notes for High-risk changes.
- **`[GUIDELINE]`** Keep the PR body proportional to the change — concise for small changes, more detailed evidence for complex or High-risk ones.
- **`[GUIDELINE]`** Prefer structured evidence over repeated narrative; use `N/A — <reason>` instead of leaving a section blank; redact sensitive information from logs and screenshots.

## Issue Linking

Use `Closes #123` only when the PR fully completes the issue. Use `Refs #123` or `Part of #123` when the PR is related to a larger issue but does not fully complete it.

---

# 10. Pull Request Review

When reviewing a PR, inspect the actual diff and available evidence. Do not rely only on the PR description.

## Review Checklist

Determine: does the change solve the stated problem or linked issue; is the approach clear and maintainable; is the stated scope and declared risk level accurate; are tests or manual validation steps documented and do they meaningfully cover the changed behavior; are there security, data, deployment, compatibility, or rollback concerns; is sensitive information present; are undocumented behavior changes present in the diff; are material claims unsupported by evidence; does the change require human review under the risk policy; and does the PR modify CI/CD, deployment, auth, data, or internal-system boundaries.

## Review Comment Style

Clearly state whether a finding is **Blocking** (must be corrected before merge), a **Suggestion** (recommended but non-blocking), a **Question** (clarification required), or an **Observation** (relevant context with no requested action).

Avoid vague comments. Include file and location, evidence, impact, required or recommended correction, and confidence when uncertainty exists.

## Copilot Review Output

Use this format:

```md
## Review Outcome

- **Outcome:** `NO_BLOCKING_FINDINGS | CHANGES_REQUIRED | HUMAN_REVIEW_REQUIRED`
- **Assessed risk:** `Low | Medium | High`
- **Confidence:** `High | Medium | Low`
- **Human review required:** `Yes | No`

## Blocking Findings

### Finding 1

- **Severity:** `Critical | High | Medium`
- **Category:** `Correctness | Security | Data | Reliability | Testing | Compatibility | CI/CD | Deployment | Documentation`
- **Location:** `<file>:<line or range>`
- **Evidence:** <What supports the finding>
- **Impact:** <What can fail and who or what is affected>
- **Required correction:** <Specific expected resolution>
- **Confidence:** `High | Medium | Low`

## Non-Blocking Suggestions

- ...

## Unverified Claims or Missing Evidence

- ...

## Required Human Review Areas

- ...
```

Do not represent an AI review outcome as a human approval.

---

# 11. Risk Classification

Risk determines the required review, testing, documentation, and rollback controls.

| Risk | Description | Examples | Requirements |
|---|---|---|---|
| **Low** | Small, isolated changes with minimal likelihood of breaking production or exposing sensitive data | Documentation, small UI polish, simple internal cleanup, test-only changes | Copilot review; required CI checks when applicable; rollback plan not normally required |
| **Medium** | Changes that affect behavior, maintainability, dependencies, or normal application flow | Features, bug fixes, refactors, dependency updates, non-critical CI changes | Copilot review; required CI checks; rollback planning recommended, especially when behavior changes |
| **High** | Changes that affect security, production stability, data integrity, production deployment, or other internal systems | Auth/authorization, security boundaries, production deployment, DB migrations, order/customer data, secrets, infrastructure, self-hosted runners, CI/CD release paths, privileged integrations, breaking API/schema changes | Copilot review; required CI checks; at least one human reviewer; rollback plan required |

## Risk Escalation

- **`[POLICY]`** If risk is uncertain, treat the change as High risk until a reviewer or owner confirms otherwise.
- **`[POLICY]`** High-risk changes must not merge until review, testing, and rollback requirements are satisfied.
- **`[GUIDELINE]`** Do not lower risk merely to reduce review requirements.
- **`[GUIDELINE]`** Reassess risk after the final diff, not only when the PR is first opened.

---

# 12. Merging Procedures

## Default: Rebase Merge

Rebase merge is the default when the PR contains clean, atomic, meaningful commits — messages follow the convention, each commit is a useful logical step, preserving individual commits improves traceability, and the history has no unnecessary work-in-progress noise.

## Fallback: Squash Merge

Use squash merge when the branch contains `wip`, `fix`, `try again`, or similarly vague commits; review fixes created many low-value commits; the history is messy or the individual commits don't provide useful history; or the PR is clearer as one final commit.

The final squash commit must follow:

```text
<type>(<scope>): <description>
```

## Merge Rules

- **`[POLICY]`** Do not merge before required reviews and CI checks are complete; apply the review requirement associated with the final PR risk.
- **`[POLICY]`** If new work is required after a PR has merged, open a new PR.
- **`[GUIDELINE]`** Prefer rebase merge for clean history and squash merge when the commit history is not worth preserving.
- **`[GUIDELINE]`** Do not recommend a normal merge commit unless a repository-specific process or approved exception requires preserving branch topology.

---

# 13. CI/CD Guidelines

Production, shared-library, infrastructure, and automation repositories should use CI where applicable, scaled to the repository’s importance and risk.

## Minimum CI for Production Code

CI normally includes:

```text
Install or restore locked dependencies
→ Lint
→ Typecheck, when applicable
→ Test
→ Build
→ Security or dependency checks, when applicable
```

## CI Policies

- **`[POLICY]`** Required CI checks must pass before merge. Production secrets must never be exposed to PR workflows. Workflow permissions should use the least access required.
- **`[GUIDELINE]`** CI should run on pull requests by default. Protect build servers with appropriate authentication, authorization, and role-based access.
- **`[GUIDELINE]`** Integrate security testing early rather than as an afterthought. Treat CI/CD workflow, runner, permission, and deployment changes as Medium or High risk depending on impact.
- **`[GUIDELINE]`** Keep CI and production deployment responsibilities clearly separated; do not claim deployment success when only CI checks were run.

## Scheduled Workflows

Use scheduled workflows (commonly daily or weekly) for heavy end-to-end/integration tests, security scans, performance tests, dependency update validation, or other resource-intensive checks that should not block normal development.

---

# 14. Deployment and Release

Deployment and release rules apply only to repositories that deploy or publish artifacts. Such repositories should document environments, deployment triggers, deployment process, required approvals, validation steps, and rollback expectations.

## Branches and Environments

- `main` is the production source of truth.
- `develop` is used for staging and production-like validation under the develop-branch-first workflow.

## Release Tags

- **`[POLICY]`** Use release tags for meaningful release points; never reuse or invent a release tag or build number.
- **`[GUIDELINE]`** Do not tag every commit, PR, or deleted branch. Let CI generate version and build identifiers when the repository uses automated versioning.

## Automatic Versioning

Where configured, CI owns build-number generation: build numbers must be unique and monotonically increasing, and developers should not manually generate a CI-controlled build number. Release tags should reflect meaningful releases, not every test build.

When major, minor, and patch versions are used: patch = backward-compatible fix or hotfix; minor = backward-compatible feature or update; major = breaking or backward-incompatible change.

---

# 15. Hotfix Process

A hotfix is an urgent production-level fix. Urgency may expedite the process but does not remove traceability or required safety controls.

When preparing a hotfix: create a `hotfix/` branch, make the smallest safe correction, run required CI and QA checks, obtain review according to the actual risk level, deploy through the approved production process, tag the patch release when applicable, and create follow-up issues for cleanup, tests, documentation, or root-cause remediation.

- **`[POLICY]`** Do not skip required human review for a High-risk hotfix unless an approved emergency exception applies.
- **`[GUIDELINE]`** Restore stability first, then complete follow-up improvements. Keep emergency changes narrowly scoped.

---

# 16. Architecture Decision Records

Create an ADR for significant, long-term, or standards-deviating technical decisions, such as framework selection, deployment platform, authentication/session strategy, mobile/wrapper strategy, major vendor or tooling choices, breaking architecture or compatibility changes, CI/CD strategy, or significant data/integration architecture.

An ADR should document context, decision drivers, alternatives considered, the selected decision, benefits, costs/drawbacks, accepted tradeoffs, and consequences.

- **`[GUIDELINE]`** Keep ADRs concise; store them in Markdown under `docs/` or `docs/adr/`.
- **`[GUIDELINE]`** Use ADRs to encourage collaborative architectural awareness rather than relying on a single architect.

---

# 17. Spike Standards

A spike is a time-boxed research or prototype effort.

## Spike Rules

- **`[POLICY]`** Mark spike code as non-production; do not promote spike code into production without review.
- **`[GUIDELINE]`** Begin with a testable question or hypothesis and a narrow scope; use the simplest proof of concept needed to answer the question.
- **`[GUIDELINE]`** Time-box the spike (a few days to a small number of weeks) rather than open-ended research, and do not turn a feasibility spike into an undocumented production scaffold.

A spike result should document the question/hypothesis, scope, what was tested, evidence, findings, benefits, drawbacks, risks, recommendation, and remaining unknowns.

---

# 18. Exceptions

Exceptions are allowed when the normal process does not fit the situation — for example, an especially urgent hotfix, a critical security patch, limited reviewer availability during an emergency, template fields that genuinely do not apply, an immediate revert/rollback, a temporary external tool or service outage, or an approved release-specific process variation.

## Exception Rules

- **`[POLICY]`** Do not silently assume an exception; identify the normal policy or guideline being bypassed, and use the smallest safe change.
- **`[GUIDELINE]`** Document the reason and approver when possible, record the risk and follow-up work, and perform follow-up review after emergency work.
- **`[GUIDELINE]`** Do not allow a temporary exception to become an undocumented permanent process.

When documenting an exception, use:

```md
## Exception

- **Reason:**
- **Normal process bypassed:**
- **Risk:**
- **Approver:**
- **Verification performed:**
- **Rollback or recovery plan:**
- **Follow-up issue:**
```

The general rule is to use discretion without sacrificing traceability, safety, or long-term process clarity.

---

# 19. Output-Specific Instructions

Quick reference — apply the detailed rules from the referenced section, not just the summary below.

| When Asked To | Do This |
|---|---|
| Name a branch (§4) | Return `<type>/<issue-id>-<short-description>` only. Never invent an issue ID; ask for one or omit it only for genuinely trivial work. |
| Write a commit message (§5) | Return `<type>(<scope>): <description>`. Add a body only when the reason isn't obvious. |
| Draft a PR (§9) | Use every required PR heading; use `N/A — <reason>` instead of omitting a section. |
| Review a PR (§10) | Use the structured review output format and independently assess risk rather than repeating the author's claim. |
| Create an issue (§3) | Include title, problem/outcome, context, acceptance criteria, type, priority, and area/risk when useful. |
| Generate CI/CD (§13) | Inspect the repo first; use its actual runtime, package manager, lockfile, commands, branches, and runner labels — never invented ones. |
| Prepare a release (§14) | Never invent a version or build number; follow the repo's established versioning mechanism (CI-generated, repo-controlled, tag-controlled, or platform-managed). |

---

# Final Rule

Prefer changes that are small, safe, complete, traceable, testable, reviewable, revertible, and proportional to risk.

Do not add unnecessary processes, but do not bypass required controls merely for speed.
