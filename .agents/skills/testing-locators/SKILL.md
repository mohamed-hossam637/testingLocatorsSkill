---
name: testing-locators
description: Automatically audit and add stable testing locators after completing UI tasks when required by project agent instructions; also supports explicit invocation. Scope changes to the current task and preserve behavior.
---

# Testing Locators — Post-Feature Skill

## Purpose

Run **after feature implementation**, before the developer's final commit or PR. Audit changed UI and add only missing, meaningful, stable testing locators. This skill is **not** a project-wide selector migration.

## When to invoke

- **Automatic:** After implementing UI changes, select the skip, quick-check, or full-pass mode below before the final task report. The developer does not need to mention the skill.
- **Manual:** Run when the user explicitly asks to add/audit testing locators, including audit-only requests.
- **Skip:** Backend-only changes, configuration-only changes, non-UI refactors, and tasks without relevant rendered UI changes. Do not run merely because a Blade file was touched if the change cannot affect UI testability.
- **No recursive invocation:** Once the locator pass completes, do not trigger another pass solely because the skill itself changed UI markup.
- Supports Laravel Blade, Livewire, Alpine.js, and Blade components; apply general rules to other UI technologies only if conventions are confirmed.

## Select the smallest applicable mode

Choose by testability impact, not line count. A testability change adds, removes, or changes an interaction, an asserted state, a test target's structure or identity, or a locator API.

- **Skip:** Styling, spacing, typography, or copy-only edits that leave test targets, interactions, and locator contracts unchanged; backend/configuration/non-UI work. Do not add hooks or run locator-specific checks for these changes. The main task's required tests still apply.
- **Filament scope:** Filament-generated resources, tables, forms, actions, and panels are excluded from automatic locator edits. Do not insert Blade `@testid` directives into PHP schemas. Use existing Filament testing APIs for feature verification. An explicit Filament locator request requires inspecting its supported attribute APIs first; custom Blade within a panel may use this workflow only for the task's changed markup.
- **Quick check:** A small, localized change to existing markup or an interaction, with no new screen/component, substantial flow change, repeated-item identity change, or shared locator API change. Inspect only session-known changed elements and their existing hooks. Reuse hooks; add a missing hook only to an affected test target when necessary. Check the exact diff, safe values, and duplicates. Read only references needed for that edit. Reuse the main task's targeted verification rather than repeating tests. Report the outcome in one sentence; no five-role report, branch-history investigation, or automatic locale/authentication/production matrix is required. Escalate if inspection reveals full-pass criteria.
- **Full pass:** New screens or components, substantial changes to existing interactions/states, repeated-item identities, or shared component locator APIs. Run the five roles below, scoped to the task. A substantial change includes a new form flow, changed conditional state structure, or moving targets between components; a one-line shared locator API change also qualifies.

In either active mode, check production omission when directive behavior or environment handling changes; check Arabic/English or guest/authenticated variants when the affected markup or behavior differs across those variants. Do not repeat a matrix already covered by the main task's tests. Run configured/unconfigured caller checks when a shared locator API changes. These rules scale locator verification only; they do not waive project test requirements.

## Automatic task-completion handoff

1. Complete the requested feature/fix first. Do not run midway through unfinished implementation.
2. Determine the **current task's** changed UI elements using the task context, relevant commits and working tree. A branch-wide diff is a discovery aid, **not** permission to edit every changed file.
3. Select the mode above. For a quick check, follow its inline procedure; for a full pass, run the five roles below only on those UI elements, including shared components only when necessary.
4. Validate, then include locator results in the task's final report. Do not silently skip: if blocked, explain the blocker.
5. Never stage, commit, or push as part of this skill without an explicit user request. If the user's main task includes a commit, run the locator pass **before** that commit when possible; follow the user's commit instructions without independently creating extra commits.
6. Treat instructions as agent-guided automation, not a guaranteed external post-task hook. The agent must have discovered and followed the repository instructions.

## Full-pass roles (execute sequentially; one agent may perform all roles)

Read references progressively, resolving paths relative to this canonical skill directory at `.ai/skills/testing-locators`. Boost manages discovery entrypoints for Codex, Claude, and Cursor; do not maintain handwritten pointers or independent specifications in their folders.

- Scope Detective: [scope detection](references/scope-detection.md).
- Locator Auditor: [coverage](references/coverage-spec.md), [naming and repeated elements](references/naming-conventions.md), and relevant [project examples](references/real-project-examples.md).
- Selector Implementer: [Blade components](references/blade-components.md); also [Livewire and Alpine](references/livewire-alpine.md) for reactive markup.
- Regression Guardian: [validation and acceptance](references/validation-checklist.md).
- Reporter: use the outcomes and Definition of Done in that validation reference.

These specifications refine the roles below. Every repeated card/item assigned a testing locator must have a unique rendered value. Prefer its database primary key, or a stable unique domain identifier, after a meaningful feature/context prefix. Parent scoping does not replace item uniqueness. Keep collection hooks static and values in the project's kebab-case naming convention. Preserve legacy contracts unless modification is explicitly in scope; report noncompliant legacy hooks rather than migrating unrelated UI.

