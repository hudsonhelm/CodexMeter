# CodexMeter UI / Tray Instrument Enhancements — Handoff Addendum

This addendum covers the additional changes approved after the previous handoff.

The objective is to evolve CodexMeter into a genuinely useful Windows tray instrument:

- Full popup = detailed dashboard
- Hover tooltip = customizable compact status report
- Tray icon = one selectable live metric
- Settings = centralized configuration
- Installer = proper Windows installation and startup integration

Preserve all existing functionality unless explicitly changed below.

---

# 1. Fix Popup Scrollbars

The current limits popup is incorrectly displaying:

- a vertical scrollbar on the right
- a horizontal scrollbar along the bottom

Neither should be present.

The popup contains a small fixed amount of content and should size correctly without scrolling.

## Requirements

- Remove both scrollbars.
- Fix the underlying layout rather than merely visually hiding them.
- Disable `AutoScroll` if it is responsible and is not required.
- Ensure the popup client area is large enough for all expected controls.
- Adding the banked-reset row or other approved content must not cause scrollbars to return.
- Preserve the current polished dark popup design.

---

# 2. Hover Tooltip Redesign

Replace the current tooltip:

```text
Codex - Remaining
5-hour: 10% left
Weekly: 75% left
```

with a customizable tooltip.

The preferred default is:

```text
Codex Meter
5 Hour: 10% remaining
Resets in 2:06:14 11:08 PM
Weekly: 75% remaining
Resets in 6:22:06:14
```

## Terminology

Use `Codex Meter`, not `Codex - Remaining`.

Use `5 Hour`, not `5-hour`.

Use `remaining`, rather than `left`.

---

# 3. Live Reset Countdown Behavior

Reset countdowns must update locally once per second.

They must NOT trigger an app-server usage refresh every second.

Usage/rate-limit polling continues to use the configured refresh interval.

Countdowns simply calculate remaining time against the cached reset timestamp.

## 5 Hour Tooltip Countdown

Format:

```text
Resets in H:MM:SS HH:MM AM/PM
```

Example:

```text
Resets in 2:06:14 11:08 PM
```

The actual reset clock time must use the user's local time and Windows 12-hour formatting unless the application's existing locale handling strongly dictates otherwise.

## Weekly Tooltip Countdown

Format:

```text
Resets in D:HH:MM:SS
```

Example:

```text
Resets in 6:22:06:14
```

If appropriate, an optional weekly reset day/date may exist as a separately selectable tooltip field, but it must not be forced into the default weekly countdown line.

---

# 4. Customizable Tooltip Fields

All hover-tooltip content should be configurable in Settings.

The user must be able to choose which lines appear.

Supported fields should include at least:

- Codex Meter title
- 5 Hour remaining
- 5 Hour reset countdown
- 5 Hour reset time
- Weekly remaining
- Weekly reset countdown
- Weekly reset day/date
- Banked resets
- Last updated time

## Requirements

- Each field can be enabled/disabled independently.
- Preserve a sensible default matching the default tooltip shown above.
- Allow the fields to be reordered.
- Persist selections and ordering between application launches.
- Provide a Restore Defaults option if straightforward within the existing settings UI.
- Do not treat unavailable data as zero.

The native Windows NotifyIcon tooltip has a limited text capacity, so ensure the generated tooltip stays within the platform-supported maximum.

If enabled fields would exceed the native tooltip limit, handle this gracefully rather than throwing an exception or silently corrupting the tooltip.

Possible acceptable approaches:

1. prevent an invalid combination;
2. truncate the lowest-priority configured fields;
3. otherwise enforce the platform limit cleanly.

Do NOT replace the native tooltip with a custom hover window as part of this change.

---

# 5. Settings Entry in Right-Click Menu

Simplify the tray context menu to:

```text
Show limits
Refresh now
────────────
Settings...
────────────
Start with Windows
────────────
Quit
```

Preserve the existing checked state for Start with Windows where appropriate.

Do not place all new options directly into the right-click menu.

The right-click menu should remain simple.

