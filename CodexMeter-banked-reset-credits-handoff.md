# CodexMeter — Banked Reset Credits Feature Handoff

## Objective

Add support for displaying **Codex banked usage resets** in CodexMeter.

The goal is deliberately narrow: CodexMeter already reads Codex usage/rate-limit data and is nearly perfect as-is. Extend the existing implementation so the tray popup also shows the number of banked resets currently available, matching the information shown by OpenAI's Codex usage UI.

Example desired display:

```text
Usage remaining

5h                         21%    11:08 PM
Weekly                     76%    Oct 27

Usage resets               2 available
```

Exact spacing/layout should follow the application's existing UI conventions rather than forcing this literal formatting.

---

## Repository State

Assume the following are already complete:

- The upstream `Bangerz/CodexMeter` repository has already been forked to the Hudson Helm GitHub account/organization.
- A local Git repository already exists and is connected appropriately.
- Do **not** recreate, reclone, reinitialize, or replace the repository.
- Do **not** make unrelated architectural or cosmetic changes.

Before modifying anything, inspect the current repository, current branch, remotes, build instructions, test structure, and existing implementation.

---

## Git Workflow

Create a dedicated feature branch from the current upstream-compatible main branch.

Suggested branch name:

```text
feature/banked-reset-credits
```

Keep the change tightly scoped.

Before editing:

1. Confirm the working tree is clean.
2. Confirm the expected fork and upstream remotes.
3. Confirm the current branch is based on the latest appropriate `main`.
4. Do not overwrite or discard existing user work.

When complete:

- Commit the implementation with a clear commit message.
- Push the feature branch to the Hudson Helm fork.
- Prepare the work so it is suitable for an upstream pull request to `Bangerz/CodexMeter`.
- Do **not** merge into `main` unless explicitly instructed.

Suggested commit message:

```text
Add banked Codex reset credits to usage popup
```

---

## Existing Architecture to Preserve

CodexMeter already obtains usage data from the local Codex app-server. The banked-reset feature should use the **existing app-server response and refresh path**.

Do not:

- scrape the ChatGPT or Codex web UI;
- read browser cookies;
- add a second authentication mechanism;
- read OpenAI credentials directly unless CodexMeter already does so as part of its established architecture;
- add a separate external HTTP request when the value is already available through the existing app-server response;
- consume model tokens merely to retrieve quota data.

The implementation should remain consistent with CodexMeter's current security and polling model.

---

## Data to Add

Inspect the current response returned by the existing Codex rate-limit/account usage request.

The Codex app-server protocol currently exposes banked reset information under a structure equivalent to:

```text
rateLimitResetCredits
    availableCount
    credits
```

For this feature, the required value is:

```text
rateLimitResetCredits.availableCount
```

### Required semantics

Represent the value in the application model as nullable unless the current codebase has a better-established equivalent.

Conceptually:

```csharp
int? AvailableResetCredits
```

The distinction matters:

- `null` = the server did not provide the information / unavailable / unsupported
- `0` = the server explicitly reports zero banked resets
- `1+` = confirmed number of available banked resets

Do **not** collapse `null` into `0`.

This is important both for correctness and for forward/backward compatibility with Codex app-server versions.

---

## Implementation Tasks

### 1. Inspect the Existing Usage Model and Parser

Locate the model representing the current usage snapshot and the code that parses the app-server rate-limit response.

Likely concepts may include names such as:

- `UsageSnapshot`
- usage/rate-limit parsing
- `CodexClient`
- app-server response DTOs

Use the actual current codebase rather than assuming exact filenames or class names.

Extend the existing snapshot/model with a nullable banked-reset count.

Example concept only:

```csharp
public int? AvailableResetCredits { get; init; }
```

Follow the project's existing style for records/classes/properties.

---

### 2. Parse the Existing Response

During the same refresh that currently parses the 5-hour and weekly usage windows, extract:

```text
rateLimitResetCredits.availableCount
```

Requirements:

- Parse integer values safely.
- Preserve `null` when the property or containing object is absent.
- Preserve an explicit value of `0`.
- Do not fail the entire usage refresh merely because the banked-reset property is absent or malformed.
- Do not change existing 5-hour/weekly parsing behavior.

If the project uses typed app-server protocol objects, extend/use those rather than adding unnecessary ad-hoc JSON parsing.

If it currently parses JSON dynamically, stay consistent unless a small refactor is clearly justified.

---

### 3. Display the Value in the Tray Popup

Add a row to the existing usage popup.

Preferred label:

```text
Usage resets
```

Preferred value:

```text
2 available
```

For singular grammar:

```text
1 available
```

is acceptable and avoids unnecessary wording complexity.

If the value is explicitly zero, display:

```text
0 available
```

If the value is unavailable (`null`), choose whichever behavior best matches the current UI conventions:

**Preferred:** omit the row entirely.

Acceptable alternative if the application normally displays unavailable metrics:

```text
Usage resets               Unavailable
```

Do not display `0 available` when the data was simply not returned.

The popup must continue to size/layout correctly with the additional row.

---

### 4. Do Not Change the Tray Icon Metric

Unless the current design strongly dictates otherwise, **do not include banked reset credits in the tray icon's percentage calculation**.

The existing icon should continue to represent the most constrained ordinary rate-limit window exactly as it does now.

Banked resets are supplemental information, not another percentage-based quota window.

---

### 5. Detailed Credit Metadata Is Out of Scope

The app-server protocol may also expose individual reset-credit records, potentially including expiration information.

Do **not** expand this change into displaying:

