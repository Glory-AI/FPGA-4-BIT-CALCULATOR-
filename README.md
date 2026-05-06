# FPGA-4-BIT-CALCULATOR-
A simple but functional calculator implemented on an FPGA using VHDL.

* FPGA: Field-Programmable Gate Array
* VHDL: VHSIC (Very High-Speed Integrated Circuit) Hardware Description Language

This project performs basic arithmetic operations (addition and subtraction) on two 4-bit binary inputs and displays the result on a dual seven-segment display.

## Overview

This project was built to explore digital design using FPGA from binary arithmetic to real-time hardware display.

The idea is straightforward:

* Take two 4-bit binary numbers

* Use a selector to choose the operation

* Display the result in decimal (tens and units) on HEX displays


What made it more interesting was how the output was handled and displayed efficiently.




## Features

* 4-bit input operands

* Addition and subtraction support

* Real-time result display on 7-segment (HEX)

* Automatic conversion from binary result to decimal digits

* Clean and scalable VHDL design



## Inputs

| Input | Description |
| ------| ----------- |
|SW8–SW5 | First operand (A)|
|SW4–SW1 |Second operand (B) |
| SEL |	Operation selector |


* SEL = 0 → Addition
* SEL = 1 → Subtraction


## Outputs

| Display | Function |
| ------- | -------- |
| HEX1|	Tens digit|
|HEX0|	Ones digit|



## How It Works

#### 1. Arithmetic Logic

The calculator computes:

   * Result = A + B   (SEL = 0)
   * Result = A - B   (SEL = 1)


#### 2. Digit Extraction 

At first, all possible outputs were hardcoded using a case statement — not scalable at all.

The better approach was to split the result using division and modulus:

```vhdl
process(calc_result)
    variable result_int : integer;
    variable tens, ones : integer;
begin
    result_int := to_integer(unsigned(calc_result));

    tens := result_int / 10;
    ones := result_int mod 10;

    tens_digit_out <= std_logic_vector(to_unsigned(tens, 4));
    ones_digit_out <= std_logic_vector(to_unsigned(ones, 4));
end process;
```
This automatically maps any result into:

* Tens digit → HEX1
* Ones digit → HEX0



#### 3. Display Mapping

Each digit is sent to a 7-segment decoder which drives the FPGA HEX display.


Example Outputs

| A | B	| Operation | Result Display |
| - | - | --------- | -------------- |
|2|	2|	Add	4|	04|
|3|	3|	Add	6|	06|
|7|	4|	Subtract 3|	03|



#### Challenges & Fixes

❌ Hardcoded CD mapping- Too long, error-prone

✅ Switched to modulus approach- Cleaner, scalable, efficient


❌ Display inconsistencies during debugging

✅ Fixed by correcting binary-to-decimal mapping



## What I Learned

* Difference between <= (signals) and := (variables) in VHDL
  
     . Variable: When you assign a value to a variable, it is available for use on the very next line of code.
  
     . Signal: When you assign a value to a signal inside a process, the change is "scheduled." The signal retains its old value throughout the current execution of the process and only updates when the process pauses. 

* Efficient number handling in hardware design

* Why scalable logic always beats hardcoding

* Debugging FPGA designs requires patience 




## Acknowledgment

Shoutout to Emmanuel Peter for the guidance and support throughout this project.



## Future Improvements

* Support for multiplication and division

* Signed number handling (negative results)

* Extending to 8-bit operations

* Cleaner modular design (separate decoder component)
* Full adder and subtractor implementation 
