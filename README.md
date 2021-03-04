# Marlin-Ender-3-Max w/ BL-Touch
Ender 3 Max Configs for Marlin bugfix-2.0.x branch - [MarlinFirmware/Marlin@fd270dd](https://github.com/MarlinFirmware/Marlin/commit/fd270ddc6c5b4d78437d590ae8066326850555d7)

This configuration is based on the following mods:

* [Gulfcoast Robotics All Metal Hotend Conversion Kit w/Polished Titanium Heatbreak](https://amzn.to/3rg7BvT)
* [WINSINN Dual Gear Extruder](https://amzn.to/3qgkBQC)
* [3dmakerengineering.com Tungsten Carbide 3D Printer Nozzle](https://www.3dmakerengineering.com/collections/3d-printer-nozzles/products/tungsten-carbide-3d-printer-nozzle?variant=14784857112631)
* [PTFE Teflon® Premium Bowden Tube](https://www.3dmakerengineering.com/collections/accessories/products/ptfe-teflon-premium-bowden-tube)
* [BL-Touch](https://amzn.to/384td6M)

Due to these changes I would recommend the following if you are using one of the compiled .bin files (see [releases](https://github.com/ChadDevOps/Marlin-Ender-3-Max/releases), otherwise update the config and compile locally:

- Tune your PID for the hotend
- Calibrate E-Steps for your extruder (default is 93 for Ender 3 printers, my config has higher e-steps due to the dual gear extruder). 
- Calibrate Probe Z-Offset 
- Remove the Z-stop if your X-axis stops before the probe can reach the bed

## Stock configs

I did not start using Marlin until recently (3/3/2021). The first month of my printer experience was using Klipper. Due to this, I do not have a 'stock' config as I have made several upgrades to the printer. If anyone wants to take a crack at it, please do so. Fork the https://github.com/MarlinFirmware/Configurations repo, create a branch and do your pull request. Leave out BL-Touch settings/etc (e.g. just the stock printer).  

## Cura Start Code

After leveling the bed, and saving it to EEPROM (`M500`), add `M420 S1` after `G28` in your start code. You do not need to run a bed-level before every print. This will load the mesh if you have already leveled your bed. Also pre-heat the bed before leveling due to expansion.

```
; Ender 3 Custom Start G-code
G92 E0 ; Reset Extruder

M140 S{material_bed_temperature_layer_0} ;Start heating bed
M190 S{material_bed_temperature_layer_0} ;Wait for bed to reach temp before proceeding

G28 ; Home all axes
M104 S{material_print_temperature_layer_0} ;Start heating extruder

; BED_MESH_PROFILE LOAD=default ; klipper load mesh
;G29 ; Auto bed leveling
M420 S1 ; Load bed mesh - marlin - NOTE: Level bed first and SAVE - issue M500 after leveling, then print.

G1 Z2.0 F3000 ; Move Z Axis up little to prevent scratching of Heat Bed
M109 S{material_print_temperature_layer_0} ;Wait for extruder to reach temp before proceeding
G1 X0.1 Y20 Z0.3 F5000.0 ; Move to start position

G1 X0.1 Y300.0 Z0.3 F1500.0 E30 ; Draw the first line
G1 X1.3 Y300.0 Z0.3 F5000.0 ; Move to side a little
G1 X1.3 Y20 Z0.3 F1500.0 E50 ; Draw the second line
G92 E0 ; Reset Extruder
G1 Z2.0 F3000 ; Move Z Axis up little to prevent scratching of Heat Bed
G1 X5 Y20 Z0.3 F5000.0 ; Move over to prevent blob squish

; Klipper stuff 
;SET_VELOCITY_LIMIT SQUARE_CORNER_VELOCITY=1 ACCEL=2000 ACCEL_TO_DECEL=500

;M900 K0.66 ;Linear Advance - enable if using, make sure to calibrate first
```

## The Printer

<img src="./Ender-3-Max.jpeg?raw=true" width="250">

It's still a work in progress. 

The stabilizers are from [Befenybay](https://amzn.to/3rhibTq). They are NOT designed for the Ender 3 Max. If you look at the bottom left you'll see I flipped the bracket. It sits below the bottom of the printer. You'll need to lift the printer, or put stabilizer feet under the 6 pads (which gives it enough height). I added the bars as I was seeing wobble with fast printing speeds.  Or design your own bracket and use a tap (the bolts are M5).
