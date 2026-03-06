# Helios - Home Assistant Configuration

## Deploying Changes

This repo lives on a dev machine, not the HA server. To deploy:

```
scp -O <file> <ha-server>:/config/<path>
```

Then reload in HA:
```
hass-cli service call automation.reload
```

Or `homeassistant.reload_all` for broader changes (input_boolean, timer, etc).

SSH user matches local whoami. Use `-O` flag for SCP (legacy protocol required).

## hass-cli

Installed via homebrew. Requires `HASS_SERVER` and `HASS_TOKEN` env vars (sourced from `~/.zsh/homeassistant.zsh`). If not in the shell environment, pass `--server` and `--token` explicitly.

The `raw` subcommand does not work. Use `state get/list` and `service call`.

## Project Structure

- `configuration.yaml` — main config, includes everything else
- `automations/` — one file per room/feature, merged via `!include_dir_merge_list`
- `scenes/` — same pattern
- `integrations/` — individual includes (`!include integrations/foo.yaml`)
- `blueprints/` — reusable automation/template blueprints
- `esphome/` — ESPHome device configs

## Vestaboard

- HACS integration (natekspencer/hacs-vestaboard), uses local API
- Service: `vestaboard.message` — accepts `message`, `device_id`, `justify`, `align`
- VBML parameter does not work with local API, but `{number}` character codes work inline in the `message` field
- Color codes: `{63}`=red, `{64}`=orange, `{65}`=yellow, `{66}`=green, `{67}`=blue, `{68}`=violet, `{69}`=white, `{70}`=black
- Board: 6 rows x 22 columns
