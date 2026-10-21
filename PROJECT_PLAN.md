# CodexMeter project plan

Date: 2026-10-20, America/New_York.

Status: Planning and repository preparation complete; implementation pending.

## Outcome

Deliver the Hudson Helm fork of CodexMeter with the available banked usage reset count visible in its existing popup. Finish when we have enough evidence to confidently send the tested download to friends and a working download link is published on the HudsonHelm website.

The original handoff defines the feature. This plan extends completion through distribution and website verification. It does not authorize sending messages to friends. Commodore makes the final readiness decision from the recorded evidence.

## Verified starting point

- Workspace: `P:\CodexMeter`.
- Fork: <https://github.com/hudsonhelm/CodexMeter>; upstream: <https://github.com/Bangerz/CodexMeter>.
- Both main branches matched `7c6a4121144685a01b8f6dcb7b24e258709990eb` at setup.
- Existing local Git repository was empty except for the supplied handoff. Source was fetched into that repository without recloning or reinitializing it.
- Feature branch: `feature/banked-reset-credits`; local `main` tracks `origin/main`.
- Application: C# Windows Forms, .NET 10, Windows x64. Source stays at the repository root.
- Parser/model: `Usage.cs`; read-only RPC client: `CodexClient.cs`; popup: `UsagePopup.cs`; existing console test harness: `tests/Program.cs`.
- Existing build/package scripts: `build.ps1` and `release.ps1`. Preserve the framework-dependent ZIP distribution.
- .NET SDK 10.0.402 is installed. Build/test outcomes belong in `STATUS.md`.

## Scope

Read top-level `rateLimitResetCredits.availableCount` from the existing `account/rateLimits/read` result. Model the value as nullable; show `N available` beside `Usage resets` for valid counts including zero, and omit the row when unavailable. Include the row in popup sizing, scrolling, accessibility, and stale-data handling.

Do not change quota-window parsing, ordering, dates, plan handling, tray calculations, refresh frequency, startup, authentication, or optional automation. Do not display credit metadata or add redemption. Keep the existing framework, branding, dependencies, and installer approach.

Official protocol reference, checked 2026-10-20: <https://learn.chatgpt.com/docs/app-server#6-rate-limits-chatgpt>. The documented count is authoritative; credit detail rows may be capped or unavailable. Live support in the installed Codex version still requires verification.

## Milestones and exit gates

| Milestone | Work | Exit evidence | State |
| --- | --- | --- | --- |
| 0. Establish baseline | Read complete handoff; verify fork/upstream; preserve source structure; create feature branch and project records; run baseline build and tests. | Source SHA, remotes, commands/results, current limitations in status. | Complete |
| 1. Implement count | Add nullable model property and safe parsing; add popup row and height/scroll handling using existing conventions. | Focused diff; no new RPC/auth/polling path; no tray calculation changes. | Pending |
| 2. Automated and visual regression | Extend existing harness; test parser edge cases, request sequence, ordinary usage behavior, and popup states. | Passing test/build output and inspected synthetic previews recorded in validation report. | Pending |
| 3. Live confidence | Compare live count with the signed-in Codex usage UI; test refresh, reopen, stale/failure recovery, startup/exit and display scaling. | Same-account observations with timestamps, Codex version, Windows/display details, and explicit unknowns. | Pending |
| 4. Friend-ready candidate | Assign distinct fork version; package exact tested commit; verify ZIP contents/checksums; try clean standard-user installation and another Windows machine. | Download instructions, candidate hashes, independent-machine result, defect disposition and Commodore readiness decision. | Pending |
| 5. Publish download | Publish a versioned fork release; inspect site; back up affected content; add HudsonHelm download link; verify public navigation and downloaded bytes. | Release URL, site URL, final target, downloaded ZIP SHA-256, timestamp and rollback record. | Pending |
| 6. Close and upstream preparation | Commit/push feature branch; review final diff; prepare accurate upstream PR description and final handoff. | Commit/push confirmation and completed acceptance record. No main merge or upstream PR opening unless requested. | Pending |

