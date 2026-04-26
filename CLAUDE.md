# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A collection of Home Assistant dashboard configurations and automation packages. Files here are deployed manually into a running Home Assistant instance — there is no build step or test runner.

## Repo structure

- `dashboards/` — Lovelace dashboard YAML files, pasted into HA's raw configuration editor or placed in `config/` for YAML mode
- `packages/` — HA package YAML files (template sensors + automations), dropped into `config/packages/`

## Deployment

These files are copied into a Home Assistant instance; they are not deployed via any script in this repo. See README.md for the two installation paths (raw config editor vs YAML mode).

## Conventions

### Thresholds

Battery levels use two thresholds defined in **both** files and must stay in sync:

| Level | Threshold | Locations |
|---|---|---|
| Low | `< 20` | `dashboards/battery-monitor.yaml` (two `state:` filters) and `packages/battery_alerts.yaml` (two `val < 20` checks) |
| Critical | `< 10` | same files, `state: "< 10"` and `val < 10` |

When changing a threshold, update all four occurrences.

### Dependencies

The `custom:auto-entities` card (HACS → Frontend) must be installed in the HA instance for `dashboards/battery-monitor.yaml` to render. The `packages/battery_alerts.yaml` file has no HACS dependencies.
