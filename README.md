
# VSDSquadron-Research--By-Rishabh

# About VSDSquadron mini board

![Screenshot 2025-03-22 145658](https://github.com/user-attachments/assets/f2f833da-6db8-4c9b-955e-eb50446aefdf)

* FPGA:
	– Powered by the Lattice UltraPlus ICE40UP5K FPGA
	– Offers 5.3K LUTs, 1Mb SPRAM, 120Kb DPRAM, and 8 multipliers for versatile design
	capabilities
* Connectivity:
	– Equipped with an FTDI FT232H USB to SPI device for seamless communication
	– All FTDI pins are accessible through test points for easy debugging and customization
* General Purpose I/O (GPIO):
	– All 32 FPGA GPIOs brought out for easy prototyping and interfacing
* Memory:
	– Integrated 4MB SPI flash for data storage and configuration
* LED Indicators:
	– RGB LED included for status indication or user-defined functionality
* Form Factor:
	– Compact design with all pins accessible, perfect for fast prototyping and embedded applications
	The VSDSquadron FPGA Mini (FM) board is an affordable, compact tool for prototyping and
	embedded system development. With powerful ICE40UP5K FPGA, onboard programming, versatile
	GPIO access, SPI flash, and integrated power regulation, it enables efficient design, testing, and
	deployment, making it ideal for developers, hobbyists, and educators exploring FPGA applications.



<details><summary>Installation and Settings
</summary>




Download VSDSquadron FPGA Mini (FM) Software on your laptop as given in datasheet

 You should see a terminal window as shown in below


![Screenshot 2025-03-22 232318](https://github.com/user-attachments/assets/f8b2a65d-a4a8-4f3c-a7bc-760b5166fd82)




run the commands as given below 

```bash

cd
cd VSDSquadron_FM
cd blink_led
```


On the Virtual Machine, click on ”Devices → USB → FTDI Single RS232-HS [J900]” 

To confirm if the board is connected to the USB, type the ‘lsusb‘ command in the terminal.
You should see a line stating ”Future Technology Devices International,”


Then follow these commands

– Run the following command to clean up previous builds. Refer to Fig. 16:

```bash
make clean
```

Build the binaries for the FPGA board using below command.
```bash
make build
```
Flash the code to the external SRAM with the following command:

```bash
sudo make flash
```
Once the code is successfully flashed, you will see the RGB lights on the FPGA board
blinking.



![Screenshot 2025-03-14 183026](https://github.com/user-attachments/assets/b8fdef5e-e1b5-4ba9-9fdf-af6ce4daa6c5)




https://github.com/user-attachments/assets/7390f54b-6ee4-48ed-be00-6438f2513c5a



</details>



## TASK 1 

### OBJECTIVE

To understand and document the provided Verilog code, create the necessary PCF file, and integrate the design with the VSDSquadron FPGA Mini board using the provided [datasheet](https://www.vlsisystemdesign.com/wp-content/uploads/2025/01/VSDSquadronFMDatasheet.pdf). (install tools as explained in datasheet)

### CONTANT

#### STEP 1 Understanding the Verilog Code
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

     

#### STEP 2  Creating and understanding the PCF File

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


#### STEP 3: Integrating with the VSDSquadron FPGA Mini Board

<details>

#### Create all the following files in task1-ledblue,task1-ledgreen,task1-ledred and run the following commands

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

#### STEP 4. Pin mapping and Challenges faced

<details>

##### Pin mapping details from the PCF file


![Screenshot 2025-03-24 130736](https://github.com/user-attachments/assets/66bf2cd0-d881-4385-9d21-c7cb1a08c8d0)


##### Challenges faced
 
 I was completely blank at the beginning after connecting the FPGA Mini. However, when it worked well, I felt confident that I could do it. But after writing the Verilog and PCF files, I again faced problems connecting the board. Fortunately,Kunal sir and TA helped me solve the problem. 

google and AI helped me to understand verilog code and PCF file
</details>



## TASK 2

### Objective

Implement a UART loopback mechanism where transmitted data is immediately received back, facilitating testing of UART functionality.

#### STEP 1 Analyzing the Existing Code

<details> 
	
##### UART:

The Universal Asynchronous Receiver/Transmitter (UART) is a fundamental serial communication protocol prevalent in embedded systems and digital hardware. Its asynchronous nature distinguishes it from synchronous protocols, relying on pre-configured baud rates for timing synchronization between communicating entities.UART is implemented in a wide array of devices, from microcontrollers and embedded systems to personal computers and various communication interfaces. It can be seen in this [link](https://github.com/thesourcerer8/VSDSquadron_FM/blob/main/uart_loopback/top.v) it belonges to this [repository](https://github.com/thesourcerer8/VSDSquadron_FM/tree/main/uart_loopback)

* Module Declaration:
	
 	*```module top (...)```: Defines a module named top with input and output signals.

	*```output wire led_red, // Red```: Declares an output wire named led_red for a red LED.

	*```output wire led_blue, // Blue```: Declares an output wire named led_blue for a blue LED.

	*```output wire led_green, // Green```: Declares an output wire named led_green for a green LED.

	*```output wire uarttx, // UART Transmission pin```: Declares an output wire for UART transmission.

	*```input wire uartrx, // UART Transmission pin```: Declares an input wire for UART reception.

	*```input wire clk```: Declares an input wire for a clock signal.

* Internal Signals:

	*```wire int_osc```: Declares a wire named int_osc for the internal oscillator signal.

	*```reg [27:0] frequency_counter_i```: Declares a 28-bit register named frequency_counter_i to count clock cycles.

* Internal Oscillator:

	*```SB_HFOSC #(.CLKI_DIV("0b10")) U_SB_HFOSC (.CLKHFPU(2'b11), .CLKHFEN(1'b1), .CLKHF(int_osc));```: Instantiates a high-frequency oscillator (HFOSC) primitive

	*```CLKI_DIV("0b10")```: Sets the input clock divider.

	*```CLKHFPU(2'b11)```: Enables the high-frequency output.

	*```CLKHFEN(1'b1)```: Enables the HFOSC.

	*```CLKHF(int_osc)```: Connects the HFOSC output to the int_osc signal.

* UART Assignment:

	*```assign uarttx = uartrx```: Assigns the value of uartrx to uarttx, likely for echoing received data.

* Counter

	*This section defines a counter that increments on the rising edge of the ```int_osc ```signal.

	*f```requency_counter_1``` is incremented by 1 on each clock cycle.

	*The comment suggests this counter is related to generating a ```9600 Hz clock signal```, but the actual clock generation logic isn't shown here.

* Instantiate RGB primitive

	*These lines serve as comments, indicating that the following code instantiates an ```RGB LED driver```.

* RGB Driver instantiation

	*```SB_RGBA_DRV RGB_DRIVER (...) ```instantiates a module (likely a pre-defined primitive in the FPGA library) to drive the RGB LED.

	*```RGBLEDEN(1'b1)```: Enables the RGB LED.

	*```RGBBPMM(uartrx), AGBIPMM(uartrx), RGB2PMM(uartrx)```: These likely control the pulse-width modulation (PWM) for the blue, green, and red components of the RGB LED, respectively. uartrx suggests that these are controlled by a UART receive signal.

	*```CURREN(1'b1)```: might set the current limit for the LED.

	*```RGB0(led_green), RGB1(led_blue), RGB2(led_red)```: Connect the RGB driver outputs to the actual LED signals.

* Parameter definitions

	*```defparam RGB_DRIVER.RGB0_CURRENT = "66000001"```;

	*```defparam RGB_DRIVER.RGB1_CURRENT = "86000001"```;

	*```defparam RGB_DRIVER.RGB2_CURRENT = "0b000001"```;

	*These lines define the current settings for the ```red, green, and blue LEDs```. The values are specified in binary format. These parameters likely control the brightness or intensity of the LEDs.

* Endmodule

	*This line indicates the end of the module definition.


  understanding uart loopback


  Module Declaration:
  
	module ```uart_txlni (...) ```defines the start of the module named uart_txlni and lists its input and output signals.


* Inputs
	
 	```clk```: Clock signal for synchronous operation.

	```txbyte```: 8-bit data to be transmitted.

	```senddata```: Trigger signal to start transmission.

* Outputs:

 	```txalone```: Signal indicating transmission completion.

	```tx```: Serial output data line.

	```Parameters```:

	```STATE_IDLE, STATE_STARTTX, STATE_TXIMS, STATE_TXOONE```: Define the states of a finite state machine (FSM) that controls the transmission process. These states likely correspond to:

	```IDLE```: Waiting for data to transmit.

	```STARTTX```: Sending the start bit.

	```TXIMS```: Transmitting data bits.

	```TXOONE```: Sending the stop bit.

* State Variables:

* state: Current state of the FSM.


  	```buf_tx```: Buffer to hold the data being transmitted.

 	```bits_sent```: Counter to track the number of bits transmitted.

	```tabit```: Current bit being transmitted.

	```txadone```: Flag indicating transmission is done.

* Wiring:

	```assign tx = tabit```: Continuously assigns the value of tabit to the output signal tx.

* Always Block:

 	(The always block is incomplete in the image but is a crucial part of the design)

 	This block would contain the logic for the FSM, describing how the state transitions occur based on the input signals and current state. It would also handle loading data into buf_tx, shifting out bits, and managing the txalone signal.

* Clocked Process:

 	The always @(posedge clk) block indicates a process that executes on every rising edge of the clock signal clk. This is a common way to implement sequential logic in hardware.

	State Machine:

	The code implements a state machine to manage the different stages of the serial transmission. The states appear to be:

	```STATE_BLE```: Initial state, potentially waiting for data to send.

	```STATE_STARTTX```: Sending the start bit.

	```STATE_TXING```: Transmitting the data bits.

	```STATE_TXDONE```: Transmission complete.

	```STATE_IDLE```: Idle state after transmission.

* Sending Data:

 	The code checks if senddata is high and the current state is ```STATE_BLE```. If true, it initiates the transmission by transitioning to ```STATE_STARTTX```, loading the data to be transmitted ```(tmytej)``` into a ```buffer (buf_tx)```, and setting

  	```txalone``` to low.

	In the ```STATE_STARTTX```, it sets txbit to low to send the ```start bit``` and transitions to ```STATE_TXING```.

	In ```STATE_TXING```, it transmits the data bits from ```buf_tx``` one by one on each clock cycle until all 8 bits are sent.

	After sending all bits, it transitions to ```STATE_TXDONE```.

	In ```STATE_TXDONE```, it sets ```txalone``` to high and returns to ```STATE_IDLE```.

* Variables:

 	senddata: Signal to initiate data transmission.

 	state: Current state of the state machine.

	```buf_tx```: Buffer to hold the data being transmitted.

	```txalone```: Signal indicating if the transmitter is idle or busy.

	```txbit```: The current bit being transmitted.

	```bits_sent```: Counter for the number of bits sent.

	```tmytej```: Data to be transmitted.

</details>


#### STEP 2 Documenting

<details>

![Screenshot 2025-03-26 173239](https://github.com/user-attachments/assets/291c7cda-3898-490d-a7e3-4be80dac5903)

key components :

   - High frequency osillator [int_osi}
   - Frequency counter

* Block diagram 

 ![Screenshot 2025-03-26 184547](https://github.com/user-attachments/assets/9d55b122-5e7a-454b-a9cd-89c337079693)



 <summary> Circuit Diagramc</summary>
</details>

#### STEP 3 Implementation:

<details>

First we need to create folder with files  [Makefile](https://github.com/rishabh7823/VSDSquadron-Research--By-Rishabh/blob/main/uart_loopback/Makefile) , [PCF](https://github.com/rishabh7823/VSDSquadron-Research--By-Rishabh/blob/main/uart_loopback/VSDSquadronFM.pcf) , [uart.trx](https://github.com/rishabh7823/VSDSquadron-Research--By-Rishabh/blob/main/uart_loopback/uart_trx.v) .The folder would be named as [uart_loopback](https://github.com/rishabh7823/VSDSquadron-Research--By-Rishabh/tree/main/uart_loopback).

![Screenshot 2025-03-26 185253](https://github.com/user-attachments/assets/57cac362-59e9-42f9-9b83-2bcf046df79c)

![Screenshot 2025-03-26 185308](https://github.com/user-attachments/assets/125458f5-8a34-4774-9466-0df6686b9f26)

We should use these commands -

```
cd

cd VSDSquadron_FM

cd uart_loopback

lsusb

make clean

make build

sudo make flash

```

![Screenshot 2025-03-25 144313](https://github.com/user-attachments/assets/9c70fe8e-a586-41ba-a24b-628e22150033)

![Screenshot 2025-03-26 185230](https://github.com/user-attachments/assets/487d24a0-72bf-4402-9dc3-70c9fa67c6fe)

</details>


#### STEP 4 Verification

<details>

First we should download a app known as Docklight version 2.4

Then you should ensure that the baud rate should be 9600 then the communication mode should be on send/recieve then the COMs would be desided by the device if the FGI board is connected.

Then double click on the small blue box below name in send sequences and enter a name then select a format and then type your message, click OK and then verify that this has entered in send sequences.

![Screenshot 2025-03-26 201726](https://github.com/user-attachments/assets/87a67f93-19ff-4a29-b0b7-950f73190028)

![Screenshot 2025-03-26 202849](https://github.com/user-attachments/assets/a176aeaa-b803-4705-bcd9-f35d315723e7)

![Screenshot 2025-03-25 173121](https://github.com/user-attachments/assets/bd14fa8e-3fa0-4ffc-b529-b34e52c6448f)

</details>


#### STEP5 Documentation 

<details>

* Circuit and Block diagram 


![Screenshot 2025-03-26 173239](https://github.com/user-attachments/assets/be830e02-0e92-4c2b-a6d2-1babfafb6209)


key components :

   - High frequency osillator [int_osi}
   - Frequency counter

![Screenshot 2025-03-26 184547](https://github.com/user-attachments/assets/554b2659-69da-4dde-81c0-41a453137a6d)



* Testing results

![Screenshot 2025-03-25 173121](https://github.com/user-attachments/assets/16ebbb6b-6370-4124-863a-bc2624ad3b69)

The final video 

https://github.com/user-attachments/assets/1cfc7557-5063-4234-a985-b70969fcd68b

</details>


## TASK 3

### Objective

Developing a UART transmitter module capable of sending serial data from the FPGA to an external device.

#### Step 1 analyzing the Existing Code

<details>

UART_TRX.V

 1. File Inclusion

  * This includes the Verilog file ```uart_trx.v```, which likely contains the UART transmission module ```uart_tx_8n1```.

2. Module Declaration

 * The module ```top``` has:

   output pins for RGB LEDs

   output for UART transmission ```uarttx```

   input clock signal ```hw_clk```

 3. Internal Signals

  * ```int_osc```: Stores the internal oscillator signal.

  * ```frequency_counter_i```: 28-bit counter used for LED control and timing operations.

4. 9600 Hz Clock Generation
 
  * 9600 Hz clock ```clk_9600``` is derived from the 12 MHz internal oscillator.

  * The counter ```cntr_9600``` toggles ```clk_9600``` when it reaches ```625``` cycles ```12,000,000 / 9600 ≈ 625```.

5. UART Transmission

  * UART Module ```uart_tx_8n1```:

     Clock Input: ```clk_9600```

     Byte to Transmit: "D" (ASCII character 'D')

     Send Condition: When ```frequency_counter_i[24]``` is high.

     TX Output: Connected to ```uarttx```.

6. Internal Oscillator Setup

   * SB_HFOSC is a high-frequency oscillator with a division factor ```0b1```, which likely divides the base clock to 12 MHz.

7. Frequency Counter & 9600 Hz Clock Logic

   * The counter increments on each positive edge of ```int_osc```.

   * A 9600 Hz clock is generated by toggling ```clk_9600``` every 625 cycles.

  8.RGB LED Control

 * RGB LED Driver ```SB_RGBA_DRV```:

   - ```RGBLEDEN(1'b1)```: Enables the LED driver.

   - RGBxPWM signals control the color based on ```frequency_counter_i```:

     Green ```led_green```: When ```frequency_counter_i[24]``` & ```frequency_counter_i[23]``` is high.

     Blue ```led_blue```: When ```frequency_counter_i[24]``` & ```~frequency_counter_i[23]``` is high.

     Red ```led_red```: When ```~frequency_counter_i[24]``` & ```frequency_counter_i[23]``` is high.

* These conditions cycle LED colors based on the counter.

9. LED Brightness Configuration

   * Sets the current levels for RGB LEDs to a low value ```0b000001```.

TOP.V

1. Module Overview

This module has:

Inputs:

   ```clk```: Clock signal.

   ```txbyte```: 8-bit data to be transmitted.

   ```senddata```: Signal to initiate transmission.

Outputs:

   ```txdone```: Indicates transmission completion.

   ```tx```: UART output signal (serial data line).

2. Parameter Definitions

   These define different states of the UART transmitter:

   ```STATE_IDLE``` (0) → Waiting for data.

   ```STATE_STARTTX``` (1) → Sending start bit (low signal).

   ```STATE_TXING``` (2) → Sending 8 data bits.

   ```STATE_TXDONE``` (3) → Sending stop bit and marking transmission as complete.

3. State Variables

   ```state```: Stores the current state of the UART module.

   ```buf_tx```: Holds the byte being transmitted.

   ```bits_sent```: Counts how many bits have been sent.

   ```txbit```: Holds the TX pin state (idle is high).

   ```txdone```: Set to 1 when transmission is finished.

4.  TX Wire Connection

  The output signal ```tx``` is directly assigned to ```txbit```, which is modified in the always block.

5.  Always Block (Sequential Logic)

   The UART transmission operates on the rising edge of the clock.

6. Start Condition

   If ```senddata``` is 1 and the module is IDLE, it:

   Moves to ```STATE_STARTTX```.

   Loads ```txbyte``` into ```buf_tx```.

Clears txdone.

If still IDLE, it ensures txbit stays HIGH (UART idle state).

7. Start Bit Transmission

   The start bit (0) is sent to the ```tx``` line.

   The module moves to ```STATE_TXING```.

8. Sending Data Bits

   The LSB (Least Significant Bit) is sent first.

   The buffer ```buf_tx``` is shifted right so the next bit moves to ```buf_tx[0]```.

   ```bits_sent``` is incremented.

9. Stop Bit and Transmission Completion

    After 8 data bits, a stop bit (HIGH 1) is sent.

    The bit counter resets.

    the module moves to ```STATE_TXDONE```.

10. Mark Transmission as Done

    The ```txdone``` flag is set to 1, indicating that the transmission has finished.

    The module returns to IDLE mode.

</details>

#### STEP2 Designing Documentation

<details>

 block diagram detailing the UART transmitter module.

 ![Screenshot 2025-03-31 122928](https://github.com/user-attachments/assets/eff6a7a2-3876-42d0-9e91-4460f7d8f670)

 circuit diagram illustrating the FPGA's UART TX pin connection to the receiving device.

![Screenshot 2025-03-31 121139](https://github.com/user-attachments/assets/03f4d68f-b6e7-44e5-aaee-51879f9b919a)

</details>

#### STEP3 Implementation

<details>

 Hardware setup

 * Refer to the [Datasheet](https://www.vlsisystemdesign.com/wp-content/uploads/2025/01/VSDSquadronFMDatasheet.pdf) for board details.
 * Ensure that you have connected the USB
 * Then make the folder named as uart_tx
 * Then put these commands

   ``` cd ```
   
   ``` cd VSDSquadron_FM ```
   
   ``` cd uart_tx ```
   
   ``` lsusb ```
   
   ``` make clean ```
   
   ``` make build ```
   
   ``` sudo make flash ```

* Like this

![Screenshot 2025-03-29 140217](https://github.com/user-attachments/assets/9dc5b566-fbdf-4377-bfba-171232818a21)

![Screenshot 2025-03-29 140233](https://github.com/user-attachments/assets/08a6cfb3-b483-48d4-b64d-61371b87b563)

![Screenshot 2025-03-29 140246](https://github.com/user-attachments/assets/a9db10c5-4776-48bb-a077-12f4f11e6fe9)

</details>

#### STEP4 Testing and Verification

<details>

 * Instal PUtty by this [link](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
 * make sure you know your communication number [in my case it was COM3]
 * Then check if the `D's are showing up
 * Like this :-

   ![Screenshot 2025-03-29 141226](https://github.com/user-attachments/assets/4f9300e5-149c-4bca-a221-a5ddeb473c56)

  https://github.com/user-attachments/assets/c91b4c03-955f-43ac-bed0-3ac1a3e8861b

  https://github.com/user-attachments/assets/c3d32de6-4399-4993-8841-e48542625c2f

  ![Screenshot 2025-03-31 130233](https://github.com/user-attachments/assets/ab74fef8-5442-4ff3-bddc-0b790a179b0a)

  ![Screenshot 2025-03-31 130242](https://github.com/user-attachments/assets/ddd43daa-6f06-4867-a479-0f368a3bfa9c)

  ![Screenshot 2025-03-31 130252](https://github.com/user-attachments/assets/c89a30a2-7594-468c-903b-feff03621a3c)

  ![Screenshot 2025-03-31 130259](https://github.com/user-attachments/assets/063dbf65-97c1-478a-80f3-09d20bcc1ab1)

</details>

#### STEP5 Documenting

<details>

 Block diagram -

 ![Screenshot 2025-03-31 121139](https://github.com/user-attachments/assets/11a23414-4aec-4ece-a985-bb43db4fb690)

 Circuit diagram -

 ![Screenshot 2025-03-31 122928](https://github.com/user-attachments/assets/cd73bd81-1f40-48fd-b7ae-997a37003217)

</details>


## Task 4

### Objective: Implement a UART transmitter that sends data based on sensor inputs, enabling the FPGA to communicate real-time sensor data to an external device.

#### STEP1 analyzing existing code

<details>

TOP.V

1. File Inclusion

   This includes an external Verilog file ```uart_trx.v```, which likely contains the UART transmitter ```(uart_tx_8n1)``` and possibly a UART receiver.

2. Module Declaration

   LEDs (led_red, led_blue, led_green) are used to indicate status based on received UART data.

   ```uarttx``` (UART Transmit Pin) sends data to an external device.

   ```uartrx```(UART Receive Pin) receives data, used to control LEDs.

   ```hw_clk``` (Hardware Clock Input) is the system clock.

3. Internal Signals

   ```int_osc```: Internal oscillator signal.

   ```frequency_counter_i```: A 28-bit counter used for timing operations.

4. Generating 9600 Hz Clock from 12 MHz

   ```clk_9600```: A clock signal for UART transmission.

   ```cntr_9600```: A counter used to divide the 12 MHz system clock.

   ```period_9600``` = 625: Defines how often the clock toggles to achieve 9600 baud rate.

5. UART Transmitter

   Instantiates the ```uart_tx_8n1``` module, which transmits ASCII character "D".

   ```senddata```(frequency_counter_i[24]) triggers data transmission periodically.

6. Internal Oscillator

   ```SB_HFOSC``` is an FPGA primitive used for generating an internal high-frequency oscillator clock.

   ```CLKHF_DIV``` ("0b10") sets the frequency division.

7. Counter for Clock Division

   Increments ```frequency_counter_i``` on every clock pulse.

   Generates a 9600 Hz clock by toggling ```clk_9600``` every 625 cycles.

8. RGB LED Control (Using UART RX Data)

   RGB LEDs are controlled directly by the received UART signal ```(uartrx)```.

   This means when data is received via UART, the LEDs will turn on/off accordingly.

   ```SB_RGBA_DRV```is an FPGA primitive for driving RGB LEDs.

9. LED Current Configuration

    Sets the current drive strength for each LED.

UART_TRX,V

1. Module I/O

   ```clk``` → Input clock for timing UART transmission.

   ```txbyte``` → The 8-bit data to be transmitted.

   ```senddata``` → Signal to start transmission.

   ```txdone``` → Output signal indicating the transmission is complete.

   ```tx ```→ UART TX output (connected to the receiving device's RX pin).

2. UART 8N1 Transmission Format

   Start Bit (0): Signals the beginning of data transmission.

   8 Data Bits: Actual data being sent (LSB first).

   Stop Bit (1): Marks the end of transmission.

3. Parameters: UART State Machine

   ```STATE_IDLE``` (0) → UART is idle, waiting for data.

   ```STATE_STARTTX``` (1) → Sends the start bit (0).

   ```STATE_TXING``` (2) → Sends 8 data bits (LSB first).

   ```STATE_TXDONE``` (3) → Sends stop bit (1), then returns to idle.

4. Registers (State Variables)

   ```state``` → Holds the current state of the UART FSM.

   ```buf_tx``` → Stores txbyte temporarily while transmitting.

   ```bits_sent``` → Tracks the number of bits sent.

   ```txbit``` → Stores the TX line value (default = 1, idle).

   `txdon`e → Indicates transmission completion.

5. TX Line Output

   Assigns the `txbit` value to tx, ensuring it drives the TX pin.

6. IDLE STATE: Waiting for Data

   If senddata == 1, the module loads txbyte into `buf_tx` and moves to `STATE_STARTTX`.

   Otherwise, TX line remains high (1) (idle state).

7. `START BIT`: Send Low (0)

   The TX line is pulled low (0) to indicate the start of transmission.

   Moves to `STATE_TXING` to begin sending data bits.

8. TRANSMIT 8 DATA BITS

   TX line is set to the LSB `(buf_tx[0])`.

   Right shift (>>1) the buffer to get the next bit.

   Increment `bits_sent` until all 8 bits are transmitted.

9. STOP BIT: Send High (1)

    Sends stop bit (1) to indicate end of transmission.

    Resets `bits_sent` to 0 and moves to `STATE_TXDONE`.

10. TX DONE & RETURN TO IDLE

    `txdone` is set to 1 to indicate transmission completion.

    Returns to `STATE_IDLE` to wait for new data.
 
</details>






   

