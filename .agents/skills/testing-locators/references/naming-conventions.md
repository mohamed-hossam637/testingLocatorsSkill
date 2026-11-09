# Naming and repetition

Existing names use lowercase English kebab-case: feature/context followed by element/action, such as `user-programs-container`, `vacancies-search-input`, `request-demo-submit-button`. Prefixes include `tenant-`, `user-`, `events-`, `lms-`; derive additions from nearby coherent names. Preserve existing `-page`/`-container` and `-empty`/`-empty-state` variants instead of renaming.

For new static names use `[a-z0-9]+(?:-[a-z0-9]+)*`, the project naming convention, even though quoted output safely supports spaces, not a migration rule. Derived suffixes must also be safe. State names describe stable purpose, not current text/count. Control hooks remain unchanged while disabled/loading/selected unless purpose changes.

| Good | Avoid for new hooks | Why |
| --- | --- | --- |
| `vacancies-apply-filters` | `button-1` | Purpose, not position |
| `request-demo-email-error` | translated error text | Locale independence |
| `tenant-session-card-101` from fixture record ID | identical `tenant-session-card` on every item | Unique rendered instance, stable for that record |
| prefix + `-option-summit` | translated option label | Stable domain key |
| `events-pagination-page-2-button` | card + loop index | Page number is action semantics; order is not item identity |
| `featured-course-card-101` / `recent-course-card-101` | `course-card-101` in both sections | Distinct simultaneous contexts |

Never use sensitive values, email, titles, translations, timestamps, loop indexes, random render values, Livewire internals or CSS classes as locator identity. Prefer the item's database primary key when available and appropriate; a stable unique domain code is also allowed. Validate identifier output against the safe token rule above; if normalization would collapse distinct identities, use another safe unique identifier instead. Do not expose secrets or personal data even if they are unique.

## Repeated element procedure

1. Keep the collection hook static, such as `scheduled-sessions-list`. When assigning a hook to a repeated card/item, append a stable unique identifier to a meaningful feature-specific prefix. Identical item values in one collection are forbidden, even with parent scoping.
2. Prefer the item's primary key (`$session->id` or its actual model key); otherwise use a safe stable unique domain identifier. For collection/array records inspect their actual shape instead of assuming object access. Never alter `wire:key` or component identity.
3. Same entity rendered in multiple sections or responsive variants? Add stable section/context before the identifier, such as `ongoing-session-card-101` and `upcoming-session-card-101`. If an entity occurs twice within one context, use a stable occurrence/relationship identifier or further meaningful context; never append a loop index. If none exists, report the unresolved identity rather than inventing one.
4. Parent scope organizes tests but cannot replace unique assigned item hooks. Derive independently needed child hooks from the same item-specific prefix, such as `scheduled-session-card-101-recording-button`. Do not reuse the exact item hook on a nested wrapper/component root.
5. Assert one rendered item per expected hook. Obtain expected IDs from fixtures or created records; the literal numbers below illustrate output only. Database keys may differ across environments: uniqueness on a rendered page does not imply a universal cross-environment locator string. Identity should remain stable for the same record through sorting, filtering and rerenders.
6. No safe unique identifier? Report the gap and seek an identity contract for the scoped change; do not claim completion or fall back to identical hooks, `nth()`, random values or translated content. Preserve out-of-scope legacy contracts and report their violations separately.

## Blade examples (proposed convention, not a claim of existing UI migration)

Incorrect: `@testid('scheduled-session-card')` on every loop item.

```blade
<div @testid('scheduled-sessions-list')>
    @foreach ($sessions as $session)
        <div @testid('scheduled-session-card-' . $session->id)>
            ...
        </div>
    @endforeach
</div>
```

Records with fixture IDs 101 and 102 produce `scheduled-session-card-101` and `scheduled-session-card-102`. For a component supporting optional `testId`, pass `:test-id="'ongoing-session-card-' . $session->id"`; inspect its API and ensure children derive their hooks from that unique prefix. Use PHP concatenation in the directive/bound prop, never nested `{{ }}` interpolation.

## Conflicting history

`qualifying/course-section.blade.php` appends section/lecture database IDs and `PublicMarketingLocatorsTest.php` asserts them. Event/course/banner tests also include numeric identities. These support the allowed primary-key approach, but still require context if an entity appears more than once. Historical identical item hooks are noncompliant with the new rule, not examples to copy. Preserve their contracts unless changing them is explicitly in scope. `payment-methods.blade.php` uses `@testid("payment-{ $method->value }")`; inspect rendered output before adopting or correcting this unusual expression.

Precedence: enforce unique hooks for new or explicitly revised repeated locators; preserve existing out-of-scope contracts and report legacy violations. Follow coherent local naming only where it satisfies this requirement. Do not perform an unrelated migration.