### 1. Scope Detective

1. Inspect `git status --short`, current branch, and available diff against the task base branch. Use the user's specified base branch if given; otherwise infer carefully from repository context and state the choice. Consider staged, unstaged, and untracked task files. Do not assume `HEAD` alone captures all task changes.
2. Identify modified or added UI templates/components and the *specific affected elements*. Read nearby unchanged markup only for context. Avoid adding selectors across untouched portions of a file.
3. If unrelated work is mixed into the diff, separate it and leave it untouched. Continue known-scope work; if unresolved scope prevents dependent edits, report the ambiguity and ask one concise question before those edits.
4. Keep a private inventory of changed elements and current selectors.

### 2. Locator Auditor

1. Inspect the project's actual `@testid()` directive implementation and established conventions before editing. Verify environment behavior; do not assume its generated attribute name or production behavior.
2. Reuse existing functional HTML IDs and existing test hooks when stable and appropriate. Do not rename or duplicate them.
3. Prioritize selectors on: form fields, submit/actions, filters, tabs, dialogs, error/success states, important content containers, cards, empty/loading states, pagination, and navigation needed by tests.
4. Do not add locators to every decorative span/div. Keep collection-level hooks static; every assigned repeated item hook must uniquely identify its rendered instance. Parent scope remains useful for organization, never as a substitute for uniqueness.
5. Prefer the item's database primary key when available and appropriate; a stable unique domain identifier is also allowed. Never use loop indexes, timestamps, random values, translated text, sensitive data, Livewire internal IDs, or other unstable values. Tests derive expected IDs from fixtures/created records, not fixed cross-environment assumptions.
6. Keep HTML IDs unique per rendered page. Check item hook uniqueness within each collection and collisions across sections, adding stable context prefixes when the same entity appears more than once. Preserve existing contracts unless their modification is explicitly in scope; report legacy violations left untouched.

### 3. Selector Implementer

1. For native Blade/HTML elements, use `@testid('feature-element')` for test-only hooks. Use `id` only for an appropriate unique functional element, respecting existing label/JS/ARIA relationships.
2. **Never** manually write `data-testid` or `test-id` on native HTML where the project directive is required.
3. For Blade components, inspect implementation first. Preserve existing `test-id` props when the component internally invokes `@testid()`; they are valid. Check that the locator lands on the intended interactive element, not an unrelated wrapper.
4. If a component lacks an appropriate hook, make the smallest **optional, backward-compatible** component change necessary. Preserve props, defaults, attributes, markup, styles, Livewire/Alpine bindings, events, ARIA, routing, and behavior for all existing callers. Check other usages before changing shared components. Avoid duplicate rendered test attributes.
5. Never change `wire:key` to create a locator. Do not change translations, business logic, authorization, validation, API calls, payment/booking behavior, CSS, or responsive layouts.
6. Keep the diff minimal. Do not auto-format entire files or refactor unrelated code.

### 4. Regression Guardian

1. Inspect `git diff --check` and the exact changed lines. Verify no duplicate HTML IDs or testing attributes, distinct rendered item hooks within collections and across section contexts, correct dynamic Blade expressions, and no unsafe or unstable selector values. Database IDs need not match across environments.
2. Verify directive rendering for test and production environments when directive behavior or environment handling changes and tooling permits. Do not claim this was verified if it was not.
3. Run targeted existing tests or render checks, plus applicable lightweight formatting/static checks. Check guest/authenticated and Arabic/English variants where relevant and feasible.
4. If a shared component changed, check at least one configured and one unconfigured caller and preserve legacy prop support.
5. Never claim browser, runtime, or full-suite validation unless actually executed. If checks are unavailable, explain why.
6. If a check fails because of the change, fix it; if unrelated or blocked, report it clearly without hiding it.

### 5. Reporter

Provide a concise report containing:
- Diff base and exact files inspected/modified.
- Table: file, selectors added, existing selectors reused, component changes, notes.
- Selector names grouped by feature; distinguish HTML `id`, `@testid()`, and component `test-id` prop.
- Shared-component backward-compatibility notes.
- Tests/checks actually run and their results, plus limitations.
- Any pre-existing issues intentionally left unchanged.
- **Do not commit, push, reset, rebase, or stage files unless the user explicitly requests it.**

## Stop conditions

Leave uncertain or breaking edits untouched and report the concrete constraint. Continue independent work within known scope; ask for direction only when unresolved scope, an unavoidable breaking shared-component change, or an unidentified target prevents the remaining required work. An unknown diff base alone does not block known session changes.

## Completion criteria

Every newly introduced or meaningfully changed testable interaction/state in the current task has an appropriate stable locator or an explicit documented reason why not. Existing UI behavior and callers remain unchanged.

For a full pass, apply the ten-point Definition of Done in [validation and acceptance](references/validation-checklist.md). A quick check uses its inline procedure and one-sentence report instead. Report checks as PASS, FAIL, or NOT VERIFIED; audit-only proposals do not imply implementation or runtime verification.
