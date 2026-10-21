# Validation and acceptance record

Status: Pending unless a row has dated evidence. Do not substitute synthetic previews for live-account verification.

## Automated checks

| Check | Expected result | Result / evidence |
| --- | --- | --- |
| Positive count, including 1 and 2 | Exact integer retained | Pending |
| Explicit zero | Zero retained, never confused with unknown | Pending |
| Missing object or count; explicit null | Nullable count is unknown; usage survives | Pending |
| String, Boolean, array or malformed object | Unknown; no refresh failure | Pending |
| Fractional, overflow and negative count | Unknown; no refresh failure | Pending |
| Detail records absent, capped or inconsistent | Use availableCount; never derive it from array length | Pending |
| Legacy and multiple-bucket responses | Same windows, order, percentages, dates, plan and most-limited window as baseline | Pending |
| Poll protocol | Existing initialize / initialized / account/rateLimits/read sequence only | Pending |
| Ordinary error handling | Existing sign-in, cancellation and stale behavior preserved | Pending |
| Default test suite | Pass | Pending |
| Prompt and goal suites with fake helpers | Pass; no live prompt or goal mutation | Pending |
| Release build and package | Successful from final recorded commit | Pending |

## UI and live checks

| Check | Expected result | Result / evidence |
| --- | --- | --- |
| Positive and zero previews | Usage resets row displays N available | Pending |
| Unknown preview | Row omitted, no fake zero | Pending |
| Existing windows plus added row | No clipping/overlap; scrolling and tray positioning correct | Pending |
| DPI / display | Inspect 100%, 150%, 200% where practical; record actual coverage | Pending |
| Keyboard / accessibility | Label/value readable; refresh, close and Escape still work | Pending |
| Live account comparison | Same account and close timestamps; UI and Meter agree | Pending |
| Reopen/manual/automatic refresh | Count and ordinary usage update correctly | Pending |
| Failure / unavailable / recovery | No crash, recurring dialog or misleading fresh/zero state | Pending |
| Tray, reset dates, startup/exit | Baseline behavior retained | Pending |
| Poll behavior | No additional request/auth mechanism or model turn; inspect code and helper trace | Pending |
| Primary-machine trial | Proposed ordinary working day, several scheduled polls, sleep/resume if available | Pending |
| Fresh standard-user installation | Correct runtime guidance; launch/quit/uninstall without developer environment | Pending |
| Other Windows machine or clean VM | Downloaded candidate works; record OS, runtime, Codex version and result | Pending |

Use only synthetic fixtures in committed previews. Record sanitized observations here; put account-bearing diagnostics in ignored `artifacts/private/`. For live comparison record count, time, app/Codex versions and result without account identifiers. If a positive live state is unavailable, record that limitation and rely on fixtures for positive rendering.

## Release decision

- Candidate version and commit: Pending.
- ZIP filename and SHA-256: Pending.
- Open defects and compatibility limitations: Pending.
- Trial duration and other-machine tester result: Pending.
- Commodore's confidence/readiness decision and date: Pending.
- Public release and HudsonHelm page URLs: Pending.
- Downloaded ZIP hash matches tested candidate: Pending.
- Downloaded-copy smoke test and website rollback record: Pending.
