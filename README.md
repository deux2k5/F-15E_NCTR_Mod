# F-15E NCTR Mod

Expands the F-15E's Non-Cooperative Target Recognition (NCTR) system in DCS World to recognize additional aircraft, including modded and newly released ones.

**This mod does not break Integrity Check (IC).**

---

## Installation

1. Navigate to `Saved Games/DCS.openbeta/Config`.
2. If the `F-15E` folder doesn't exist, create it.
3. Copy `NCTR.lua` into `Saved Games/DCS.openbeta/Config/F-15E`.

---

## How It Works

The mod extends the F-15E's NCTR capabilities by adding entries to the `nctr_table`. Each entry follows this format:

```
nctr_table["Aircraft_Internal_Name"] = {"Radar_Display_Name", Range_Scale, Aspect_Angle}
```

- **Aircraft_Internal_Name:** The internal DCS identifier for the aircraft. Found in the mod/aircraft's entry.lua.
- **Radar_Display_Name:** The name displayed on your radar screen.
- **Range_Scale:** Maximum range at which the aircraft can be identified by NCTR.
- **Aspect_Angle:** Maximum angle (in degrees) at which the aircraft can be identified. Use `-1.0` for no aspect restriction.

## Adding or Removing Aircraft

### Adding an Aircraft

Add a new line to `NCTR.lua`:

```lua
nctr_table["New_Aircraft_Name"] = {"DISPLAY", 10.0, 45.0}
```

### Removing an Aircraft

Delete the corresponding line from `NCTR.lua`.

### Adjusting Detection Parameters

```lua
nctr_table["F-16C"] = {"F-16", 15.0, 45.0} -- Longer range detection
nctr_table["F-16C"] = {"F-16", 5.0, 45.0}  -- Shorter range detection
nctr_table["F-16C"] = {"F-16", 10.0, 60.0} -- Wider aspect angle
nctr_table["F-16C"] = {"F-16", 10.0, 30.0} -- Narrower aspect angle
```

---

## Credits

- **Brody** - Contributions to the NCTR table.

---

**Disclaimer:** Use this mod at your own risk. Always backup your original files before making any modifications. This mod is not endorsed by Eagle Dynamics or Razbam.
