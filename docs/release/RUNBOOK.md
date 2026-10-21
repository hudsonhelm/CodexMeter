# Release and website runbook

Status: Procedure prepared; no candidate or publication exists yet.

## Before packaging

Read `PROJECT_PLAN.md`, `STATUS.md`, `docs/project/UI_TRAY_SETTINGS.md` and `docs/validation/ACCEPTANCE.md`. The UI/tray addendum now requires an installer; the existing ZIP script alone cannot satisfy release gates. Check branch, remotes and working tree; preserve user work. Finish validation and review for scope, personal data and generated files.

Select a distinct fork semantic version and update the project version plus any displayed/reported client version consistently. Write accurate candidate release notes and install instructions; preserve upstream attribution, CC0 license and existing branding. Review fork download links and image references in the packaged README. The old upstream release notes are historical evidence, not results for this build.

Release notes and install guidance must cover banked resets, scrollbar fix, Settings-based refresh, configurable native hover, tray selections/styles/colors, exhaustion precedence and installer/startup behavior. Document fresh defaults separately from migrated preferences. Replace baseline 10-minute/lowest-window/ZIP-only claims only as implementation delivers their replacements. Verify no one-second network polling, all settings persistence, manual/launch/resume behavior and CPU/resource results.

## Build and test

Run from the project root. Each command below is independently copyable. Record output and exit status in status/acceptance records.

```powershell
.\build.ps1
```

```powershell
dotnet run --project .\tests\CodexMeter.Tests.csproj -c Release
```

```powershell
dotnet run --project .\tests\CodexMeter.Tests.csproj -c Release -- --prompt-check
```

```powershell
dotnet run --project .\tests\CodexMeter.Tests.csproj -c Release -- --goal-check
```

Use the existing fake helpers for automation checks. Do not exercise real prompts/goals for a display feature. Inspect synthetic preview output; complete live and independent-machine validation against the same candidate.

The following existing command creates a portable ZIP only. After setting the project version, use its matching version if retaining the optional ZIP; replace the example before executing:

```powershell
.\release.ps1 -Version '<candidate-version>'
```

The script builds and runs the default suite, then creates `artifacts/releases/CodexMeter-v<version>-win-x64.zip` and `SHA256SUMS.txt`. Additional suites above are separate gates. The current script can overwrite a same-version ZIP; do not rerun against published artifacts or reuse a version for changed bytes.

Inspect ZIP contents and extract into a fresh directory. Confirm executable, README, license, referenced documentation images and executable checksum are present; no local data, credentials or private diagnostics may be included. Verify both executable and ZIP SHA-256. Document Windows x64, .NET 10 Desktop Runtime, signed-in Codex requirement, unsigned application behavior, startup option and uninstall steps. Fix any broken package references with the smallest justified change.

## Build and validate the installer

Prefer Inno Setup with per-user installation; document a compelling reason for another conventional installer and a strong reason for MSIX. No installer script/compiler check exists yet: inspect availability/prerequisites, then add a repeatable build invocation and record it here. Do not invent a working command before tooling is established.

Choose stable application identity, destination and upgrade detection. Inspect runtime prerequisite handling and any migration needed for settings/private prompt-rule data currently stored beside the executable. Preserve per-user preferences and actual startup choice on upgrades. Respect a running app, avoid duplicate instances and preserve notification shortcut/registration metadata at the installed path.

Require Start Menu shortcut, Add/Remove Programs entry, clean uninstall and the selectable Start CodexMeter with Windows checkbox. A fresh checkbox is shown checked as requested; installation honors either choice, and upgrades preserve existing startup state. Installer, tray and Settings controls must update/read the same actual registration, with no duplicate startup paths. Prefer no admin elevation where feasible.

In a clean standard-user environment test install with startup on and off, app/menu/Settings startup changes, restart, installed upgrade with nondefault settings, portable-to-installed migration, and uninstall. Record OS/runtime/compiler/version, results and any elevation. Confirm all fields/order/colors/metric/style/options and private data survive upgrade. Document preference/data retention and deliberate removal on uninstall; remove only owned registrations/files. Install and smoke-test the exact release bytes, not only a local development publish folder.

Hash the installer and any optional ZIP separately. Package only the intended executable/resources/license/docs; exclude all local settings, account diagnostics, prompt data and signing material. Record immutable filenames, source commit/tag, installer version and SHA-256 before publication.

## Publish the fork release

Record the exact tested commit and create a distinct release tag on that commit. Commit/push on the feature branch; do not merge main. Publish the versioned installer, checksums and accurate notes to the Hudson Helm fork; label any portable ZIP as optional. Verify the tag and downloaded asset hashes. Do not point the website to upstream assets or an untested moving latest download.

## Add the website link

1. Inspect the live HudsonHelm website, current source and actual deployment route. Identify the page/navigation placement and any existing download conventions. Confirm access without storing credentials here.
2. Prepare a dated backup of only the affected content and record how to restore it. Keep the prior download target if one exists.
3. Prepare a small content change identifying the Windows installer, version, prerequisites, fork attribution and release notes/checksums. Link the verified version-specific fork installer or release page. Clearly distinguish any optional portable ZIP.
4. Deploy the reviewed content using the site's established mechanism. This scope covers the download link/content, not DNS, mail, Exchange, IIS, TLS or hosting changes.
5. Navigate publicly from the website to the download, check redirects/HTTPS and installer filename, and download it. Match SHA-256 to the tested candidate. Install it in the clean test environment and confirm tray/settings/tooltip/reset behavior and startup integration; verify uninstall/upgrade against the recorded matrix.
6. Record the public page URL, target URL, source commit, candidate hash, verification timestamp and rollback details in the acceptance record. A page returning HTTP 200 alone does not prove the correct binary was delivered.

## Rollback and maintenance

If the link or release is defective, restore the previous website content/target promptly; do not leave friends with a known bad download. Preserve the failing artifact and evidence privately for diagnosis. Use a new version for corrected bytes and repeat the applicable tests. Document any withdrawn release clearly.

For future updates, repeat build, regression, fresh-download verification and website-target checks. Verify compatibility after relevant Codex protocol changes. Keep a versioned record of what was actually released.

## Closeout record

Record candidate version, commit/tag, fork branch, release/asset URL, installer compiler/version/SHA-256, optional ZIP hash, public website URL, downloaded-installer lifecycle checks, all presentation/settings/performance results, trial observations, Commodore's readiness decision, limitations and rollback location. Prepare accurate upstream descriptions in coherent feature groups; some enhancements may remain fork-specific. Follow contribution instructions if any have appeared; do not open the PR unless instructed.
