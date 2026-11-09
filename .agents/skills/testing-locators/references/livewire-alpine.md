# Livewire and Alpine safeguards

Hooks describe targets; they do not control DOM identity/state. Never add wrappers or alter directives merely to simplify selection.

| Mechanism | Rule | Check |
| --- | --- | --- |
| `wire:model` | Target native control via supported prop; preserve path and live/debounce/lazy modifiers | Edit/control validation and resulting state |
| `wire:click` / submit | Target action node; preserve method, arguments, prevent/stop, button type and payload | Action result; server action call does not prove browser click wiring |
| `wire:loading` | Hook independently asserted state node; retain targets/remove/class/attr | Idle/loading/result; server rendering does not prove transient visibility |
| `wire:key` | Never change/create keys as testing hooks | Item action identity after reorder/filter |
| `wire:ignore` | Hook owned boundary or supported generated control | Preserve ignore boundary; do not expect Livewire to update ignored descendants |
| Blade condition/morph | Same purpose retains hook; distinct actions may differ | Both branches and rerender; reacquire locator, avoid stale DOM handles |
| `x-show`/cloak | Hook shown node; hidden elements still exist | Visibility, not just presence; preserve transitions/cloak |
| `x-if` | Hook actual node inside template | Wait for insertion/removal; template hook is not rendered target |
| `x-for` | Hook rendered item with unique feature/context plus stable item identifier; preserve `:key` | Distinct rendered values, no index suffix; inspect client-generated DOM, not template alone |
| Modal | Visible dialog, trigger, close/actions | Open/dismiss/reopen; preserve entangle/events/reset behavior |
| Dropdown/tabs | Triggers and meaningful panels | Open/select/close and state after rerender |
| Async render | Stable eventual state/action | Wait for observable condition, not arbitrary delay; failure/empty paths if changed |

`forms/select.blade.php` renders the directive with `Blade::render` into Preline `data-hs-select.toggleTag`, deriving `-trigger`. Native select hooks may target a hidden control; inspect configuration and runtime generated button. Preserve JSON escaping and initialization; never inject unconditional raw hooks into generated markup.

Tenant `scheduled-sessions/partials/session-popup.blade.php` keeps a database-derived `wire:key` independently of optional `testIdPrefix`, then forwards a child prefix. Primary keys are preferred for assigned repeated-item hooks too, but changing a hook must never change the key or Livewire identity. Client-generated Alpine item hooks must follow the same unique safe-value and environment behavior as server-generated hooks; inspect the existing integration rather than adding unconditional raw attributes.

Do not change model state, auth gates, payment/booking actions, translation/RTL classes, routing or lifecycle. Browser access missing means NOT VERIFIED, not permission to rewrite reactivity.
