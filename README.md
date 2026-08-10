# MedNotify

MedNotify is a Home Assistant automation Blueprint for medication reminders.

It schedules reminders from a fixed start time and interval, sends actionable notifications,
accepts a Taken action, supports optional timeout handling, and can run optional user-provided
helper update actions.

## Blueprint

The blueprint is located at:

- `blueprints/automation/mednotify/mednotify_v100.yaml`

## What It Implements

- Person, medication, and dose text in the notification.
- Start date/time based schedule.
- Recurring frequency, with zero frequency meaning daily reminders.
- Finite duration or forever mode.
- One or more notification targets (provided as notify services).
- Actionable Taken button.
- Optional timeout handling without changing schedule cadence.
- Optional acknowledgement and timeout action hooks for user-managed helpers.

## Install

1. In Home Assistant, import the blueprint from:
   - Repository: [lePuffin/MedNotify](https://github.com/lePuffin/MedNotify)
   - Blueprint file: [mednotify_v100.yaml](https://github.com/lePuffin/MedNotify/blob/main/blueprints/automation/mednotify/mednotify_v100.yaml)
2. Create an automation from the MedNotify blueprint.
3. Configure:
   - Medication fields (person, medication, dose)
   - Schedule (start, frequency, duration/forever)
   - Notification services list (for example `notify.mobile_app_lepuffin_phone`)
   - Optional acknowledgement and timeout actions

## Notes

- MedNotify does not create or manage databases, dashboards, or helpers.
- History is intentionally left to user-defined Home Assistant helpers and automations.
