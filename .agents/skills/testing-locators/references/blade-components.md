# Blade integration

## Actual directive contract

`app/Providers/AppServiceProvider.php`, `registerTestingDirectives()`, registers `testid`. Its generated PHP checks `!app()->environment('production')` at rendering and emits a double-quoted `data-testid` attribute with the value escaped by `e()`. Spaces and special characters remain part of the attribute value. New hooks must still follow the project's kebab-case naming convention. When embedding rendered attributes inside JSON, JSON-encode the complete HTML string, then HTML-escape the JSON for its surrounding attribute; HTML escaping alone does not escape JSON quotes. Exact production omits directive hooks; local/testing/staging emit them. Existing HTML IDs and raw attributes are unaffected. Do not change the directive in a locator pass.

Native example from account programs:

Null and empty-string expressions emit no attribute. Nonempty values use the quoted contract described above.

```blade
<div @testid('user-programs-container') class="space-y-6">
```

Use PHP directive expressions, never nested Blade interpolation such as `@testid('{{ ... }}')`, and never wrap the directive as an attribute value. Native HTML must not use component `test-id` props. Existing raw `data-testid` contracts do not justify raw attributes for new hooks; preserve them unless separately scoped consumer checks justify conversion.

## Component decision procedure

Always guard optional locator props at the element, even though the directive also omits null and empty values. Use kebab-case values without spaces, and derive child hooks only inside the same guard:

```blade
<article @if ($testId !== null && $testId !== '') @testid($testId) @endif class="card">
    <h2 @if ($testId !== null && $testId !== '') @testid($testId . '-title') @endif>{{ $title }}</h2>
</article>
```

1. Resolve implementation, including class-backed components. Inspect props, attributes, branches, nested components and caller usages. A declared prop alone does not prove placement.
2. Supported `testId` prop: caller uses `test-id="feature-control"` or `:test-id="$prefix"`. `ui.button` puts this on its anchor or button. Do not assume a directive on a component tag forwards to its target.
3. Existing stable HTML ID reaching the control? Reuse it. Verify forwarding: `ui.modal` needs its explicit prop; arbitrary attributes are not forwarded onto its visible dialog.
4. Unsupported hook? First try existing parent scope plus reliable control selectors. If insufficient, add the smallest optional null-default API without requiring callers to change.
5. Preserve component conventions. Program card and tenant session card manually consume `$attributes->get('test-id', $attributes->get('testId'))` then remove both with `except`. Preserve spelling support and precedence; do not leak `test-id` into HTML.
6. Derive nested suffixes only when prefix is non-null and nested API supports them. Repeated callers must supply unique item-specific prefixes, preferably using primary keys; add section context for repeated appearances of the same entity. Never place the exact same hook on both an outer item wrapper and nested component root. Omitted props must not generate `-title` or other orphan hooks. Preserve slots, defaults, merged classes, bindings, attributes, events and accessibility in all branches.
7. One producer per target's `data-testid`: check directive and forwarded attributes. Do not supply prop and raw testing attribute together. A functional HTML ID can coexist.

Real call in tenant sessions: `<x-ui.page-header test-id="tenant-sessions-header" :title="__('Scheduled sessions')" />`. Modal derives `-close-button` only with header. Pagination uses `testIdPrefix`, not automatically `test-id`; inspect its caller style. Breadcrumb uses item `testid` keys with responsive suffixes.

Shared API changes require configured/unconfigured caller renders, legacy spelling where supported, null prefix, and changed alternate branches. If no backward-compatible implementation is possible, leave that edit unchanged, report concrete incompatibility, and ask for direction while completing independent safe work.
