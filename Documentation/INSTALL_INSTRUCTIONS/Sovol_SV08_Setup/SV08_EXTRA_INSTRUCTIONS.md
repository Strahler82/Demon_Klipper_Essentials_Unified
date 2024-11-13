
If you feel these macros are valuable enough to download & use please consider hitting that "sponsor this project" button or the Ko-Fi image below or at https://ko-fi.com/3dprintdemon & buying me a beer/coffee. Its always very much appreciated & anything you do send goes towards helping me continue putting my ideas out there for the whole 3D printing community. Sending even a little makes a difference! Thank you & happy printing!!

[<img width="171" alt="kofi_s_tag_dark" src="https://github.com/3DPrintDemon/Demon_Klipper_Essentials_Unified/assets/122202359/08473899-563b-4b4d-9409-5e6602d6ec44">](https://ko-fi.com/3dprintdemon)

****************************************************************************************************************************
### IF YOU RAN V1.0-V2.8 BE SURE TO UPDATE YOUR SLICER'S START GCODE OR NEW FEATURES WONT WORK!
**Also you must update ALL the macro files as this new version will NOT work correctly with old files!**
****************************************************************************************************************************

- ### [UPDATING FROM A PREVIOUS VERSION CLICK HERE!!](https://github.com/3DPrintDemon/Demon_Klipper_Essentials_Unified/blob/main/Other_Files/Version_Config_Changes/V2.9_Config_Changes.md)


****************************************************************************************************************************

### Orca Slicer

Make sure you have the latest 2.2.0 (beta2 or later) version of Orca Slicer DO NOT USE ORCA SLICER DOT NET!!! BAD!!!

This link GOOD!

- https://github.com/SoftFever/OrcaSlicer/releases

******************************

# IF YOU'RE USING A SOVOL SV08 PRINTER! 

![DEMON_Display_Menu](https://github.com/user-attachments/assets/eb7965ac-fcd2-4fd4-9487-7cbcaf2828e7)

BE SURE TO INSTALL THE STUFF YOU NEED TO INSTALL!!!

- https://github.com/3DPrintDemon/SV08/releases/tag/v1.0
- https://github.com/3DPrintDemon/KAMP_LiTE/releases/tag/v1.0
- https://github.com/3DPrintDemon/Voron-Stealthburner/blob/main/Firmware/RGB_LEDs.cfg
- https://github.com/3DPrintDemon/Non_Blocking_Wait/releases/tag/Heat_Soak_Timers_V1.0
  
###### Note: These files are required for the macro pack to work correctly. Install even if you dont have any LEDs & set a dummy pin.

## OPTIONAL INSTALL for stock system!!
![DEMON_Armbian_Boot](https://github.com/user-attachments/assets/fb98da30-1f13-4490-9dfd-861b4a979488)

- [HDMI DEMON Boot Screen!](https://github.com/3DPrintDemon/Demon_Klipper_Essentials_Unified/blob/main/Documentation/INSTALL_INSTRUCTIONS/DEMON_Armbian_HDMI_Boot_Screen/DEMON_HDMI_Boot_Screen_Install.md)

## Other Great Upgrade Options For All SV08's
Here is my SV08 nozzle cleaning & purge bucket!

- https://www.printables.com/model/873006-sovol-sv08-silicone-nozzle-cleaner-purge-bucket-mi

Anti-vibration feet

- https://www.printables.com/model/867321-sovol-sv08-low-profile-vibration-isolation-feet

M12 Probe mount

- https://www.printables.com/model/870302-sovol-sv08-m12-inductive-probe-mount-lj12a3-4-zax

DIN rail mount

 - https://www.printables.com/model/867676-sovol-sv08-recessed-din-rail-mount-bracket

****************************************************************************************************************************

# Edit your printer.cfg 

Be sure your `printer.cfg` file `[extruder]` section contains...

```
max_extrude_only_velocity: 15
```
This is used by the load/unload macros & is converted into feedrate of mm/s & is limited to 20

```
[idle_timeout]
gcode:
    _DEMON_IDLE_TIMEOUT
timeout: 3600
```

This will call the new Demon timeout macro after 1 hour & execute the functions of your choice in the demon_user_settings file. You csan set the time (seconds) to anything you like.


YOU MUST, I REPEAT MUST DISABLE ALL THE SOVOL MACROS BY COMMENTING OUT THE macro.cfg FILE INCLUDE IN THE printer.cfg FILE! 
To do this use a # at the start of the line like this:
```
# [include Macro.cfg]
```
Then set your fans like this in the printer.cfg file:
```
[multi_pin fan_pins]
pins: extra_mcu:PA7, extra_mcu:PB1

[fan]
pin:multi_pin: fan_pins
max_power: 1.0
shutdown_speed: 0
kick_start_time: 0.5
off_below: 0.10
```
Comment out the stock entry do not delete it, it must look like this:
```
# [fan_generic fan0] # back model cooling fan
# pin: extra_mcu:PA7
# max_power: 1.0

# [fan_generic fan1] # front model cooling fan
# pin: extra_mcu:PB1
# max_power: 1.0
```

Rename the `[output_pin Main_LED]` section & change the cycle_time. As the stock value of `5` is incorrect. 

```
[output_pin Printer_Lights]
pin:PA3
pwm: 1
value:1
cycle_time: 0.01
shutdown_value: 0
```

Be sure to comment out the Sovol [homing_override] section if you want to use the demon_homing_control file. If not leave this commented in & disable the demon file by commenting out its full content.

```
# [homing_override]
# gcode:
#    {% if not 'Z' in params and not 'Y' in params and 'X' in params %}
#      G28 X
#      G0 X348 F1200
#    {% elif not 'Z' in params and not 'X' in params and 'Y' in params %}
#      G28 Y
#      G0 Y360  F1200
#    {% elif not 'Z' in params and 'X' in params and 'Y' in params %}
#      G28 Y
#      G0 Y360  F1200
#      G4 P2000
#      G28 X
#      G0 X348  F1200
#    {% elif 'Z' in params and not 'X' in params and not 'Y' in params %}
#      G90
#      G0  X191 Y165 F3600
#      G28 Z
#      G0  Z10 F600
#    {% else %}
#      G90
#      G0 Z5 F300
#      G28 Y
#      G0 Y360  F1200
#      G4 P2000
#      G28 X
#      G0 X348  F1200
#      G90
#      G0  X191 Y165 F3600
#      G28 Z
#      G0  Z10 F600
#    {% endif %}
# axes: xyz
# set_position_z: 0
```

Now... There's one more thing I really recommend doing & that's modifying the settings below for your meshing & probing sections! This will improve the accuracy of your system when using the probe. Do check these values against your current values & DO NOT overwrite your stock sections!! Comment them out & paste these in below them so you can always go back if you need to!

THIS FIRST SECTION IS OPTIONAL TO SLOW THE MACHINE DOWN - as the stock settings are waaaaaaaaay too fast & are unrealistic.
```
[printer] 
kinematics: corexy
max_velocity: 700
max_accel: 8200
max_accel_to_decel: 4500
max_z_velocity: 20
max_z_accel: 500
square_corner_velocity: 5.0
```
 
```
[probe]
pin: extra_mcu:PB6
x_offset: -17
y_offset: 10
# z_offset: 0 
speed: 5
samples: 3
samples_result: median
sample_retract_dist: 5.0
lift_speed: 50
samples_tolerance: 0.01
samples_tolerance_retries: 10
```
 
```
[bed_mesh]
speed: 350                   
horizontal_move_z: 5         
mesh_min: 10,10              
mesh_max: 333,340            
probe_count: 11,11             
algorithm: bicubic   
bicubic_tension: 0.4
split_delta_z: 0.01
mesh_pps:3,3
adaptive_margin: 5
fade_start: 0
fade_end: 10
fade_target: 0
```
 
```
[quad_gantry_level]
gantry_corners:              
	-60,-10
	410,420
points:
	36,10
	36,320
	346,320
	346,10
speed: 350                   
horizontal_move_z: 5       
retry_tolerance: 0.01      
retries: 5                  
max_adjust: 10  

```

## Timelapse Use

If you wish to continue to use Timelapse be sure to add this back in your slicer's `Before Layer Change` gcode box.
```
TIMELAPSE_TAKE_FRAME
```

## Mainline users

You will need to add these sections to your printer.cfg if they're not defined already. 

```
[board_pins]
aliases:
    # EXP1 header
    EXP1_1=PA8,   EXP1_2=PC9,
    EXP1_3=PA10,  EXP1_4=PA9,
    EXP1_5=PC11,  EXP1_6=PC10,
    EXP1_7=PD8,   EXP1_8=PC12,
    EXP1_9=<GND>, EXP1_10=<5V>,

    # EXP2 header
    EXP2_1=PB14,  EXP2_2=PB13,
    EXP2_3=PC7,   EXP2_4=PB12,
    EXP2_5=PC6,   EXP2_6=PB15,
    EXP2_7=PC8,   EXP2_8=<RST>,
    EXP2_9=<GND>, EXP2_10=<5V>

[display]                    
lcd_type: uc1701
cs_pin: EXP1_3
a0_pin: EXP1_4
rst_pin: EXP1_5
encoder_pins: ^EXP2_5, ^EXP2_3
click_pin: ^!EXP1_2
contrast: 63
spi_software_miso_pin: EXP2_1
spi_software_mosi_pin: EXP2_6
spi_software_sclk_pin: EXP2_2

[neopixel Screen_Colour]     
pin: EXP1_6
chain_count: 3
color_order: RGB             
initial_RED: 0.5             
initial_GREEN: 0.4           
initial_BLUE: 0.7  
```


****************************************************************************************************************************





I hope this macro pack makes a nice difference to your printing life, don't forget, if you feel its valuable enough to use please consider hitting that "sponsor this project" button & buying me a beer/coffee. Its always very much appreciated. Thank you & happy printing!!

[<img width="171" alt="kofi_s_tag_dark" src="https://github.com/3DPrintDemon/Demon_Klipper_Essentials_Unified/assets/122202359/08473899-563b-4b4d-9409-5e6602d6ec44">](https://ko-fi.com/3dprintdemon)

******************************

- [Head on to General Setup For All Printers](https://github.com/3DPrintDemon/Demon_Klipper_Essentials_Unified/blob/main/Documentation/INSTALL_INSTRUCTIONS/General%20_Setup_For_All_Printers/INSTALL_INSTRUCTIONS.md)
