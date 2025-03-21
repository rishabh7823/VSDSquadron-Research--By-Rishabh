# VSDSquadron-Research--By-Rishabh

## TASK 1 

### Objective 

to understand and document the provided Verilog code, create the necessary PCF file, and integrate the design with the VSDSquadron FPGA Mini board using the provided [datasheet](https://www.vlsisystemdesign.com/wp-content/uploads/2025/01/VSDSquadronFMDatasheet.pdf). (install tools as explained in datasheet)

### Content

#### Step 1 Understanding the Verilog Code

  1.  This could be understood and complete with help of this [link](https://github.com/thesourcerer8/VSDSquadron_FM/blob/main/led_blue/top.v)



  <details><summary>2.Review the module declaration 
</summary>
 * Inputs and Outputs: 
	  • output wire led_red, led_blue, led_green: Declares three output signals, likely connected to LEDs. The wire keyword indicates they are simple connections, not memory elements. 
	  • input wire hu_clk: Declares an input signal, likely a clock signal from a hardware oscillator. 
	  • output wire testwire: Another output signal, its purpose is revealed later. 

  * Internal Signals: 
	  • wire int_osc: Declares an internal wire, likely intended as a clock signal. 
	  • reg [27:0] frequency_counter_i: Declares a 28-bit register named frequency_counter_i. Registers store values and are used for counting or storing state.

  * Assignment: 
  	• assign testwire = frequency_counter_i[5]; This line continuously assigns the value of the 6th bit (index 5) of the frequency_counter_i register to the testwire output.
  * Always Block: 
	  • always @(posedge int_osc) begin ... end: This block describes sequential logic that executes on the rising edge of the int_osc signal. 
	  • frequency_counter_i <= frequency_counter_i + 1'b1: Inside the block, the frequency_counter_i register is incremented by 1 on each rising edge of int_osc. 1'b1 represents a 1-bit binary value of 1.
     
  * Internal Oscillator Configuration: 
	  • SB_HFOSC: This seems to be a module or macro representing a high-frequency oscillator. 
	  • #(.CLKHF_DIV("0b10")): This part configures the clock divider for the oscillator. "0b10" likely sets the division factor to 2 (binary representation). 
	  • u_SB_HFOSC: This is the instance name of the oscillator module. 
	  • (.CLKHFPU(1'b1), .CLKHFEN(1'b1), .CLKHF(int_osc)): This connects signals to the oscillator instance: 
		• .CLKHFPU(1'b1): Probably enables the clock pull-up. 
		• .CLKHFEN(1'b1): Likely enables the clock output. 
		• .CLKHF(int_osc): Connects the oscillator output to the signal int_osc. 

  * RGB Primitive Instantiation: 
	  • SB_RGBA_DRV: This is likely a module for controlling an RGB LED. 
	  • RGB_DRIVER: This is the instance name of the RGB driver module. 
    • RGBLEDEN (1'b1): Enables the RGB LED. 1'b1 represents a 1-bit value set to 1 (high). 
	  • RGB0PWM (1'b0), // red: Controls the pulse-width modulation (PWM) for the red component of the RGB LED. 1'b0 means it's initially off. 
	  • RGB1PWM (1'b0), // green: Controls the PWM for the green component, also initially off. • RGBLEDEN (1'b1): Enables the RGB LED. 1'b1 represents a 1-bit value set to 1 (high). 
	  • RGB0PWM (1'b0), // red: Controls the pulse-width modulation (PWM) for the red component of the RGB LED. 1'b0 means it's initially off. 
	  • RGB1PWM (1'b0), // green: Controls the PWM for the green component, also initially off. 
	  • RGB2PM (1'b1), // blue: Controls the PWM for the blue component, initially on. 
	  • CURREN (1'b1): Enables the current source for the LED. 
	  • RGB0 (led_red), RGB1 (led_green), RGB2 (led_blue): Connects the module's internal signals to external signals for the red, green, and blue LEDs. 

  * Parameter Overrides: 
	  • //Actual Hardware connection: This comment suggests the following lines configure hardware-specific parameters. 
	  • defparam RGB_DRIVER.RGB0_CURRENT = "0b000001";: Sets the current for the red LED to a binary value of 000001. 
	  • defparam RGB_DRIVER.RGB1_CURRENT = "0b000001";: Sets the current for the green LED. 
	  • defparam RGB_DRIVER.RGB2_CURRENT = "0b000001";: Sets the current for the blue LED. 
  </details>  
