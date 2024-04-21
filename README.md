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
You can persistently save which mesh you are currently using by calling SET_DEFAULT_MESH DEFAULT_MESH=<name>.
If you call BED_MESH_PROFILE LOAD=default INTELLIGENT=1, instead of the profile called default, the profile you saved will be loaded together with a predefined z-offset.
