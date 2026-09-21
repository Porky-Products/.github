# Porky Agent Instructions

Apply to issues, branches, commits, PRs, reviews, merges, CI/CD, releases, hotfixes, ADRs, spikes, and exceptions. Integrity rules apply to every task.

`[POLICY]` = required unless an approved exception applies. `[GUIDELINE]` = default, adaptable to repository/task needs. Unmarked instructions retain their stated scope. Repository rules may be stricter; never silently override organization policy. Identify conflicting/unclear instructions and request clarification.

## Integrity

- [POLICY] Never fabricate IDs, approvals, versions, build numbers, links, screenshots, logs, runtime evidence, or test/build/review/deployment/migration/release results. Claim success only with evidence; distinguish assumptions from facts and CI success from deployment success.
- [POLICY] Never expose/reproduce secrets or sensitive data, including credentials, tokens, cookies, keys, service-account material, customer data, or order data. Never commit secrets or include sensitive values in issues, commit messages, PRs, logs, screenshots, fixtures, or documentation. Example configuration uses placeholders only. Store shared/production secrets in GitHub Secrets, GitHub Environments, or an approved secrets manager.
- [GUIDELINE] Make the smallest safe, complete change. Preserve issue-to-deployment traceability; scale process to risk without removing required controls.
- [GUIDELINE] State what was not tested, inspected, or proven. Use `N/A — <reason>` for inapplicable required fields. Check logs, screenshots, fixtures, and examples for sensitive data before drafting/reviewing PRs. Local secrets may use ignored `.env` files.

## Repository setup

When creating/evaluating production repositories, confirm:

- `README.md`: purpose, owner team, setup, runtimes, common commands, deployment/readiness notes.
- `.gitignore`: build output, local config, IDE state, logs, secrets, local artifacts.
- `.env.example`: placeholder-only local configuration.
- `docs/`: relevant API, ADR, deployment, development, maintenance, or user documentation.

[GUIDELINE] Consider `.github/dependabot.yml`, `.github/workflows/*.yml`, and organization-approved `.github/copilot-instructions.md`. Scale automation to production impact; avoid unnecessary CI/releases for documentation-only, archival, or non-executable repositories.

Do not commit generated/local artifacts unless explicitly required: dependencies, build/coverage output, `.env`, `.DS_Store`, temporary logs, IDE state, personal AI scratch/prompt files. Organization-approved instruction files may be committed.

## Workflow and issues

Production-impacting work should follow `Issue → Branch → Commit → PR → Review → CI → Merge → Deploy → Close Issue` unless a documented exception applies. Other work may omit inapplicable steps.

Use issues for non-trivial bugs, features, maintenance, debt, research, refactors, docs, CI/CD, infrastructure, dependencies, security, and production follow-up. Include title, problem/outcome, why it matters, context/constraints, and expected behavior/acceptance criteria.

- [POLICY] Each non-trivial issue needs one primary type and a priority.
- Types: `bug` (incorrect behavior), `feature` (new functionality), `chore` (maintenance), `tech-debt` (reduce future engineering risk), `spike` (time-boxed research), `docs`, `security`, `ci`, `infra`.
- Priorities: `p0` (actively blocks production, security, or major business operations), `p1` (soon), `p2` (normal planned work), `p3` (backlog/nice-to-have). Priority is independent of effort.
- [GUIDELINE] Add area/scope when useful; add risk for security, data, deployment, infrastructure, internal-system, or production impact. Avoid labels without filtering/routing/planning/reporting value.

## Branches and rebasing

`main` is production source of truth; `develop` is staging/integration for production-like validation and QA. Other branches should be short-lived.

- [POLICY] Never push directly or force-push to `main`, delete `main`, or rebase `main`. Create ordinary working branches from latest `develop`. Do not rebase `develop` to update a working branch.
- [GUIDELINE] Branch from another working branch only for an intentional, documented dependency. Merge promptly (preferably daily); otherwise rebase regularly onto latest `develop`. If mistakenly based on `main`, create a branch from `develop` and cherry-pick relevant commits.

Name branches `<type>/<issue-id>-<short-description>` (e.g. `feat/MOE-123-catalog-search`). Use issue type `feature` but branch/commit type `feat`.

- [POLICY] Use an existing issue ID when available; never invent one.
- Omit the ID only for genuinely trivial untracked work, e.g. `docs/update-readme`; otherwise ask for it. When asked only to name a branch, return the name only once the ID requirement is resolved.
- [GUIDELINE] Description: lowercase kebab-case, 1–3 meaningful words. Avoid developer names as primary identifiers and vague names.

