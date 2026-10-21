# UI, tray, settings and installer specification

Status: approved scope integrated; implementation and validation pending. Source: [unaltered handoff addendum](../../CodexMeter-UI-Tray-Enhancements-Handoff-Addendum.md). This document reconciles it with earlier requirements; it does not claim delivered functionality.

Subsequent planned display-text change: expanded-view heading must be exactly `CodexMeter`, without the plan suffix. Pending implementation; preserve existing layout/scrolling/styling and verify the rendered text/layout when authorized. This does not change the native hover title `Codex Meter`; scrollbar removal remains a separate requirement. Commodore clarified that the current request adds to the plan only.

## Requirement precedence

The latest user-approved addendum replaces earlier restrictions on tray presentation, a new installer, popup scrolling, direct-menu interval selection and leaving presentation timers unchanged. Preserve banked-reset null/zero semantics, usage-window calculations/order, authentication and read-only polling. Preserve existing optional features and the dark dashboard, with configuration moved into Settings. Keep the current branch and automatic commit/push workflow.

## Surfaces and defaults

| Area | Required behavior / fresh-install default |
| --- | --- |
| Popup | Detailed dark dashboard, correctly measured client area, no horizontal or vertical scrollbar; banked resets included without clipping. Human-readable countdowns are allowed. |
| Context menu | Show limits; Refresh now; separator; Settings...; separator; Start with Windows; separator; Quit. Startup check reflects actual registration. |
| General settings | Startup and polling selector: 1/5/15/30/60 minutes, default 15; retain immediate application and persistence. |
| Hover settings | Independent field enable/disable, reorder and Restore Defaults; native tooltip remains the quota hover surface. |
| Tray settings | One normal metric, percentage style, per-metric color picker, automatic status colors toggle and exhaustion-countdown toggle. |
| Appearance | Add only useful controls; no empty placeholder tab. Fewer sections are acceptable. |
| Normal tray metric | 5 Hour Remaining for fresh configuration. Unknown/missing 5 Hour data must not masquerade as zero. |
| Percentage style | Proposed Small Number + Bar, since the current renderer has a number and bottom bar; confirm at actual icon sizes. Big Number is also required. |
| Automatic status colors | Off by default; fixed user color remains selectable. |
| Automatic exhausted countdown | On by default, with an explicit switch to disable. |
| Startup | Explicit installation checkbox, shown checked as requested; user may clear it. In-app and installer controls use the same actual state; preserve an existing upgrade choice. |

Preferred fresh native tooltip (illustrative values):

```text
Codex Meter
5 Hour: 10% remaining
Resets in 2:06:14 11:08 PM
Weekly: 75% remaining
Resets in 6:22:06:14
```

Title is `Codex Meter`; use `5 Hour` and `remaining`. Default fields in order: title, 5 Hour remaining, 5 Hour countdown, 5 Hour reset time, Weekly remaining, Weekly countdown. The 5 Hour countdown and reset time compose the same default line when adjacent, but each must be independently switchable and reorderable. The formatter must preserve understandable labels when separated. Weekly reset day/date is selectable, not forced into the default line. Other required selectable fields: banked resets and last updated time.

## Shared metrics and formatting

Evolve a small shared metric abstraction with stable IDs, names, data access and surface-specific formatters/tray capability, rather than duplicating quota/countdown rules. Suggested IDs are FiveHourRemaining, FiveHourResetCountdown, FiveHourResetTime, WeeklyRemaining, WeeklyResetCountdown, WeeklyResetDate, BankedResets and LastUpdated. Title is a selectable hover field; it need not be a tray metric. Standard CodexMeter Icon is also a tray selection.

Required tray choices: 5 Hour Remaining, Weekly Remaining, 5 Hour Reset Countdown, Weekly Reset Countdown, Banked Resets and Standard CodexMeter Icon. Display only one at a time. Preserve multi-bucket popup data; select the intended Codex 300-minute/10080-minute windows deterministically rather than confusing another bucket with the normal Codex windows. Record the missing-window and legacy-metric compatibility decision before implementing selection.

