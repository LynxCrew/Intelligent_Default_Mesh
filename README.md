# Intelligented Default Bed-Mesh

## Intention
You can slice your model once and then print it on any build-plate you want without having to reslice it

## Installation
SSH into you pi and run:
```
cd ~
wget -O - https://raw.githubusercontent.com/LynxCrew/Intelligent_Default_Mesh/master/install.sh | bash
```

then add this to your moonraker.conf:
```
[update_manager intelligent_default_mesh]
type: git_repo
channel: dev
path: ~/intelligent_default_mesh
origin: https://github.com/LynxCrew/Intelligent_Default_Mesh.git
managed_services: klipper
primary_branch: master
install_script: install.sh
```

you also need to add
```
[save_variables]
filename: ~/database.cfg
```
to your printer.cfg

## What it does:
You can persistently save which mesh you are currently using by calling `SET_DEFAULT_MESH DEFAULT_MESH=<name>`.
If you call `BED_MESH_PROFILE LOAD=default INTELLIGENT=1 TEMPERATURE_EXTRUDER={EXTRUDER_TEMP} TEMPERATURE_BED={BED_TEMP} FILAMENT_PROFILE='{FILAMENT_PROFILE}'`, instead of the profile called default, the profile you saved will be loaded together with a predefined z-offset for that profile, temperature and filament.

## Config Reference:
```
[gcode_macro _MESH_VARIABLES]
```
Here you set the general offset to be applied for the given mesh name
`default: -0.1` would apply a z-offset of -0.1 if the mesh 'default' is loaded

```
[gcode_macro _TEMPERATURE_OFFSETS_EXTRUDER]
```
Here you can set different offsets for different Extruder temperatures
The lowest temp will act as a lower bound (if the given temp is below that, the offset will still be used, so lets say you define 210: -0.1 and print at 200C, you will still get -0.1)
Similar the highest temp will act as an upper bound
If the printing temp is between two specified temperatures, the script will do a linear interpolation

```
[gcode_macro _TEMPERATURE_OFFSETS_BED]
```
Same as above, just for the bed

```
[gcode_macro _FILAMENT_OFFSETS]
```
Filament profile specific offsets, set the name of the filament profile as the key and the offset will be used when the profile name is passed to the command

```
[gcode_macro _MAX_BED_TEMPS]
```
Can be used to specify max temps for build plates and check them in start_print for example
