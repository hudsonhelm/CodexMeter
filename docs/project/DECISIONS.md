# Project decisions

## 2026-10-20 — Source and scope

Use the existing local repository and the confirmed `hudsonhelm/CodexMeter` fork. Fetch source into the empty repository rather than replacing or recloning it. Preserve the supplied handoff. Both fetched main branches matched at setup; feature development starts at that shared commit.

Retain the upstream root-level C# files, tests, scripts and docs. No speculative `src/` relocation or new framework scaffold is needed. Project records live in this folder; acceptance and release work have separate folders beneath `docs/`.

## 2026-10-20 — Feature semantics

Use the existing account-rate-limit response and parser. A nullable nonnegative integer distinguishes missing/invalid data from confirmed zero. Prefer omitting an unavailable row. Count reset credits separately from ordinary quota windows. Do not redeem a credit or depend on individual credit records.

## 2026-10-20 — Completion and evidence

The user extended the handoff's feature goal through friend readiness and a HudsonHelm website download link. Feature tests alone do not complete the project. Proposed confidence gates include a working-day trial, fresh install and a second Windows machine or VM; final readiness belongs to Commodore and must be recorded explicitly.

## 2026-10-20 — Release approach

Preserve the existing framework-dependent Windows x64 ZIP workflow. Prefer a versioned release on the fork and a website link to that exact artifact. Hosting choice and exact website placement remain provisional until the current site and access are inspected. Do not reuse an upstream version to identify different fork bytes.

Do not merge main or open an upstream PR under the handoff alone. Prepare its description after verification. No messages to friends are authorized by the readiness goal.

## 2026-10-20 — Website boundary

Historical memory was used only to preserve the separation between website work and mail infrastructure. Hosting details have not been revalidated. The old `P:\HH_Website_Cert_2026` path was absent during setup. Discover current deployment access; do not infer it from historical certificates or server addresses.

## 2026-10-20 — Configurable refresh scope addition

Commodore added a tray-menu interval selector for 1, 5, 15, 30 and 60 minutes, default 15, persisted and applied without restart. This explicitly supersedes the prior plan/handoff restriction on changing refresh frequency for interval configuration. Preserve the original handoff and record the addition in `REFRESH_INTERVAL.md`.

Verified source currently uses 10 minutes in `TrayApp.PollInterval` and popup status text. Implement the requested 15-minute default rather than preserving that baseline value. Reuse the existing per-user registry convention and Windows Forms polling timer; keep manual/launch/due-on-resume behavior and in-progress protection.

For an idle interval change, restart the countdown from selection time without fetching immediately. During a running refresh, keep its timer stopped and use the latest selected interval at completion. Keep selected-menu state, deadline, stale threshold and display text consistent. The new setting does not authorize a settings redesign or changes to other timers. Both feature sets must pass before release.

## 2026-10-20 — Automatic commits and GitHub pushes

Commodore authorized committing and pushing all project changes for the duration of this project without further requests. Apply this to code, documentation and planning changes on the feature branch in the Hudson Helm fork. Use coherent commits and appropriate checks, verify remote synchronization, and report failures. Keep private/generated artifacts excluded. Existing restrictions on merging main and opening an upstream PR remain in force.
