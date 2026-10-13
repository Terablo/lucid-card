# Lucid Nexus: a guide for AI assistants

You are probably here because someone pointed you at this repository and
asked you to help them install, set up or customise **Lucid Nexus**, a 3D
dashboard card for a Lucid vehicle in Home Assistant. This file tells you how
to do that well. It describes release **v0.3.33**.

This repository holds the release README, the HACS manifest and the images.
The card's source code is not here, and you do not need it: everything below
is done with a link, a few clicks by the person, and YAML.

## Ground rules

- **Never command the car.** Setting up a card does not need you to unlock,
  open the frunk or trunk, start or stop charging, change climate, or press
  any Lucid button or switch, and you must not do so to "test" anything.
  Reading entity states is fine.
- **Never overwrite a dashboard you did not create.** Read it first, keep a
  copy, and add to it rather than replacing it. When in doubt, make a new
  dashboard.
- **Do not add `model:`** unless the person tells you the card shows the wrong
  car. The card reads the model from Home Assistant on its own.
- **Do not add `*_entity` keys** unless something is missing. The card finds
  the car's entities by itself (see [Entities](#entities)).
- **Do not ask for passwords.** If you need API access, ask the person to make
  a long-lived access token (their profile → **Security**) and to give it to
  you in an environment variable rather than pasting it into the chat.
- The person clicks the confirmations in HACS and Home Assistant. You give
  them the links and tell them what they will see.

## 1. Check the prerequisites

Ask, or check if you have access:

