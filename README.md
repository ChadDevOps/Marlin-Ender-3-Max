# Marlin-Ender-3-Max
Ender 3 Max Configs for Marlin bugfix-2.0.x branch. 

This configuration is based on the following mods:

* [Gulfcoast Robotics All Metal Hotend Conversion Kit w/Polished Titanium Heatbreak](https://amzn.to/3rg7BvT)
* [WINSINN Dual Gear Extruder](https://amzn.to/3qgkBQC)
* [3dmakerengineering.com Tungsten Carbide 3D Printer Nozzle](https://www.3dmakerengineering.com/collections/3d-printer-nozzles/products/tungsten-carbide-3d-printer-nozzle?variant=14784857112631)
* [PTFE Teflon® Premium Bowden Tube](https://www.3dmakerengineering.com/collections/accessories/products/ptfe-teflon-premium-bowden-tube)
* [BL-Touch](https://amzn.to/384td6M)

Due to these changes I would recommend the following if you are using the compiled .bin file, otherwise update the config and compile locally:

- Tune your PID for the hotend
- Calibrate E-Steps for your extruder
- Calibrate Probe Z-Offset 
- Remove the Z-stop if your X-axis stops before the probe can reach the bed

