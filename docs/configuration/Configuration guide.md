By default, you get a single bar at the bottom of all your screens.
To change that, you'll unsurprisingly need a config file.

This page details putting together the skeleton for your config to get you to a stage where you can start configuring
modules.
It may look long and overwhelming, but that is just because the bar supports a lot of scenarios!

If you want to see some ready-to-go config files check
the [examples folder](https://github.com/JakeStanger/ironbar/tree/master/examples)
and the example pages in the sidebar.

## 1. Create config file

The config file lives inside the `ironbar` directory in your `XDG_CONFIG_DIR`, which is usually `~/.config/ironbar`.

Ironbar supports a range of configuration formats, so you can pick your favourite:

- `config.json`
- `config.toml`
- `config.yaml`
- `config.corn` (Experimental, includes variable support for re-using blocks.
  See [here](https://github.com/jakestanger/corn) for info)

You can also override the default config path using the `IRONBAR_CONFIG` environment variable.

## 2. Pick your use-case

Ironbar gives you a few ways to configure the bar to suit your needs.
This allows you to keep your config simple and relatively flat if your use-case is simple,
and make it more complex if required.

### a) I want the same bar across all monitors

Place the bar config inside the top-level object. This is automatically applied to each of your monitors.

<details>
<summary>JSON</summary>

```json
{
  "position": "bottom",
  "height": 42,
  "start": [],
  "center": [],
  "end": []
}
```

</details>

<details>
<summary>TOML</summary>

```toml
position = "bottom"
height = 42
start = []
center = []
end = []
```

</details>

<details>
<summary>YAML</summary>

```yaml
position: "bottom"
height: 42
start: [ ]
center: [ ]
end: [ ]
```

</details>

<details>
<summary>Corn</summary>

```
{
  position = "bottom"
  height = 42
  start = []
  center = []
  end = []
}
```

</details>

### b) I want my config to differ across one or more monitors

Create a map/object called `monitors` inside the top-level object.
Each of the map's keys should be an output name,
and each value should be an object containing the bar config.

You can still define a top-level "default" config to use for unspecified monitors.
Alternatively, leave the top-level `start`, `center` and `end` keys null to hide bars on unspecified monitors.

> [!TIP]
> To find your output names, run `wayland-info | grep wl_output -A1`.

<details>
<summary>JSON</summary>

```json
{
  "monitors": {
    "DP-1": {
      "start": []
    },
    "DP-2": {
      "position": "bottom",
      "height": 30,
      "start": []
    }
  }
}
```

</details>

<details>
<summary>TOML</summary>

```toml
[monitors]

[monitors.DP-1]
start = []

[monitors.DP-2]
position = "bottom"
height = 30
start = []
```

</details>

<details>
<summary>YAML</summary>

```yaml
monitors:
  DP-1:
    start: [ ]
  DP-2:
    position: "bottom"
    height: 30
    start: [ ]
```

</details>

<details>
<summary>Corn</summary>

```
{
  monitors.DP-1.start = []
  monitors.DP-2 = {
    position = "bottom"
    height = 30
    start = []
  }
}
```

</details>

### c) I want one or more monitors to have multiple bars

Create a map/object called `monitors` inside the top-level object.
Each of the map's keys should be an output name.
If you want the screen to have multiple bars, use an array of bar config objects.
If you want the screen to have a single bar, use an object.

To find your output names, run `wayland-info | grep wl_output -A1`.

<details>
<summary>JSON</summary>

```json
{
  "monitors": {
    "DP-1": [
      {
        "start": []
      },
      {
        "position": "top",
        "start": []
      }
    ],
    "DP-2": {
      "position": "bottom",
      "height": 30,
      "start": []
    }
  }
}
```

</details>

<details>
<summary>TOML</summary>

```toml
[monitors]

[[monitors.DP-1]]
start = []

[[monitors.DP-2]]
position = "top"
start = []

[monitors.DP-2]
position = "bottom"
height = 30
start = []
```

</details>

<details>
<summary>YAML</summary>

```yaml
monitors:
  DP-1:
    - start: [ ]
    - position: "top"
      start: [ ]
  DP-2:
    position: "bottom"
    height: 30
    start: [ ]
```

</details>

<details>
<summary>Corn</summary>

```corn
{
  monitors.DP-1 = [
    { start = [] }
    { position = "top" start = [] }
  ]
  monitors.DP-2 = {
    position = "bottom"
    height = 30
    start = []
  }
}
```

</details>

## 3. Write your bar config(s)

Once you have the basic config structure set up, it's time to actually configure your bar(s).

Documentation on the available options can be found in the following locations:

- [Top-level options](top-level-options)
- [Module-level options](module-level-options)
- A [example config file](config) for a fully configured bar in each file format.
- Module-specific options on each module documentation page.
