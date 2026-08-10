# 💊 Medication Notification

Medication reminders for Home Assistant, built as an atomic blueprint.

[GitHub Repository](https://github.com/lePuffin/MedNotify)

---

## Features

- Fixed schedule from start date and time.
- Recurring frequency with daily fallback when frequency is zero.
- Duration window or forever mode.
- Device picker for notification targets.
- Notification Group ID override support.
- Actionable Taken button.
- Android and iOS notification options.
- Optional timeout handling using 00:00:00 as the disable value.
- Optional acknowledgement and timeout action hooks.
- No built-in database or dashboard management.

## Blueprint

The blueprint is located at:

- [blueprints/automation/mednotify/mednotify.yaml](blueprints/automation/mednotify/mednotify.yaml)

## What It Implements

- Person, medication, and dose text in the notification.
- Start date/time based schedule.
- Recurring frequency, with zero frequency meaning daily reminders.
- Finite duration or forever mode.
- One or more notification targets (selected as devices).
- Actionable Taken button.
- Optional timeout handling without changing schedule cadence.
- Optional acknowledgement and timeout action hooks for user-managed helpers.

## Install

1. In Home Assistant, import the blueprint from:
   - Repository: [lePuffin/MedNotify](https://github.com/lePuffin/MedNotify)
   - Blueprint file (GitHub raw): [mednotify.yaml](https://raw.githubusercontent.com/lePuffin/MedNotify/main/blueprints/automation/mednotify/mednotify.yaml)
2. Create an automation from the MedNotify blueprint.
3. Configure:
   - 💊 Pill Appointment (required schedule fields)
   - 📲 Notifications (required delivery fields + optional Android/iOS fields)
   - 🧩 Helpers (optional acknowledgement/timeout actions)
4. Optional behavior details:
   - Notification Group ID is optional and overrides selected devices when provided.
   - Timeout uses 00:00:00 to disable timeout handling.
   - Optional fields are marked as optional in the blueprint descriptions and include examples.

## Notes

- MedNotify does not create or manage databases, dashboards, or helpers.
- History is intentionally left to user-defined Home Assistant helpers and automations.