All detailed configuration belongs under `Settings...`.

---

# 6. Settings Window

Create a proper Settings UI.

Suggested organization:

## General

- Start with Windows
- Refresh interval:
  - 1 minute
  - 5 minutes
  - 15 minutes — default
  - 30 minutes
  - 60 minutes

The previous refresh-interval requirements remain in effect.

## Hover

- Show/hide each tooltip field
- Reorder tooltip fields
- Restore default tooltip configuration

## Tray

- Tray metric selection
- Tray rendering style
- Tray color
- Automatic status colors
- Automatic exhausted-quota countdown behavior

## Appearance

Reserve this section for appearance-related options where appropriate.

Avoid creating empty or pointless tabs. If the settings UI is cleaner with fewer sections initially, use good judgment.

---

# 7. Tray Metric Selection

The user must be able to select what information is rendered directly into the Windows notification-area icon.

Conceptually, any useful hover metric should be eligible to become the tray metric where practical.

At minimum support:

```text
5 Hour Remaining
Weekly Remaining
5 Hour Reset Countdown
Weekly Reset Countdown
Banked Resets
Standard CodexMeter Icon
```

Only ONE tray metric is displayed at a time.

Some tooltip-only values such as the `Codex Meter` title or `Last Updated` do not need to be available as tray metrics.

---

# 8. Internal Metric Model

Avoid implementing tooltip and tray formatting as unrelated duplicated code.

Create or evolve an internal metric abstraction so the same underlying metric can expose appropriate representations for different surfaces.

Conceptually:

```text
Metric
    Id
    DisplayName
    HoverFormatter
    TrayFormatter
    SupportsTray
    UserColor
```

Possible metrics include:

```text
FiveHourRemaining
FiveHourResetCountdown
FiveHourResetTime
WeeklyRemaining
WeeklyResetCountdown
WeeklyResetDate
BankedResets
LastUpdated
```

This is conceptual guidance, not a required exact class/interface design.

Use the style appropriate to the existing codebase.

The goal is maintainability: adding another useful metric later should not require separately reimplementing it for every UI surface.

---

# 9. Percentage Tray Rendering Styles

For percentage-based metrics such as 5 Hour Remaining and Weekly Remaining, provide two user-selectable display styles.

## Style A — Big Number

Example:

```text
10
```

The percentage number occupies as much of the icon as is reasonably legible.

Do not include a `%` symbol if doing so materially harms readability at normal tray sizes.

## Style B — Small Number + Bar

Example concept:

```text
10
━━━━
```

The number is smaller, with a compact horizontal bar beneath it.

The bar visually represents the selected usage metric.

The exact pixel design should be optimized for Windows notification-area icon sizes rather than mechanically copying the sketch.

## Requirements

- User chooses the style.
- Persist the selected style.
- Both styles must remain legible at common Windows scaling settings.
- Render cleanly at actual tray-icon resolution.
- Test at least common 100%, 125%, and 150% Windows scaling if practical.

---

# 10. Countdown Tray Rendering

Countdown tray icons must use a SINGLE-LINE compact representation.

Do NOT use the previously discussed two-line countdown design.

Compress according to the relevant remaining unit.

Examples:

```text
3D
2H
47M
32s
```

Capitalization is intentional:

- Days: uppercase `D`
- Hours: uppercase `H`
- Minutes: uppercase `M`
- Seconds: lowercase `s`

Choose the most useful single unit for the current remaining duration.

Suggested behavior:

- >= 1 day: `nD`
- >= 1 hour: `nH`
- >= 1 minute: `nM`
- < 1 minute: `ns`

No two-line countdown mode.

The full second-accurate countdown remains available in the hover tooltip and popup where space permits.

---

# 11. User-Selectable Tray Colors

Tray metric colors must be selectable by the user.

Each tray metric should retain its own selected color where practical.

For example:

```text
5 Hour Remaining -> user-selected color A
Weekly Remaining -> user-selected color B
5 Hour Reset Countdown -> user-selected color C
Weekly Reset Countdown -> user-selected color D
Banked Resets -> user-selected color E
```

