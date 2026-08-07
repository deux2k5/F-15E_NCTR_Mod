# F-15E NCTR Mod

Expands the DCS F-15E's Non-Cooperative Target Recognition (NCTR) table with missing DCS aircraft and community-mod aircraft.

The file uses DCS's Saved Games override and does not modify the F-15E module files. It is intended to remain Integrity Check compatible, although Eagle Dynamics can change IC behavior.

## Installation

1. Create `%USERPROFILE%\Saved Games\DCS\Config\F-15E` if it does not exist.
2. Copy `NCTR.lua` into that folder.
3. Restart DCS.

The final path should be:

```text
%USERPROFILE%\Saved Games\DCS\Config\F-15E\NCTR.lua
```

For a legacy Open Beta installation, replace `DCS` with `DCS.openbeta`. Delete the copied file to uninstall the mod.

## What “realistic” means here

The APG-70 NCTR description supplied with the DCS F-15E says the radar analyzes returns from engine turbine and fan blades and compares them with an onboard recognition library. That real library, its cockpit labels, and its recognition thresholds are not public.

This mod therefore uses the installed DCS F-15E table as its source of truth. Stock parameters are retained, obvious label errors are corrected, and added aircraft inherit the closest stock DCS signature class. Stock `F-14D` and `F-111F` placeholders are omitted because DCS cannot register either aircraft type. Real-world manufacturer and military sources are used to choose aircraft families, but they cannot establish an authentic classified NCTR result.

The current baseline was checked against DCS `2.9.26.23303`:

```text
<DCS install>\Mods\aircraft\F-15E\Cockpit\Scripts\NCTR.lua
```

## Table format

```lua
nctr_table["DCS_Type_Name"] = {"Radar_Display_Label", dB_Value, Aspect_Angle}
```

- `DCS_Type_Name` is the exact runtime `Name` registered by the aircraft's `add_aircraft()` call.
- `Radar_Display_Label` is the family-level label shown by the F-15E.
- `dB_Value` is the parameter DCS itself logs as `dB`. It is **not a range value**; its exact public interpretation is undocumented.
- `Aspect_Angle` is the parameter DCS logs as `angle`. Stock DCS uses `-1.0` for several rotorcraft and UAV entries; the exact special-case behavior is undocumented.

For added aircraft, this table reuses only stock F-15E baselines:

| Added aircraft class | Stock DCS analogues | dB / angle |
| --- | --- | --- |
| Fighter, attack aircraft, or jet trainer | F-16, Su-27, A-10 | `10.0 / 45.0` |
| Transport, tanker, airliner, or AEW aircraft | C-130, C-17, KC-135, E-3 | `5.0 / 60.0` |
| Rotorcraft, tiltrotor, or UAV | UH-60, MQ-9 | `10.0 / -1.0` |
| Aircraft with no closer public analogue | Nearest stock entry, documented as an approximation | varies |

The B-2 entry uses the stock F-117's `20.0 / 15.0` values as an approximation. No public source supports a unique B-2 dB value.

Public radar cross-section, aircraft size, or detection-range figures do not support finer per-aircraft values here: NCTR uses engine-return modulation, and the real recognition thresholds are not public.

## Family labels

- All listed F/A-18 variants use `F/A-18`. This follows the stock table's family-level convention; it does **not** claim that a real NCTR library could never distinguish a legacy Hornet from a Super Hornet.
- The F/A-18E, F/A-18F, and EA-18G share the F414 engine and substantial platform commonality. Legacy F/A-18A-D aircraft use the F404, so the common display text is a conservative family label rather than an engine-equivalence claim.
- `J-11A` uses the stock DCS `SU27` family result, normalized to `SU-27`.
- `UH-60L` uses the stock `UH-60` family label and rotorcraft parameters.
- The Eurofighter mod's `Eurofighter` runtime type displays as `EF-2000`.

## Adding an aircraft

Use the aircraft's exact runtime `Name`, then copy the values of the closest stock DCS analogue instead of treating the dB field as detection range:

```lua
nctr_table["New_Aircraft_Name"] = {"DISPLAY", 10.0, 45.0}
```

Lua keeps the last value assigned to a duplicate key, so each runtime name should appear only once. On startup, `dcs.log` reports successful registrations in this form:

```text
ARF: Added NCTR type "..." (...) angle=45.000000 dB=10.000000
```

An `Unknown NCTR` line normally means the aircraft's runtime name is absent or does not match.

## Sources

- Eagle Dynamics, [DCS: F-15E Flight Manual](https://www.digitalcombatsimulator.com/en/downloads/documentation/dcs-f15e_flight_manual_en/).
- Steve Davies, [*Be Afraid of the Dark* official DCS sample](https://www.digitalcombatsimulator.com/images/newsletter/20230623/Steve_Davies_Be_Afraid_of_the_Dark_F-15E_Book_Sample.pdf), p. 47, for the APG-70 NCTR mechanism.
- DCS runtime-log examples showing the fields as [`angle` and `dB`, and rejecting the stale `F-14D` and `F-111F` entries](https://forum.dcs.world/topic/301560-crash/).
- U.S. Navy, [F/A-18A-D Hornet and F/A-18E/F Super Hornet fact file](https://www.navy.mil/Resources/Fact-Files/Display-FactFiles/Article/2383479/fa-18a-d-hornet-and-fa-18ef-super-hornet-strike-fighter/), for the F404/F414 distinction.
- NAVAIR, [EA-18G Growler](https://www.navair.navy.mil/product/EA-18G-Growler), for its F/A-18F basis, F414 engines, and platform commonality.
- U.S. Army ODIN, [UH-60L Black Hawk](https://odin.t2com.army.mil/WEG/Asset/UH-60L_Black_Hawk_American_Utility_Helicopter), for its relationship to the UH-60A.
- U.S. Air Force, [KC-46A Pegasus fact sheet](https://www.af.mil/About-Us/Fact-Sheets/Display/Article/104537/kc-46a-pegasus/).
- Airbus, [A330 MRTT](https://www.airbus.com/en/products-services/defence/military-aircraft/a330-mrtt) and [A400M](https://www.airbus.com/en/products-services/defence/military-aircraft/a400m).
- Eurofighter, [aircraft and EJ200 engine features](https://www.eurofighter.com/the-aircraft/features).
- Rosoboronexport, [AL-31F family data](https://roe.ru/pdfs/pdf_4785.pdf), which identifies the AL-31FN application to the J-10A and the AL-31F applications to several Flanker variants.

## Credits

- **Brody** — contributions to the NCTR table.

## Disclaimer

This community mod is not endorsed by Eagle Dynamics, RAZBAM, or the aircraft manufacturers and military organizations cited above.
