# Room Comfort Card

A one-row Lovelace card for a room's temperature or humidity. It shows:

- the room name and its comfort status (for example *Cold*, *Comfortable*), worked out from your thresholds
- the current value, coloured by status
- a five-zone bar built from the same thresholds, with a dot at the current value
- an icon next to the value on the lowest and highest room of a group you pick

It needs no other custom card. With only `entity` set, it uses the default thresholds and colour map below.

## Install

1. In HACS, open the three-dot menu, choose **Custom repositories**, add this repository's URL, and set the type to **Dashboard**.
2. Install **Room Comfort Card** and reload the browser.
3. Add **Room Comfort Card** from the card picker. The main options are in the visual editor.

HACS installs from the latest GitHub release when one exists, so publish a release after you push.

## Examples

```yaml
type: custom:room-comfort-card
mode: temperature
entity: sensor.living_room_climate_temperature
name: Living Room
icon: mdi:sofa
compare_entities:
  - sensor.living_room_climate_temperature
  - sensor.bedroom_climate_temperature
  - sensor.bathroom_climate_temperature
tap_action:
  action: navigate
  navigation_path: '#living-room-climate'
```

```yaml
type: custom:room-comfort-card
mode: humidity
entity: sensor.living_room_climate_humidity
name: Living Room
icon: mdi:sofa
```

## Mode

`mode` is `temperature` (default) or `humidity`. It sets the default thresholds, zone order and colours, labels, icons, unit and decimals.

## Status

The status always comes from the thresholds:

| Mode | Below `low` | Below `comfort_low` | Up to and including `comfort_high` | Up to and including `high` | Above `high` |
|---|---|---|---|---|---|
| temperature | Very cold | Cold | Comfortable | Warm | Very hot |
| humidity | Very dry | Dry | Comfortable | Humid | Very humid |

The colour of the row, icon, value and dot is the colour for that label in the colour map.

## Lowest and highest icon

Pick the room sensors in `compare_entities`. The card's own entity is always included. The room with the lowest value gets the min icon and the highest gets the max icon. When all values are equal, or when `compare_entities` is empty, no icon shows. Nothing is detected automatically, so sensors that are not rooms never take part.

## Options

| Option | Default | Description |
|---|---|---|
| `entity` | required | Numeric sensor shown as the value. |
| `mode` | `temperature` | `temperature` or `humidity`. |
| `name` | friendly name | Row title. |
| `icon` | entity icon, else mode default | Main icon. |
| `compare_entities` | none | Room sensors to compare for the lowest / highest icon. |
| `thresholds` | see below | `low`, `comfort_low`, `comfort_high`, `high`. Each is a number or an `input_number` entity id. |
| `labels` | mode labels | Five status labels, from lowest to highest zone. Use this to translate them. |
| `zone_colors` | mode zones | Five colours for the bar zones. Each can be a CSS colour, an entity id, or a name: `cyan`, `blue`, `green`, `lime`, `yellow`, `orange`, `red`, `purple`, `gray`. |
| `color_map` | see below | `label: colour` pairs that override the colour map. |
| `color_map_entity` | `sensor.comfort_color_map` | Sensor with a `colors` attribute (label to colour) and an optional `default_gray` attribute. |
| `default_color` | gray | Colour for unavailable values or unknown labels. |
| `min_icon` / `max_icon` | mode icons | Icons for the lowest and highest room. |
| `unit` | entity unit, else mode default | Unit shown after the value. |
| `decimals` | 1 (temperature), 0 (humidity) | Decimals shown. |
| `scale_padding` | 5 (temperature), 10 (humidity) | Units added below `low` and above `high` for the bar's range. |
| `tap_action` | `more-info` | `navigate`, `more-info`, `toggle`, `perform-action`, `url` or `none`. |

## Defaults

Thresholds are read from these helpers first and fall back to the number in brackets when a helper is missing.

| Threshold | Temperature | Humidity |
|---|---|---|
| `low` | `input_number.temperature_low_threshold` (18) | `input_number.humidity_low_threshold` (30) |
| `comfort_low` | `input_number.temperature_comfort_low_threshold` (20) | `input_number.humidity_comfort_low_threshold` (40) |
| `comfort_high` | `input_number.temperature_comfort_high_threshold` (25) | `input_number.humidity_comfort_high_threshold` (55) |
| `high` | `input_number.temperature_high_threshold` (28) | `input_number.humidity_high_threshold` (60) |

Colours are read from `input_text.apple_<name>` helpers first and fall back to built-in Apple-style colours.

Colour map order, lowest to highest priority: built-in map (status label to zone colour), the `colors` attribute of `color_map_entity`, then `color_map` in the card config.

Built-in labels and colours:

- Temperature: Very cold (cyan), Cold (blue), Comfortable (green), Warm (orange), Very hot (red)
- Humidity: Very dry (orange), Dry (yellow), Comfortable (green), Humid (cyan), Very humid (blue)

## Notes

- `navigate` with a `#hash` path opens Bubble Card pop-ups. The card changes the URL and fires a `location-changed` event, which is how Bubble Card listens.
- Hold and double-tap actions are not supported.