When the user switches tray metrics, the saved color associated with that metric should be restored.

Use a normal Windows color picker or similarly appropriate color-selection UI.

Persist colors between launches.

---

# 12. Automatic Status Colors

Add an OPTIONAL `Automatic status colors` setting.

This must be an explicit user-selectable option.

When disabled:

- use the user's chosen fixed metric color.

When enabled:

- dynamically select the tray color based on quota status / urgency.

Exact thresholds can be selected sensibly, but they should be clear and deterministic.

Conceptually:

- plenty remaining -> normal/safe color
- getting low -> warning color
- nearly exhausted -> critical color

Do not eliminate the user's ability to choose fixed colors.

Automatic status coloring is optional, not mandatory behavior.

If the current architecture makes configurable thresholds easy, that may be considered later, but threshold customization is NOT required in this change.

---

# 13. Automatically Switch to Reset Countdown When Exhausted

Add a tray behavior option:

```text
Automatically show reset countdown when exhausted
```

This should be enabled/disabled explicitly by the user.

When enabled, the user's normal selected tray metric may be temporarily overridden when usable quota reaches zero.

## Precedence

The logic must be:

1. If WEEKLY usage is exhausted:
   - display the WEEKLY reset countdown.

2. Else, if 5 HOUR usage is exhausted:
   - display the 5 HOUR reset countdown.

3. Otherwise:
   - display the user's normally selected tray metric.

Weekly takes precedence if both are exhausted.

This matters because a 5-hour reset is not actionable if the weekly quota remains exhausted.

## Automatic Return

As soon as refreshed data confirms that the applicable quota is usable again:

- stop showing the temporary reset countdown;
- automatically return to the user's configured normal tray metric.

Do not require user intervention.

## Countdown Rendering

The temporary exhausted-state countdown follows the same single-line compact format:

```text
3D
2H
47M
32s
```

with:

- `D`, `H`, `M` uppercase
- `s` lowercase

---

# 14. Installer

CodexMeter now needs a real Windows installer.

Prefer a lightweight conventional Windows installer.

Inno Setup is preferred unless inspection of the project reveals a compelling reason to use another appropriate installer system.

Avoid MSIX unless there is a strong technical reason.

## Installer Requirements

The installer should:

- install CodexMeter normally;
- create an Add/Remove Programs entry;
- support clean uninstall;
- create a Start Menu shortcut;
- support upgrading an existing installation;
- preserve existing application settings during upgrades;
- avoid requiring administrator privileges if a per-user installation can accomplish everything cleanly.

## Start With Windows During Installation

The installer must provide a checkbox:

```text
☑ Start CodexMeter with Windows
```

The user can select or clear it during installation.

The in-app `Start with Windows` setting remains available afterward.

Both mechanisms must stay synchronized with the actual startup state.

Do not create duplicate startup entries.

---

# 15. Settings Persistence

All newly configurable settings must persist across application restarts.

This includes at minimum:

- refresh interval;
- tooltip enabled fields;
- tooltip field order;
- selected tray metric;
- tray percentage rendering style;
- per-metric user colors;
- automatic status color option;
- automatically-show-reset-countdown option;
- Start with Windows state.

Use the existing configuration mechanism if one exists.

If none exists, add the smallest clean persistence layer appropriate for this application.

Do not introduce an unnecessary database or oversized configuration framework.

---

# 16. Live UI Update Timer

There are now two distinct concepts:

## Data refresh

Controlled by the configured polling interval:

```text
1 / 5 / 15 / 30 / 60 minutes
```

This reads fresh usage/rate-limit information from Codex.

## Local presentation timer

Runs approximately once per second.

It updates:

- tooltip countdown values;
- tray countdown presentation;
- any visible reset countdowns requiring live ticking.

It does NOT contact Codex every second.

Keep these responsibilities separate.

Avoid overlapping or duplicate app-server refresh operations.

---

# 17. Popup Behavior

Keep the current dark popup as the detailed usage dashboard.

