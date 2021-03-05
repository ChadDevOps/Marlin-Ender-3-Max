# Marlin-Ender-3-Max w/ BL-Touch
Ender 3 Max Configs for Marlin bugfix-2.0.x branch - [MarlinFirmware/Marlin@fd270dd](https://github.com/MarlinFirmware/Marlin/commit/fd270ddc6c5b4d78437d590ae8066326850555d7)

Update: The posted configration files are based on stock hotend and extruder. See [releases](https://github.com/ChadDevOps/Marlin-Ender-3-Max/releases) for a compiled version.

After applying a new firmware, it's best to run `M502` and `M500` to load and save the default settings.

```
Reset settings and save them to EEPROM

M502 ; reset
M500 ; saved
```

In order to use this config with the mods listed below, invert the following comments:

```
// INSINN Dual Gear Extruder
//#define DEFAULT_AXIS_STEPS_PER_UNIT   { 80, 80, 400, 139.28 }

// Stock Extruder
#define DEFAULT_AXIS_STEPS_PER_UNIT   { 80, 80, 400, 93 }
```
NOTE: I still recommend that you calibrate your E-Steps (139.28 above).

```    
    // [Gulfcoast Robotics]
    //#define DEFAULT_Kp  37.01
    //#define DEFAULT_Ki   5.19
    //#define DEFAULT_Kd  66.01
    // Creality Stock
    #define DEFAULT_Kp  21.39
    #define DEFAULT_Ki   1.56
    #define DEFAULT_Kd  73.30
```

* [Gulfcoast Robotics All Metal Hotend Conversion Kit w/Polished Titanium Heatbreak](https://amzn.to/3rg7BvT)
* [WINSINN Dual Gear Extruder](https://amzn.to/3qgkBQC)
* [3dmakerengineering.com Tungsten Carbide 3D Printer Nozzle](https://www.3dmakerengineering.com/collections/3d-printer-nozzles/products/tungsten-carbide-3d-printer-nozzle?variant=14784857112631)
* [PTFE Teflon® Premium Bowden Tube](https://www.3dmakerengineering.com/collections/accessories/products/ptfe-teflon-premium-bowden-tube)
* [BL-Touch](https://amzn.to/384td6M)

I would recommend the following as these can differ for every machine:

- Tune your PID for the hotend
- Tune your POD for the bed (optional)
- Calibrate E-Steps for your extruder (default is 93 for Ender 3 printers) 
- Calibrate Probe Z-Offset 
- Remove the Z-stop if your X-axis stops before the probe can reach the bed

https://youtu.be/qPDBNBgdW6o?t=680 - Dr Vax on building the Marlin Firmware with VS Code.

## Stock configs

I did not start using Marlin until recently (3/3/2021). The first month of my printer experience was using Klipper. Due to this, the config in this repo is my best guess for stock settings. Another user sent me their tuned hotend PID temps for the stock hotend. 

The offical marlin config repo does not have a stock Ender 3 Max yet, if you have experience in compiling marlin and have a stock printer without the BL-Touch, please fork the https://github.com/MarlinFirmware/Configurations repo, create a branch and do your pull request with the stock config. 

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
