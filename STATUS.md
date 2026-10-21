# CodexMeter status

Updated: 2026-10-20, America/New_York.

## Current state

Repository and planning setup are complete. The expanded-view heading now displays `CodexMeter`; this isolated display-text change is implemented and preview-verified. Banked resets, refresh settings and other approved UI/tray/settings/installer enhancements remain pending. No application release has been published and no website change has been made.

The original handoff is preserved at `CodexMeter-banked-reset-credits-handoff.md`. Read `PROJECT_PLAN.md` for the full definition of done.

Public roadmap: [GitHub issue #1](https://github.com/hudsonhelm/CodexMeter/issues/1), labeled enhancement and assigned to hudsonhelm. Issues were enabled on the fork to publish the scope, milestone checklist, baseline evidence and definition of done. No upstream issue or pull request was opened.

Scope update, 2026-10-20: `docs/project/REFRESH_INTERVAL.md` covers the requested 1/5/15/30/60-minute selector, default 15, persisted and applied immediately. The UI/tray addendum now places it in Settings > General. Source uses 10 minutes, HKCU settings and an in-progress guard; these are baseline inputs, not implemented changes.

UI/tray addendum integrated: `CodexMeter-UI-Tray-Enhancements-Handoff-Addendum.md` is preserved unchanged, with all 21 sections mapped in `docs/project/UI_TRAY_SETTINGS.md`. Plan, refresh spec, acceptance matrix, decisions, release/website guidance, agent instructions and public roadmap incorporate the expanded scope. New requirements supersede earlier fixed-tray, scrolling, direct-menu refresh and ZIP-only assumptions.

Verified source constraints: popup currently enables AutoScroll and caps its height; optional cost hover suppresses native tooltip text; tray uses MostLimited with pace colors and a bottom bar. Resolve layout, hover coexistence, fresh defaults and legacy migration explicitly during implementation. Installer tooling/prerequisites and exact data-migration behavior remain unverified.

## Repository evidence

- Existing repository reused; source fetched from the confirmed Hudson Helm fork.
- `origin`: `https://github.com/hudsonhelm/CodexMeter.git`.
- `upstream`: `https://github.com/Bangerz/CodexMeter.git`.
- Baseline: `7c6a4121144685a01b8f6dcb7b24e258709990eb` (`Use direct goal lifecycle controls for quota rules`), equal on both fetched main branches.
- Current branch: `feature/banked-reset-credits`, created from `origin/main`.
- Local `main` tracks `origin/main`; feature branch tracks `origin/feature/banked-reset-credits` and was first pushed successfully at `db061d7`.
- Standing instruction: automatically commit and push completed project changes to the Hudson Helm fork for this project's duration. Verify remote synchronization and report failures.
- No `AGENTS.md`, contribution guide or GitHub PR template existed in the fetched baseline. Root project instructions were added during planning.
- SDK available: .NET 10.0.402.

## Validation log

Baseline commands run successfully on 2026-10-20 against the unchanged upstream application source:

| Command | Result |
| --- | --- |
| `.\build.ps1` | Exit 0; Release win-x64 publish succeeded; `artifacts/publish/CodexMeter.exe` produced. |
| `dotnet run --project .\tests\CodexMeter.Tests.csproj -c Release` | Exit 0; 95 focused checks passed. |
| `dotnet run --project .\tests\CodexMeter.Tests.csproj -c Release -- --prompt-check` | Exit 0; 36 focused prompt checks passed; no model calls. |
| `dotnet run --project .\tests\CodexMeter.Tests.csproj -c Release -- --goal-check` | Exit 0; 14 focused goal checks passed; no real goals changed. |

The first test invocation used the default .NET CLI environment and printed its first-run setup notice, including local HTTPS development certificate installation. Later invocations used repository-local CLI/cache paths with first-run and certificate generation disabled, consistent with the build script. No production website or certificate was changed.

These results establish a baseline only. The application has not been launched for live verification, and all feature and release acceptance rows remain pending. Generated build output stays ignored under the existing ignore rules.

## Pending work

Expanded-heading verification: `.\build.ps1` passed after changing only the two heading text assignments in `UsagePopup.cs`. The original heading combined Codex with the account plan; the fixed heading is CodexMeter and the secondary text is empty. Plan parsing/data is unchanged. Existing `--preview` output was generated before/after under ignored `artifacts/label-before/` and `artifacts/label-after/`. Visually inspected updated two-window and weekly previews: heading is readable, no header overlap; existing styling/layout/scrollbars remain. Before/after dimensions match: two-window 344x428, weekly 344x297, unavailable 344x271. No new tests or test-framework changes were needed for this text-only edit; no live-account comparison is claimed. The separate scrollbar-removal gate is still pending.

Documentation integration checks: all 21 addendum sections mapped; local Markdown links resolved; Git diff whitespace check passed. No application source, build scripts or tests changed in this update, so baseline test results above were not rerun or represented as new-feature results.

1. Implement and test reset count parsing and popup presentation.
2. Fix popup layout without scrollbars or clipped content at supported display scales/work areas.
3. Implement shared persistent Settings/migration, simple menu and configurable polling; preserve existing detailed controls.
4. Implement shared metrics, native configurable hover, one-second cached countdowns, tray selections/styles/colors and weekly-first exhaustion switching.
5. Verify all automated/visual/live matrices, CPU/resources, restart/resume and no-extra-request behavior.
6. Build per-user installer, preferably Inno Setup; test clean install, upgrade, uninstall, data preservation and synchronized startup.
7. Complete working-day trial and another-machine downloaded-installer test; obtain Commodore's readiness decision.
8. Publish versioned fork installer/checksums, add website link, verify downloaded bytes/installation and record rollback.
9. Keep GitHub roadmap/branch current and prepare coherent upstream contribution description without merging or opening a PR.

## Unresolved release facts

Candidate version/source commit, installer tool/version/hash, optional ZIP hash, live count, tested Codex versions, migration decisions, measured CPU/resource results, independent test machine, readiness decision and public release/download URLs remain unestablished. No credentials or deployment details are stored here.
