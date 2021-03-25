# LAB 4 WALKTHROUGH

Ran the synthesis, implementation, bitstream. Programmed into the device and the result can be seen in the below image.
![20210325_125036](https://user-images.githubusercontent.com/32800667/112512067-6aee0b80-8d69-11eb-924f-4b8edc71d84b.jpg)

In the .xdc file, add this code to use buttons. Can be changed as preferred:
```sh
set_property -dict { PACKAGE_PIN N17   IOSTANDARD LVCMOS33 } [get_ports { bt_clr }]; #IO_L9P_T1_DQS_14 Sch=btnc
set_property -dict { PACKAGE_PIN M18   IOSTANDARD LVCMOS33 } [get_ports { bt_plus }]; #IO_L4N_T0_D05_14 Sch=btnu
set_property -dict { PACKAGE_PIN P17   IOSTANDARD LVCMOS33 } [get_ports { bt_eq }]; #IO_L12P_T1_MRCC_14 Sch=btnl
set_property -dict { PACKAGE_PIN P18   IOSTANDARD LVCMOS33 } [get_ports { bt_sub }]; #IO_L9N_T1_DQS_D13_14 Sch=btnd
```

Add all of the code from the DSD repo.
* hexcalc.vhdl is responsible for programming the button functions (+, -, =, etc.)
* leddec16 is used for the display and leading zero suppression
	```sh anode <= "1110" WHEN dig = "00" and data /= X"0000" ELSE -- digit 0
	         "1101" WHEN dig = "01" and data(15 downto 4) /= X"000" ELSE -- digit 1
	         "1011" WHEN dig = "10" and data(15 downto 8) /= X"00" ELSE -- digit 2
	         "0111" WHEN dig = "11" and data(15 downto 12) /= X"0" ELSE -- digit 3
	         "1111"; ```
   ![20210325_125102](https://user-images.githubusercontent.com/32800667/112513608-e3a19780-8d6a-11eb-9d65-eca46de57bc0.jpg)
Display without leading zeros
