# MedNotify — v1 Specification

## 1. Purpose

MedNotify is a Home Assistant automation Blueprint for scheduling medication reminders.

At the configured times, it sends an actionable notification to one or more configured notification targets. The user can acknowledge the reminder by pressing a button in the notification.

MedNotify does not create or manage a database, dashboard, helpers, or other Home Assistant infrastructure.

## 2. Configuration

Each MedNotify automation instance configures:

### Medication

- **Person** — name of the person taking the medication.
- **Medication** — medication name.
- **Dose** — dose/instructions displayed in the notification.

### Schedule

- **Start date and time** — date and time of the first reminder.
- **Frequency** — interval between scheduled reminders.
- **Duration** — period for which reminders are generated.

The frequency is independent of when the user acknowledges a reminder.

Example:

```text
Start:     2026-08-10 08:00
Frequency: 8 hours
Duration:  3 days

Reminders:
08:00
16:00
00:00
08:00
...
```

Acknowledging a reminder at 08:17 does not move the next reminder from 16:00 to 16:17.
Frequency zero means once every day until reached duration.
Duration might be forever or have a user set value. Zero is a valid user value so it can't be used as a forever trigger.

### Notifications

- **Notification target(s)** — one or more Home Assistant notification services or a notification group.
- The notification contains the person, medication, dose, and scheduled time.
- The notification contains an actionable **Taken** button.

### Helpers

The user may optionally provide Home Assistant helper entities that MedNotify updates when an action occurs.

MedNotify does not create these helpers.

The exact helper types and supported roles are defined by the Blueprint implementation.

### Timeout

Timeout handling is optional.

When disabled:

- The reminder remains actionable until acknowledged.
- No timeout action is performed.

When enabled:

- The user configures a timeout period.
- If the reminder is not acknowledged within that period, MedNotify performs the configured timeout action.
- Timeout handling must not alter the medication schedule.

## 3. Reminder Lifecycle

For each scheduled dose:

```text
Scheduled
    ↓
Send notification
    ↓
Wait for acknowledgement
    ├── Taken → record configured result
    └── Timeout (optional) → record configured timeout result
```

The next scheduled dose is calculated from the original schedule, not from the acknowledgement time.

## 4. Acknowledgement

The notification provides a **Taken** action.

When the user presses it:

- The reminder is considered acknowledged.
- Configured acknowledgement helper(s) are updated.
- The notification should be cleared from the configured notification target(s), where supported.

If multiple phones receive the same reminder, acknowledgement by one phone should represent acknowledgement of that scheduled dose.

## 5. Timeout

Timeout is disabled by default.

If enabled, the user specifies:

- Timeout duration.
- Optional helper/action to record the timeout.

A timeout means only that the reminder was not acknowledged within the configured period. MedNotify must not interpret this as proof that medication was not taken.

## 6. History

MedNotify does not maintain its own database or history system.

History is the responsibility of the Home Assistant user and may be implemented using helpers, automations, templates, or other Home Assistant functionality.

MedNotify only exposes the events needed for the user to build their own history.

## 7. Scope

### Included in v1

- Scheduled medication reminders.
- Person, medication, and dose information.
- Start date/time.
- Recurring frequency.
- Duration.
- One or more notification targets.
- Actionable **Taken** notification.
- Optional user-managed helper updates.
- Optional timeout.
- Schedule independent of acknowledgement time.

## 8. Design Principle

MedNotify should remain an **atomic Home Assistant Blueprint**.

It is responsible for:

> **Scheduling → notifying → receiving an action → optionally updating user-provided entities.**

Everything else belongs to the Home Assistant user.
