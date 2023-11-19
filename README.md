# Intelligented Default Bed-Mesh
## Installation

SSH into you pi and run:
```
cd ~
wget -O - https://raw.githubusercontent.com/LynxCrew/Intelligent_Default_Mesh/main/install.sh | bash
```

then add this to your moonraker.conf:
```
[update_manager intelligent_default_mesh]
type: git_repo
channel: dev
path: ~/intelligent_default_mesh
origin: https://github.com/LynxCrew/Intelligent_Default_Mesh.git
managed_services: klipper
primary_branch: main
install_script: install.sh
```