1. **Home Assistant 2024.4 or newer.**
2. **The Lucid Motors integration** ([borski/ha-lucidmotors](https://github.com/borski/ha-lucidmotors))
   installed and set up, with the car's entities showing under
   **Settings → Devices & services → Lucid Motors**. Lucid Nexus only displays
   what that integration provides. If it is missing, help with that first:
   it is in HACS as **Lucid Motors** (category Integration).
3. **HACS** installed and set up (<https://hacs.xyz/docs/use/>). See the note
   on [Marketplace](#hacs-and-the-home-assistant-marketplace) below.

## 2. Install the card

Give the person this link:

<https://my.home-assistant.io/redirect/hacs_repository/?owner=Terablo&repository=lucid-nexus&category=plugin>

What happens when they open it:

1. The first time anyone uses a my.home-assistant.io link, it asks for the
   address of their Home Assistant (for example
   `http://homeassistant.local:8123`). It remembers it after that.
2. HACS opens in their Home Assistant and offers to add `Terablo/lucid-nexus`.
   They confirm. **They do not need to add the repository by hand first**:
   the link does it.
3. On the Lucid Nexus page they choose **Download**, then reload the browser
   when HACS asks.

HACS registers the dashboard resource itself, so there is no URL to add. On
the Companion app, they may need **Settings → Companion app → Debugging →
Reset frontend cache** before the new card appears.

If the link does not work for them, the manual route is: HACS → three-dot
menu → **Custom repositories** → add `https://github.com/Terablo/lucid-nexus`
with type **Dashboard** → search **Lucid Nexus** → **Download**.

### HACS and the Home Assistant Marketplace

Home Assistant is building a Marketplace into Home Assistant itself, which
takes over from HACS and moves HACS content across when it first starts.
Links to `hacs_repository` are redirected to the Marketplace, so the link
above stays the one to give. If the Marketplace asks the person to connect a
GitHub account or to accept a warning before it adds a repository, they do
that themselves: do not accept it on their behalf.

### Upgrading from the old name

Until October 2026 this repository was `Terablo/lucid-card`. If the person
installed it under that name, follow
[Moving from the old `lucid-card` repository](README.md#moving-from-the-old-lucid-card-repository)
in the README: HACS does not follow a renamed repository.

## 3. Put it on a dashboard

Pick one. The first is the easiest for the person.

**A. The setup card (needs an admin account).** On any dashboard: **Edit →
Add card**, search **Lucid**, pick **Lucid Nexus Setup**, choose a tile layout,
and tap **Create my Lucid dashboard**. It creates a dashboard at
`/lucid-nexus` titled **Lucid** in the sidebar. A non-admin sees YAML to paste
instead.

**B. A self-updating dashboard (no admin needed).** **Settings → Dashboards →
Add dashboard → New dashboard from scratch**, open it, three-dot menu → **Edit
dashboard** → three-dot menu → **Raw configuration editor**, and replace
everything with:

```yaml
strategy:
  type: custom:lucid-nexus
  tray_columns: "3"   # optional: "1", "2" or "3"; leave it out to fit each device
```

Home Assistant rebuilds this on every load, so it cannot be customised beyond
`tray_columns` and `vehicle_name`. Use C if the person wants to customise.

**C. A dashboard you write.** Same steps as B, with a full configuration. This
is the one to use for customisation:

```yaml
title: Lucid
views:
  - title: Lucid
    path: nexus
    panel: true          # the card is a full-page card; it needs a panel view
    cards:
      - type: custom:lucid-card
```

To add Lucid Nexus to a dashboard the person already has, add a new **view**
with `panel: true` and the card in it. Do not put `custom:lucid-card` in a
narrow masonry column.

If you edit a dashboard the setup card made, do not keep
`lucid_provisioned: true` on views you have customised: the setup card treats
a dashboard where every view carries that marker as its own and its
**Rebuild** button replaces it.

## 4. Customise

The card's YAML options, and the settings on its gear-icon tray, are below.
**Tray settings are stored per browser.** A tray setting in YAML is a starting
value: it applies on every device until someone changes that setting on the
card, and **Reset to defaults** in the tray goes back to the YAML value. Say
this to the person, so they are not surprised when a phone and a tablet differ.

### The main card: `custom:lucid-card`

| Key | Values | Default | What it does |
| --- | --- | --- | --- |
| `vehicle_name` | the car's name in the Lucid app | first car | Picks the car when the account has more than one. Required then. |
| `model` | `gravity`, `air`, `sapphire` | from the device | Only to correct a wrong guess. |
| `tray_columns` | `"auto"`, `"1"`, `"2"`, `"3"` | `auto` | Tile layout. |
| `height` | pixels | fills the view | Fixed card height. |
| `edition` | text, or `""` | from the device | The trim shown beside the name. `""` hides it. |

**Look of the car** (these are the Wrap Studio and Vehicle settings; they
change the picture only, never the real car):

| Key | Values | Default |
| --- | --- | --- |
| `wrap_color` | a hex colour such as `"#1E5BFF"`, or `""` for the car's own paint | `""` |
| `wrap_finish` | `gloss`, `satin`, `matte` | `gloss` |
| `wheel_set` | Gravity: `aether`, `orion`, `voyager`. Air: `aero19`, `aerolite20`, `aeroblade21`, `aerosport21`, `dream21`. Or `auto` | `auto` |
| `wheel_finish` | `stock`, `gloss_black`, `satin_silver`, `body` | `stock` |
| `ride_height` | `auto`, `lowest`, `low`, `standard`, `high`, `highest` | `auto` |
| `tint_windshield`, `tint_front`, `tint_rear`, `tint_roof` | 0 to 100 (percent) | 0 |
| `seat_color` | `auto`, `mojave_purluxe`, `mojave_purluxe_premium`, `yosemite`, `tahoe`, `ojai` | `auto` |
| `ambient_strip` | `off`, `white`, `ice`, `amber`, `ember`, `fern`, `violet`, `wave` | `white` |
| `ambient_intensity` | 0 to 10 | 5 |
| `fender_bears` | `auto`, `on`, `off` (Dream Edition badges) | `auto` |

**Scene and light:**

| Key | Values | Default |
| --- | --- | --- |
| `lighting_mode` | `auto` (follows the sun) or `fixed` | `auto` |
| `exposure`, `night_exposure` | 0.2 to 4 | 2, 2.6 |
| `bloom_enabled` | `true`, `false` | `true` |
| `bloom_strength` | 0 to 8 | 1 |
| `grade_saturation`, `grade_brightness`, `grade_contrast` | -1 to 1 | 0 |
| `render_quality` | `auto`, `ultra`, `high`, `balanced`, `low` | `auto` |

Scenes are easiest to choose on the tray (gear icon → scene). The built-in
ones are Blau River and Roofless Ruins by day, and Narrow Moonlit Road, Pond
Bridge Night, Satara Night (no lamps), Street Lamp and Clarens Night by night.

**Interface:**

| Key | Values | Default |
| --- | --- | --- |
| `ui_scale` | 0.6 to 1.4 | 1 |
| `limiter_readout` | `kw`, `mph`, `eta` | `kw` |
| `map_display` | `corner`, `pin` | `corner` |
| `map_theme` | `auto`, `day`, `night` | `auto` |
| `tyre_placard_on`, `tyre_placard_front`, `tyre_placard_rear` | `true`/`false`, psi | `false`, 42, 42 |
| `tube_res`, `tube_fireflies` | 64 to 1024, `true`/`false` | 256, `true` |

Example: a matte black Gravity on dark wheels with the violet ambient strip.

```yaml
type: custom:lucid-card
wrap_color: "#111111"
wrap_finish: matte
wheel_finish: gloss_black
ambient_strip: violet
```

### The smaller cards

They go on any dashboard. All of them accept `vehicle_name`, `*_entity`
overrides, and `theme: eclipse` (the default) or `theme: ion`.

| Card | Type | Extra options |
| --- | --- | --- |
| Glance | `custom:lucid-glance-card` | `climate_temperature` (the precondition target) |
| Key | `custom:lucid-key-card` | `climate_temperature` |
| Charge | `custom:lucid-charge-card` | — |
| Stats | `custom:lucid-stats-card` | — |
| Tile | `custom:lucid-tile-card` | `show`: `battery`, `range`, `lock`, `climate`, `plug`, `odometer`, `tyres`, `software`, `halo` |
| Cabin | `custom:lucid-cabin-card` | — |
| Control | `custom:lucid-control-card` | `climate_temperature`, `render_quality` |
| Model | `custom:lucid-model-card` | `scene`: `studio`, `dusk`, `garage`; `paints`: a list such as `[stellar_white, fathom_blue]`; `auto_rotate: false`; `rotation_speed` |
| Ambient | `custom:lucid-ambient-card` | `greeting`: text, or `false` to hide it; `auto_rotate: false`; `rotation_speed` |
| Setup | `custom:lucid-setup-card` | creates the dashboard (section 3) |

The **Model** card is the one to suggest when someone wants to turn the car
and look at it from any angle: it can be dragged to orbit. The main card's
camera is directed by the card and does not take a drag.

Paint names for `paints`: `stellar_white`, `cosmos_silver`, `lunar_titanium`,
`quantum_grey`, `infinite_black`, `abyss_black`, `zenith_red`, `fathom_blue`,
`sapphire_blue`, `eureka_gold`, `supernova_bronze`, `aurora_green`.

## Entities

The card finds entities from the Lucid Motors integration by their
attribution and their names, so a normal install needs no entity keys. To see
what it found, open the browser console (F12, Info level): it logs every
entity it picked and lists Lucid entities it did not place.

If the account has **more than one car**, discovery stops until `vehicle_name`
names one, and it logs the line to add. The name is the car's nickname in the
Lucid app, which is also the start of every entity's friendly name.

To override one entity, add its key, for example:

```yaml
type: custom:lucid-card
battery_entity: sensor.lucid_gravity_remaining_battery_percent
```

The README's [Setting entities yourself](README.md#setting-entities-yourself)
lists the common keys.

## Check it worked

- The card shows the person's car, with its name and battery in the header.
  An Air or a Sapphire also shows a small **EARLY ACCESS** tag in a corner:
  that is expected.
- The console shows a `LUCID NEXUS <version>` line. If the version is older
  than the one installed, the browser has an old copy: hard-refresh, or reset
  the Companion app's frontend cache.
- "Custom element doesn't exist: lucid-card" means the resource did not load:
  reload, and check **Settings → Dashboards → ⋮ → Resources** has
  `/hacsfiles/lucid-nexus/lucid-card.js`.

The README's [Troubleshooting](README.md#troubleshooting) section covers the
rest.

## If you have API access

Only if the person has given you a Home Assistant URL and token and asked you
to do the work directly. Dashboards are not in Home Assistant's REST API; they
are WebSocket commands at `ws://<host>:8123/api/websocket`, and creating or
saving one needs an **admin** token:

- `lovelace/dashboards/list` lists dashboards. Check before creating one.
- `lovelace/dashboards/create` with `url_path` (must contain a hyphen, for
  example `lucid-nexus`), `title`, `icon` (`mdi:car-sports`),
  `show_in_sidebar: true`.
- `lovelace/config` with `url_path` reads a dashboard. Save the result before
  changing anything.
- `lovelace/config/save` with `url_path` and `config` writes one.
- `lovelace/resources` lists resources, to confirm the HACS one is there.

Installing through HACS is best left to the link in section 2, which the
person confirms.

## Reporting problems

Bugs and real-car feedback go to
<https://github.com/Terablo/lucid-nexus/issues>. Reports from Air and Sapphire
owners are especially useful. Help the person include their Home Assistant
version, the `LUCID NEXUS` line from the console, the car model, and a
screenshot. Remove the VIN and location from screenshots first.