Allowed branch/commit types: `feat`, `fix`, `chore`, `docs`, `test`, `refactor` (no intended behavior change), `perf`, `ci`, `spike`, `hotfix` (urgent production fix), `build`, `revert`, `release` (large release preparation/staging), `style` (formatting only), `security`.

To update a working branch:

```bash
git fetch origin
git switch <working-branch>
git rebase origin/develop
# Resolve conflicts, then:
git add <resolved-files>
git rebase --continue
# To cancel: git rebase --abort
```

[GUIDELINE] Rebase only owned branches or branches with coordinated history rewrites. After rebasing a pushed branch, use `git push --force-with-lease`, never plain `--force`.

## Commits

Use `<type>(<scope>): <description>`; scope is optional. When asked for a commit message, return that subject and a body only when the reason/tradeoff is not obvious.

Scopes may identify a feature or concern: `auth`, `ui`, `api`, `db`, `ci`/`actions`, `deps`, `docs`, `config`, `infra`, `tests`.

- [POLICY] Each commit represents one logical change.
- [GUIDELINE] Use a concise, specific subject; explain why in the body when needed. Keep commits understandable, revertible, buildable, and tested where practical. Do not mix unrelated feature, dependency, formatting, generated-file, database, or CI changes to reduce commit count. Prefer a clear revert over silently rewriting shared history. Squash WIP noise before merge.

## Pull requests

Open a PR for each focused unit of work, including docs, tests, spikes, and operational changes. Describe its actual type; not every PR is a feature.

[POLICY] Include every heading below, exact test commands/verification methods and observed results, risk with rationale, and rollback notes for High risk. Use `N/A — <reason>` for inapplicable sections. [GUIDELINE] Keep detail proportional to complexity/risk; prefer structured evidence over repeated narrative.

```md
## Summary
<What changed>

## Why is this change needed?
<Problem/requirement; meaningful alternatives when applicable>

## How was it tested?
- `<Exact command or verification method>` — <Observed result>

## Risk Level
Low | Medium | High
**Risk rationale:** <Why>

## Linked Issue
Closes #___ | Refs #___ | Part of #___ | N/A — <reason>

## Screenshots / Logs
<Links or N/A — reason>

## Deployment Notes
<Environment variables, configuration, migrations, deployment, versioning, or N/A — reason>

## Rollback Notes
<Required for High risk; otherwise N/A — reason>
```

Use `Closes` only when the PR fully completes the issue; use `Refs`/`Part of` for related or partial work.

## Reviews and risk

Inspect the actual diff and available evidence. Independently assess problem resolution, maintainability, scope/risk accuracy, meaningful test coverage, undocumented behavior changes, unsupported claims, sensitive data, security/data/deployment/compatibility/rollback concerns, and changes to CI/CD, auth, or internal-system boundaries. Determine required human review.

Classify comments as **Blocking** (must fix), **Suggestion** (non-blocking), **Question** (clarification), or **Observation** (context, no action). Include location, evidence, impact, correction when applicable, and confidence when uncertain. Use:

```md
## Review Outcome
- **Outcome:** NO_BLOCKING_FINDINGS | CHANGES_REQUIRED | HUMAN_REVIEW_REQUIRED
- **Assessed risk:** Low | Medium | High
- **Confidence:** High | Medium | Low
- **Human review required:** Yes | No

## Blocking Findings
### Finding 1
- **Severity:** Critical | High | Medium
- **Category:** Correctness | Security | Data | Reliability | Testing | Compatibility | CI/CD | Deployment | Documentation
- **Location:** <file>:<line or range>
- **Evidence:** <Supporting evidence>
- **Impact:** <Failure and affected users/systems>
- **Required correction:** <Resolution>
- **Confidence:** High | Medium | Low

## Non-Blocking Suggestions
<Findings or none>

## Unverified Claims or Missing Evidence
<Details or none>

## Required Human Review Areas
<Areas or none>
```

| Risk | Scope/examples | Controls |
|---|---|---|
| Low | Small, isolated, minimal production/data risk: docs, UI polish, simple cleanup, tests | Copilot review; required CI when applicable; rollback plan normally unnecessary |
| Medium | Behavior, maintainability, dependencies, normal application flow: features, fixes, refactors, non-critical CI | Copilot review; required CI; rollback planning recommended, especially for behavior changes |
| High | Security, production stability/deployment, data integrity, other internal systems: auth, security boundaries, DB migrations, customer/order data, secrets, infrastructure, self-hosted runners, release paths, privileged integrations, breaking APIs/schemas | Copilot review; required CI; at least one human reviewer; rollback plan |

