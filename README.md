# Intelligented Default Bed-Mesh

### Wozu?
Falls ihr verschiedene Druckoberflächen verwendet (texturiert und glatt zum Beispiel) werdet ihr sicher schon fest gestellt haben, dass ihr für jede Oberfläche ein eigenes Mesh und vielleicht sogar einen eigenen Z-Offset benötigt.
Aktuelle Systeme dafür hinterlegen diese Informationen meist in der gesliceten Datei oder im START_PRINT Makro, was beides umständlich ist, weil man nun, falls man ein Objekt auf einer anderen Oberfläche drucken will es entweder neu slicen muss, oder das START_PRINT anpassen und dann die Firmware neu starten muss.
Wir haben nun ein System, was es euch erlaubt, einfach zu jedem gespeicherten BED_MESH ganz einfach einen Z-Offset zu definieren und zu hinterlegen, welche Oberfläche ihr nutzt.
Zum ändern der hinterlegten Oberfläche muss nur ein Makro ausgeführt werden, ohne nervige Neustarts, neu slicen oder sonstige Dinge. <br>
Und das beste daran? Die Informationen bleiben sogar über einen Neustart hinweg erhalten.
<br>
<br>

### Installation
# LynxCrew Klicky Macros

SSH into you pi and run:
```
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

then add `[include Klicky/_include.cfg]` and `[include Variables/klicky_variables.cfg]` to your printer.cfg.



<br>
<br>

### Setup
Im `Intelligent-Default-Mesh` Ordner findet ihr eine Datei die `variables.cfg` heißt. Dort ist ein Makro namens `INTELLIGENT_MESH_VARIABLES` <br>
Standard: 
```
[gcode_macro INTELLIGENT_MESH_VARIABLES]
variable_surfaces:
  { 'TEXTURED' : 0.00,
    'SMOOTH' : 0.00 }

gcode:
  # Do Nothing
```

Hier könnt ihr die Namen eurer MESHs eintragen und die passenden Z-Offsets dazu 
Also z.B. falls ihr ein mesh habt, was 'Glas_Platte' heißt, mit einem Offset von 0.05, dann würde das so aussehen:
```
[gcode_macro INTELLIGENT_MESH_VARIABLES]
variable_surfaces:
  { 'Glas_Platte' : 0.05 }

gcode:
  # Do Nothing
```
Falls nun noch ein Mesh Glas_Platte2 mit 0.10mm Offset dazu kommt, habt müsst ihr dann das eintragen:
```
[gcode_macro INTELLIGENT_MESH_VARIABLES]
variable_surfaces:
  { 'Glas_Platte' : 0.05,
    'Glas_Platte2' : 0.10 }

gcode:
  # Do Nothing
````
Wenn ihr nun `SET_DEFAULT_MESH DEFAULT_MESH=Glas_Platte` in die Konsole eingebt, wird `Glas_Platte` als intelligentes default mesh hinterlegt. <br>
Vereinfachen lässt sich das ganze, indem man ein Makro hinterlegt, was diesen Befehl auf ruft. <br>
Beispielsweise: 
```
[gcode_macro SET_DEFAULT_MESH_TEXTURED]
gcode:
  SET_DEFAULT_MESH DEFAULT_MESH='TEXTURED'
```
(In `examples.cfg` sind zwei Beispiele dafür hinterlegt)
Wenn ihr nun `BED_MESH_PROFILE LOAD=default INTELLIGENT=1` in der Konsole aus führt, wird statt dem Profil default, das Profil Glas_Platte geladen und der Z-Offset auf 0.05 gesetzt.
