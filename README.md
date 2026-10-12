# Lucid Nexus

[![Release](https://img.shields.io/github/v/release/Terablo/lucid-nexus)](https://github.com/Terablo/lucid-nexus/releases)
[![Validate](https://github.com/Terablo/lucid-nexus/actions/workflows/validate.yml/badge.svg)](https://github.com/Terablo/lucid-nexus/actions/workflows/validate.yml)
[![Home Assistant 2024.4+](https://img.shields.io/badge/Home%20Assistant-2024.4%2B-41BDF5)](https://www.home-assistant.io/)

A live 3D dashboard for your Lucid, built for Home Assistant. The car on screen
follows the real one: doors, frunk, trunk, windows, charge port, lights and
ride height all move when the car does, and the controls you use most are one
tap away.

![Lucid Nexus showing a Lucid Gravity on a Home Assistant dashboard](https://raw.githubusercontent.com/Terablo/lucid-nexus/main/images/hero.webp)

[![Open your Home Assistant instance and open this repository in HACS](https://my.home-assistant.io/badges/hacs_repository.svg)](https://my.home-assistant.io/redirect/hacs_repository/?owner=Terablo&repository=lucid-nexus&category=plugin)

> **Community project.** Lucid Nexus is not affiliated with, endorsed by or
> sponsored by Lucid Motors. It reads and controls your car only through the
> [Lucid Motors integration](https://github.com/borski/ha-lucidmotors) in your
> own Home Assistant.
>
> Built by [MakeshiftArtisan](https://github.com/MakeshiftArtisan) and
> [Terablo](https://github.com/Terablo).

## Highlights

**The car mirrors your car.** Each door opens on its own, along with the
frunk, trunk and charge-port door. The windows sit where the car says they
are. The mirrors fold when you lock, the door handles come out when you
unlock, and the lamps play the lock and unlock light sequence. Paint, wheels
and the Dream Edition badges match what your car reports.

![Opening and closing the driver door](https://raw.githubusercontent.com/Terablo/lucid-nexus/main/images/doors.webp)

**Charging, shown as it happens.** When you plug in, the camera swings to the
port, the flap opens, the cable seats and the session panel slides out with
the live power trace, time to finish, range added and a Stop button. The
Charging page also has the charge limit, the current limiter and your charge
history.

![Plugging in: the port opens, the cable seats and the session panel appears](https://raw.githubusercontent.com/Terablo/lucid-nexus/main/images/charging.webp)

**Driving view.** Shift out of Park and the car takes to the road. The lane
markings move at the car's real speed, the scenery streams past, and the
headlights light up the road at night. While the car is moving, the controls
that would command it are locked.

![Driving at night](https://raw.githubusercontent.com/Terablo/lucid-nexus/main/images/driving.webp)

**Day and night.** The scene follows the sun, from a daylight river valley to
a moonlit night. Seven scenes are included, with exposure, colour and glow
controls if you want to tune the look.

![The same car at night, headlights on](https://raw.githubusercontent.com/Terablo/lucid-nexus/main/images/night.webp)

**Climate and seats.** Set the cabin temperature and mode, and drive every
heated or ventilated seat and the steering wheel from a seat-by-seat cabin
plan. The card finds these controls automatically.

![The climate page](https://raw.githubusercontent.com/Terablo/lucid-nexus/main/images/climate.webp)

**Make it yours.** The Wrap Studio has factory paints, any custom colour,
gloss, satin or matte finishes, and wheel designs and finishes. You can also
set window tint, ride height, the ambient light strip and upholstery colour.
These are display choices only: the car itself is not changed.

![Repainting the car in the Wrap Studio](https://raw.githubusercontent.com/Terablo/lucid-nexus/main/images/wrap.webp)

**Phone, tablet or wall.** The layout adapts from a desktop down to a phone,
including the Home Assistant Companion app. Layout choices such as columns,
interface scale and the information panel's position are remembered per
device.

![Lucid Nexus on a phone, with its climate page](https://raw.githubusercontent.com/Terablo/lucid-nexus/main/images/phone.webp)

**A family of cards.** Beside the full dashboard there are ten smaller cards
you can drop into the dashboards you already have. Most of them need no 3D,
so they load instantly.

**Light on your device.** The card picks a render quality to suit the device
and stops drawing when nothing on screen is moving. If the integration briefly
drops out, it keeps showing the last known state, and it recovers on its own
if the browser resets the 3D context.

## Requirements

- Home Assistant **2024.4** or newer
- The [Lucid Motors integration](https://github.com/borski/ha-lucidmotors)
  (search "Lucid Motors" in HACS), set up and reporting entities
- A browser with WebGL. Any current desktop or mobile browser has it, as does
  the Companion app.

## Installation

### HACS (recommended)

[![Open your Home Assistant instance and open this repository in HACS](https://my.home-assistant.io/badges/hacs_repository.svg)](https://my.home-assistant.io/redirect/hacs_repository/?owner=Terablo&repository=lucid-nexus&category=plugin)

The button opens HACS in your own Home Assistant with this repository filled
in. To do it by hand instead:

1. In HACS, open the three-dot menu, choose **Custom repositories**, and add
   `https://github.com/Terablo/lucid-nexus` with type **Dashboard**.
2. Search for **Lucid Nexus** and download it.
3. Reload your browser when prompted.

HACS registers the dashboard resource for you, so there is no URL to add.
Everything the card needs ships in the release, including the Gravity and Air
3D models, the scenes and the fonts. The download is about 27 MB on disk: the
16 MB of files plus a pre-compressed copy of each, which Home Assistant serves
to browsers in their place. The first time a browser opens the card it fetches
about 3 MB, and after that it loads from cache.

### Moving from the old `lucid-card` repository

Until October 2026 this repository was `Terablo/lucid-card`. HACS stops
updating a repository once its name changes, so if you installed it under the
old name, install it again:

1. In HACS, open **Lucid Nexus**, choose **Remove**, and reload the browser.
2. If `https://github.com/Terablo/lucid-card` is still listed under
   **Custom repositories**, delete it there too.
3. Install again with the button above.
4. Under **Settings → Dashboards → ⋮ → Resources**, delete any
   `/hacsfiles/lucid-card/lucid-card.js` entry that is still there. The new one
   is `/hacsfiles/lucid-nexus/lucid-card.js`.

Your dashboards do not change: the card types (`custom:lucid-card` and the
rest) are the same, and the card's settings live in your browser.

### Manual

1. Download **every asset** from the
   [latest release](https://github.com/Terablo/lucid-nexus/releases/latest) into
   `config/www/lucid-nexus/`. Keep the folder flat, with no subfolders, and
   include the `.br` and `.gz` files.
2. Add the resource under **Settings → Dashboards → ⋮ → Resources**:
   ```yaml
   url: /local/lucid-nexus/lucid-card.js
   type: module
   ```

## Quick start

Choose one of three ways. They are listed from least to most setup.

### 1. Let the setup card do it

On any dashboard, choose **Add card**, search for **Lucid**, and pick
**Lucid Nexus Setup**. It lists the entities it found on your car and asks how
the tiles should be laid out. One tap then creates a **Lucid** dashboard in
your sidebar with the Nexus card on it, already set to full screen. Nothing is
written until you tap, and the card never edits a dashboard it did not create.

This needs an administrator account. If you are not an admin, the setup card
shows you the configuration to paste instead.

![The setup card](https://raw.githubusercontent.com/Terablo/lucid-nexus/main/images/setup.webp)

### 2. A self-updating dashboard

Go to **Settings → Dashboards → Add dashboard**. Open the new dashboard's
three-dot menu, choose **Raw configuration editor**, and enter:

```yaml
strategy:
  type: custom:lucid-nexus
  tray_columns: "3"    # optional; leave it out to lay out per device
```

Home Assistant rebuilds this dashboard each time it opens, so it picks up new
defaults and new cards as Lucid Nexus is updated. It does not need admin
rights.

### 3. By hand

Add a **Manual** card:

```yaml
type: custom:lucid-card
```

Put it in a **Panel (single card)** view or another full-width layout. It is a
full-page card, and a narrow masonry column squeezes it.

### The smaller cards

All of these are in the same **Add card** dialog. Search for "Lucid" to find
them.

| Card | Type | What it shows | 3D |
| --- | --- | --- | --- |
| Lucid Nexus | `custom:lucid-card` | The full 3D dashboard | yes |
| Glance | `custom:lucid-glance-card` | One-row strip: battery, range, doors, climate, frunk | no |
| Key | `custom:lucid-key-card` | Photo tile with unlock, precondition, frunk and the four doors | no |
| Charge | `custom:lucid-charge-card` | Live session: dial, kW, range added, time to full | no |
| Stats | `custom:lucid-stats-card` | Seven days of driving: distance, efficiency, energy | no |
| Map | `custom:lucid-map-card` | Live location and speed on Home Assistant's map | no |
| Tile | `custom:lucid-tile-card` | One fact at tile size: battery, range, doors, tyres and more | no |
| Cabin | `custom:lucid-cabin-card` | Target temperature, modes, heated and ventilated seats | no |
| Control | `custom:lucid-control-card` | Phone column with a 3D header and one-tap controls | yes |
| Model | `custom:lucid-model-card` | A live 3D tile you can orbit, re-light and repaint | yes |
| Ambient | `custom:lucid-ambient-card` | Wide wall-tablet tile with the car slowly turning | yes |
| Setup | `custom:lucid-setup-card` | Creates the Lucid dashboard for you | no |

## Supported models

| Model | Status |
| --- | --- |
| **Gravity** | Fully supported |
| **Air** | Early access |
| **Sapphire** | Early access. The model is streamed rather than bundled (see [Privacy](#privacy)). |

The card reads your model from Home Assistant's device registry. To choose it
yourself, set `model: gravity | air | sapphire`.

**Early access** means nobody has checked that model against a real car yet.
The card was built and tuned on a Gravity. Air and Sapphire stay switched off
until you acknowledge that:

```yaml
type: custom:lucid-card
model: air
early_access_ack: true
```

Without the acknowledgement, the card shows a notice where the car would be.
With it, an EARLY ACCESS badge stays in the corner, and the notice lists the
features that model cannot show yet. Depending on the model, these include
fender flares, seat colour, stealth trim, the animated light sequences and the
fender badges. The animated charge cable is also Gravity-only for now. If you own an Air or a Sapphire,
reports from you are what will move it out of early access.

## Configuration

### Auto-discovery

You normally do not need any configuration. Every entity the Lucid Motors
integration creates is tagged the same way, so on first load the card finds
your car's entities and fills in anything you have not set. A value you set in
YAML always wins.

If you have more than one Lucid on the account, choose the car with
`vehicle_name`. It must match the name set in the Lucid app (**Vehicle Info →
Edit → Vehicle Name**):

```yaml
type: custom:lucid-card
vehicle_name: Lucid GRAVITY
```

You can see what the card found in the browser console (F12, with the **Info**
level shown). It logs each entity it picked and lists any Lucid entities it
does not use yet.

### Setting entities yourself

The integration names entities after your car's nickname. A car named
"Lucid GRAVITY" gets `sensor.lucid_gravity_gear_position` for its "Gear
position" sensor. To get the exact IDs, go to **Settings → Devices & services →
Entities**, filter by the Lucid Motors integration, and copy them from there.

```yaml
type: custom:lucid-card
# Only needed to override auto-discovery:
battery_entity: sensor.lucid_gravity_remaining_battery_percent
range_entity: sensor.lucid_gravity_remaining_range
climate_entity: climate.lucid_gravity_climate_control
driving_entity: sensor.lucid_gravity_gear_position
charging_entity: switch.lucid_gravity_charging
trunk_entity: cover.lucid_gravity_trunk
frunk_entity: cover.lucid_gravity_frunk
lock_entity: lock.lucid_gravity_door_locks
door_fl_entity: binary_sensor.lucid_gravity_front_left_door
door_fr_entity: binary_sensor.lucid_gravity_front_right_door
door_rl_entity: binary_sensor.lucid_gravity_rear_left_door
door_rr_entity: binary_sensor.lucid_gravity_rear_right_door
# The charge panel's rate row uses the range-per-hour estimate, not the kW rate:
charge_range_rate_entity: sensor.lucid_gravity_charging_rate_range_estimate
```

### Other options

| Key | Default | What it does |
| --- | --- | --- |
| `model` | from the device registry | `gravity`, `air` or `sapphire` |
| `early_access_ack` | `false` | Required to load the Air or the Sapphire |
| `vehicle_name` | first car found | Selects the car when the account has more than one |
| `tray_columns` | auto | `"1"`, `"2"` or `"3"` fixes the tile layout on every device |
| `tube_res`, `tube_fireflies` | `256`, `true` | Render height and firefly effect of the driving scenery (YAML only) |

### The Settings tray

Everything else is set on the card itself, under the gear icon in the tray:
scene and lighting, Wrap Studio, window tint, ride height, light glow, render
quality, interface scale, tile layout, information panel position, the map,
and charge-history options. These settings are stored in your browser, so each
device keeps its own. Reset to defaults puts them back without touching the
car.

## Good to know

- **Commands that matter ask first.** Unlocking, opening or closing the frunk
  or trunk, opening the windows, stopping a charge and installing a software
  update all ask for confirmation.
- **Controls lock while the car is moving** or installing an update. The card
  says why. You can still use the card's own settings.
- **Charge panel and screen shape.** On a wide (landscape) card with a two- or
  three-column layout, the charge panel slides out beside the car. On a phone
  or a portrait card, the information panel shows the session instead, and the
  full details are on the Charging page.
- **Charge history stays in your browser.** It is built from Home Assistant's
  history and kept on each device.

## Troubleshooting

**"Custom element doesn't exist: lucid-card".** The resource has not loaded.
Reload the browser. If you installed manually, check the resource URL. In the
Companion app, use **Reset frontend cache** (under the app's debugging
settings).

**The card looks older than the version you installed.** The browser is
running a cached copy. The console prints a `LUCID NEXUS <build>` line on load,
which tells you the version it is running. Do a hard refresh, or reset the
frontend cache in the Companion app.

**Something is missing from a page.** Open the console (Info level). The card
lists the Lucid entities it could not place, so you can set the right one by
hand. If a value you expect is not there, the integration may not report it
yet.

**An Air or a Sapphire shows a notice instead of the car.** Add
`early_access_ack: true`. See [Supported models](#supported-models).

**It is slow on an older phone or tablet.** In the Settings tray, set
**Render Quality** to *Balanced* or *Low*. The no-3D cards (Glance, Key,
Charge, Tile, Cabin) are another option for small screens.

**The car shows as asleep, or the data looks old.** The card can only show what
the integration last received. The information panel says how long the car has
been asleep. Use the Wake button if your integration has one.

**You run a Content-Security-Policy on a reverse proxy.** Add
`https://lucidwraps.studio` to `connect-src` if you use the Sapphire. Stock
Home Assistant needs no change.

## Privacy

- The card talks to **your Home Assistant**. Every reading and command goes
  through the Lucid Motors integration's entities, and the card never contacts
  Lucid's servers itself.
- Everything for the Gravity and the Air ships in the release and is served by
  your Home Assistant. That includes the models, scenes, fonts and 3D decoder.
- **The Sapphire model is the exception.** It is streamed from
  `lucidwraps.studio` and then cached in your browser. It is only fetched if
  you choose the Sapphire. The same host also keeps a few optional extra
  scenes that a default card never loads.
- **Map tiles** for the card's map come from OpenStreetMap.
- Settings and charge history are stored in your browser. Nothing is sent
  anywhere else.

## Authors

- **[MakeshiftArtisan](https://github.com/MakeshiftArtisan)**, who wrote most
  of the card
- **[Terablo](https://github.com/Terablo)**

Bug reports and real-car feedback, especially from Air and Sapphire owners,
are welcome in [Issues](https://github.com/Terablo/lucid-nexus/issues).

## Licensing

The card's own code is covered by [LICENSE](LICENSE). It is free for personal
use with Home Assistant. The release also contains components this project
does **not** own and grants no rights to, each under its own terms. See
[NOTICE.md](NOTICE.md) for the full list. The most important of these are the
Lucid vehicle 3D models and imagery. They are derived from Lucid Motors'
assets, heavily modified and extended by this project, and included only so the card
can render your car. Third-party components, including model-viewer, three.js,
the Poly Haven scenes, the fonts and the tyre geometry, are credited in
NOTICE.md.

This project is not affiliated with, endorsed by or sponsored by Lucid Motors.
"Lucid", "Lucid Gravity" and the Lucid logo are trademarks of Lucid Group,
Inc., used here only to identify vehicle compatibility.
