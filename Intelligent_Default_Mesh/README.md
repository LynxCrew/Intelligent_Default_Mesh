# Installation
Einfach diesen Ordner herunter laden, im Webinterface eurer Wahl hochladen und `[include Intelligent_Default_Mesh/_include.cfg]` in eure printer.cfg einfügen, den `BED_MESH_PROFILE LOAD=...` Aufruf in eurem START_PRINT Makro durch `BED_MESH_PROFILE LOAD=default INTELLIGENT=1` ersetzen und die `save_variables` section aktivieren <br>Standard:
```
[save_variables]
filename: ~/database.cfg
```


<br>
<br>

# Setup
Im `Intelligent-Default-Mesh` Ordner findet ihr eine Datei die `variables.cfg` heißt. Dort ist ein Makro namems `INTELLIGENT_MESH_VARIABLES` <br>
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
Wenn ihr nun `SET_DEFAULT_MESH DEFAULT_MESH=Glas_Platte` in die Konsole eingebt, wird `Glas_Platte` als intelligentes default mesh hinterlegt.
Wenn ihr nun `BED_MESH_PROFILE LOAD=default INTELLIGENT=1` in der Konsole aus führt, wird statt dem Profil default, das Profil Glas_Platte geladen und der Z-Offset auf 0.05 gesetzt.