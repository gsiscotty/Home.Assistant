# Home Assistant Blueprints

This repository stores Home Assistant automation blueprints.

## Folder layout

```text
.
└── blueprints/
    └── automation/
        └── zigbee2mqtt/
            └── ikea-symfonisk-e2123/
                ├── ikea_symfonisk_e2123_v0.8.7.yaml
                ├── ikea_symfonisk_e2123_v1.0.yaml
                └── ikea_symfonisk_e2123_v2.0.yaml
```

## What is included

- `ikea_symfonisk_e2123_v2.0.yaml`: latest version (recommended)
- `ikea_symfonisk_e2123_v1.0.yaml`: previous version, needs the legacy Zigbee2MQTT action sensor
- `ikea_symfonisk_e2123_v0.8.7.yaml`: older version

## v2.0 (recommended)

[![Open your Home Assistant instance and show the blueprint import dialog with this blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fgsiscotty%2FHome.Assistant%2Fblob%2Fmain%2Fblueprints%2Fautomation%2Fzigbee2mqtt%2Fikea-symfonisk-e2123%2Fikea_symfonisk_e2123_v2.0.yaml)

What is new:
- Listens to the MQTT action topic (`zigbee2mqtt/<remote name>/action`). Works on Zigbee2MQTT 1.x and 2.x; the legacy action sensor is not needed.
- Every press runs at once (`mode: parallel`). Buttons never block or cancel each other.
- Built-in volume control for one or more speakers: slow click 4 %, fast click 3 % (0.5 to 1.0 s after the previous press), hold 4 % per signal (presses closer than 0.5 s, or `_hold` signals). Holding volume up stops at the next cap above where you started (40 %, 70 %, 100 %); release for at least 0.8 s and hold again to pass it. Volume down has no cap. All values can be changed.
- Works with both remote firmwares: the one that sends `volume_up_hold` and the one that repeats `volume_up` while the button is held.
- Inputs are grouped in sections. The action inputs keep the v1.0 names, so existing actions can be copied over.

Setup:
1. Click the import button above (or copy the file as described below).
2. Create a Text helper per remote (`Settings` -> `Devices & services` -> `Helpers` -> `Create helper` -> `Text`, max length 255). It remembers the last volume press and the level where a hold started.
3. Create an automation from the blueprint. Enter the remote's Zigbee2MQTT friendly name exactly as shown in Zigbee2MQTT, pick your speakers and the helper, and assign the button actions.

## v1.0 and older

These blueprints are for:
- Zigbee2MQTT
- IKEA SYMFONISK Sound Remote Gen 2 (`E2123`)
- Home Assistant automation domain

## How to use

### 1) Copy blueprint file to Home Assistant

Copy the version you want into your Home Assistant config directory:

```bash
mkdir -p "<HA_CONFIG>/blueprints/automation/zigbee2mqtt"
cp "blueprints/automation/zigbee2mqtt/ikea-symfonisk-e2123/ikea_symfonisk_e2123_v1.0.yaml" "<HA_CONFIG>/blueprints/automation/zigbee2mqtt/"
```

Example common config paths:
- Home Assistant OS / Supervised: `/config`
- Home Assistant Container (host bind): your mapped config directory

### 2) Reload blueprints in Home Assistant

- Open Home Assistant
- Go to `Settings` -> `Automations & Scenes` -> `Blueprints`
- Click the 3-dot menu -> `Reload blueprints`

### 3) Create an automation from the blueprint

- In `Blueprints`, find the imported SYMFONISK blueprint
- Click `Create automation`
- Set `Controller Entity` to the Zigbee2MQTT action sensor for your remote
  - Example pattern: `sensor.<device_name>_action`
- Assign actions for:
  - Play/Pause
  - Volume up/down (single + hold)
  - Next/Previous track
  - Shortcut buttons (single/double/long)
- Save and enable the automation

### 4) Test button actions

- Press each remote button once
- Confirm expected media action runs
- If hold actions are too fast/slow, adjust your action sequences in the automation

## Troubleshooting

- No actions firing:
  - Confirm Zigbee2MQTT updates the selected action sensor state.
  - Confirm the automation is enabled.
- Blueprint not visible:
  - Re-check file path under `<HA_CONFIG>/blueprints/automation/...`.
  - Reload blueprints.
- Wrong entity:
  - Use the sensor entity that changes state with button presses (not the battery sensor).
