# Validation and acceptance

Outcomes: **PASS** = executed and expected result; **FAIL** = executed and violated expectation; **NOT VERIFIED** = unavailable/not executed, with reason. Record inapplicability as an explained omission, not an executed pass. Static, rendered and browser evidence are distinct.

## Concrete checks

This is the full-pass checklist. Quick checks use the inline procedure in SKILL.md. Apply checks only to affected behavior, reuse verification already completed for the main task, and do not expand a minor edit into an unrelated test matrix.

1. Inspect exact task diff and `git diff --check`; check staged diff if relevant, explicitly inspect untracked files. No unrelated formatting, functional or visual changes.
2. Check directive syntax/prop consumption and render affected templates via tests. Text search cannot prove compiled Blade. No leaked `@testid`, `<x-...>` or `test-id` prop in rendered HTML.
3. Check rendered HTML IDs document-wide, including hidden responsive copies. Check raw output for duplicate `data-testid` attributes on a single start tag as parsers can discard duplicates. Every assigned repeated item hook must be distinct within its collection; also check page-wide collisions when the same entity appears in multiple sections, nested components or responsive copies. Parent scoping never excuses identical item values. Report out-of-scope legacy violations without migrating them.
4. Verify safe deterministic names/derived values and all prior selector, label/ARIA/fragment/JS contracts.
5. Shared component: configured/unconfigured props, null, legacy spelling if supported, changed branches and nested forwarding. One hook on intended target, preserved classes/attributes/bindings.
6. When directive behavior or environment handling changes, render testing and exact production environments; directive hooks appear only outside production. Raw attributes and HTML IDs may persist. Inspect generated select configuration if affected; restore environment per existing test isolation conventions.
7. Check affected conditional states: empty/populated, success/error, guest/authenticated, tenant/non-tenant, selected/unselected. Check English/Arabic variants when the changed markup or behavior differs by locale; hook names remain identical and RTL structure/classes are preserved.
8. Run minimum affected tests; add/update meaningful rendering/action coverage for code changes per project rules. Browser checks are needed to prove Alpine visibility, transient loading, Preline initialization and DOM morphing. If unavailable mark them NOT VERIFIED; server action tests do not prove browser behavior.
9. PHP edits require project Pint instructions. Do not format unrelated templates. Fix introduced failures, disclose unrelated/infrastructure failures.

## Repeated-item validation procedure

For future UI changes, render a collection with at least two fixture/created records and collect item-root `data-testid` values (not all descendant hooks). Assert the count equals the number of assigned item locators, the set has the same size, and each expected feature/context-plus-key locator occurs exactly once. Use actual fixture keys rather than assuming IDs such as 101/102 across environments. A record's ID is stable for that record; it need not match a different database's ID.

Render the same entity in two sections simultaneously and assert distinct context-prefixed values across the page, including hidden responsive copies if relevant. If repeated relationship instances exist within one section, test their stable occurrence identifiers as well. Test ordering/filtering/rerendering without changing identity or hook values for retained records.

Verify dynamic expressions use PHP concatenation in `@testid('prefix-' . $item->id)` or bound `:test-id="'prefix-' . $item->id"`, with actual record access and safe non-sensitive values. Compile/render via existing Blade tests to prove syntax and the quoted, escaped directive contract; static examples alone do not prove runtime compatibility. Check omitted props and exact-production omission too. No changes to `wire:key`, events, behavior or appearance are permitted for uniqueness.

For specification-only tasks, validate documentation consistency and example construction; report application rendering/browser checks as NOT VERIFIED, not as executed uniqueness tests.

## Existing test entrypoints

- `tests/Feature/PublicMarketingLocatorsTest.php`: locale rendering, target placement, production omission, omitted props, modal, repeated course contexts, pagination.
- `tests/Feature/BusinessPlus/ProgramCardComponentTest.php`: optional/legacy locator APIs and target checks.
- `tests/Feature/Livewire/BusinessPlus/ListScheduledSessionsTest.php`: popup and card compatibility.
- `tests/Feature/Livewire/Account/UserDashboardTest.php`: dashboard behavior; read before selecting filters.

Example: `php artisan test --compact tests/Feature/PublicMarketingLocatorsTest.php --filter='omits directive-generated locators'`. Use configured Herd PHP if PATH PHP is unavailable. A filter passing does not mean the whole suite passed.

## Definition of Done

1. Current-task UI elements and scope evidence identified.
2. Relevant interactions, regions and states audited.
3. Appropriate existing locators reused.
4. Missing required hooks added, or audit-only proposals recorded.
5. Names follow local conventions and safe-output rules; assigned repeated item locators are unique, with legacy violations explicitly identified if outside modification scope.
6. Existing functionality/reactive identity preserved.
7. Shared APIs optional/backward compatible, callers checked when changed.
8. Applicable validation performed, introduced failures resolved.
9. Unverified checks and blocked elements explicitly disclosed.
10. Final report includes exact scope, additions/reuse/omissions, compatibility, outcomes and untouched pre-existing issues.

Unfinished required edits mean partial completion. Unavailable runtime checks can remain reported limitations, never implied passes.