The popup may continue using a more human-readable countdown format such as:

```text
Resets in 2h 6m · Tue, Oct 20, 11:08 PM
```

There is no requirement to make the popup use the same compressed formatting as the tray.

The UI hierarchy should be:

```text
Popup:
    detailed/readable

Hover:
    compact but precise

Tray:
    extremely compact/glanceable
```

---

# 18. Testing

Add appropriate tests for the new settings and presentation logic.

At minimum verify:

## Tooltip

- default tooltip fields
- enabled/disabled fields
- reordered fields
- live countdown formatting
- tooltip remains within Windows native length limits

## Countdown formatting

Verify:

```text
days -> D
hours -> H
minutes -> M
seconds -> s
```

Examples:

```text
3D
2H
47M
32s
```

## Tray selection

- 5 Hour Remaining
- Weekly Remaining
- 5 Hour reset countdown
- Weekly reset countdown
- Banked resets
- Standard icon

## Percentage styles

- Big Number
- Small Number + Bar

## Colors

- fixed custom color persists
- per-metric colors persist
- automatic status colors can be enabled/disabled

## Exhaustion switching

Test:

```text
5 Hour > 0, Weekly > 0
=> normal selected metric

5 Hour == 0, Weekly > 0
=> 5 Hour reset countdown

Weekly == 0
=> Weekly reset countdown

5 Hour == 0 AND Weekly == 0
=> Weekly reset countdown

Quota becomes available again
=> return automatically to configured normal metric
```

## Settings persistence

Restart/reload configuration and verify all relevant choices survive.

## Installer

Verify:

- clean install
- clean uninstall
- upgrade install
- settings survive upgrade
- Start with Windows checkbox works
- in-app startup toggle remains synchronized
- no duplicate startup registration

---

# 19. Scope / Design Guardrails

Do NOT:

- replace the native Windows tray tooltip with a custom popup;
- display more than one metric simultaneously inside the notification-area icon;
- use a two-line countdown icon;
- poll Codex every second;
- redesign the entire limits popup;
- introduce telemetry;
- require cloud services;
- create a database for settings;
- add unnecessary dependencies;
- break the existing manual Refresh now behavior;
- discard the existing refresh-on-launch/resume behavior;
- lose existing user settings during installer upgrades.

---

# 20. Desired Default Configuration

Unless current application behavior strongly requires otherwise, use these defaults:

## Refresh

```text
15 minutes
```

## Tooltip

```text
Codex Meter
5 Hour: N% remaining
Resets in H:MM:SS HH:MM AM/PM
Weekly: N% remaining
Resets in D:HH:MM:SS
```

## Tray

Normal metric:

```text
5 Hour Remaining
```

Rendering style:

Use whichever of the two approved styles most closely preserves the current CodexMeter tray appearance, unless the existing current icon makes another choice clearly preferable.

Automatic status colors:

```text
Off by default
```

Automatically show reset countdown when exhausted:

```text
On by default
```

If there is ambiguity about an existing CodexMeter default or migration behavior, preserve existing user behavior wherever possible.

---

# 21. Final Verification

Before considering this work complete:

1. Build successfully.
2. Run all automated tests.
3. Verify the application manually from the tray.
4. Confirm no popup scrollbars.
5. Verify tooltip contents and live ticking.
6. Verify tooltip configuration.
7. Verify tray metric switching.
8. Verify both percentage rendering styles.
9. Verify user-selected colors.
10. Verify automatic status colors.
11. Verify exhausted-quota automatic reset countdown behavior.
12. Verify return to normal metric after reset.
13. Verify settings persistence.
14. Build and install using the new installer.
15. Verify installer Start with Windows option.
16. Verify upgrade preserves settings.
17. Verify uninstall behaves cleanly.
18. Verify CPU usage remains negligible while the one-second presentation timer is active.
19. Verify there are no additional model calls or unnecessary OpenAI network requests.

Keep the implementation clean enough for eventual upstream contribution, even if some of these enhancements ultimately remain specific to the Hudson Helm fork.
