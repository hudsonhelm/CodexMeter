# Release and website runbook

Status: Procedure prepared; no candidate or publication exists yet.

## Before packaging

Read `PROJECT_PLAN.md`, `STATUS.md` and `docs/validation/ACCEPTANCE.md`. Check branch, remotes and working tree; preserve user work. Finish required validation and review the diff for scope, personal data and accidental generated files.

Select a distinct fork semantic version and update the project version plus any displayed/reported client version consistently. Write accurate candidate release notes and install instructions; preserve upstream attribution, CC0 license and existing branding. Review fork download links and image references in the packaged README. The old upstream release notes are historical evidence, not results for this build.

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

After setting the project version, package with that matching version; replace the example before executing:

```powershell
.\release.ps1 -Version '<candidate-version>'
```

The script builds and runs the default suite, then creates `artifacts/releases/CodexMeter-v<version>-win-x64.zip` and `SHA256SUMS.txt`. Additional suites above are separate gates. The current script can overwrite a same-version ZIP; do not rerun against published artifacts or reuse a version for changed bytes.

Inspect ZIP contents and extract into a fresh directory. Confirm executable, README, license, referenced documentation images and executable checksum are present; no local data, credentials or private diagnostics may be included. Verify both executable and ZIP SHA-256. Document Windows x64, .NET 10 Desktop Runtime, signed-in Codex requirement, unsigned application behavior, startup option and uninstall steps. Fix any broken package references with the smallest justified change.

## Publish the fork release

Record the exact tested commit and create a distinct release tag on that commit. Commit/push on the feature branch; do not merge main. Publish the versioned ZIP, checksums and accurate notes to the Hudson Helm fork. Verify that the tag resolves to the recorded commit and the downloaded assets match local hashes. Do not point a website link to the upstream project's release or to an untested moving latest asset.

## Add the website link

1. Inspect the live HudsonHelm website, current source and actual deployment route. Identify the page/navigation placement and any existing download conventions. Confirm access without storing credentials here.
2. Prepare a dated backup of only the affected content and record how to restore it. Keep the prior download target if one exists.
3. Prepare a small content change with a clear CodexMeter download label, version, Windows/runtime requirements, fork attribution and access to release notes/checksums. Use the verified version-specific fork release or asset URL.
4. Deploy the reviewed content using the site's established mechanism. This scope covers the download link/content, not DNS, mail, Exchange, IIS, TLS or hosting changes.
5. Navigate publicly from the website to the download, check redirects/HTTPS and filename, and download the artifact. Match its SHA-256 to the tested candidate. Launch the downloaded copy in the clean test environment and confirm ordinary usage/reset behavior.
6. Record the public page URL, target URL, source commit, candidate hash, verification timestamp and rollback details in the acceptance record. A page returning HTTP 200 alone does not prove the correct binary was delivered.

## Rollback and maintenance

If the link or release is defective, restore the previous website content/target promptly; do not leave friends with a known bad download. Preserve the failing artifact and evidence privately for diagnosis. Use a new version for corrected bytes and repeat the applicable tests. Document any withdrawn release clearly.

For future updates, repeat build, regression, fresh-download verification and website-target checks. Verify compatibility after relevant Codex protocol changes. Keep a versioned record of what was actually released.

## Closeout record

Record candidate version, commit/tag, fork branch, release/asset URL, ZIP SHA-256, public website URL, downloaded-copy check, test results, trial observations, Commodore's readiness decision, known limitations and rollback location. Prepare the upstream PR description from the final implementation and actual results; follow upstream contribution instructions if any have appeared. Do not open the PR unless instructed.
