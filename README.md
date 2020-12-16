Took me forever to figure this out and calibrate perfectly. Had to reflash octoprint on my raspberrypi4 twice, #reinstall klipper a few times as well. Kept receiving a no serial-id error. Eventually was succesful and now have klipper firmware running on my Ender 3 Pro w/ SKR mini v2.0 and a BL-touch clone. Prints are looking pretty great and hitting 100mm/s without too much quality loss. 

Running a direct drive and hotend from MicroSwiss-- mostly hatchbox PLA, but also some TPU every now and then. I also used the cool custom menu options from the klipper examples repository. Run Probe_Calibrate in terminal to setup your Z offset and calibrate your e-steps. Hopefully this config file will be helpful to anyone that's struggling out there. 

##########################
# happy printing
##########################


[stepper_x]
step_pin: PB13
dir_pin: !PB12
enable_pin: !PB14
step_distance: .0125
endstop_pin: ^PC0
position_endstop: 0
position_max: 235
homing_speed: 50

[tmc2209 stepper_x]
uart_pin: PC11
tx_pin: PC10
uart_address: 0
microsteps: 16
run_current: 0.580
hold_current: 0.500
stealthchop_threshold: 250

[stepper_y]
step_pin: PB10
dir_pin: !PB2
enable_pin: !PB11
step_distance: .0125
endstop_pin: ^PC1
position_endstop: 0
position_max: 235
homing_speed: 50

[tmc2209 stepper_y]
uart_pin: PC11
tx_pin: PC10
uart_address: 2
microsteps: 16
run_current: 0.580
hold_current: 0.500
stealthchop_threshold: 250

[stepper_z]
step_pin: PB0
dir_pin: PC5
enable_pin: !PB1
step_distance: .0025
endstop_pin:probe: z_virtual_endstop
position_max: 250
position_min: -5

[tmc2209 stepper_z]
uart_pin: PC11
tx_pin: PC10
uart_address: 1
microsteps: 16
run_current: 0.580
hold_current: 0.500
stealthchop_threshold: 5

[extruder]
step_pin: PB3
dir_pin: PB4
enable_pin: !PD2
max_extrude_only_distance: 900.0
max_extrude_cross_section: 50.0
step_distance: 0.00715768
nozzle_diameter: 0.400
filament_diameter: 1.750
heater_pin: PC8
sensor_type: EPCOS 100K B57560G104F
sensor_pin: PA0
control: pid
pid_Kp: 21.527
pid_Ki: 1.063
pid_Kd: 108.982
min_temp: 0
max_temp: 300

[tmc2209 extruder]
uart_pin: PC11
tx_pin: PC10
uart_address: 3
microsteps: 16
run_current: 0.650
hold_current: 0.500
stealthchop_threshold: 5

[heater_bed]
heater_pin: PC9
sensor_type: ATC Semitec 104GT-2
sensor_pin: PC3
control: pid
pid_Kp: 54.027
pid_Ki: 0.770
pid_Kd: 948.182
min_temp: 0
max_temp: 130

[heater_fan nozzle_cooling_fan]
pin: PC7

[fan]
pin: PC6

[mcu]
serial: /dev/serial/by-id/usb-Klipper_stm32f103xe_37FFD6054253373723641357-if00

[printer]
kinematics: cartesian
max_velocity: 300
max_accel: 3000
max_z_velocity: 5
max_z_accel: 100

[static_digital_output usb_pullup_enable]
pins: !PA14

[board_pins]
aliases:
    # EXP1 header
    EXP1_1=PB5, EXP1_3=PA9,   EXP1_5=PA10, EXP1_7=PB8, EXP1_9=<GND>,
    EXP1_2=PA15, EXP1_4=<RST>, EXP1_6=PB9,  EXP1_8=PB15, EXP1_10=<5V>

[display]
lcd_type: st7920
cs_pin: EXP1_7
sclk_pin: EXP1_6
sid_pin: EXP1_8
encoder_pins: ^EXP1_5, ^EXP1_3
click_pin: ^!EXP1_2

[output_pin beeper]
pin: EXP1_1


[display_glyph bed]
data:
  ................
  ................
  ................
  ................
  ................
  ................
  ................
  ................
  ................
  ................
  ...**********...
  ..*..........*..
  .*............*.
  *..............*
  ****************
  ................

[display_glyph bed_heat1]
data:
  ................
  ......*...*.....
  ......*...*.....
  .....*...*......
  .....*...*......
  ......*...*.....
  ......*...*.....
  .....*...*......
  .....*...*......
  ................
  ...**********...
  ..*..........*..
  .*............*.
  *..............*
  ****************
  ................

[display_glyph bed_heat2]
data:
  ................
  .....*...*......
  .....*...*......
  ......*...*.....
  ......*...*.....
  .....*...*......
  .....*...*......
  ......*...*.....
  ......*...*.....
  ................
  ...**********...
  ..*..........*..
  .*............*.
  *..............*
  ****************
  ................

[display_glyph feedrate]
data:
  ................
  ................
  ................
  ......******....
  ....**********..
  ...****....****.
  ..***........**.
  .***..........*.
  .**..*..........
  ***...*.........
  **.....*........
  **......**......
  **......***.....
  **.......**.....
  ................
  ................

[bltouch]
sensor_pin: ^PC2
control_pin: PA1
x_offset: -45.5
y_offset: -03
#z_offset: 0
pin_move_time: 0.4
speed: 20

[bed_mesh]
speed: 100
horizontal_move_z: 8
mesh_min: 25, 25
mesh_max: 150, 210
probe_count: 5
fade_start: 0
mesh_pps: 2, 2
algorithm: bicubic
bicubic_tension: 0.2
move_check_distance: 5
split_delta_z: .025

[gcode_macro G29]
gcode:
    BED_MESH_CLEAR
    BED_MESH_CALIBRATE
    BED_MESH_PROFILE SAVE=default
    BED_MESH_PROFILE LOAD=default


[safe_z_home]
home_xy_position: 145,115
speed: 80.0
z_hop: 10.0
z_hop_speed: 10.0

[virtual_sdcard]
path: ~/.octoprint/uploads/


#*# <---------------------- SAVE_CONFIG ---------------------->
#*# DO NOT EDIT THIS BLOCK OR BELOW. The contents are auto-generated.
#*#
#*# [bltouch]
#*# z_offset = 1.690
#*#
#*# [bed_mesh default]
#*# version = 1
#*# points =
#*# 	  -0.140000, -0.105000, -0.097500, -0.082500, -0.110000
#*# 	  -0.165000, -0.152500, -0.160000, -0.147500, -0.162500
#*# 	  -0.130000, -0.110000, -0.127500, -0.120000, -0.162500
#*# 	  -0.067500, -0.087500, -0.120000, -0.137500, -0.175000
#*# 	  -0.075000, -0.062500, -0.085000, -0.097500, -0.145000
#*# x_count = 5
#*# y_count = 5
#*# mesh_x_pps = 2
#*# mesh_y_pps = 2
#*# algo = bicubic
#*# tension = 0.2
#*# min_x = 25.0
#*# max_x = 150.0
#*# min_y = 25.0
#*# max_y = 210.0
