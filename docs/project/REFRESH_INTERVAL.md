# Configurable refresh interval

Requested by Commodore on 2026-10-20. Status: specified; implementation pending.

## Required behavior

Add `Refresh interval` to the existing tray context menu, naturally beside `Refresh now`. Its mutually exclusive checked choices are:

- 1 min
- 5 min
- 15 min — default
- 30 min
- 60 min

Persist the selected interval across restarts and apply it immediately to the active timer without restarting the app. A fresh install or absent/invalid saved setting uses 15 minutes. This is authoritative even though the fetched implementation currently uses a fixed 10-minute interval.

Preserve the existing manual `Refresh now` handler and popup Refresh action: when idle they run immediately regardless of the interval. If a refresh is already running, retain the existing guard against starting another. Preserve initial launch refresh and the existing resume behavior: refresh when the next poll is due, without inventing a refresh on every resume.

## Implementation approach

Reuse the current Windows Forms timer and `RefreshAsync` path in `TrayApp.cs`, including its `loading || exiting` guard and stop-while-refreshing behavior. No new background thread, polling loop or timer is needed.

Reuse the existing per-user registry convention in `CostHistorySettings` (`HKCU\Software\CodexMeter`) for a small integer interval setting, proposed name `RefreshIntervalMinutes`. Accept only the five supported values. Preserve unrelated registry values and existing preferences. Treat a missing, malformed or unreadable saved value as the 15-minute default. Follow existing settings-error conventions if saving fails; do not silently claim persistence or display a checked value that disagrees with the active interval.

Replace fixed-interval uses consistently: active timer, `nextPoll`, stale-data grace logic, popup `Every N min` text and polling diagnostics. The existing one-minute repaint/color timers are separate and stay unchanged. Polling still sends the same handshake and usage-read request; frequency alone becomes user-selectable. Optional automation continues observing successful quota checks through its existing path.

Scheduling decision for implementation: changing to a different interval while idle starts a new countdown from the selection time and updates `nextPoll` immediately; selection itself does not request quota. If a poll is running, update the chosen setting but keep the polling timer stopped. Completion of that poll schedules the next check using the newest interval. Selecting the already-active choice is a no-op. A manual refresh continues to reset the subsequent automatic countdown on completion as it does now. Keep these operations on the existing UI thread.

## Acceptance criteria

- [ ] Fresh install / no saved setting uses 15 minutes and checks only that menu choice.
- [ ] Selecting each of 1, 5, 15, 30 and 60 minutes immediately updates the active interval, checked choice and next deadline without restart.
- [ ] The selected interval survives application restart.
- [ ] Invalid or unsupported saved values safely fall back to 15 minutes.
- [ ] Setting-save failure is reported consistently; active timer, checked choice and persistence result remain truthful.
- [ ] Manual Refresh now and popup Refresh still run immediately when idle for every selected interval.
- [ ] Startup still performs the initial refresh independently of the saved interval.
- [ ] Resume when overdue refreshes; resume before the next deadline preserves the existing wait.
- [ ] Timer, manual, menu and resume events during an active refresh cannot launch overlapping quota polls.
- [ ] A selection made during a running refresh controls its next countdown; exiting does not rearm polling.
- [ ] Popup interval text, stale-data detection and diagnostics follow the selected interval.
- [ ] Existing ordinary usage, banked-reset semantics, tray metric and request sequence remain unchanged.
- [ ] No new threads, polling loops, authentication mechanism, model calls or settings framework are introduced.

Use the existing test harness with fake helpers and isolated settings for automated coverage. Verify the visible menu, actual selection, restart persistence, manual refresh and sleep/resume manually. Record results in `../validation/ACCEPTANCE.md`; do not mark specification work as passing implementation tests.