Hover countdowns use cached reset timestamps: 5 Hour `H:MM:SS` followed by local `h:mm tt` reset time when enabled; Weekly `D:HH:MM:SS`. Use the user's local time and requested Windows 12-hour presentation unless an established locale convention warrants a documented exception. Full precision belongs in hover and, where useful, visible popup. No negative countdown or inferred replenishment when a deadline passes; display an explicit due/unavailable state until a successful refresh confirms quota.

Tray countdowns are a single line using the largest meaningful unit: at least one day -> `nD`; at least one hour -> `nH`; at least one minute -> `nM`; below one minute -> `ns`. Preserve D/H/M uppercase and s lowercase. Proposed rounding: floor whole days/hours/minutes, ceil positive sub-minute seconds; test exact boundaries and expiry. Never add a two-line mode.

Enforce the verified platform-supported native tooltip maximum before assigning NotifyIcon.Text. Proposed overflow policy: retain configured order, remove complete lowest-priority trailing fields until it fits, and make this behavior clear in Settings/preview. Avoid broken Unicode, partial misleading countdowns, exceptions and silent corruption. Test long dates, unavailable text and maximum counts; no custom replacement hover window. Decide/document all-fields-disabled behavior (proposed empty native text) and any overflow indicator before final UI work.

Percentage styles: Big Number maximizes readable digits and omits `%` if necessary; Small Number + Bar shows the selected percentage with a proportionate bar. Inspect actual small-icon resolution, 0/1/10/40/100, light/dark taskbars and 100/125/150% scaling (200% if practical). Banked resets remain a count and are never converted to a percentage or included in the ordinary quota calculation. Define a recognizable standard icon asset consistent with current branding.

Persist each practical metric's fixed color and restore it when switching back. Use a normal Windows color picker. With automatic status colors disabled, use the configured metric color; with it enabled, use documented deterministic quota urgency thresholds. Existing pace logic may be reused if clearly explained and suitable; record exact thresholds, behavior for non-quota metrics and stale/error overrides before implementation. Configurable thresholds are not required. Unknown/stale states remain visibly honest in either mode.

## Exhausted quota override

When enabled, apply this order from confirmed quota data:

1. Weekly exhausted -> Weekly reset countdown.
2. Otherwise 5 Hour exhausted -> 5 Hour reset countdown.
3. Otherwise -> the user's normal selected metric.

Both exhausted means Weekly wins. The override is temporary: do not replace the persisted normal metric. A refreshed snapshot confirming usable quota removes the applicable override; if Weekly recovers but 5 Hour remains exhausted, show 5 Hour countdown before returning to normal. A local countdown reaching zero, unknown data, or a failed refresh cannot confirm recovery. Define a truthful unavailable countdown when exhausted data has no usable reset timestamp, and preserve stale/error indications. Do not treat a floored display percentage as confirmed exhaustion when actual remaining quota is still positive.

## Settings, migration and compatibility

Reuse the small per-user settings mechanism already present (HKCU Software/CodexMeter); no database or broad configuration framework. Persist polling interval, hover fields/order, normal tray metric, percentage style, per-metric colors, both automatic options and actual startup state. Startup registration is the source of truth, not a disconnected duplicate Boolean. Apply accepted changes without restart where practical and preserve existing immediate interval behavior. Failed saves must not appear successful. Restore Hover Defaults affects only hover choices/order.

Move notification testing, prompt-rule editor access, cost-history enable/path controls into appropriate Settings sections so the simple context menu loses no existing function. Keep existing automation opt-in behavior and safe defaults unchanged. The optional cost hover already exists; retain its function without substituting it for native quota hover or blanking required native tooltip text. Inspect collision behavior before selecting its precise coexistence interaction.

Distinguish fresh installation from upgrade. Preserve explicit existing choices and unrelated settings, existing startup choice and private prompt-rule data. Missing/invalid new fields use documented defaults; unknown future fields should not destroy valid settings. The old tray uses the lowest remaining window with pace colors; where preserving existing behavior requires a legacy selectable mode or migration mapping, document and test it rather than silently changing users' behavior. No settings implementation or migration is delivered by this planning update.

## Two clocks and resource use

Data refresh uses the selected minute interval and the existing in-progress guard. Local presentation runs approximately once per second on the UI thread and updates native tooltip/countdown tray/visible countdowns from cached timestamps. Reuse/consolidate relevant repaint timers where sensible; existing unrelated hover dismissal behavior remains separate. No per-second app-server calls, disk reads, persistence writes, model calls or goal actions. Refresh invalidated displays only as needed; dispose replaced icons/fonts/handles and stop timers at exit. Measure idle CPU, UI responsiveness and resource growth with the timer active; record method/duration and investigate sustained load or growth rather than claiming negligible CPU from unit tests.