- [POLICY] Treat uncertain risk as High until a reviewer/owner confirms otherwise. High-risk changes cannot merge until review, testing, and rollback requirements are satisfied. AI review never substitutes for required human review or constitutes human approval.
- [GUIDELINE] Reassess risk against the final diff; do not lower it to reduce review requirements.

## Merging

- [POLICY] Complete required reviews and CI before merge, applying the final risk level. Work needed after merge requires a new PR.
- [GUIDELINE] Default to rebase merge for clean, atomic, meaningful commits worth preserving. Squash vague/WIP commits, noisy review fixes, or history clearer as one commit; final squash subject must follow the commit format. Use a normal merge commit only when repository process or an approved exception requires preserving topology.

## CI/CD

Inspect the repository before generating workflows; use its actual runtimes, package manager, lockfile, commands, branches, and runner labels.

Production, shared-library, infrastructure, and automation repositories should use CI where applicable, scaled to importance/risk. Production CI normally restores locked dependencies, lints, typechecks when applicable, tests, builds, and runs applicable security/dependency checks.

- [POLICY] Required checks must pass before merge. Never expose production secrets to PR workflows. Use least-privilege workflow permissions.
- [GUIDELINE] Run CI on PRs by default. Protect build servers with authentication, authorization, and role-based access. Integrate security testing early. Assess workflow/runner/permission/deployment changes as Medium or High according to impact. Separate CI from production deployment.
- Schedule resource-intensive E2E/integration, security, performance, or dependency validation checks (commonly daily/weekly) when they should not block normal development.

## Deployment and releases

Applies only to repositories that deploy or publish artifacts. Document environments, triggers, process, approvals, validation, and rollback. Follow the repository's CI-, repo-, tag-, or platform-controlled versioning mechanism.

- [POLICY] Tag meaningful releases; never reuse or invent release tags/build numbers.
- [GUIDELINE] Do not tag every commit, PR, deleted branch, or test build. Where automated, let CI generate versions/build identifiers.
- CI-controlled build numbers must be unique and monotonically increasing; do not generate them manually.
- Semantic versions: patch = compatible fix/hotfix; minor = compatible feature/update; major = breaking change.

## Hotfixes

For urgent production fixes: create a `hotfix/` branch, make the smallest safe correction, run required CI/QA, obtain risk-appropriate review, deploy through the approved process, tag the patch release when applicable, and create cleanup/test/docs/root-cause follow-up issues.

- [POLICY] High-risk hotfixes require human review unless an approved emergency exception applies.
- [GUIDELINE] Restore stability first; keep emergency changes narrow and complete follow-up improvements.

## ADRs and spikes

Create ADRs for significant, long-term, or standards-deviating decisions: frameworks, deployment, auth/sessions, mobile/wrappers, vendors/tools, breaking architecture/compatibility, CI/CD, data/integrations. Include context, decision drivers, alternatives, decision, benefits, costs, tradeoffs, and consequences. [GUIDELINE] Keep concise Markdown in `docs/` or `docs/adr/`; encourage collaborative architectural awareness.

- [POLICY] Mark spike code non-production; require review before production promotion.
- [GUIDELINE] Start with a testable question/hypothesis and narrow scope. Use the simplest adequate proof of concept; time-box to days or a few weeks. Do not turn a feasibility spike into an undocumented production scaffold.
- Report question/hypothesis, scope, tests, evidence, findings, benefits, drawbacks, risks, recommendation, and unknowns.

## Exceptions and draft handling

Exceptions may fit emergencies, immediate rollback, tool outages, inapplicable fields, or approved release variations.

- [POLICY] Explicitly identify the policy/guideline bypassed; use the smallest safe change. Do not silently assume an exception.
- [GUIDELINE] Document reason/approver when possible, risk, and follow-up work. Review emergency work afterward; do not let temporary exceptions become undocumented permanent process.

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

- [POLICY] For command-line operations with large multiline text, write to temporary files in the persistent artifacts directory and avoid heredoc usage.
- [POLICY] When you need to create drafts or other large multiline text messages, write to temporary files in the persistent artifacts directory. Use the same files in subsequent tool calls whenever possible to avoid manually reproducing tokens.
- [POLICY] When GUI canvas tools are available, always use the `open_canvas` for presenting draft files to the user to allow them to edit them directly.
