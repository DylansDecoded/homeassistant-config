# Project agent memory

This file is the project's committed home for project-intrinsic agent knowledge: build, test, release, architecture, and sharp-edge notes that should travel with the code.

## Repository layout

- Home Assistant configuration lives under `config/`.
- `config/configuration.yaml` is the root configuration file.
- Each file under `config/automations/` must contain a YAML list because the root configuration uses `!include_dir_merge_list`.

## Validation

- Run `docker compose config -q` to validate the Compose file.
- With Docker running, run `docker compose run --rm homeassistant python -m homeassistant --script check_config -c /config` to validate the Home Assistant configuration with the pinned image.

## Maintaining this file

Keep this file for knowledge useful to almost every future agent session in this project.
Do not repeat what the codebase already shows; point to the authoritative file or command instead.
Prefer rewriting or pruning existing entries over appending new ones.
When updating this file, preserve this bar for all agents and keep entries concise.
