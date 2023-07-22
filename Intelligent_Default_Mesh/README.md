### Installation
1. Stellt sicher, dass ihr das BED_MESH Feature von Klipper nutzt und korrekt eingerichtet habt <br>
2. Ladet euch `Intelligent_Default_Mesh.zip` herunter <br>
3. Entpackt `Intelligent_Default_Mesh.zip` <br>
4. Ladet den Ordner `Intelligent_Default_Mesh` im Webinterface eurer Wahl hoch <br>
5. Fügt `[include Intelligent_Default_Mesh/_include.cfg]` in eurer printer.cfg hinzu <br>
6. Aktiviert falls ihr es nicht bereits habe die `[save_variables]` section in Klipper (https://www.klipper3d.org/Config_Reference.html#save_variables) <br>
7. Ersetzt den `BED_MESH_PROFILE LOAD=...` Aufruf in eurem START_PRINT Makro duch `BED_MESH_PROFILE LOAD=default INTELLIGENT=1` <br>
8. Optional: Falls ihr am Ende des Drucks das BED_MESH und den GCODE_OFFSET wieder entladen wollt, ruft einfach `BED_MESH_CLEAR INTELLIGENT=1` in eurem END_PRINT Makro auf


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