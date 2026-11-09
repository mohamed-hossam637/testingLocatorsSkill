# Project evidence map

Paths are repository-relative. Read only examples relevant to the task; this map avoids rescanning the repository. Recheck source when implementations change. Names below are observations, not a mandate for every content node to receive a hook.

| File | Observed pattern / implication |
| --- | --- |
| `app/Providers/AppServiceProvider.php` | Runtime exact-production omission, escaped, double-quoted attribute output. |
| `resources/views/livewire/app/auth/login-form.blade.php` | HTML IDs `login-form`, `email-input`, `password-input`; conditional phone steps/countdown; ignored `firebase-recaptcha`. Preserve integration IDs. |
| `resources/views/livewire/app/checkout.blade.php` | `payment-method-container` directive alongside `promocode-input`, `apply-discount`, `checkout-submit-button` IDs. Mixed mechanisms are established. |
| `resources/views/components/payment-methods.blade.php` | Unusual payment expression: inspect render before adopting/correcting. |
| `resources/views/components/qualifying/course-card.blade.php` | Optional prefix and derived details/enroll hooks in alternate branches. |
| `resources/views/components/qualifying/course-section.blade.php` | Recursive database-derived section/lecture suffixes: evidence for primary-key naming; check context collisions if rendered in multiple sections. |
| `resources/views/components/business-plus/program-card.blade.php` | Manually consumes kebab/camel attributes, derives root/link/title hooks. |
| `resources/views/livewire/business-plus/scheduled-sessions/list-scheduled-sessions.blade.php` | Historical identical `tenant-session-card` hooks are noncompliant for repeated items; section scope alone does not satisfy the new rule. Preserve until modification is explicitly in scope. |
| `resources/views/livewire/business-plus/scheduled-sessions/partials/session-popup.blade.php` | Optional prefix independent of `wire:key`, forwards card prefix. |
| `resources/views/components/qualifying/tenant/scheduled-session-card.blade.php` | Legacy-compatible optional prefix, content/action derivatives, conditional recording hook. |
| `resources/views/livewire/events-page.blade.php` | Filters and `events-filter-modal`; inner-dialog placement tested. |
| `resources/views/components/events/event-card.blade.php` | Prefix-derived view/register action hooks checked by tests. |
| `resources/views/livewire/app/hiring/vacancies/list-vacancies.blade.php` | Page/filter/state hooks, stable search/apply/sort/layout IDs and functional anchor; historical identical chip/card item hooks need unique identities when explicitly revised. |
| `resources/views/livewire/app/account/user/user-programs.blade.php` | Root/list/empty and browse/tab IDs reusable; historical identical card hooks conflict with new uniqueness requirement. |
| `resources/views/livewire/business-plus/auth/tenant-get-access.blade.php` | Tenant scope and loading hook, existing input/action IDs. |
| `resources/views/livewire/business-plus/landing-page/request-demo.blade.php` | Form/errors/loading/submit hooks; short existing field IDs reused. |
| `resources/views/components/forms/checkbox-list.blade.php` | Actual trigger/search/option-value/empty hooks; preserves ARIA IDs. |
| `resources/views/components/forms/select.blade.php` | Native select and directive-generated Preline trigger config; runtime initialization needs browser check. |
| `resources/views/components/ui/modal.blade.php` | Explicit prop targets inner visible dialog, close hook depends on header. |
| `resources/views/components/ui/pagination.blade.php` | Optional `testIdPrefix`, semantic page-number suffix and per-page select. |
| `resources/views/components/ui/breadcrumb.blade.php` | Per-item `testid`, desktop/mobile suffixes. |
| `tests/Feature/View/NoDataComponentTest.php` | Raw testing attribute forwarding consumer: preserve, do not generalize to new hooks. |
| `tests/Feature/PublicMarketingLocatorsTest.php` | Actual DOM/XPath consumers, locale/environment/optional APIs. Reuse rendering patterns; repeated-item tests must now assert distinct values and derive expected keys from fixtures/records. |

## Dry-run audit: account programs (no UI edits)

Assume only user-programs view is audit scope. Reuse root/list/empty hooks and browse/tab HTML IDs. Identical `user-programs-card` hooks are a legacy violation under the new requirement. If locator modification is explicitly in scope, propose `@testid('user-programs-card-' . $program->id)` after confirming the record shape/key; otherwise report and preserve the existing contract. Parent scoping alone is insufficient. Nested program card currently receives no prefix; if child hooks are needed, pass an item-specific prefix after API inspection and avoid duplicating the wrapper's exact hook on its root.

Tenant tabs exist only in tenant state. Empty copy changes with tenant/tab while empty hook stays stable. Decorative icon/prose need no dedicated hooks. This static audit identifies a migration candidate without modifying UI. Check proposed item values within the list and across page sections; fixture primary keys must come from test records. Actual rendered uniqueness, tenant/non-tenant actions, full-document duplicate IDs, locale output and browser behavior still need execution and cannot be reported as passed from source inspection. Recheck current source before assuming historical examples remain unchanged.
