# Home Assistant Garage Auto Close

A Home Assistant automation blueprint for automatically closing a garage door after a visible timer expires.

Activity sensors reset the countdown timer. Obstruction sensors prevent auto-close if active and can trigger the same optional notification action used for successful close notifications.

## Features

- Auto-close a garage door after a timer helper finishes
- Uses an existing Home Assistant `timer` helper for visible countdown state
- Activity sensors reset/restart the timer while the garage door is open
- Obstruction sensors block auto-close when active
- Optional notification action for both obstruction and successful auto-close events
- Works with standard Home Assistant `cover` entities, including garage door integrations such as ratgdo

## Repository structure

```text
blueprints/
  automation/
    cmstone/
      garage_auto_close.yaml
.github/
  ISSUE_TEMPLATE/
    bug_report.yml
    feature_request.yml
.gitignore
LICENSE
README.md
```

## Installation

### Option 1: Import from GitHub

Use Home Assistant's blueprint import feature with this repository URL:

```text
https://github.com/cmstone/homeassistant-garage-auto-close
```

### Option 2: Manual install

Copy this file into your Home Assistant configuration directory:

```text
blueprints/automation/cmstone/garage_auto_close.yaml
```

Then reload automations or restart Home Assistant.

## Required Home Assistant helpers

Create a timer helper before using the blueprint:

1. Go to **Settings → Devices & services → Helpers**.
2. Create a **Timer** helper.
3. Set the timer duration to the desired auto-close delay.
4. Select that helper in the blueprint automation.

The blueprint starts this timer whenever the door opens or activity is detected while the door remains open.

## Inputs

| Input | Description |
| --- | --- |
| Garage Name | Friendly name used in notification messages |
| Garage Door Cover | The garage door `cover` entity to close |
| Activity Sensors | Binary sensors that reset the countdown timer |
| Obstruction Sensors | Binary sensors that block auto-close if active |
| Timer Helper | Existing timer helper used as the visible countdown |
| Notification Action | Optional action sequence for notifications |

## Notification action

The blueprint exposes a `notification_message` variable before running the optional notification action.

Example action:

```yaml
- service: notify.mobile_app_your_phone
  data:
    title: Garage Door
    message: "{{ notification_message }}"
```

The same notification action is used for:

- Auto-close prevented by obstruction
- Garage door automatically closed

## Safety note

Garage doors can cause property damage or injury. Use this only with properly functioning garage door safety sensors and obstruction detection. Test thoroughly before relying on the automation.

## License

MIT
