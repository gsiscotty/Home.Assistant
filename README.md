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
                └── ikea_symfonisk_e2123_v1.0.yaml
```

## What is included

- `ikea_symfonisk_e2123_v0.8.7.yaml`: previous stable version
- `ikea_symfonisk_e2123_v1.0.yaml`: latest version (recommended)

Both blueprints are for:
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
