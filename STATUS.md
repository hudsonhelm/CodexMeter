# CodexMeter status

Updated: 2026-10-20, America/New_York.

## Current state

Repository and planning setup are complete. The banked-reset feature and configurable refresh interval are not implemented. No release has been published and no website change has been made.

The original handoff is preserved at `CodexMeter-banked-reset-credits-handoff.md`. Read `PROJECT_PLAN.md` for the full definition of done.

Public roadmap: [GitHub issue #1](https://github.com/hudsonhelm/CodexMeter/issues/1), labeled enhancement and assigned to hudsonhelm. Issues were enabled on the fork to publish the scope, milestone checklist, baseline evidence and definition of done. No upstream issue or pull request was opened.

Scope update, 2026-10-20: added `docs/project/REFRESH_INTERVAL.md` for the requested 1/5/15/30/60-minute tray selector, persisted settings and immediate timer application. Required default is 15 minutes. Source inspection confirms the fetched baseline uses 10 minutes, existing settings use `HKCU\Software\CodexMeter`, and polling already has an in-progress guard. These are implementation inputs, not evidence that the new behavior is implemented.

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

1. Implement and test reset count parsing and popup presentation.
2. Implement configurable interval with 15-minute default, existing per-user persistence, checked menu, immediate timer/deadline updates, dynamic status text and concurrency/launch/resume regression checks.
3. Verify live count, interval selection/restart/resume and ordinary tray behavior; record compatibility and account limitations.
4. Complete friend-readiness trial, clean install and another-machine test.
5. Create versioned fork candidate and verify package contents and hashes.
6. Inspect current HudsonHelm website and deployment access, publish link, verify downloaded artifact and rollback.
7. Obtain Commodore's readiness decision, commit/push completed work and prepare upstream description.

## Unresolved release facts

Candidate version, source commit, ZIP hash, live count, tested Codex versions, independent test machine, readiness decision, public release URL and website download URL are not yet established. No credentials or deployment details are stored in these documents.
