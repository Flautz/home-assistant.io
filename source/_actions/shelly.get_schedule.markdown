---
title: "Get schedule"
action: shelly.get_schedule
domain: shelly
description: "Read the schedule stored on a Shelly BLU TRV."
since: "2026.12"
related_actions:
  - shelly.set_schedule
---

The **Get schedule** action reads the schedule stored on a Shelly BLU TRV. The schedule lives on the thermostat itself, so it keeps working when Home Assistant or the network is unavailable. You can create and change it with the [**Set schedule**](/actions/shelly.set_schedule/) action or in the Shelly app.

This action returns its result in a response variable, which you can use in later steps of the same automation or script.

The action is available for Shelly BLU TRV thermostats connected through a Shelly BLU Gateway Gen3. Other climate entities return an error.

{% include actions/ui_header.md %}

To use this action in an automation or script:

1. Go to {% my automations title="**Settings** > **Automations & scenes**" %}.
2. Open an existing automation or script, or select **Create automation** > **Create new automation**.
3. If you're creating an automation, add a trigger in the **When** section.
4. In the **Then do** section, select **Add action**.
5. Under **By target** (see [Targets](#targets)), pick the climate entity of the BLU TRV.
6. From the actions shown for that target, select **Get schedule**.
7. Select **Save**.

{% include actions/yaml_header.md %}

In YAML, refer to this action as `shelly.get_schedule`. Store the result in a response variable so you can use it in later steps:

{% example %}
action: |
  action: shelly.get_schedule
  target:
    entity_id: climate.bedroom_trv
  response_variable: schedule
{% endexample %}

{% include actions/targets.md domain="climate" %}

## Response data

The response contains a `rules` list for each entity. Each rule has these fields:

- `enabled`: Whether the rule is active.
- `time`: The time of day the rule runs, for example `06:30:00`.
- `weekdays`: The days the rule runs on, for example `[mon, tue, wed, thu, fri]`.
- `temperature`: The target temperature the rule sets, in °C. Present for temperature rules.
- `valve_position`: The valve position the rule sets, from 0 to 100 percent. Present for valve position rules.

Rules that use a schedule other than a time of day on weekdays have a `timespec` field with the raw Shelly schedule expression instead of `time` and `weekdays`.

```yaml
climate.bedroom_trv:
  rules:
    - enabled: true
      time: "06:30:00"
      weekdays: [mon, tue, wed, thu, fri]
      temperature: 21
    - enabled: true
      time: "22:00:00"
      weekdays: [mon, tue, wed, thu, fri, sat, sun]
      temperature: 17
```

## Good to know

- Reading the schedule goes over Bluetooth through the gateway. The first call after a pause can take 10 to 30 seconds.

{% include actions/stuck.md %}

{% include actions/related.md %}
