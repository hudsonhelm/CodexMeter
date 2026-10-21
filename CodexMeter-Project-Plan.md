# CodexMeter project plan

Prepared for Commodore • 2026-10-21 • Planning edition

**Current stage:** requirements and planning. Application implementation has not been authorized. This document explains the intended product, work phases, checks and release process. It is not a claim that the planned features or installer are available.

## What we are building

CodexMeter is a native Windows tray application for viewing Codex usage. We are extending the Hudson Helm fork so it provides useful information at three levels:

- **Expanded popup:** a detailed dark dashboard showing usage windows, reset information and available banked resets.
- **Native hover tooltip:** a configurable compact status report with live countdowns.
- **Tray icon:** one selected metric that is readable at notification-area size.

A centralized Settings window will control the options. A conventional Windows installer will make it practical for friends to install, upgrade and remove the application.

The project finishes when Commodore is confident sharing the tested installer and the HudsonHelm website has a working download link to that exact tested version. A feature-complete build alone is not the finish line.

## Installer commitment

The primary download must be **one `.msi` installer, preferably**, or **one `.exe` installer if MSI cannot meet the installation requirements cleanly**. A ZIP or the bare application executable does not fulfill this requirement. Avoid loose external payload files beside the installer. An optional portable ZIP may be offered separately.

