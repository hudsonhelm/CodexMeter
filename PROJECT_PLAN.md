# CodexMeter project plan

Date: 2026-10-20, America/New_York.

Status: Planning and repository preparation complete; implementation pending.

GitHub roadmap and progress checklist: [issue #1](https://github.com/hudsonhelm/CodexMeter/issues/1).

## Outcome

Deliver the Hudson Helm fork as a Windows tray instrument: a scrollbar-free detailed popup, configurable native hover report, one selectable live tray metric, centralized Settings, banked resets, configurable polling and a proper Windows installer. Finish when we have enough evidence to confidently send the tested installer to friends and a working download link is published on the HudsonHelm website.

The original handoff defines banked-reset data semantics. The refresh request and the user-approved `CodexMeter-UI-Tray-Enhancements-Handoff-Addendum.md` extend it. The addendum supersedes earlier restrictions on fixed tray behavior, installer introduction and direct-menu configuration. `docs/project/UI_TRAY_SETTINGS.md` integrates all 21 addendum sections and records remaining design decisions. This plan extends completion through distribution and website verification. It does not authorize sending messages to friends. Commodore makes the final readiness decision from the recorded evidence.

## Verified starting point

- Workspace: `P:\CodexMeter`.
- Fork: <https://github.com/hudsonhelm/CodexMeter>; upstream: <https://github.com/Bangerz/CodexMeter>.
- Both main branches matched `7c6a4121144685a01b8f6dcb7b24e258709990eb` at setup.
- Existing local Git repository was empty except for the supplied handoff. Source was fetched into that repository without recloning or reinitializing it.
- Feature branch: `feature/banked-reset-credits`; local `main` tracks `origin/main`.
- Application: C# Windows Forms, .NET 10, Windows x64. Source stays at the repository root.
- Parser/model: `Usage.cs`; read-only RPC client: `CodexClient.cs`; popup: `UsagePopup.cs`; existing console test harness: `tests/Program.cs`.
- Existing build/package scripts: `build.ps1` and `release.ps1`, currently producing a framework-dependent ZIP. Add the approved installer build/release path; retain a portable ZIP only as an optional secondary artifact.
- .NET SDK 10.0.402 is installed. Build/test outcomes belong in `STATUS.md`.

## Scope

Read top-level `rateLimitResetCredits.availableCount` from the existing `account/rateLimits/read` result. Model the value as nullable; show `N available` beside `Usage resets` for valid counts including zero, and omit the row when unavailable. Include the row in popup sizing, accessibility and stale-data handling. Fix the underlying layout so neither horizontal nor vertical scrollbars appear for supported content/display scenarios; preserve the polished dark dashboard.

Put refresh choices of 1, 5, 15, 30 and 60 minutes in Settings > General; default to 15 without a valid saved setting. Reuse the existing per-user settings convention; persist/apply immediately without restart. Preserve manual, launch and due-on-resume behavior and the single-poll guard. Keep deadline, stale logic, text and diagnostics consistent. See `docs/project/REFRESH_INTERVAL.md`.

Centralize configuration in Settings (General, Hover and Tray; Appearance only if useful). Keep the context menu simple: Show limits, Refresh now, Settings..., Start with Windows, Quit. Preserve existing optional prompt rules, cost-history controls and notification test access by relocating them into Settings. Add shared metric formatting for native hover and tray; native quota hover must remain available with optional cost features enabled.

Support independently enabled/reordered hover fields and defaults, native tooltip length enforcement, all six required tray selections, both percentage styles, per-metric fixed colors, optional automatic status colors, and weekly-first exhausted-countdown switching with confirmed recovery. Update presentation from cached timestamps approximately once per second, separately from polling. Add a conventional per-user installer, preferably Inno Setup, with install/upgrade/uninstall, Start Menu, Add/Remove Programs, settings preservation and a synchronized startup checkbox.

Preserve quota-window parsing/order, reset timestamps, plan handling, authentication, optional automation semantics and existing startup/launch/resume functionality. Approved presentation and installation changes replace only their earlier restrictions. No redemption, detailed credit metadata, custom replacement tooltip, two-line tray countdown, telemetry, cloud dependency, database, framework switch or broad dependency upgrade.

Official protocol reference, checked 2026-10-20: <https://learn.chatgpt.com/docs/app-server#6-rate-limits-chatgpt>. The documented count is authoritative; credit detail rows may be capped or unavailable. Live support in the installed Codex version still requires verification.

## Milestones and exit gates

| Milestone | Work | Exit evidence | State |
| --- | --- | --- | --- |
| 0. Establish baseline | Read complete handoff; verify fork/upstream; preserve source structure; create feature branch and project records; run baseline build and tests. | Source SHA, remotes, commands/results, current limitations in status. | Complete |
| 1. Data and layout | Add nullable banked-reset parsing and popup row; resolve scrollbar causes without clipping or redesign. | Parser tests and scrollbar-free synthetic/live layouts. | Pending |
| 1a. Shared settings and polling | Add small persistent settings, migration, centralized UI, simple context menu and immediate interval application. | All settings reload; existing features reachable; manual/launch/resume and concurrency checks pass. | Pending |
| 1b. Native hover and live metrics | Shared metric model, configurable fields/order/defaults, tooltip budget, second-accurate cached countdowns. | Exact formatting, unavailable/expired behavior, native length and no-poll tick tests. | Pending |
| 1c. Tray instrument | Metric selector, percentage styles, compact countdown, per-metric colors, automatic colors and weekly-first exhaustion override. | Selection/rendering/color/precedence/recovery tests and actual-resolution previews. | Pending |
| 2. Regression and live confidence | Run all tests/builds; inspect UI at 100/125/150% (200% where practical), verify live account, timer behavior, startup/exit and CPU/resource use. | Recorded automated, visual, live and idle-vs-ticking measurements; no added quota requests from presentation. | Pending |
| 3. Installer | Inspect packaging/runtime/startup/notification needs; implement preferably per-user Inno Setup and test install/upgrade/uninstall. | Start Menu and Add/Remove Programs work, settings survive upgrade, one synchronized startup entry; no unjustified elevation. | Pending |
| 4. Friend-ready candidate | Assign distinct fork version; package exact tested commit; verify installer/checksums; complete working-day trial and clean installation on another machine. | Candidate hashes, independent-machine results, defect disposition and Commodore readiness decision. | Pending |
| 5. Publish download | Publish a versioned fork installer; inspect site; back up affected content; add HudsonHelm download link; verify public navigation, downloaded bytes and installation. | Release URL, site URL, final target, downloaded installer SHA-256, install smoke test, timestamp and rollback record. | Pending |
| 6. Close and upstream preparation | Commit/push feature branch; review final diff; prepare accurate upstream PR description and final handoff. | Commit/push confirmation and completed acceptance record. No main merge or upstream PR opening unless requested. | Pending |

Implement data/settings foundations before shared presentation, then complete regression and installer gates before the release trial and publication. Review installer feasibility early to avoid late startup/runtime surprises. Upstream PR preparation may run alongside packaging after tests pass; keep coherent feature commits even if some enhancements remain fork-specific. Upstream acceptance is not a release prerequisite. Record blockers explicitly.

## Required validation

Use `docs/validation/ACCEPTANCE.md` for the detailed matrix. Required parser cases include positive, zero, absent object/count, explicit null, wrong types, fractional values, overflow, and negative values. Treat invalid supplemental data as unavailable without losing normal usage data. Preserve existing windows, plan, timestamps, bucket order and most-limited selection in fixtures.

Run the existing default, prompt and goal checks. Those optional-feature checks use fake helpers; do not run real prompts or change live goals. Review popup behavior for zero, positive, unknown, stale and multi-bucket snapshots at common display scales. Keep all account-specific evidence private.

The addendum matrix in `docs/validation/ACCEPTANCE.md` is mandatory: tooltip fields/order/length, exact hover countdowns, D/H/M/s boundaries, every tray metric, both percentage styles, color persistence and toggle behavior, weekly-first exhaustion/recovery, all settings reload/migration, scrollbar-free layout, installer lifecycle/startup synchronization and negligible CPU with the one-second timer. Never infer replenished quota from a cached countdown reaching zero.

Add focused interval checks for a fresh/default setting, every allowed value, restart persistence, invalid saved values, immediate timer/deadline updates, selection during a running poll, manual refresh, launch, due/not-due resume, and no overlapping polls. Use deterministic scheduling checks/fake helpers rather than waiting 60 minutes in automated tests or changing the user's real registry preferences. Verify actual menu and restart behavior manually.

Proposed confidence threshold: a complete ordinary working day on the primary machine, including several automatic refreshes and sleep/resume if available, plus a successful fresh installation and smoke test on at least one other Windows 11 x64 machine or clean VM. Record actual duration and limitations. If the live account only has zero or unavailable reset data, fixture tests establish positive display behavior, and the live limitation must be disclosed in the readiness decision.

## Distribution and website

Publish an immutable, version-specific Windows installer on the Hudson Helm GitHub fork and link it from the existing website. Prefer Inno Setup, per-user installation and no administrator requirement where feasible. Record any justified alternative; avoid MSIX without a strong reason. A portable ZIP may remain optional and cannot substitute for installer acceptance. Confirm current site content/source/access/placement before website work.

Include license, install/upgrade/uninstall/startup instructions, runtime requirements, accurate signing status and hashes for every artifact. Review fork release links. The fetched source/README use 10 minutes and a lowest-window tray; final documentation must reflect 15-minute configurable polling, fresh-install tray defaults, settings migration, native hover, automatic override and the installer. Historical release notes remain historical; do not claim planned features are already available.

Use `docs/release/RUNBOOK.md`. A successful upload alone is insufficient: click through the public website path, download the file, match its hash to the tested candidate, and smoke-test that downloaded copy. Capture the previous website content and a rollback path before publication.

## Completion checklist

- [ ] Original banked-reset data acceptance criteria are satisfied; superseded presentation/installer restrictions are resolved by the addendum.
- [ ] Configurable refresh acceptance criteria in `docs/project/REFRESH_INTERVAL.md` pass, including 15-minute default, persistence, immediate application and no overlapping polls.
- [ ] All 21 addendum sections are covered by implementation and the evidence matrix in `docs/project/UI_TRAY_SETTINGS.md`.
- [ ] Popup has no scrollbars/clipping; native tooltip and selectable tray presentation work with persisted settings and required defaults.
- [ ] Second-based countdowns use cached timestamps only; measured CPU/resource behavior is negligible and no extra quota requests occur.
- [ ] Installer clean install, upgrade/settings preservation, Start Menu, Add/Remove Programs, uninstall and synchronized startup checks pass.
- [ ] Required automated suites and Release build pass on the final candidate.
- [ ] Popup previews and live/manual observations support the intended behavior.
- [ ] Fresh install, independent-machine smoke test and primary-machine trial are recorded.
- [ ] No unresolved defect prevents safe installation, accurate quota display, refresh or exit; remaining limitations are documented.
- [ ] Commodore has reviewed the evidence and is confident sending the package to friends.
- [ ] Versioned fork installer, installation notes, license and SHA-256 are public and consistent; optional ZIP is separately identified and verified.
- [ ] HudsonHelm website has a public, working download link to the tested artifact; downloaded bytes and clean launch are verified.
- [ ] Website rollback details and maintenance instructions are recorded.
- [ ] Final changes are committed/pushed on the feature branch; upstream PR description is prepared.
- [ ] `STATUS.md` names the release, source commit, artifact hash, public URLs and any residual caveats.

## Risks and next actions

1. Installed Codex may not expose resets yet: verify availability, preserve null, and record the compatibility limit.
2. Current popup uses AutoScroll and a capped height. Resolve underlying layout for expected content and monitor work areas, including multiple buckets, without simply hiding overflow; record any physical fit limit before release.
3. Existing packages are unsigned and require .NET 10 Desktop Runtime: inspect prerequisite handling, stable installation identity, startup/notification registration and settings migration before choosing installer details. Do not promise signing or runtime bundling without evidence.
4. Optional prompt/goal functionality already exists: keep it disabled in clean-install validation and preserve it through fake-helper regression checks.
5. Website deployment access and current download placement are unverified. A historical website TLS project path was not present during setup; discover the current source/access before publishing.
6. Keep project planning separate from the eventual minimal upstream implementation diff.
7. The current fixed interval is used by timer, deadline and staleness logic, and the popup hardcodes 10 minutes. Update these consistently; keep the in-progress guard effective when menu/timer/resume events coincide.
8. Existing optional cost hover suppresses native tooltip text. Preserve optional cost features while keeping the required native quota tooltip available; do not create a new custom quota hover.
9. Fresh tray defaults differ from the old most-limited/pace-color behavior. Explicitly separate fresh defaults from preserving legacy preferences; finalize migration and missing-window behavior before implementation tests.
10. Tooltip length, small-icon readability, expired/stale timestamps and one-second resource churn need deterministic tests and actual tray checks. A standard icon asset and exhausted-countdown unknown fallback require documented choices.

Next implementation step: inspect all snapshot/preview/settings/startup/notification paths and installer feasibility; settle bounded presentation/migration decisions from `docs/project/UI_TRAY_SETTINGS.md`. Implement in the milestone order above with separately reviewable commits. Baseline checks are complete; all requested features remain pending.
