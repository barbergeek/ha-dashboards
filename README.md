# HA Dashboards

Home Assistant dashboard configurations.

---

## Battery Monitor

Displays all devices with battery sensors, sorted from lowest to highest charge,
with a dedicated "Needs Replacement" section for anything below 20%.

### What you get

| Card | Description |
|---|---|
| **Need Attention** | Count of devices below 20% |
| **Critical** | Count of devices below 10% |
| **Needs Replacement** | Auto-discovered list of every battery < 20%, sorted ascending |
| **All Batteries** | Full list of every battery sensor, sorted ascending |

### Prerequisites

1. **[HACS](https://hacs.xyz/)** installed in Home Assistant
2. **[auto-entities](https://github.com/thomasloven/lovelace-auto-entities)** card installed via HACS
   - HACS → Frontend → search "auto-entities" → Install

### Installation

#### 1. Template sensors + daily alert automation

Copy `packages/battery_alerts.yaml` into your HA `config/packages/` directory.

If you don't already have packages enabled, add this to `configuration.yaml`:

```yaml
homeassistant:
  packages: !include_dir_named packages
```

Restart Home Assistant. This creates two sensors:
- `sensor.low_battery_count` — devices below 20%
- `sensor.critical_battery_count` — devices below 10%

To disable the daily 9 AM notification, remove or disable the
`Daily Low Battery Notification` automation in Settings → Automations.

#### 2. Dashboard

**Option A — Raw configuration editor (easiest)**

1. Settings → Dashboards → Add Dashboard → give it a name
2. Open the new dashboard → three-dot menu → Edit → three-dot menu → Raw configuration editor
3. Paste the contents of `dashboards/battery-monitor.yaml`
4. Save

**Option B — `ui_lovelace_minimalist` / YAML mode**

If your HA is in YAML dashboard mode, add to `configuration.yaml`:

```yaml
lovelace:
  mode: yaml
  dashboards:
    battery-monitor:
      mode: yaml
      title: Battery Monitor
      icon: mdi:battery
      show_in_sidebar: true
      filename: dashboards/battery-monitor.yaml
```

Copy `dashboards/battery-monitor.yaml` to your HA `config/` directory and restart.

### Thresholds

| Level | Range | Meaning |
|---|---|---|
| Critical | < 10% | Replace immediately |
| Low | 10 – 19% | Replace soon |
| Good | ≥ 20% | No action needed |

To change the thresholds, edit the `state: "< 20"` / `state: "< 10"` values in
`dashboards/battery-monitor.yaml` and the matching `val < 20` / `val < 10` values
in `packages/battery_alerts.yaml`.
