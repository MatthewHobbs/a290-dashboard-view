# Alpine A290 Dashboard

An **Alpine A290** Home Assistant dashboard, adapted from
[**renault-5-dashboard-view**](https://github.com/Topolino65/renault-5-dashboard-view)
by [**Topolino65**](https://github.com/Topolino65) — full credit to them for the
original dashboard and design. See [Credits](#credits).

Two styles are included: the **standard** Alpine A290 dashboard (closest to the original)
([`YAML/front-end.txt`](YAML/front-end.txt)) and a **Bubble Card** version
([`YAML/front-end-bubble.txt`](YAML/front-end-bubble.txt)).

> **The data now comes from an add-on.** All of the car's data (battery, range,
> charging, plug, location, last-charge stats, …) is provided by the companion
> **[Alpine A290 add-on](https://github.com/MatthewHobbs/a290-ha-addon)**, which
> polls the Renault/Kamereon API and publishes `sensor.alpine_a290_*` entities via
> MQTT auto-discovery. **This is the largest change from the original.**
> — install the add-on, enter your details on its Configuration page, and the entities appear automatically.
>
> **The add-on can install this dashboard too.** Set its `deploy_dashboard` option to
> `standard` or `bubble` and it deploys the dashboard for you — nothing to copy into
> `/config/www`, no `configuration.yaml` edits, no raw-editor paste. This is the
> **recommended install**; see [INSTALLATION.md](INSTALLATION.md). (Install the HACS cards
> first.)

## Documentation

- **[INSTALLATION.md](INSTALLATION.md)** — full step-by-step installation guide.
- **[USER-GUIDE.md](USER-GUIDE.md)** — what each part of the dashboard shows.

## Prerequisites

**Data layer (required):**

1. **[Alpine A290 add-on](https://github.com/MatthewHobbs/a290-ha-addon)** — add it as
   a custom add-on repository, install, configure and start it.
2. **Mosquitto broker** add-on — the add-on uses it for MQTT auto-discovery.

**Frontend (install via HACS):**

3. **HACS** itself
4. **card-mod**
5. **Mushroom Cards**
6. **Button Card**
7. **Bubble Card** *(only for the Bubble dashboard)*
8. **Browser Mod** *(for the pop-up dialogs)*


The car's location uses Home Assistant's **built-in `map` card** — no API key and no
extra map plugin needed.

> The old prerequisites — *Advanced SSH & Web Terminal* and a separate *EV charger
> integration / charger adapter* — are **no longer required**. Charge power and
> session stats are derived by the add-on.

## API login details

You no longer put credentials in `secrets.yaml`. Enter them **once** on the
**Alpine A290 add-on → Configuration** page: My Alpine username & password, VIN,
locale, and (optionally) your Kamereon account id (leave blank to auto-discover).
Distance units follow the locale — **miles for `en_GB`**, km elsewhere.

## Pretty location (optional)

`sensor.alpine_pretty_location` ([`Templates/a290_pretty_location.yaml`](Templates/a290_pretty_location.yaml))
turns the add-on's `device_tracker.alpine_a290_location` into a friendly name
("Driveway" / "Home" / town). It degrades gracefully without the optional
`zone.driveway` or the [`places`](https://github.com/custom-components/places) integration.

## Map

The location tile uses Home Assistant's **built-in `map` card** on
`device_tracker.alpine_a290_location` — no Google Maps API key and no extra map plugin
required.

## Test mode

A self-contained simulation ([`Packages/a290_test.yaml`](Packages/a290_test.yaml) +
[`Templates/a290_test_panel_timers.yaml`](Templates/a290_test_panel_timers.yaml))
lets you preview the charge panels without a live charge. Toggle
`input_boolean.a290_test_mode`. It is optional and independent of the car data.

## Using images of your own car

This release ships only the Alpine A290 renders (in `Images/`). To use photos/renders
of your own car:

1. Drop your images into Home Assistant's `/config/www/backgrounds/` folder (via the
   File Editor or Samba).
2. In the dashboard YAML, replace the references to `/local/backgrounds/alpine_a290_background.png`
   and `/local/backgrounds/alpine_a290_side.png` with your own filenames.

## Credits

This project **originally started from** [**renault-5-dashboard-view**](https://github.com/Topolino65/renault-5-dashboard-view)
by [**Topolino65**](https://github.com/Topolino65) — full credit and thanks for the
original project (the dashboard concept and the renault-api integration approach).

It is now developed as an **independent project**: it is not affiliated with the
original and does not contribute changes back upstream. It has been
**substantially extended and reworked**:

- Rebranded Renault 5 → Alpine A290 throughout.
- A second dashboard style (Bubble Card version) alongside the standard one.
- **The data layer is now the [Alpine A290 add-on](https://github.com/MatthewHobbs/a290-ha-addon)** —
  a proper Home Assistant add-on (Docker + MQTT auto-discovery) replacing the old
  `venv` + shell-script + `secrets.yaml` setup. Credentials are entered on its config page.
- Resilient charge-session tracking, plug-state / stuck-plug detection, locale-aware
  distance units (miles for the UK), and derived charging-flap state — all in the add-on.
- The dashboards bind directly to the add-on's `sensor.alpine_a290_*` entities.
- Removed the per-model/trim image sets; documented how to use images of your own car.
- Guides converted to Markdown.

Both projects are released under the [MIT License](LICENSE); the original copyright
notice is retained there.

## Contributing

Issues and PRs for **this** project are welcome here. It's maintained independently and
changes are not submitted upstream to the original.
