# Project decisions

## 2026-10-20 — Source and scope

Use the existing local repository and the confirmed `hudsonhelm/CodexMeter` fork. Fetch source into the empty repository rather than replacing or recloning it. Preserve the supplied handoff. Both fetched main branches matched at setup; feature development starts at that shared commit.

Retain the upstream root-level C# files, tests, scripts and docs. No speculative `src/` relocation or new framework scaffold is needed. Project records live in this folder; acceptance and release work have separate folders beneath `docs/`.

## 2026-10-20 — Feature semantics

Use the existing account-rate-limit response and parser. A nullable nonnegative integer distinguishes missing/invalid data from confirmed zero. Prefer omitting an unavailable row. Count reset credits separately from ordinary quota windows. Do not redeem a credit or depend on individual credit records.

## 2026-10-20 — Completion and evidence

The user extended the handoff's feature goal through friend readiness and a HudsonHelm website download link. Feature tests alone do not complete the project. Proposed confidence gates include a working-day trial, fresh install and a second Windows machine or VM; final readiness belongs to Commodore and must be recorded explicitly.

## 2026-10-20 — Release approach

Original decision: preserve the existing framework-dependent Windows x64 ZIP workflow. Superseded by the UI/tray addendum: a conventional Windows installer, preferably per-user Inno Setup, is now required and is the primary download; a portable ZIP may remain secondary. Prefer a versioned fork release and website link to that exact tested installer. Hosting choice and exact website placement remain provisional until inspected. Do not reuse an upstream version for different fork bytes.

Do not merge main or open an upstream PR under the handoff alone. Prepare its description after verification. No messages to friends are authorized by the readiness goal.

## 2026-10-20 — Website boundary

Historical memory was used only to preserve the separation between website work and mail infrastructure. Hosting details have not been revalidated. The old `P:\HH_Website_Cert_2026` path was absent during setup. Discover current deployment access; do not infer it from historical certificates or server addresses.

## 2026-10-20 — Configurable refresh scope addition

Commodore added an interval selector for 1, 5, 15, 30 and 60 minutes, default 15, persisted and applied without restart. Originally placed in the tray menu; the later addendum moves it into Settings > General. This supersedes the prior frequency restriction. Preserve the original handoff and record the integrated requirement in `REFRESH_INTERVAL.md`.

Verified source currently uses 10 minutes in `TrayApp.PollInterval` and popup status text. Implement the requested 15-minute default rather than preserving that baseline value. Reuse the existing per-user registry convention and Windows Forms polling timer; keep manual/launch/due-on-resume behavior and in-progress protection.

For an idle interval change, restart the countdown from selection time without fetching immediately. During a running refresh, keep its timer stopped and use the latest selected interval at completion. Keep Settings selection, deadline, stale threshold and display text consistent. The later addendum explicitly authorizes centralized Settings and a separate one-second local presentation timer. All approved feature groups must pass before release.

## 2026-10-20 — Automatic commits and GitHub pushes

Commodore authorized committing and pushing all project changes for the duration of this project without further requests. Apply this to code, documentation and planning changes on the feature branch in the Hudson Helm fork. Use coherent commits and appropriate checks, verify remote synchronization, and report failures. Keep private/generated artifacts excluded. Existing restrictions on merging main and opening an upstream PR remain in force.

## 2026-10-20 — UI/tray addendum integrated

The user explicitly requested full integration and publication of `CodexMeter-UI-Tray-Enhancements-Handoff-Addendum.md`. Preserve it unchanged. `UI_TRAY_SETTINGS.md` maps all 21 sections into requirements and validation gates. Latest approved scope supersedes the old fixed-tray/no-installer/direct-refresh-menu restrictions; ordinary usage data semantics and authentication stay preserved.

Central Settings hosts detailed controls, including existing prompt rules, notification testing and cost history. The context menu follows the addendum's simple layout. Native quota hover remains native; preserve the existing optional cost feature without blanking quota tooltip text. A shared metric model supports surface-specific formatting and one-second cached presentation without second-based polling.

Fresh defaults: 15-minute polling, requested default hover fields/order, 5 Hour Remaining tray metric, automatic colors off and exhausted-countdown override on. Proposed initial percentage style is Small Number + Bar to follow the existing renderer. Preserve legacy user choices through explicit migration; exact legacy-mode mapping and unknown-window behavior remain implementation decisions. Weekly exhaustion wins, and local time passing cannot confirm recovery.

Installer is now required, preferably per-user Inno Setup, with startup checkbox, one actual startup registration, Start Menu, Add/Remove Programs, clean upgrades and uninstall. Preserve settings/data on upgrades. Finalize prerequisite handling, data migration and uninstall retention from source inspection and lifecycle tests. Publishing these documents is not an application release.

## 2026-10-20 — Expanded-view heading text

Commodore requested exactly `CodexMeter` in place of the expanded view's visible `Codex Plus`. Source composes that heading from a fixed Codex label and a dynamic plan label. Change the fixed text to CodexMeter and clear the plan-label text, retaining existing controls, font, positions, dimensions, AutoScroll, sizing and event behavior. Keep plan data/parser unchanged. This is scoped separately from the still-pending scrollbar fix and does not rename hover text, tray labels or other surfaces. Release build passed and synthetic weekly/two-window renders show the new text with unchanged dimensions/layout.
