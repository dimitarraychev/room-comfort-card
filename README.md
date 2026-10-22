# Room Comfort Card

A clean, standalone, single-row Lovelace card for Home Assistant designed to monitor room climate conditions. It works **completely out of the box** with zero required integrations, HACS dependencies, or helper entities.

The card provides a rich set of visual indicators in a compact design:
* **Dynamic Status Text:** Calculates room comfort level (e.g., *Cold*, *Comfortable*, *Humid*) based on threshold values.
* **Color-Coded Value & Pill:** Colors the card elements dynamically based on the current status zone.
* **5-Zone Range Bar:** Displays a visual range bar divided into five comfort zones with an animated dot representing the current reading.
* **Group Comparisons:** Optionally highlights the room with the lowest or highest reading across a specified group of sensors using min/max icons.

---

## Operating Modes

Room Comfort Card operates in two distinct modes out of the box:

1. **`temperature` (Default):** Tailored for temperature monitoring (°C).
   * **Default Thresholds:** 18°C (Low) | 20°C (Comfort Low) | 25°C (Comfort High) | 28°C (High)
   * **Default Zones:** Cyan (*Very cold*) → Blue (*Cold*) → Green (*Comfortable*) → Orange (*Warm*) → Red (*Very hot*)
2. **`humidity`:** Tailored for humidity monitoring (%).
   * **Default Thresholds:** 30% (Low) | 40% (Comfort Low) | 55% (Comfort High) | 60% (High)
   * **Default Zones:** Orange (*Very dry*) → Yellow (*Dry*) → Green (*Comfortable*) → Cyan (*Humid*) → Blue (*Very humid*)

---

## Installation

### Manual Installation
1. Copy `room-comfort-card.js` into your `config/www/` folder.
2. In Home Assistant, go to **Settings** → **Dashboards** → **Three dots (top right)** → **Resources**.
3. Add a new resource with URL `/local/room-comfort-card.js` and type **JavaScript Module**.

### HACS (Custom Repository)
1. Open **HACS** → **Dashboards** → **Three-dot menu (top right)** → **Custom repositories**.
2. Paste this repository URL, set the type to **Dashboard**, and click **Add**.
3. Install **Room Comfort Card** and refresh your browser.

---

## Usage Examples

### Minimal Configuration
```yaml
type: custom:room-comfort-card
entity: sensor.living_room_temperature
```

### Full Temperature Configuration with Room Group Comparison
```yaml
type: custom:room-comfort-card
mode: temperature
entity: sensor.living_room_temperature
name: Living Room
icon: mdi:sofa
compare_entities:
  - sensor.living_room_temperature
  - sensor.bedroom_temperature
  - sensor.bathroom_temperature
thresholds:
  low: 17
  comfort_low: 19
  comfort_high: 24
  high: 27
tap_action:
  action: navigate
  navigation_path: '#living-room-climate'
```

### Custom Humidity Configuration
```yaml
type: custom:room-comfort-card
mode: humidity
entity: sensor.bedroom_humidity
name: Bedroom Humidity
icon: mdi:water-percent
thresholds:
  low: 25
  comfort_low: 35
  comfort_high: 50
  high: 65
```

## Configuration Options

All features can be configured visually in the dashboard editor or via YAML:

| Option | Default | Description |
|---|---|---|
| `entity` | **Required** | Sensor entity ID to display. |
| `mode` | `temperature` | Operating mode: `temperature` or `humidity`. |
| `name` | Friendly name | Card title. Falls back to entity friendly name. |
| `icon` | Entity icon / Mode default | Icon displayed on the left. |
| `compare_entities` | `[]` | List of room sensors to compare for lowest/highest icons. |
| `thresholds` | Mode defaults | Map of threshold points (`low`, `comfort_low`, `comfort_high`, `high`). Can be raw numbers or sensor entity IDs. |
| `labels` | Mode defaults | List of 5 custom status labels (from lowest to highest zone). Ideal for translations. |
| `zone_colors` | Mode defaults | List of 5 colors for range zones. Accepts named presets (`cyan`, `green`, etc.), CSS hex codes, or entity IDs. |
| `color_map` | `null` | Key-value mapping of `label: color` to override colors per status. |
| `color_map_entity` | `null` | Optional entity ID containing a `colors` attribute mapping labels to colors. |
| `default_color` | `gray` (`#8E8E93`) | Fallback color when value is unknown or unavailable. |
| `min_icon` / `max_icon` | Mode defaults | Icons used when the room is the lowest or highest in `compare_entities`. |
| `unit` | Sensor unit / Mode default | Measurement unit suffix. |
| `decimals` | `1` (temp), `0` (humidity) | Number of decimal places displayed. |
| `scale_padding` | `5` (temp), `10` (humidity) | Visual margin added below `low` and above `high` on the bar range. |
| `tap_action` | `more-info` | Tap action (`navigate`, `more-info`, `toggle`, `perform-action`, `url`, `none`). |

## Thresholds & Colors

- **Flexible Threshold Inputs:** You can set threshold values as static numbers (e.g., `low: 18`) or pass any numeric sensor/helper entity ID (e.g., `low: input_number.custom_low_temp`).
- **Flexible Color Inputs:** Zone colors support named presets (`cyan`, `blue`, `green`, `lime`, `yellow`, `orange`, `red`, `purple`, `gray`), custom hex codes (`#30D158`), or state entity IDs holding color strings.