Milestones 1–5 depend on the preceding gate. Upstream PR preparation may run alongside packaging after tests pass. Upstream acceptance is not a prerequisite for a fork release. Any discovered blocker stays visible rather than being marked passed by assumption.

## Required validation

Use `docs/validation/ACCEPTANCE.md` for the detailed matrix. Required parser cases include positive, zero, absent object/count, explicit null, wrong types, fractional values, overflow, and negative values. Treat invalid supplemental data as unavailable without losing normal usage data. Preserve existing windows, plan, timestamps, bucket order and most-limited selection in fixtures.

Run the existing default, prompt and goal checks. Those optional-feature checks use fake helpers; do not run real prompts or change live goals. Review popup behavior for zero, positive, unknown, stale and multi-bucket snapshots at common display scales. Keep all account-specific evidence private.

Proposed confidence threshold: a complete ordinary working day on the primary machine, including several automatic refreshes and sleep/resume if available, plus a successful fresh installation and smoke test on at least one other Windows 11 x64 machine or clean VM. Record actual duration and limitations. If the live account only has zero or unavailable reset data, fixture tests establish positive display behavior, and the live limitation must be disclosed in the readiness decision.

## Distribution and website

Prefer an immutable, version-specific ZIP on the Hudson Helm GitHub fork, linked from the existing website. Confirm the site's current content, source/deployment access and desired placement before implementation. Do not build a new website or alter hosting infrastructure for this link.

Preserve the existing ZIP model, include the license, install instructions, runtime requirements, unsigned-executable notice and hashes. Review README release links so the fork package leads to the correct fork download. Existing release notes describe an older 15-minute interval while current README/client UI use 10 minutes; write candidate notes from verified current behavior.

Use `docs/release/RUNBOOK.md`. A successful upload alone is insufficient: click through the public website path, download the file, match its hash to the tested candidate, and smoke-test that downloaded copy. Capture the previous website content and a rollback path before publication.

## Completion checklist

- [ ] All feature acceptance criteria in the original handoff are satisfied and evidenced.
- [ ] Required automated suites and Release build pass on the final candidate.
- [ ] Popup previews and live/manual observations support the intended behavior.
- [ ] Fresh install, independent-machine smoke test and primary-machine trial are recorded.
- [ ] No unresolved defect prevents safe installation, accurate quota display, refresh or exit; remaining limitations are documented.
- [ ] Commodore has reviewed the evidence and is confident sending the package to friends.
- [ ] Versioned fork artifact, installation notes, license and SHA-256 are public and consistent.
- [ ] HudsonHelm website has a public, working download link to the tested artifact; downloaded bytes and clean launch are verified.
- [ ] Website rollback details and maintenance instructions are recorded.
- [ ] Final changes are committed/pushed on the feature branch; upstream PR description is prepared.
- [ ] `STATUS.md` names the release, source commit, artifact hash, public URLs and any residual caveats.

## Risks and next actions

1. Installed Codex may not expose resets yet: verify availability, preserve null, and record the compatibility limit.
2. Popup height is calculated from usage cards: account for the additional row before calculating scroll width and height.
3. Existing packages are unsigned and require .NET 10 Desktop Runtime: test and document the actual download/install experience. Do not introduce signing or a new installer by default.
4. Optional prompt/goal functionality already exists: keep it disabled in clean-install validation and preserve it through fake-helper regression checks.
5. Website deployment access and current download placement are unverified. A historical website TLS project path was not present during setup; discover the current source/access before publishing.
6. Keep project planning separate from the eventual minimal upstream implementation diff.

Next implementation step: finish recording baseline checks, inspect all existing snapshot construction/preview paths, then implement the nullable count with focused parser tests before modifying popup layout.
