# VSDSquadron-Research--By-Rishabh

## TASK 1 

### OBJECTIVE

to understand and document the provided Verilog code, create the necessary PCF file, and integrate the design with the VSDSquadron FPGA Mini board using the provided [datasheet](https://www.vlsisystemdesign.com/wp-content/uploads/2025/01/VSDSquadronFMDatasheet.pdf). (install tools as explained in datasheet)

### CONTANT

### STEP 1 Understanding the Verilog Code
<details>
	
##### 1.This could be understood and complete with help of this [link](https://github.com/thesourcerer8/VSDSquadron_FM/blob/main/led_blue/top.v)
   
##### 2.Review the module declaration 

 
 * Inputs and Outputs: 

   	  • output ``` wire led_red, led_blue, led_green``` : Declares three output signals, likely connected to LEDs. The wire keyword indicates they are simple connections, not memory elements. 

  	  • input wire ```hu_clk```: Declares an input signal, likely a clock signal from a hardware oscillator.
   
  	  • output wire ```testwire```: Another output signal, its purpose is revealed later. 

  * Internal Signals:
    
	  • wire ```int_osc```: Declares an internal wire, likely intended as a clock signal. 

   	  • reg [27:0] ```frequency_counter_i```: Declares a 28-bit register named ```frequency_counter_i```. Registers store values and are used for counting or storing state.

  * Assignment:
     
  	• assign ```testwire``` = ```frequency_counter_i```[5]; This line continuously assigns the value of the 6th bit (index 5) of the ```frequency_counter_i``` register to the testwire output.
  
  * Always Block: 

   	• ```always @(posedge int_osc) begin ... end```: This block describes sequential logic that executes on the rising edge of the ```int_osc``` signal. 
	
   	• ```frequency_counter_i <= frequency_counter_i + 1'b1```: Inside the block, the ```frequency_counter_i``` register is incremented by 1 on each rising edge of ```int_osc```. 1'b1 represents a 1-bit binary value of 1.

##### 3. Analyze the internal components

 * Internal Oscillator Configuration: 
	
   	• ```SB_HFOSC```: This seems to be a module or macro representing a high-frequency oscillator. 
	
   	• ```#(.CLKHF_DIV("0b10"))```: This part configures the clock divider for the oscillator. "0b10" likely sets the division factor to 2 (binary representation). 
	
   	• ```u_SB_HFOSC```: This is the instance name of the oscillator module. 
	
   	• ```(.CLKHFPU(1'b1), .CLKHFEN(1'b1), .CLKHF(int_osc))```: This connects signals to the oscillator instance: 
	
  	• ```.CLKHFPU(1'b1)```: Probably enables the clock pull-up. 
	
  	• ```.CLKHFEN(1'b1)```: Likely enables the clock output. 
	
  	• ```.CLKHF(int_osc)```: Connects the oscillator output to the signal int_osc. 

  * RGB Primitive Instantiation: 
	
   	• ```SB_RGBA_DRV```: This is likely a module for controlling an RGB LED. 
	
   	• ```RGB_DRIVER```: This is the instance name of the RGB driver module. 

	• ```RGBLEDEN (1'b1)```: Enables the RGB LED. 1'b1 represents a 1-bit value set to 1 (high). 
	
   	• ```RGB0PWM (1'b0), // red```: Controls the pulse-width modulation (PWM) for the red component of the RGB LED. 1'b0 means it's initially off. 
	
   	• ```RGB1PWM (1'b0), // green```: Controls the PWM for the green component, also initially off. • RGBLEDEN (1'b1): Enables the RGB LED. 1'b1 represents a 1-bit value set to 1 (high). 
	
   	• ```RGB0PWM (1'b0), // red```: Controls the pulse-width modulation (PWM) for the red component of the RGB LED. 1'b0 means it's initially off. 
	
   	• ```RGB1PWM (1'b0), // green```: Controls the PWM for the green component, also initially off. 
	
   	• ```RGB2PM (1'b1), // blue```: Controls the PWM for the blue component, initially on. 
	
   	• ```CURREN (1'b1)```: Enables the current source for the LED. 
	
   	• ```RGB0 (led_red), RGB1 (led_green), RGB2 (led_blue)```: Connects the module's internal signals to external signals for the red, green, and blue LEDs. 

  * Parameter Overrides: 
	
   	• ```//Actual Hardware connection```: This comment suggests the following lines configure hardware-specific parameters. 
	
   	• ```defparam RGB_DRIVER.RGB0_CURRENT``` = "0b000001";: Sets the current for the red LED to a binary value of 000001. 
	
   	• ```defparam RGB_DRIVER.RGB1_CURRENT``` = "0b000001";: Sets the current for the green LED. 
	
   	• ```defparam RGB_DRIVER.RGB2_CURRENT``` = "0b000001";: Sets the current for the blue LED.
    </details>

     

### STEP 2  Creating and understanding the PCF File

<details>

 ##### 
 1.view the PCF file from this [link](https://github.com/rishabh7823/VSDSquadron-Research--By-Rishabh/blob/main/task1-ledgreen/VSDSquadronFM.pcf). 

	
 #### 2.Understanding the pins from PCF file 

* The pins -

  - ```led_red``` -> Pin 39

  - ```led_blue``` -> Pin 40

  - ```led_green``` -> Pin 41

  - ```hw_clk``` -> Pin 20

  - ```testwire``` -> Pin 17

	1. ```led_red 39```: This line assigns the signal named "led_red" to pin number 39 on the FPGA. This likely connects an LED (light-emitting diode) to that pin, allowing the design to control the LED's state (on/off).
   
	2. ```led_blue 40```: Similarly, this assigns the signal "led_blue" to pin 40, likely controlling another LED.
 
	3. ```led_green 41```: This assigns "led_green" to pin 41, controlling a third LED.
   
	4. ```hw_clk 20```: This assigns the hardware clock signal "hw_clk" to pin 20. This pin will provide the timing reference for the FPGA's internal operations.
   
	5. ```testwire 17```: This assigns a signal named "testwire" to pin 17, potentially for testing or debugging purposes.

#### 3. cross-reference of the pins 

* This mapping is crucial for correctly connecting and controlling external components or internal logic within the FPGA design. Each signal assignment defines the physical connection point on the FPGA board for that particular signal. For instance, the led_red signal is assigned to pin 39, meaning that the red LED will be controlled through this pin. Similarly, other signals like led_blue, led_green, hw_clk, and testwire are assigned to pins 40, 41, 20, and 17, respectively. These assignments are essential for proper hardware operation and must be consistent with the Verilog code and the board's hardware design.
 </details> 


### STEP 3: Integrating with the VSDSquadron FPGA Mini Board

<details>

#### Copy all the following files in task1-ledblue,task1-ledgreen,task1-ledred and run the following codammands

####
1.Reviewing the VSDSquadron FPGA Mini board [datasheet](https://www.vlsisystemdesign.com/wp-content/uploads/2025/01/VSDSquadronFMDatasheet.pdf) to understand its features and pinout.

####  
2. Use the datasheet to correlate the physical board connections with the [PCF](https://github.com/rishabh7823/VSDSquadron-Research--By-Rishabh/blob/main/task1-ledblue/VSDSquadronFM.pcf) file and [Verilog](https://github.com/rishabh7823/VSDSquadron-Research--By-Rishabh/blob/main/task1-ledblue/top.v) code.

####
3. Connecting the board to the computer as described in the datasheet using USB-C and ensuring FTDI connection

4. Follow the provided [Makefile](https://github.com/rishabh7823/VSDSquadron-Research--By-Rishabh/blob/main/task1-ledblue/Makefile) for building and flashing the Verilog code:

```
make clean
make build
sudo make flash
``` 

#### Observing the behavior of the RGB LED on the board to confirm successful programming - 

##### Follow the steps

1. ![Screenshot 2025-03-20 203212](https://github.com/user-attachments/assets/c73df39c-f278-4df8-9c36-31be7ccd5d8e)

2. ![Screenshot 2025-03-19 220519](https://github.com/user-attachments/assets/d40ad02f-8908-49df-b942-094e06c5c144)

3. ![Screenshot 2025-03-20 201130](https://github.com/user-attachments/assets/35bd9966-52b8-4f73-afd4-f1768f935e84)

4. ![Screenshot 2025-03-20 202509](https://github.com/user-attachments/assets/01f40e78-c20f-4f10-9374-9f756e6deada)

5. ![Screenshot 2025-03-20 203148](https://github.com/user-attachments/assets/0834e236-0557-4567-a8c7-886fc7f08a59)

6. ![Screenshot 2025-03-22 141419](https://github.com/user-attachments/assets/8493294e-b118-47e4-b6f3-f57b221c94e8)

7. ![Screenshot 2025-03-22 141451](https://github.com/user-attachments/assets/5bc43564-2682-463c-b4e4-cc69c901f5b0)

8. ![Screenshot 2025-03-22 141704](https://github.com/user-attachments/assets/2eca16e7-4c38-4493-9de2-dfe38db33386)

</details>

### STEP 4: Difficulty faaced

 was completely blank at the beginning after connecting the FPGA Mini. However, when it worked well, I felt confident that I could do it. But after writing the Verilog and PCF files, I again faced problems connecting the board. Fortunately, the WhatsApp group helped me solve the problem . 

google and AI helped me to understand verilog code and PCF file










   

