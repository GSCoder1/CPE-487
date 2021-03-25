# Lab 5 Walkthrough

A picture of the setup. Headphone needed to be plugged into the out port on the Pmod.
![20210323_130040](https://user-images.githubusercontent.com/32800667/112514276-865a1600-8d6b-11eb-9946-50a856410e7b.jpg)


* The dac_if (digital to analog) module takes 16-bit parallel stereo data and converts it to the serial format required by the digital to analog converter.
* The tone module generates a signed triangular wave (a tone) at a sampling rate of 48.8 KHz.
* The wail module creates an instance of the module tone and then modulates the pitch up and down to produce a “wailing” siren
* The siren module is the top level.
* More info found in the DSD repo.

## Modifications:
* In order to create a square wave instead of a triangle wave when BTNU is pressed, add this to the .xdc file:
```sh 
set_property -dict { PACKAGE_PIN M18   IOSTANDARD LVCMOS33 } [get_ports { BTN0 }]; 
```
* and add this to tone.vhdl:
```sh WITH quad SELECT
	   data_tri <= index WHEN "00", -- 1st quadrant
	        16383 - index WHEN "01", -- 2nd quadrant
	        0 - index WHEN "10", -- 3rd quadrant
	        index - 16383 WHEN OTHERS; -- 4th quadrant
	WITH quad SELECT
        data_sq <= to_signed(16383, 16) when "00", 
        to_signed(-16393, 16) when "01",
        to_signed(16383, 16) when "10", 
        to_signed(-16383, 16) when others;
    tone_select: process
    begin
    if btn_press = '1' then
        data <= data_sq;
    else 
        data <= data_tri;
    end if; ```
    
The siren will remain triangular until the button is pressed.

* To change the wailing speed, add these switches to the .xdc file:
``sh 
set_property -dict { PACKAGE_PIN J15   IOSTANDARD LVCMOS33 } [get_ports { SW0 }]; #IO_L24N_T3_RS0_15 Sch=sw[0]
set_property -dict { PACKAGE_PIN L16   IOSTANDARD LVCMOS33 } [get_ports { SW1 }]; #IO_L3N_T0_DQS_EMCCLK_14 Sch=sw[1]
set_property -dict { PACKAGE_PIN M13   IOSTANDARD LVCMOS33 } [get_ports { SW2 }]; #IO_L6N_T0_D08_VREF_14 Sch=sw[2]
set_property -dict { PACKAGE_PIN R15   IOSTANDARD LVCMOS33 } [get_ports { SW3 }]; #IO_L13N_T2_MRCC_14 Sch=sw[3]
set_property -dict { PACKAGE_PIN R17   IOSTANDARD LVCMOS33 } [get_ports { SW4 }]; #IO_L12N_T1_MRCC_14 Sch=sw[4]
set_property -dict { PACKAGE_PIN T18   IOSTANDARD LVCMOS33 } [get_ports { SW5 }]; #IO_L7N_T1_D10_14 Sch=sw[5]
set_property -dict { PACKAGE_PIN U18   IOSTANDARD LVCMOS33 } [get_ports { SW6 }]; #IO_L17N_T2_A13_D29_14 Sch=sw[6]
set_property -dict { PACKAGE_PIN R13   IOSTANDARD LVCMOS33 } [get_ports { SW7 }]; #IO_L5N_T0_D07_14 Sch=sw[7]
```
* Add wailing speed to the siren.vhdl architecture:
``` signal wail_speed: UNSIGNED (7 downto 0) := to_unsigned (8, 8); -- sets wailing speed  ```
* And finish by modifying the wailing speed in siren.vhdl
```sh 
wail_speed(0) <= SW0;
 wail_speed(1) <= SW1;
 wail_speed(2) <= SW2;
 wail_speed(3) <= SW3;
 wail_speed(4) <= SW4;
 wail_speed(5) <= SW5;
 wail_speed(6) <= SW6;
 wail_speed(7) <= SW7;
 ```
 
 