## Installer and distribution

Latest user preference, 2026-10-21: prefer a single `.msi`; otherwise a single `.exe` installer, with no required loose sidecar payload. This supersedes the addendum's Inno Setup-first choice. Evaluate a conventional MSI toolchain; Inno Setup remains an EXE fallback, and MSIX needs strong justification. Prefer per-user/no-unnecessary-admin installation. Require Add/Remove Programs, Start Menu, clean install/uninstall, upgrades preserving settings and a selectable startup checkbox sharing actual registration with app controls. Use one stable identity/entry; preserve the upgrade startup choice. Review build-tool licensing/version suitability and decide self-contained payload versus clear runtime prerequisite guidance before packaging.

Inspect .NET Desktop Runtime prerequisites, current user-local notification/COM registrations, shortcut metadata, running-app handling and writable local data before finalizing script details. Preserve/migrate prompt rules and other private app data without publishing them. Test moves from a portable installation and upgrades of installed candidates; do not accidentally start two app instances. Uninstall removes installed files, shortcuts and app-owned startup/notification registrations; explicitly document whether user preferences/data remain and provide a clear intentional removal path. Do not delete unrelated registry values or user data.

The installer is the primary release artifact. A portable ZIP may remain secondary but cannot satisfy installer gates. Version source, installed app, installer and release consistently; publish hashes and accurate prerequisite/signing information. Website download must deliver the tested versioned installer. Verify downloaded bytes and installation, not just link availability.

## Addendum traceability

Every row remains pending implementation/evidence. `../validation/ACCEPTANCE.md` records results.

| Addendum section | Integrated requirement | Milestone / validation |
| --- | --- | --- |
| 1 | Layout fix; neither scrollbar; dark popup retained | 1 / layout and DPI |
| 2 | Native tooltip default/title/terminology | 1b / default text |
| 3 | Local second-accurate hover countdown formats/timezone | 1b / format and tick tests |
| 4 | Independent fields/order/persistence/defaults/native limit | 1a–1b / hover settings/overflow |
| 5 | Exact simple context menu; checked actual startup | 1a / menu reachability |
| 6 | Central Settings, useful sections only | 1a / settings UI |
| 7 | Six required single tray metric choices | 1c / each selection |
| 8 | Shared metric data/surface formatting | 1b / shared logic regression |
| 9 | Both percentage styles, persistence and legibility | 1c / rendering and DPI |
| 10 | Single-line D/H/M/s countdown | 1c / boundary formatting |
| 11 | Color picker and saved per-metric fixed colors | 1a–1c / color reload/switch |
| 12 | Optional automatic colors; fixed mode retained | 1c / toggle/threshold tests |
| 13 | Weekly-first exhaustion and confirmed automatic return | 1c / full precedence matrix |
| 14 | Installer lifecycle/Start Menu/startup/no admin if feasible | 3 / installer matrix |
| 15 | All configuration persisted and migration-safe | 1a, 3 / restart/upgrade |
| 16 | Separate polling and second-based local timer | 1b, 2 / request trace/CPU/resources |
| 17 | Detailed readable popup; surface-specific formats | 1, 1b / popup/hover/tray QA |
| 18 | Required tests for all added behavior | 2–4 / acceptance record |
| 19 | Native tooltip, one metric, no second polling/telemetry/cloud/DB | All / scope and request review |
| 20 | Fresh defaults and preservation of existing user behavior | 1a–1c, 3 / default/migration |
| 21 | Final tray/settings/installer/performance/network verification | 2–5 / release gates |

## Decisions to resolve during implementation

Record evidence-based choices in DECISIONS.md: native text budget and overflow indication; fit strategy for unusually many buckets/small monitor work areas; standard icon asset; exact automatic color rules; missing-window/unknown/expired fallback; legacy most-limited migration; cost-hover coexistence; installer/runtime/data locations and uninstall retention. These are bounded engineering decisions, not missing authorization for the approved scope. Escalate only a material conflict that cannot meet the handoff.
