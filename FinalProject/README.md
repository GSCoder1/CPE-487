# Mouse Reader Final Project

**This code was largely based off of what Peter Ho's project and the sources he used**

**Purpose:** To be able to use a mouse on a display using a VGA cable and Nexys-A7-100T FPGA Board

**Important resources:** 
* Peter Ho's code: https://github.com/PeterHo8888/FPGA_Paint
* XDC Master for most FPGA Boards: https://github.com/Digilent/digilent-xdc
* PS/2 Protocol information: http://www.burtonsys.com/ps2_chapweske.htm

**Files Used**:
* vga_ctrl: used for vga display
* main: to assign the correct _out_ values for our variables defined in other files
* clk_wiz_0 & clk_wix_0_clk_wiz: used for clocking purposes. Very similar to files used in lab
* MouseCtl: used to edit mouse controls, such as scrolling, clicking, movement, x-y limits. Read comments to learn more about mouse bits
* MouseDisplay: edit color or pixels of mouse.
* Ps2Interface: for the ps/2 interface
* nexys4ddr.xdc: constraint file. Main things needed to make mouse work are clock signal, vga connector, USB HID. 

**Common Errors**
* "variable" is not declared: Check for typos as a variable does not match your declarations in port
* "Unspecified I/O standard: X out of Y logical ports use I/O standard...": your XDC file variables don't match your VHD files. 
