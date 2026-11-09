# Locator coverage

Apply only to current-task elements. In every row, reuse an existing stable HTML ID or hook that reaches the intended target. Add a dedicated hook only when it enables a meaningful action, assertion, or disambiguating scope. Do not tag decorative descendants.

| Category | Required when / placement | Reuse or omit / real evidence |
| --- | --- | --- |
| Page | New screen or changed root needing scope: existing outer element | One root suffices; no new wrapper solely for testing. `user-programs-container`. |
| Section/region | Independently asserted or separates identical content: existing region root | Omit layout-only wrappers. `vacancies-filters`, `user-programs-list`. |
| Action/button | New or changed testable action: actual interactive node | Reuse `checkout-submit-button` ID. Omit duplicate icon/label hooks unless separately asserted. |
| Navigation/link | Destination/flow exercised by task: actual anchor | `request-demo-back-link`; omit incidental prose links. Preserve functional fragment targets. |
| Form/input | Edited/submitted field: native control; form root when submission/grouping needs scope | Auth `login-form`, `email-input` IDs suffice. Labels/wrappers do not replace needed control selectors. |
| Search/filter | Changed search/options/apply/reset/remove: actual control | Checkbox-list derives search/option hooks. Reuse `vacancies-search-input`, `vacancies-clear-filters`. |
| Sorting/pagination | Changed ordering/page interaction: toggle and actionable choices | Vacancy sort IDs; pagination prefix derives first/previous/page/next/last/per-page hooks. Wrapper alone may be insufficient. |
| Tabs/accordions | Changed trigger and independently asserted panel: trigger/panel root | Reuse account assigned/enrolled IDs; omit panel text descendants. |
| Dropdown/select | Interacted trigger, input, ambiguous choices/search | Select exposes base and `-trigger`; stable native option value may suffice without each option having a hook. |
| Modal/dialog | Open, close, submit controls and visible dialog | Modal prop targets inner `x-show` dialog; derives close only with header. Backdrop hook only if dismissal needs it. |
| Cards/lists | Listing/asserting/interacting with collection: static collection hook and unique hook on each assigned item root | Prefer feature/context prefix plus primary key, e.g. `scheduled-session-card-101`; derive needed child actions from that prefix. Historical repeated `user-programs-card` is not the new convention. |
| Loading/empty/success/error | State independently asserted: actual conditional or visibility node | `request-demo-loading-state`, `user-programs-empty-state`, `request-demo-email-error`; no spinner-path hooks. |
| Conditional/auth UI | Branch changes tested behavior: branch root/action | Tenant-only account tabs. No hook for every `@if`; no user identity in names. |
| Async UI | Appears or changes after action: eventual state/action node | Tenant popup/modal content; reuse stable hook after morph, wait for state. |

## Decision procedure

1. Outside current-task scope? Leave unchanged except necessary minimal shared API dependency.
2. No meaningful interaction, assertion, or scope? Omit dedicated hook, recording category-level reason.
3. Existing stable selector reaches target within intended scope? Reuse unchanged if compliant. For legacy duplicate item hooks, preserve contracts unless modification is explicitly in scope and report the violation; do not silently approve them.
4. Parent plus existing child role/value/functional selector suffices? Add only missing parent when no item hook is needed; any assigned repeated item hook must still be unique. Do not depend on CSS layout or translated text as sole stable identity.
5. Otherwise add native directive or inspected component prop. Follow repetition rules and shared API decision procedure in the other references.

Paths for every example are in the project evidence map; read relevant source, not every example on each pass.
