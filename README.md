# Marlin-Ender-3-Max
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

## The Printer

<img src="./Ender-3-Max.jpeg?raw=true" width="250">

It's still a work in progress. 

The stabilizers are from [Befenybay](https://amzn.to/3rhibTq). They are NOT designed for the Ender 3 Max. If you look at the bottom left you'll see I flipped the bracket. It sits below the bottom of the printer. You'll need to lift the printer, or put stabilizer feet under the 6 pads (which gives it enough height). I added the bars as I was seeing wobble with fast printing speeds.  Or design your own bracket and use a tap (the bolts are M5).
