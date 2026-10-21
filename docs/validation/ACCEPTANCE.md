# Validation and acceptance record

Status: Pending unless a row has dated evidence. Do not substitute synthetic previews for live-account verification.

Scope: original banked-reset requirements, `../project/REFRESH_INTERVAL.md`, and all 21 sections mapped by `../project/UI_TRAY_SETTINGS.md`. The UI/tray addendum supersedes earlier fixed-tray, scrolling, direct-menu refresh selector and ZIP-only requirements.

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

## Refresh interval checks

These supplement the banked-reset checks. Full requirement: `../project/REFRESH_INTERVAL.md`. All results below are pending implementation.

| Check | Expected result | Result / evidence |
| --- | --- | --- |
| Fresh / missing setting | 15-minute timer and exactly one selected Settings choice | Pending |
| All five supported choices | 1, 5, 15, 30 and 60 minutes update timer, deadline and Settings selection immediately | Pending |
| Restart persistence | Each saved supported interval reloads accurately | Pending |
| Invalid / unreadable setting | Safe 15-minute fallback | Pending |
| Save failure | Existing error convention; truthful active/selected/saved state | Pending |
| Idle selection | New countdown from selection; no immediate extra request; same-choice selection does nothing | Pending |
| Selection during poll | No timer rearm/duplicate; newest interval used at completion | Pending |
| Manual Refresh now / popup Refresh | Immediate when idle at every interval, same handler path | Pending |
| Launch | Initial refresh at any saved interval | Pending |
| Resume before / after deadline | Existing wait before deadline; immediate single refresh when due | Pending |
| Coincident timer/manual/resume events | At most one in-flight poll; exit cannot restart timer | Pending |
| Staleness, popup text and diagnostics | Use selected interval; preserve existing stale grace | Pending |
| Usage and banked resets | Values, semantics and request sequence unchanged; tray changes follow approved selections | Pending |
| Settings / restart / sleep-resume | General selector, persistence and scheduling manually verified | Pending |

## Hover, metrics and settings checks

| Check | Expected result | Result / evidence |
| --- | --- | --- |
| Default tooltip | Codex Meter title; 5 Hour/Weekly remaining; exact default countdown lines; local 12-hour reset clock | Pending |
| Required nine hover fields | Each independently enabled/disabled, including separate 5 Hour countdown/time | Pending |
| Reordering and Restore Defaults | Order persists; adjacent countdown/time compose correctly; reset affects only hover settings | Pending |
| Native text budget | Generated text stays within verified platform limit, including long values/dates/unknowns; trailing-field removal policy is clear | Pending |
| Unicode and all fields disabled | No split characters, exception or misleading partial metric; documented empty-state behavior | Pending |
| Native hover and optional cost feature | Required quota hover remains native/available; existing cost functionality retained | Pending |
| Shared metric model | Stable IDs, consistent data/unknown semantics, surface-specific formatters; deterministic Codex-window selection | Pending |
| Hover countdowns | H:MM:SS with separately selectable local reset time; Weekly D:HH:MM:SS without forced date | Pending |
| Countdown boundaries | At days/hours/minutes/seconds boundaries produce nD/nH/nM/ns with correct case and documented rounding | Pending |
| Expired/invalid reset | No negative time, fake replenishment, crash or fabricated timestamp | Pending |
| One-second timer | Tooltip/tray/visible countdowns tick locally; fake-helper trace shows zero quota reads caused by ticks | Pending |
| Context menu | Exact simple entries/separators; startup check reads actual state | Pending |
| Existing detailed controls | Notification test, prompt rules and cost-history controls reachable through Settings; behavior preserved | Pending |
| Persist all new choices | Interval, fields/order, metric/style/colors, automatic options reload correctly | Pending |
| Defaults and migration | Fresh defaults match addendum; existing settings/startup/private data survive; invalid fields recover safely | Pending |
| Settings save error | No false success or selected/active mismatch; unrelated values preserved | Pending |

## Tray instrument checks

| Check | Expected result | Result / evidence |
| --- | --- | --- |
| Six required selections | 5 Hour remaining, Weekly remaining, each reset countdown, banked count and standard icon; one metric at a time | Pending |
| Missing 5 Hour / unknown count | Truthful unavailable representation; no zero invention or wrong-bucket substitution | Pending |
| Big Number | Legible 0/1/10/40/100, no harmful percent symbol | Pending |
| Small Number + Bar | Legible selected percentage and correctly proportioned bar; style persists | Pending |
| Actual-resolution preview | Common small icons and 100/125/150% scaling; 200% where practical; light/dark taskbars | Pending |
| Fixed colors | Picker works; per-metric colors survive switching/restart; automatic colors off uses saved fixed color | Pending |
| Automatic colors | Explicit toggle, off by default; documented deterministic thresholds and unknown/stale behavior | Pending |
| Exhaustion toggle disabled | Normal configured metric remains selected even at zero quota | Pending |
| Both quotas positive | Normal selected metric | Pending |
| 5 Hour zero, Weekly positive | 5 Hour reset countdown | Pending |
| Weekly zero, 5 Hour positive | Weekly reset countdown | Pending |
| Both quotas zero | Weekly reset countdown takes precedence | Pending |
| Weekly recovers, 5 Hour still zero | 5 Hour countdown; normal metric preference remains saved | Pending |
| All applicable quotas recover | Successful fresh data automatically restores configured normal metric | Pending |
| Clock expiry / failed refresh / stale data | No inferred recovery; truthful stale/unavailable countdown behavior | Pending |
| Fractional remaining quota | Rounded display zero alone does not trigger exhaustion | Pending |
| Startup defaults | 5 Hour Remaining, chosen documented style, auto colors off, exhaustion override on | Pending |

