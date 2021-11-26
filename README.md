# Marlin-Ender-3-Max w/ BL-Touch
Updated to 2021-11-25 (2.0.9.2 plus bugfixes to date)  - [MarlinFirmware/Marlin@981ad44ded7b3805ce54da389581403c230f7dbd](https://github.com/MarlinFirmware/Marlin/commit/981ad44ded7b3805ce54da389581403c230f7dbd)

Ender 3 Max Configs for Marlin bugfix-2.0.x branch. The posted configration files are based on stock hotend and extruder with bltouch/crtouch. See [releases](https://github.com/ChadDevOps/Marlin-Ender-3-Max/releases) for a compiled version.

**NOTE:** These settings have been tweaked from default, please search for "CHADDEVOPS" for changed settings. See MarlinFirmware's [import-2.0.x configurations](https://github.com/MarlinFirmware/Configurations/tree/import-2.0.x/config/examples/Creality/Ender-3%20Max) for defaults.

After applying a new firmware, it's best to run `M502` and `M500` to load and save the default settings. You can also run Initialize EEPROM from the lcd screen under config advanced settings. You will need to re-level the bed after issuing `M502`.

You can issue commands using pronterface or octoprint (if you have it installed on a raspberry pi - recommended)

```
Reset settings and save them to EEPROM

M502 ; reset
M500 ; saved
```

I would recommend the following as these can differ for every machine:

- Tune your PID for the hotend
- Tune your PID for the bed (optional)
- Calibrate E-Steps for your extruder (default is 93 for Ender 3 printers)
- Calibrate Probe Z-Offset
- Remove the Z-stop if your X-axis stops before the probe can reach the bed

https://youtu.be/qPDBNBgdW6o?t=680 - Dr Vax on building the Marlin Firmware with VS Code.

# Custom Mods - Updated Nov. 2021

In Use:

* [Micro Swiss Direct Drive Extruder for Creality CR-10 / Ender 3 Printers × 1](https://store.micro-swiss.com/products/micro-swiss-direct-drive-extruder)
* [Satsana Dual 4010 Fan Direct Drive with longer ducts -  fan shroud](https://www.thingiverse.com/thing:4621096) - note: in order to use the CR Touch I had to cutout a section from the bottom of the mount for the 5 wire cable. This was also resin printed (as my printer wasn't working at the time).
* [CR Touch Kit](https://amzn.to/3CVsCRv)
* [PTFE Teflon® Premium Bowden Tube](https://www.3dmakerengineering.com/collections/accessories/products/ptfe-teflon-premium-bowden-tube)
* [M2 Hardened High Speed Steel Nozzle - MK8 ](https://store.micro-swiss.com/collections/nozzles/products/micro-swiss-mk8-plated-m2-hardend-high-speed-steel-nozzle)
* [SIBOOR Ender 3 Accessories Dual Z Axis Kit](https://amzn.to/3iLilRr)
* [2-Pack CR10 Z axis T8 Anti Backlash Spring Loaded Nut ](https://amzn.to/3xoc8yU)
* 2x [ReliaBot 500mm T8 Tr8x8 Lead Screw and Brass Nut (Acme Thread, 2mm Pitch, 4 Starts, 8mm Lead)](https://amzn.to/3vqsgPb) - Note: a little long, will stick out top of printer - perhaps put some spinning Groots on top?
* [High Temperature CR-10 Plated Copper Heater Block for MK8 Extruder](https://amzn.to/3gtcM8M)
* [Copperhead™ Heat Break × 1 - Version: C-E](https://www.sliceengineering.com/collections/replacement-parts/products/copperhead%E2%84%A2-heat-break?variant=36827917713570)
* [Protronix Series 9 Extreme Performance Thermal Compound Paste](https://amzn.to/3xoYifI)
* [Gizmo Dorks PEI sheet](https://amzn.to/3mQ7442)
* [310mm x 320mm Borosilicate Glass Plate](https://amzn.to/3khk4x)
* Aluminum foil under glass plate to fix leveling (see photo at end of Readme).

Rgarding leveling: I only use the CR Touch to get the bed perfectly leveled (using bed visualizer in octopi) and verifying after printing a [bed level test](https://www.thingiverse.com/thing:2187071). I then disable mesh loading in the cura start code.

In order to use this config with the mods listed above, run the following commands (or modify as needed):

```
M301 P34.18 I3.47 D84.26 ;creality with bimetallic
M205 J0.08 ;set Junction Deviation
M851 X-47.00 Y-15.00 Z-0.16; chad's defaults for swiss dd and swiss Nozzle
M92 E140.4 ; gst3d
M900 K0.11 ;set linear advance pla
M204 P500 T500 ; accel
M500 ;save
M501 ;load
```

Not in use, but available:

* [3dmakerengineering.com Tungsten Carbide 3D Printer Nozzle](https://www.3dmakerengineering.com/collections/3d-printer-nozzles/products/tungsten-carbide-3d-printer-nozzle?variant=14784857112631)

No longer in use:

* [BL-Touch](https://amzn.to/384td6M) - it died, switched to the new [CR Touch Kit](https://amzn.to/3CVsCRv).


## Stock configs

I did not start using Marlin until recently (3/3/2021). The first month of my printer experience was using Klipper. Due to this, the config in this repo is my best guess for stock settings. Another user sent me their tuned hotend PID temps for the stock hotend.

## Cura Start Code

After leveling the bed, and saving it to EEPROM (`M500`), add `M420 S1` after `G28` in your start code. You do not need to run a bed-level before every print. This will load the mesh if you have already leveled your bed. Also pre-heat the bed before leveling due to expansion.

```
; Ender 3 Custom Start G-code
G92 E0 ; Reset Extruder

M140 S{material_bed_temperature_layer_0} ;Start heating bed
M190 S{material_bed_temperature_layer_0} ;Wait for bed to reach temp before proceeding

G28 ; Home all axes

M104 S{material_print_temperature_layer_0} ;Start heating extruder

M420 S1 ; Load bed mesh - marlin - NOTE: Level bed first and SAVE - issue M500 after leveling, then print.
; I only use the CR Touch to get the bed perfectly level using bed visualizer in octopi.
; I then disable loading the mesh after doing a [bed level print test](https://www.thingiverse.com/thing:2187071)

G1 Z2.0 F3000 ; Move Z Axis up little to prevent scratching of Heat Bed
M109 S{material_print_temperature_layer_0} ;Wait for extruder to reach temp before proceeding
G1 X0.1 Y20 Z0.3 F5000.0 ; Move to start position

G1 X0.1 Y300.0 Z0.3 F1500.0 E30 ; Draw the first line
G1 X1.3 Y300.0 Z0.3 F5000.0 ; Move to side a little
G1 X1.3 Y20 Z0.3 F1500.0 E50 ; Draw the second line
G92 E0 ; Reset Extruder
G1 Z2.0 F3000 ; Move Z Axis up little to prevent scratching of Heat Bed
G1 X5 Y20 Z0.3 F5000.0 ; Move over to prevent blob squish

;M900 K0.2 ;Linear Advance - enable if using, make sure to calibrate first
```

## The Printer

### Current as of Nov 2021

* [Micro Swiss Direct Drive Extruder for Creality CR-10 / Ender 3 Printers × 1](https://store.micro-swiss.com/products/micro-swiss-direct-drive-extruder)
* [Satsana Dual 4010 Fan Direct Drive with longer ducts -  fan shroud](https://www.thingiverse.com/thing:4621096)

<img src="./Ender-3-Max-Nov2021.jpg?raw=true" width="250">

<img src="./Ender-3-Max-MicroSwissDD-Nov2021.jpg?raw=true" width="250">

## Old Info

### Old Mods - Pre June 2021

In order to use this config with the mods listed below, run the following commands (or modify as needed):

```
M301 P37.01 I5.19 D66.01 ;gulf coast hotend
M92 E139.28 ; WINSINN Dual Gear Extruder
M92 E147.71 ;set E-Steps for 0.6mm nozzle with WINSINN Dual Gear Extruder
M205 J0.1 ;set Junction Deviation
M900 K0.20 ;set linear advance
M420 Z0 ; turn off fade height or set to X mm
M500 ;save
```
NOTE: I still recommend that you calibrate your E-Steps (139.28 above).

* [Gulfcoast Robotics All Metal Hotend Conversion Kit w/Polished Titanium Heatbreak](https://amzn.to/3rg7BvT)
* [WINSINN Dual Gear Extruder](https://amzn.to/3qgkBQC)
* [3dmakerengineering.com Tungsten Carbide 3D Printer Nozzle](https://www.3dmakerengineering.com/collections/3d-printer-nozzles/products/tungsten-carbide-3d-printer-nozzle?variant=14784857112631)
* [PTFE Teflon® Premium Bowden Tube](https://www.3dmakerengineering.com/collections/accessories/products/ptfe-teflon-premium-bowden-tube)
* [BL-Touch](https://amzn.to/384td6M)

## Old Setup

<img src="./Ender-3-Max.jpeg?raw=true" width="250">

It's still a work in progress.

The stabilizers are from [Befenybay](https://amzn.to/3rhibTq). They are NOT designed for the Ender 3 Max. If you look at the bottom left you'll see I flipped the bracket. It sits below the bottom of the printer, but just above the bottom bumpers. I added the bars as I was seeing wobble with fast printing speeds.  Or design your own bracket and use a tap (the bolts are M5).