This preference supersedes the earlier Inno Setup-first recommendation. Evaluate a conventional MSI toolchain such as WiX first; document the choice and any concrete reason for using the EXE fallback. Inno Setup remains a candidate for that fallback. WiX produces Windows Installer packages; Inno Setup documents single-EXE distribution. See [WiX's official overview](https://docs.firegiant.com/wix/) and [Inno Setup's official product documentation](https://jrsoftware.org/isinfo.php).

The installer must:

- Prefer installation for the current user without administrator rights where feasible.
- Create a Start Menu shortcut and an Add/Remove Programs entry.
- Support upgrades while preserving preferences, private application data and the user's startup choice.
- Offer a **Start CodexMeter with Windows** checkbox and share one actual startup registration with the app's controls.
- Handle an already-running application, avoid duplicate instances and support clean uninstall.
- Provide clear runtime/prerequisite guidance and accurate signing information.

**Important design decision:** one installer file does not automatically mean the .NET Desktop Runtime is included. Phase 3 will decide whether to package a self-contained application or detect the runtime and provide a clear prerequisite path. Compare package size, maintenance, first-install simplicity and offline behavior. Do not hide a separate prerequisite download behind a promise of an entirely offline installation. Bundling framework dependencies does not authorize model calls, telemetry or cloud services.

## Planned product behavior

### Expanded view and banked resets

Change the visible expanded-view heading from the composed **Codex Plus** to exactly **CodexMeter**. Treat it as an isolated display-text change: preserve its existing layout, scrolling, styling and functionality when implementing that change. The separately approved scrollbar correction then fixes underlying sizing so both scrollbars disappear without clipping content or redesigning the dashboard. Plan/account data remains available internally.

Read the banked reset count through the existing usage request and display `Usage resets — N available`. An explicit zero is valid; missing or malformed data remains unknown and must never appear as a false zero. Do not add redemption, individual-credit metadata, scraping or a second authentication path.

### Settings and refresh

The tray context menu stays simple: Show limits, Refresh now, Settings..., Start with Windows and Quit, separated as specified. Detailed options belong in Settings. Preserve existing notification testing, prompt-rule and cost-history functions by keeping them accessible there.

Refresh choices are **1, 5, 15, 30 or 60 minutes**, default **15**. Persist the choice and apply changes without restarting. Preserve immediate manual refresh, initial launch refresh, and refresh on resume when due. Keep at most one quota poll in progress.

### Native hover

Allow independent show/hide and reordering of these fields: title, 5 Hour remaining, 5 Hour reset countdown, 5 Hour reset time, Weekly remaining, Weekly countdown, Weekly reset date, banked resets and last updated time. Persist the configuration and provide Restore Defaults.

The preferred default looks like this, using illustrative values:

```text
Codex Meter
5 Hour: 10% remaining
Resets in 2:06:14 11:08 PM
Weekly: 75% remaining
Resets in 6:22:06:14
```

Reset clock time is local. Enforce the native tooltip's supported text limit without exceptions or misleading partial values. Preserve native quota hover when optional cost features are enabled; do not replace it with a custom quota hover window.

### Tray instrument

Support 5 Hour Remaining, Weekly Remaining, both reset countdowns, Banked Resets and a Standard CodexMeter Icon. Show one metric at a time. Percentage styles are Big Number and Small Number + Bar. Countdown icons use one compact line such as `3D`, `2H`, `47M` or `32s`.

Provide saved colors per metric and an optional automatic status-color mode. Fresh defaults are 5 Hour Remaining, a style closest to the current appearance, automatic colors off and automatic exhausted-countdown switching on. Preserve existing users' explicit choices through migration.

When the automatic exhaustion option is enabled:

1. Exhausted Weekly quota shows the Weekly countdown, including when both windows are exhausted.
2. Otherwise exhausted 5 Hour quota shows its countdown.
3. Otherwise show the user's normal selected metric.

Keep the user's normal metric saved. Return when fresh usage data confirms recovery; the clock reaching zero does not prove replenishment.

### Two separate clocks

Usage polling follows the configured minute interval. A separate presentation timer updates cached countdowns approximately once per second. It must not call Codex, start model turns, write settings or reread files every second. Share metric data and formatting rules across surfaces while keeping formats appropriate to each surface.

## Work phases and completion gates

| Phase | Work | Evidence required to leave the phase | Current state |
| --- | --- | --- | --- |
| 0 — Baseline | Preserve handoffs; inspect source, branch and remotes; establish plan and existing test baseline. | Recorded source revision, successful baseline build and tests. | Complete |
| 0a — Heading | Implement the isolated CodexMeter expanded-heading change. | Correct rendered text with existing layout/scrolling/styling preserved. | Pending authorization |
| 1 — Data and layout | Banked-reset parsing/display; underlying scrollbar/layout correction. | Positive/zero/unknown tests; all expected content visible without scrollbars or clipping. | Pending |
| 1a — Settings and polling | Central Settings, persistent preferences/migration, simple menu and refresh choices. | Reload/save/default tests; immediate interval change; manual/launch/resume and no-overlap checks. | Pending |
| 1b — Hover and metrics | Shared metric representation, field selection/order/defaults and local live countdowns. | Exact formatting, native length, unavailable/expired-state and no-per-second-poll tests. | Pending |
| 1c — Tray presentation | Metric selection, both styles, colors and exhausted-quota switching. | Every selection/style/color tested, including weekly-first override and automatic return. | Pending |
| 2 — Verification | Regression, live-account comparison, scaling, accessibility and performance. | Passing automated checks, inspected UI, recorded live limitations and negligible resource cost. | Pending |
| 3 — Installer | Prefer one MSI; otherwise one EXE. Resolve prerequisites, install/upgrade/uninstall and startup integration. | Clean standard-user install; upgrade preserves settings; one startup registration; clean uninstall. | Pending |
| 4 — Share-readiness | Versioned candidate, working-day trial, installation on another machine or clean VM. | Exact artifact hashes, trial results, resolved blockers and Commodore's readiness decision. | Pending |
| 5 — Publication | Versioned GitHub release and HudsonHelm download link. | Public download matches candidate hash; downloaded installer installs successfully; website rollback recorded. | Pending |
| 6 — Closeout | Final status, maintenance notes and coherent upstream contribution preparation. | Recorded source/tag/assets/results/limitations; accurate contribution description. | Pending |

Review installer feasibility early, even though full installer acceptance is Phase 3. Build small, reviewable changes in dependency order. Repeat tests after changes that could invalidate earlier results. Commit and push completed project changes automatically to the feature branch; do not merge main or open an upstream PR without a separate instruction.

## What we should not overlook

These are review findings and implementation decisions to resolve in the relevant phase. They do not authorize unrelated product expansion.

| Risk or improvement | Planned treatment |
| --- | --- |
| Settings stored beside executable may become unwritable or be lost during upgrade | Choose a stable per-user writable data location; migrate private prompt data carefully and preserve existing registry preferences. |
| Fresh defaults differ from legacy tray behavior | Explicit migration rules; distinguish missing values from invalid values and existing choices. |
| Plan/account or Codex version changes | Revalidate snapshot-dependent state; test unsupported fields, weekly-only accounts and sign-in failures. No fabricated quota. |
| Windows sleep, clock changes and daylight-saving transitions | Calculate against timestamps; test expiry, resume and local formatting without negative countdowns or fake resets. |
| Tooltip length and independent reset-time fields | Preview configured output and truncation policy; keep disabled/reordered fields understandable. |
| Tiny icons and high DPI | Inspect actual tray sizes, light/dark taskbars, 100/125/150% scaling and 200% where practical. |
| Many buckets or small monitor work area | Determine supported fit behavior without hidden clipping or scrollbars; document physical limits. |
| One-second redraws leaking icons or consuming CPU | Reuse/dispose resources; avoid unchanged redraws; record CPU, memory and handle observations over a stated duration. |
| Accessibility and save failure | Keyboard access, readable names/focus, contrast and truthful persistence errors; no requirement to rely only on color. |
| Installer repair or upgrade re-enables startup | Test install/upgrade/repair where supported; actual user choice must win and no duplicate startup entry may appear. |
| Interrupted upgrade, reinstall or old-version install | Define safe failure/recovery and version policy; do not corrupt preferences or leave unusable startup paths. |
| Notifications tied to an old executable/shortcut path | Verify installed notification identity and activation after upgrade and uninstall. |
| Runtime is absent on a friend's PC | Decide self-contained packaging versus a clear prerequisite path; test with and without the runtime. |
| Unsigned download and release trust | Decide whether signing is feasible; publish accurate publisher/signing information and SHA-256. Checksums alone do not remove Windows trust prompts. |
| A future Codex change breaks usage retrieval | Clear compatibility/troubleshooting guidance and a small support path through the fork's Issues; keep private account data out of reports. |
| Installer tool maintenance or licensing obligations | Review selected tool/version/license before committing to it; pin and document the build toolchain. |

Keep automatic app updates, extra operating-system architectures, cloud syncing, telemetry, credit redemption and a new settings database outside this release unless separately requested.

## Validation and definition of done

The existing baseline passed a Release build, 95 default checks, 36 fake-helper prompt checks and 14 fake-helper goal checks. Those results describe the original source, not the pending features.

Final acceptance requires:

- Parser and presentation tests covering confirmed counts, unknown data and ordinary quota behavior.
- Settings/default/migration/restart tests; correct tooltip formatting and limits; every tray style and exhaustion transition.
- Existing automation regression tests using fake helpers, without real goal changes or model calls.
- Visual checks and live-account comparison where available; clearly documented limits when a live positive-reset state is unavailable.
- Recorded one-second timer resource behavior and evidence that it causes no additional quota requests.
- Installer lifecycle checks with startup on/off, runtime absent/present, upgrade/settings preservation and clean uninstall.
- A normal working-day trial and a clean installation/smoke test on at least one other Windows machine or VM.
- No unresolved defect blocking accurate quota display, reasonable resource use, installation, refresh, settings preservation or exit.
- Commodore's confidence in sharing the candidate.
- A versioned single-file installer and checksums on the fork, plus a public HudsonHelm link verified by downloading, hashing and installing that copy.

Record exact source commit/tag, version, installer format/tool, hashes, commands/results, environments, known limitations and rollback instructions. A checklist tick requires evidence. Do not mark the project complete because the code builds, because a file uploaded successfully or because implementation time is nearly spent.

## Project records and next step

The [GitHub roadmap](https://github.com/hudsonhelm/CodexMeter/issues/1) tracks progress. Detailed engineering records are the [working plan](https://github.com/hudsonhelm/CodexMeter/blob/feature/banked-reset-credits/PROJECT_PLAN.md), [status](https://github.com/hudsonhelm/CodexMeter/blob/feature/banked-reset-credits/STATUS.md), [UI/tray specification](https://github.com/hudsonhelm/CodexMeter/blob/feature/banked-reset-credits/docs/project/UI_TRAY_SETTINGS.md), [acceptance matrix](https://github.com/hudsonhelm/CodexMeter/blob/feature/banked-reset-credits/docs/validation/ACCEPTANCE.md) and [release runbook](https://github.com/hudsonhelm/CodexMeter/blob/feature/banked-reset-credits/docs/release/RUNBOOK.md).

The latest installer preference is MSI first, otherwise one EXE; original handoffs remain preserved as historical inputs. Resolve installer/tool/runtime and other bounded engineering choices during implementation. The immediate next step remains review of this plan and explicit authorization to begin implementation.
