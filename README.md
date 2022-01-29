# KP3S - STM32F407


Marlin 2.0.9.3 built for KP3S tia
OEM titan extruder
bltouch, babysteps etc.





###
; -- BL-TOUCH START GCODE --
G21                      ; metric values
G90                      ; absolute positioning
M82                      ; set extruder to absolute mode
M107 ; start with the fan off

; heat bed/hot-end sametime. the wait is after xy homing
M140 S{material_bed_temperature}; set bed temp, don't wait
M104 S{material_print_temperature} ; set nozzle, don't wait

; confirm BL-touch safety
M280 P0 S160             ; BL-Touch Alarm release
G4 P100 ; Delay for BL-Touch homing

; homing, bed temp before z to drip at edge
G28 X0 Y0                ; move X/Y to min endstops
M190 S{material_bed_temperature}; set bed temp (again) and wait
G28 Z0 ; move Z to min endstops

; reconfirm BL-touch safety
M280 P0 S160             ; BL-Touch Alarm realease
G4 P100 ; Delay for BL-Touch

; bed leveling: obmit to load prev mesh
;G29; Auto leveling
;M420 Z5 ; set LEVELING_FADE_HEIGHT
;M500 ; save data of G29 and M420

; load mesh
M420 S1 ; enable bed leveling

; add the "Intro Line"
G92 E0                   ;zero the extruded length
G1 Z2 F5000              ;move up slightly


G1 X20 Y2.1 Z0.3 F5000.0 ; Move to start position
G1 X150 Y2.1 Z0.3 F1500.0 E15 ; Draw the first line
G1 X150 Y2.4 Z0.3 F5000.0 ; Move to side a little
G1 X20 Y2.4 Z0.3 F1500.0 E30 ; Draw the second line

; prepare hot-end
G92 E0                   ; zero the extruded length
G1 F200 E3               ; extrude 3mm of feed stock

M117 Printing...

; -- end of BL-TOUCH START GCODE --

###
; KP3S Custom End G-code
G91 ;Relative positioning
G1 E-2 F2700 ;Retract a bit
G1 E-2 Z0.2 F2400 ;Retract and raise Z
G1 X5 Y5 F3000 ;Wipe out
G1 Z10 ;Raise Z more
G90 ;Absolute positioning

G1 X5 Y170 ;Present print
M106 S0 ;Turn-off fan
M104 S0 ;Turn-off hotend
M140 S0 ;Turn-off bed

M84 X Y E ;Disable all steppers but Z ;If T8x2 add Z
####

### cura offsets
-38
-29
20
34
16
1
apply extruder to gcode


Flow rate: 85.11