- individual reset entries;
- expiration dates;
- reset history;
- credit IDs;
- tooltips for each credit;
- notifications based on credit expiration.

Those can be considered separately later.

For this change, only implement the **available count**.

---

## Tests

Inspect the repository's existing test setup and add tests in the same style.

At minimum, cover these parser/model cases:

### Case A — Positive count

Input contains:

```json
{
  "rateLimitResetCredits": {
    "availableCount": 2
  }
}
```

Expected:

```text
AvailableResetCredits == 2
```

### Case B — Zero count

Input contains:

```json
{
  "rateLimitResetCredits": {
    "availableCount": 0
  }
}
```

Expected:

```text
AvailableResetCredits == 0
```

### Case C — Property absent

Input does not contain `rateLimitResetCredits`.

Expected:

```text
AvailableResetCredits == null
```

Existing usage parsing must still succeed.

### Case D — Existing usage windows unchanged

Use an existing representative rate-limit response and verify that adding reset-credit parsing does not alter current:

- 5-hour usage values;
- weekly usage values;
- reset timestamps/countdowns;
- plan parsing;
- window selection/order.

### UI test

If the project already has practical UI/view-model tests, add coverage verifying:

- positive/zero counts render;
- unavailable count does not masquerade as zero.

Do not introduce a heavyweight UI testing framework solely for this change.

---

## Manual Verification

After implementation:

1. Build the application using the repository's documented build process.
2. Run all existing automated tests.
3. Run any new tests.
4. Launch the built application locally.
5. Confirm existing Codex usage information still refreshes normally.
6. Confirm the popup displays the actual banked reset count for the signed-in Codex account.
7. Compare the value against the Codex/OpenAI usage UI.
8. Confirm the popup behaves correctly when reopened/refreshed.
9. Confirm no additional authentication prompt appears.
10. Confirm there are no new outbound requests beyond the application's established behavior.
11. Confirm the tray icon behavior is unchanged.
12. Confirm startup/exit behavior remains unchanged.

If the live account currently has zero resets, verify the zero state and rely on fixture/unit tests for the positive-count state.

---

## Compatibility / Failure Behavior

The feature must degrade gracefully with older or changed Codex app-server versions.

If `rateLimitResetCredits` is unavailable:

- the application should continue operating normally;
- existing usage metrics must still display;
- no recurring error dialog should appear;
- no crash should occur;
- no fake zero should be shown.

A missing supplemental field is not a fatal refresh failure.

---

## Scope Guardrails

Do not use this feature request as an excuse to:

- rewrite the app;
- switch UI frameworks;
- replace the JSON library;
- redesign the popup;
- change update behavior;
- change startup behavior;
- alter the tray percentage algorithm;
- add telemetry;
- introduce a new installer;
- change application branding;
- rename the project;
- modify unrelated documentation or formatting;
- perform broad dependency upgrades.

If a dependency/build issue blocks the feature, make the smallest justified correction and document it.

---

## Acceptance Criteria

The feature is complete when all of the following are true:

- [ ] CodexMeter reads `rateLimitResetCredits.availableCount` from the existing Codex app-server usage response.
- [ ] The value is represented distinctly as `null` vs `0`.
- [ ] A positive count appears in the tray popup as `N available`.
- [ ] Explicit zero appears correctly as `0 available`.
- [ ] Missing/unavailable data does not appear as zero.
- [ ] Existing 5-hour usage still works unchanged.
- [ ] Existing weekly usage still works unchanged.
- [ ] Existing reset countdown/date behavior still works unchanged.
- [ ] Existing tray icon behavior remains unchanged.
- [ ] No new authentication mechanism is introduced.
- [ ] No browser/UI scraping is introduced.
- [ ] No unnecessary external API request is introduced.
- [ ] Automated tests cover positive, zero, and missing reset-credit data.
- [ ] Existing tests pass.
- [ ] Release/debug build succeeds as appropriate for the project.
- [ ] The feature is manually verified against the live Codex usage UI where possible.
- [ ] Changes are committed on a dedicated feature branch.
- [ ] The feature branch is pushed to the Hudson Helm fork.
- [ ] The diff is clean and suitable for an upstream PR.

---

## Upstream PR Preparation

Once the implementation is tested and pushed, prepare a concise upstream PR description.

Suggested title:

```text
Add banked reset credits to usage popup
```

Suggested PR body:

```markdown
## Summary

Adds the available Codex banked-reset count to the usage popup using the
`rateLimitResetCredits.availableCount` value already returned by the existing
Codex app-server rate-limit request.

## Behavior

- Displays `N available` for banked usage resets.
- Preserves the distinction between zero resets and unavailable reset-credit data.
- Leaves existing 5-hour/weekly usage and tray-icon behavior unchanged.
- Requires no additional authentication or network request.

## Testing

- Added coverage for positive, zero, and missing reset-credit values.
- Existing tests pass.
- Verified the application builds and existing usage metrics continue to work.
```

Before opening the PR, inspect the upstream repository for `CONTRIBUTING.md`, PR templates, formatting requirements, or project-specific submission instructions and follow them.

---

## Final Report Back

When finished, report:

1. Feature branch name.
2. Files changed.
3. Brief explanation of the implementation.
4. Test/build commands run and their results.
5. Whether live banked-reset data was successfully verified.
6. Commit hash.
7. Fork branch pushed.
8. Any caveats or compatibility findings.
9. Proposed upstream PR title/body, or the PR URL if explicitly instructed to open it.

Do not claim verification for anything that was not actually tested.
