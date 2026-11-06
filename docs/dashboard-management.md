# Dashboard management

The Robson Manor dashboard uses native Home Assistant cards and YAML files tracked in this repository. It requires HA 2026.9 or newer and was checked against the live 2026.9.3 instance. The repository's Compose image still points to an older release. Do not use that image to recreate this dashboard without reconciling the runtime version first.

## Files

| File | Purpose |
|---|---|
| `config/lovelace.yaml` | Registers the YAML dashboard at `/lovelace` |
| `config/lovelace/robson-manor.yaml` | Dashboard title and view includes |
| `config/lovelace/views/home.yaml` | Daily controls and status |
| `config/lovelace/views/lights.yaml` | Lighting by room |
| `config/lovelace/views/climate.yaml` | Thermostat controls, temperature, and humidity history |
| `config/lovelace/views/security.yaml` | Alarm, entrances, windows, safety, and diagnostics |
| `config/lovelace/views/cameras.yaml` | Existing front-door snapshot, refresh control, and doorbell events |
| `config/lovelace/views/media.yaml` | Speakers and Bengals game day |
| `config/lovelace/views/scenes.yaml` | Scene library, reached from Lights or Home |
| `config/themes/robson_manor.yaml` | Native HA theme with light and dark modes |
| `DESIGN.md` | Layout, interaction, and theme decisions |

The root configuration loads `lovelace: !include lovelace.yaml` and `frontend: themes: !include_dir_merge_named themes`. Dashboard resources remain under their existing storage ownership. No custom card resource is required.

## Updating views

1. Edit the relevant view file in Git.
2. Check new entity IDs and capabilities against the running instance. Display labels can change without renaming entities.
3. Copy the changed file to the matching path beneath the server's `/config` mount.
4. Open the dashboard menu and select **Refresh**. A normal browser reload may keep a cached YAML configuration.
5. Check the changed view on a phone and desktop. Check native card errors and unavailable entities.

Existing URLs remain `/lovelace/default_view`, `/lovelace/lights`, and `/lovelace/climate`. New views use `/lovelace/security`, `/lovelace/cameras`, and `/lovelace/media`. The scene library is `/lovelace/scenes` and returns to Lights.

For theme changes, copy the theme file and call `frontend.reload_themes`. For registration changes in `configuration.yaml` or `lovelace.yaml`, check the configuration, then arrange a restart. Editing an existing view does not need a restart.

## Initial deployment and rollback

The inspected server uses the `homeassistant` container on SSH host `pi-home`. Its config mount is `/home/dylan/docker/homeassistant/config`. The live root configuration differs from this checkout. Apply the dashboard include to that file without replacing unrelated settings.

Before activation, save a copy of the live root YAML and export the current storage dashboard through the Lovelace API. The migration registers a YAML dashboard at the existing `lovelace` path. It does not delete the previous storage dashboard or modify `.storage` directly.

Validate the staged server configuration:

```bash
ssh pi-home 'docker exec homeassistant python -m homeassistant --script check_config --config /config'
```

After the user approves a restart, restart HA and verify that the `lovelace` dashboard is listed as `mode: yaml`. Check the Home, Lights, Climate, Security, Cameras, and Media views.

To restore the previous dashboard, remove only the new `lovelace: !include lovelace.yaml` registration from the live root configuration. Check the configuration and arrange another restart. HA will use the preserved storage dashboard. Restore the complete root backup only when no unrelated changes occurred since that backup.

The temporary `/dashboard-preview` storage dashboard appears as **Robson Manor (Preview)** in Settings > Dashboards and the sidebar for administrator accounts. It is for visual verification and is not the YAML source of truth. Remove it through the dashboard API after final activation is verified.

## Camera privacy

The Cameras view restores the existing front-door snapshot and its refresh control. It does not provide live video: the current camera reports no streaming capability. The Reolink/Scrypted feeds are not yet exposed as HA camera entities. Add those after configuring their integration and checking their streaming capabilities.

Dashboard visibility does not enforce camera privacy. The future owner-only off-hours requirement is recorded in [camera-access-plan.md](camera-access-plan.md) and remains unimplemented.

## References

- [YAML dashboard registration and refresh](https://www.home-assistant.io/dashboards/dashboards/#adding-yaml-dashboards)
- [Sections layout](https://www.home-assistant.io/dashboards/sections/)
- [Native tile cards](https://www.home-assistant.io/dashboards/tile/)
