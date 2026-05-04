# AGENTS.md

**Single source of truth for all coding agents** (Claude Code, Cursor, Copilot, Aider, etc.).  
This file takes precedence over `README.md`, `CLAUDE.md`, or any other instruction file.  
For Claude Code users: symlink with `ln -s AGENTS.md CLAUDE.md` or reference via `@AGENTS.md` in a minimal `CLAUDE.md`.

## Orchestration Contract
This `AGENTS.md` is the codified contract that turns prompting into orchestration. It encodes project-specific rules, boundaries, repeatable skills, and deterministic workflows so every agent session starts with the same expectations. Per the Golden Rule of Constraints: if the same correction is needed more than once, formalize it here rather than repeating it in prompts.

## Project Overview
macOS health & compliance reporting tool. Primary artifact: `Mac-Health-Check.zsh` (zsh + swiftDialog). MDM-agnostic with optional Jamf integration. Supports five operation modes: **Self Service** (default production), **Silent**, **Debug**, **Development**, and **Test**.

## Key Commands
- Validate syntax (run after **every** script edit): `zsh -n Mac-Health-Check.zsh`
- Fast iteration (recommended for development): `./Mac-Health-Check.zsh --mode Development`
- Full regression validation (before release work or cross-mode behavior changes): Test in `Self Service`, `Silent`, `Debug`, `Development`, **and** `Test` modes
- View canonical version: `cat VERSION.txt`

