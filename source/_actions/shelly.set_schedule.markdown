---
title: "Set schedule"
action: shelly.set_schedule
domain: shelly
description: "Replace the schedule stored on a Shelly BLU TRV."
since: "2026.12"
related_actions:
  - shelly.get_schedule
---

The **Set schedule** action replaces the schedule stored on a Shelly BLU TRV. Each rule sets a target temperature or a valve position at a time of day on the selected weekdays. The schedule lives on the thermostat itself, so it keeps working when Home Assistant or the network is unavailable.

The action is available for Shelly BLU TRV thermostats connected through a Shelly BLU Gateway Gen3. Other climate entities return an error.

{% include actions/ui_header.md %}

To use this action in an automation or script:

1. Go to {% my automations title="**Settings** > **Automations & scenes**" %}.
2. Open an existing automation or script, or select **Create automation** > **Create new automation**.
3. If you're creating an automation, add a trigger in the **When** section.
4. In the **Then do** section, select **Add action**.
5. Under **By target** (see [Targets](#targets)), pick the climate entity of the BLU TRV.
6. From the actions shown for that target, select **Set schedule**.
7. Under **Rules**, add one entry per switching time.
8. Select **Save**.

### Options in the UI

{% options_ui %}
Rules:
  description: The new schedule. Each rule has a time, weekdays, and either a temperature or a valve position. The schedule on the TRV is replaced by these rules.
  required: true
{% endoptions_ui %}

{% include actions/yaml_header.md %}

In YAML, refer to this action as `shelly.set_schedule`. A basic example looks like this:

{% example %}
action: |
  action: shelly.set_schedule
  target:
    entity_id: climate.bedroom_trv
  data:
    rules:
      - time: "06:30"
        weekdays: [mon, tue, wed, thu, fri]
        temperature: 21
      - time: "08:00"
        weekdays: [sat, sun]
        temperature: 21
      - time: "22:00"
        weekdays: [mon, tue, wed, thu, fri, sat, sun]
        temperature: 17
{% endexample %}

### Options in YAML

{% options_yaml %}
rules:
  description: The new schedule, a list of up to 10 rules.
  required: true
  type: list
  keys:
    time:
      description: The time of day the rule runs.
      required: true
      type: time
    weekdays:
      description: "The days the rule runs on: `mon`, `tue`, `wed`, `thu`, `fri`, `sat`, `sun`."
      required: true
      type: list
    temperature:
      description: The target temperature to set, in °C, from 4 to 30. Use either `temperature` or `valve_position`.
      required: false
      type: float
    valve_position:
      description: The valve position to set, from 0 to 100 percent. Use either `temperature` or `valve_position`.
      required: false
      type: integer
    enabled:
      description: Whether the rule is active.
      required: false
      type: boolean
      default: true
{% endoptions_yaml %}

{% include actions/targets.md domain="climate" %}

## Good to know

- A BLU TRV stores at most 10 rules.
- Writing the schedule goes over Bluetooth through the gateway. The first call after a pause can take 10 to 30 seconds.
- To change a single rule, read the schedule with [**Get schedule**](/actions/shelly.get_schedule/), change the rule, and write the whole list back.

{% include actions/stuck.md %}

{% include actions/related.md %}