## Installer checks

| Check | Expected result | Result / evidence |
| --- | --- | --- |
| Installer build | Prefer one MSI; otherwise one EXE with rationale; no required loose sidecar payload; stable identity/version; record tool/license/compiler and hash | Pending |
| Fresh per-user install | Standard account succeeds without unnecessary elevation; runtime prerequisites handled clearly | Pending |
| OS integration | Start Menu shortcut and Add/Remove Programs entry correct; app launches once | Pending |
| Startup selected / cleared | Both checkbox states honored; upgrade preserves existing choice | Pending |
| App startup toggle and Settings | Installer, tray and Settings agree with one actual registration; no duplicates | Pending |
| Installed upgrade | All settings/colors/order, private prompt data and startup choice survive | Pending |
| Portable-to-installed migration | Documented supported migration; private data retained; old-path startup not duplicated | Pending |
| Running app and notification shortcut | Safe upgrade handling; correct executable/shortcut/notification registration paths | Pending |
| Uninstall | Installed files, shortcuts, startup and app-owned registrations removed; documented preference/data retention; unrelated data safe | Pending |
| Downloaded installer | Hash matches candidate; clean install and tray smoke test succeed from public website path | Pending |
| Runtime absent / present | Explicit self-contained or prerequisite decision; install/launch behavior clear; online/offline expectations documented | Pending |
| Repair/reinstall/interrupted upgrade | Where supported, recover safely without losing settings/startup choice; older-version policy documented | Pending |
| Writable data and notification paths | Standard user can save settings/data; portable migration and installed activation remain correct | Pending |
| Signing and download experience | Record actual publisher/signing state and Windows prompts; accurate guidance and hashes | Pending |

## UI and live checks

| Check | Expected result | Result / evidence |
| --- | --- | --- |
| Expanded-view heading text | Exactly CodexMeter; no plan suffix; preserve current layout/scrolling/styling for this isolated change | Pending. Plan addition only; premature implementation reverted and its completion claim withdrawn. Verify rendered text/layout after implementation is authorized. Separate scrollbar fix pending. |
| Positive and zero previews | Usage resets row displays N available | Pending |
| Unknown preview | Row omitted, no fake zero | Pending |
| Existing windows plus added row | No horizontal/vertical scrollbar or hidden clipping; correct client size and tray positioning | Pending |
| Multi-bucket / small work area | All expected controls fit without scrolling; extreme fit limitations explicitly resolved | Pending |
| DPI / display | Inspect 100%, 125%, 150%, plus 200% where practical; record actual coverage | Pending |
| Keyboard / accessibility | Label/value readable; refresh, close and Escape still work | Pending |
| Live account comparison | Same account and close timestamps; UI and Meter agree | Pending |
| Reopen/manual/automatic refresh | Count and ordinary usage update correctly | Pending |
| Failure / unavailable / recovery | No crash, recurring dialog or misleading fresh/zero state | Pending |
| Reset dates, startup/exit and approved tray behavior | Existing data/launch/resume preserved; selected/override tray behavior matches addendum | Pending |
| Poll behavior | No additional request/auth mechanism or model turn; inspect code and helper trace | Pending |
| Primary-machine trial | Proposed ordinary working day, several scheduled polls, sleep/resume if available | Pending |
| Fresh standard-user installation | Correct runtime guidance; launch/quit/uninstall without developer environment | Pending |
| Other Windows machine or clean VM | Downloaded candidate works; record OS, runtime, Codex version and result | Pending |
| CPU / UI / resources | Record idle-vs-one-second-ticking CPU, responsiveness and handle/memory behavior over a stated duration; negligible sustained cost, no growth leak | Pending |
| Clock/DST and accessibility | Timestamp-safe expiry/resume/local display; keyboard/focus/labels/contrast; color is not the only status cue | Pending |

Use only synthetic fixtures in committed previews. Record sanitized observations here; put account-bearing diagnostics in ignored `artifacts/private/`. For live comparison record count, time, app/Codex versions and result without account identifiers. If a positive live state is unavailable, record that limitation and rely on fixtures for positive rendering.

## Release decision

- Candidate version and commit: Pending.
- Installer filename, compiler/version and SHA-256; optional ZIP hash: Pending.
- Install/upgrade/uninstall, startup synchronization and settings migration evidence: Pending.
- One-second timer CPU/resource measurements and no-extra-request trace: Pending.
- Open defects and compatibility limitations: Pending.
- Trial duration and other-machine tester result: Pending.
- Commodore's confidence/readiness decision and date: Pending.
- Public release and HudsonHelm page URLs: Pending.
- Downloaded installer hash matches tested candidate: Pending.
- Downloaded-copy installation/smoke test and website rollback record: Pending.
