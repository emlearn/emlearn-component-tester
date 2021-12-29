

# AVR transistor tester
Very popular device.
Originally developed back in 2013, but continues until 2021.
With open source hardware and software.

Supports testing a wide range of devices,
and will also measure their voltage etc
https://www.mikrocontroller.net/articles/AVR_Transistortester

Based on ATMega328 chip

Will first detect component type,
then measure the characteristics

Small OLED version of AVR transistortester
https://github.com/akshaybaweja/component-tester-oled

Available in many variations as complete units at Bangood etc
Often called Transistor tester and something with 328, like M328/G328/GM328 etc

Another modern version with OLED
https://github.com/wagiminator/ATmega-Transistor-Tester

USB version
https://hackaday.io/project/173922-usb-component-tester

Arduino port
https://github.com/kr4fty/Ardutester
Uses 3 analog pins, and 6 digital pins.
Two resistors per input.

Code as a single .ino file. 5000 lines

ArduTester original, 2013
pighixxx
https://blog.adafruit.com/2013/05/02/ardutester-arduino-component-tester/

Arduitester v1.13
https://create.arduino.cc/projecthub/plouc68000/ardutester-v1-13-the-arduino-uno-transistor-tester-dbafb4

Software mod for GM328 / AY-AT  kit from Banggood 
https://github.com/blurpy/transistor-tester

Very active thread on hardware/firmware mods
https://www.eevblog.com/forum/testgear/$20-lcr-esr-transistor-checker-project/7350/

Github repo, offical here?
https://github.com/Mikrocontroller-net/transistortester
Everything seems to be .zip files...

madires is one of the firware developers
https://github.com/madires/Transistortester-Warehouse

main source code
https://github.com/kubi48/TransistorTester-source/tree/master/trunk
Huge amount of hardware variants supported

Arduino Uno shield
https://create.arduino.cc/projecthub/baweja_akshay/component-tester-uno-shield-272b06
https://www.instructables.com/Component-Tester-UNO-Shield/
mainly resistors
Also 2.5V precision voltage reference.
Has banana jack connectors, PCB throughhole
"This is still a work in progress...
The future update will support voltage measuring, frequency counter, and frequency generator. "
Some comments

2.5V voltage reference is not needed
Should only be used if actually a precison source
Otherwise will use 5V
https://www.eevblog.com/forum/testgear/$20-lcr-esr-transistor-checker-project/3575/

"The external 2.5V voltage reference should be only enabled if it's
at least 10 times more precise than the voltage regulator. Otherwise it would make the results worse."

Nice breadboard wiring diagram for Arduino Uno
https://maker.pro/arduino/projects/component-tester-sparkfun-flowcode
Firmware outputs data over USB

## References

How do Transistor Testers work (and some other insights)
https://www.youtube.com/watch?v=4Xsg8lpP75s
Andreas Spiess

Tri-state. Output,input,input_pullup
AVR chips have ~20 ohm internal resistance for output high/low

7 states for each test-pin

10 bits ADC in ATmega
Uses time-delay for measuring capacitance ? At least large ones

Must avoid destroying devices.
?? what does this entail. What unsafe states could one have? And for which components?
any that could destroy measurement device?

Tested a BS108 MOSFET. But was shown as NPN


## How their firmware works

main.c
Starts by calling CheckPins.c

// check all 6 combinations for the 3 pins 
//       High  Low  Tri
CheckPins(TP1, TP2, TP3);
CheckPins(TP2, TP1, TP3);

CheckPins(TP1, TP3, TP2);
CheckPins(TP3, TP1, TP2);

CheckPins(TP2, TP3, TP1);
CheckPins(TP3, TP2, TP1);

void CheckPins(uint8_t HighPin, uint8_t LowPin, uint8_t TristatePin)
  Checking the characteristic of a component with the following pin assignment 
  HighPin: Pin, which will be switched to VCC at the beginning
  LowPin: Pin, which will be switch to GND at the beginning
  TristatePin: Pin, which will be undefined at the beginning
  TristatePin will be switched to GND and VCC also .

Sets the pins and measures ADC values
Has branching to handle different component cases
Diode
NPN
MOSFET

Fills structs such as
ntrans
ptrans
with info

ntrans.uBE = adc.lp1;
ntrans.gthvoltage = unsigned_diff(adc.lp1, adc.tp1);	//voltage GS (Source - Gate)
ntrans.current = (unsigned int)(((unsigned long)adc.lp1 * 10000) / RR680MI); // Id 1uA
ntrans.hfe = c_hfe;

  // a TRIAC is marked as two transistors at least (2 or 3)
  // both of NPN transistors (normal and inverse) are found, if ntrans.count == 2
  // both of PNP transistors (normal and inverse) are found, if ptrans.count == 2

PartFound
PartMode

// extrapolate the quadratic relationship between Id and Vgs, to estimate Idss
i16 = expand_FET_quadratic(ntrans.ice0,ntrans.gthvoltage,ntrans.current);

Has DebugOut  which increases about of data logged to LCD/USART
Set to 5 for maximum output

Supports devices such as
NPN/PNB. Including parasistic capacitance
PUT (Programmable Unijunction Transistor)
TRIAC
IGBT
Thyristor
JFET
Diode
opto-coupler
MOSFET. Including R_DSon
N-D-MOS (incl protection diode)
UJT is detected as 2 diodes E-B1 and E-B2...
Ceramic resonator
Crystal

//measure the Gate threshold voltage
//Switching of Drain is monitored with digital input
// Low level is specified up to 0.3 * VCC
// High level is specified above 0.6 * VCC

// still not recognized anything? then check for ceramic resonator or crystal
// these tests are time-consuming, so we do them last, and only on TP1/TP3

       ReadCapacity(ntrans.e, ntrans.b);	// read capacity of NPN base-emitter
       n_cval = cap.cval;			// save the found capacity value
       n_cpre  = cap.cpre;			// and dimension
       ReadCapacity(ptrans.b, ptrans.e);	// read capacity of PNP base-emitter

search_vak_diode
Finds protection diode

Can auto-identify part orientation / pinout

Around 20k lines of code total. 15k C, 5k ASM

Large amount of preprocessor conditionals
For hardware/processor differences. AtMega versions
Registers for outputs and ADC are handled directly, no HAL
Assumptions of 680 ohm resistors spread across code
For display type (graphics vs no graphics). LCD model shielded behind lcd_ interface 
For types of features/tests enabled
SHOW_XXX
WITH_XXX

User interface is mixed with processing and testing process.
Both LCD and input handling (rotary, manu, buttons etc)
Testing process has many branches, sub-tests are done based on first tests
Comprehensive range of possible outputs, and tests, is hard to determine from source code

Has a selftest function. AutoCheck.c
All probes should be shorted together before starting
Records offsets in pin resistances and capacitor measurements.
Saves calibration to EEPROM

Represents voltages in micro-volts, uV

A professional version would have input protection.
Especially against charged capacitors
Could be done by having a frontend where inputs are disconnected by relays,
that will discharge any input using resistors.
And with lockout, such that can only be connected into input when discharged.
Similar is described in 2.2.1 Protection of the ATmega inputs
Could also enable automatic selfcheck/calibration, by shorting probes together using relays?