## Agent Workflow
- **Always** start non-trivial changes in Plan mode (or equivalent) before writing code. Treat context like a scalpel, not a net — provide only the files, line ranges, or examples necessary for the task.
- **In Plan Mode**: Do **not** make any changes (or even propose code) until you have reached **95% confidence** in the complete solution. Ask follow-up questions until you achieve that confidence level.
- Default user-facing communication mode is `$caveman full` unless security, irreversible actions, or user confusion require temporary plain-language clarity.
- Use **surgical, minimal edits** — reference exact function names, line ranges, or list-item IDs rather than pasting large sections of code or the entire script.
- After any edit to `Mac-Health-Check.zsh`, immediately run `zsh -n` and then validate across all affected modes.
- Before starting a session, confirm this `AGENTS.md` is loaded (use your agent's context or introspection command, or equivalent).
- Prefer batching related changes into a single, well-scoped prompt when possible.
- Never introduce debug-only or development-only behavior into production paths (`Self Service` or `Silent`).
- Leverage codified **Skills** (see below) for repeatable workflows instead of re-describing the process each time.

## Skills (Repeatable Workflows)
These codified skills enforce consistent, deterministic processes. Invoke the relevant skill name in your planning prompt.

### Add New Health Check Skill
1. Plan the check using the exact template from a nearby check (humanReadableCheckName, dialogUpdate calls, logging, success/fail paths).
2. Implement only in `Mac-Health-Check.zsh`.
3. Update both the primary Self Service dialog JSON **and** the Development-mode subset.
4. Run full validation in all five modes (`Self Service`, `Silent`, `Debug`, `Development`, `Test`).
5. Document the new check in `README.md` and `CHANGELOG.md`.

### Refactoring / Style Update Skill
1. Identify every location that must change (use grep or equivalent).
2. Make the minimal surgical edit only.
3. Run `zsh -n` immediately after edit.
4. Validate across all affected modes.
5. Never leak Debug/Development behavior into production paths.

### Release Preparation Skill
1. Confirm `scriptVersion`, `VERSION.txt`, and `CHANGELOG.md` are aligned.
2. Update only the files explicitly in scope.
3. Run full regression across all five modes.
4. Never modify `Resources/` artifacts unless the task is a packaging refresh.

## Boundaries

**Always allowed without asking**
- Read any file in the repository
- Run `zsh -n`, syntax checks, or single-mode tests
- Make small, targeted edits that strictly follow the style rules below

**Ask before doing**
- Modify release artifacts under `Resources/`
- Add new production dependencies or external checks
- Change check ordering or the primary dialog JSON structure
- Change default operation mode, exit-code semantics, or logging / output contracts
- Update `VERSION.txt` or prepare a release

**Never do**
- Hardcode secrets, API keys, organization-specific data, or credentials
- Modify files outside the current task scope without explicit approval
- Leak `Debug` or `Development` behavior into `Self Service` or `Silent` modes
- Force changes that would break MDM-agnostic behavior

## Source of Truth
When files disagree, use this order:

1. `Mac-Health-Check.zsh` for implemented behavior, defaults, and supported operation modes.
2. `README.md`, `CHANGELOG.md`, and `Diagrams/` for current release documentation.
3. `VERSION.txt` for the canonical release marker.
4. `Resources/projectPlan.md` for historical product and architecture context (not current runtime truth).

## Mission
Mac Health Check should provide clear, actionable device health and compliance information to end-users in MDM Self Service, while remaining MDM-agnostic and easy for IT teams to extend.

## Product Boundaries

### In Scope
- macOS health/compliance reporting and guidance
- swiftDialog-based user experience
- logging and optional webhook notifications
- modular checks and organization-specific customization
- packaging and deployment helpers that ship or wrap the main script

### Out of Scope
- non-macOS support
- automatic remediation/enforcement as a primary behavior
- replacing MDM/EDR platforms

## Implementation Priorities
1. Preserve MDM-agnostic behavior in the core flow, while keeping Jamf-specific integrations isolated and clearly intentional.
2. Keep user-facing output clear and remediation-focused across all five modes.
3. Favor safe, incremental changes in `Mac-Health-Check.zsh` and related helper resources.
4. Maintain compatibility with recent macOS versions, swiftDialog, and common MDM workflows.
5. Keep documentation, diagrams, and version markers synchronized with actual behavior.

## Key Files
- `Mac-Health-Check.zsh`: main script, health-check logic, operation-mode branching, and release history
- `README.md`: current user/admin guidance, supported checks, and operation-mode behavior
- `CHANGELOG.md`: release notes and shipped behavior summary
- `VERSION.txt`: canonical version marker
- `Diagrams/`: execution flow, operation-mode, deployment, and health-check reference docs
- `Resources/projectPlan.md`: historical 3.0.0 planning context
- `Resources/README.md`, `Resources/Makefile`, `Resources/createSelfExtracting.zsh`, `.deployMacHealthCheck.zsh`: packaging and distribution helpers
- `external-checks/README.md` and `external-checks/`: optional integration checks and examples

## Repository Rules
- Current branch prepares `4.0.0`; use `VERSION.txt`, `scriptVersion`, and `CHANGELOG.md` as release-state truth.
- Keep `scriptVersion` (inside the script) and `VERSION.txt` aligned at all times.
- Check `git status` before editing shared docs or assets so you do not overwrite unrelated local work.
- Some supporting docs still carry `3.0.0` headings or metadata — treat those as documentation debt unless the task is explicitly historical.
- Tracked release artifacts exist under `Resources/`; do not rebuild or replace them unless the task specifically calls for a release/package refresh.
- Prefer **minimal, targeted edits** over broad rewrites.
- Keep naming and style consistent with existing script conventions.
- Avoid introducing hidden behavior changes when refactoring.
- If behavior changes, document it in `CHANGELOG.md` and the relevant docs.
- Do not add new production dependencies without explicit user confirmation.

## Scripting Style (Required)
These rules exist to eliminate ambiguity and enforce determinism — they take precedence over any ad-hoc prompting.

Maintain the established style of `Mac-Health-Check.zsh` unless the user explicitly asks for a different style.

1. Keep the sectioned structure and visual separators (`####################################################################################################` and `# # # ...`) for major script regions.
2. Keep function naming and declaration style (`function checkXxx() { ... }`, `function updateScriptLog() { ... }`) with descriptive verb-based names.
3. Continue using lower camelCase variable names for script globals and `local` variables inside functions.
4. Prefer `"${var}"` style expansion and explicit quoting consistent with existing script patterns.
5. Route operational logging through helper wrappers (`preFlight`, `notice`, `info`, `warning`, `errorOut`, `fatal`) instead of ad-hoc logging.
6. **Preserve the established health-check lifecycle** (critical):

   ```zsh
   function checkXxx() {
       humanReadableCheckName="Human Readable Name"
       notice "Starting check: ${humanReadableCheckName}"

       dialogUpdate "icon" "SF=checkmark.circle.fill,weight=semibold"
       dialogUpdate "listitem" "index: ${index}, status: wait, statustext: Checking..."
       dialogUpdate "progress" "${progressValue}"
       dialogUpdate "progresstext" "Checking ${humanReadableCheckName}..."

       # --- check logic here ---

       if [[ condition ]]; then
           dialogUpdate "listitem" "index: ${index}, status: success, statustext: Compliant"
           info "Check passed: ${humanReadableCheckName}"
       else
           dialogUpdate "listitem" "index: ${index}, status: fail, statustext: Action required"
           warning "Check failed: ${humanReadableCheckName} — remediation guidance here"
       fi
   }
   ```
   Use nearby checks as the template for local variables, icons, inspect metadata, early returns, and warning-vs-fail behavior when a check needs specialized handling.
7. Keep mode-specific guards explicit: UI-only behaviors must stay out of `Silent`, while logging and non-UI checks must still work there.
8. When changing health-check ordering or list items, review **both** the primary dialog JSON **and** the curated `Development` mode subset.
9. Keep user-facing remediation text concise, direct, and action-oriented in list item subtitles.
10. Preserve the script's existing comment voice and contributor-attribution style in section headers and history updates.

### Zsh & swiftDialog Specifics
- Dialog JSON must remain valid at all times.
- Icon and asset paths must resolve correctly in both Self Service and Silent modes.
- Prefer `local` variables inside functions; minimize script-global state.
- All user-facing strings must be concise and action-oriented.
- Use `set -euo pipefail` style safety only where it does not conflict with existing error-handling patterns.

## Mode-Specific Expectations
- `Self Service` is the default production mode.
- `Silent` runs checks and logging without launching the main dialog or non-essential UI follow-up such as detached inspect summaries.
- `Silent` with `splunkOperationMode=production` is reporting-first: suppress non-Splunk console noise and treat report-delivery success as success even when health findings exist.
- `Debug` enables verbose troubleshooting behavior and should remain obviously non-production.
- `Development` intentionally runs a curated subset of checks and list items for faster iteration.
- `Test` is a special validation path and should not accidentally become the default or leak test-only behavior into production runs.

## Quality Bar
- Pre-flight behavior must remain reliable (root, dependency, and environment checks).
- Dialog JSON generation must stay valid and resilient.
- Health checks should fail safely: warnings where possible, fatal only when required.
- Detached inspect summaries and Dock integration should degrade gracefully and must not break `Silent` mode.
- Jamf-specific inventory or external-check paths must not regress non-Jamf vendors.
- Logging should remain structured and useful for troubleshooting.
- User guidance should explain what failed and what to do next.

## Required Validation
1. Run `zsh -n` on modified Zsh scripts (**required**).
2. For script or runtime-behavior changes, review for obvious regressions in **every** operation mode touched by the change: `Self Service`, `Silent`, `Debug`, `Development`, and `Test`.
3. For docs, diagrams, or `AGENTS.md`-only changes, review rendered Markdown, terminology, and version / behavior references for cross-file consistency.
4. Update `README.md`, `CHANGELOG.md`, `Diagrams/`, and other affected docs when behavior, configuration, screenshots, or check inventory changes.
5. Keep `VERSION.txt`, `scriptVersion`, and release notes aligned when making release-affecting changes.
6. When touching packaging or deployment helpers, verify the related documentation in `Resources/README.md` and any intentionally tracked release artifacts.
7. Do not add new production dependencies without explicit user confirmation.

## Release Checklist
(Only apply when preparing a new release)

1. Keep `scriptVersion` and `VERSION.txt` aligned.
2. Ensure the top `CHANGELOG.md` entry reflects the shipped behavior and correct date.
3. Confirm `README.md` matches current defaults, supported operation modes, and user-facing checks.
4. Refresh relevant `Diagrams/*.md` references when execution flow, deployment flow, or check inventory changes.
5. Remove or clarify stale version references when they could mislead contributors.
6. Verify no debug- or development-only defaults leaked into production paths.

## Maintenance
This file is versioned with the project. When core style rules, validation requirements, or boundaries change, update `AGENTS.md` and note the change in `CHANGELOG.md` under an “Agent Experience” or “Internal” section. Keep this file under ~180 lines for optimal agent attention.