
# Implementation

## Test sequence
Table 5.1. all combinations of measurement.
6 overall pin configurations

In positive state, can be

- switched directly to VCC
- connected with the 680Ω resistor to VCC

In negative state, can be

- switched directly to GND
- connected with the 680Ω resistor to GND

In test state, that probe can be

- open (Input),
- connected with the 470kΩ resistor to VCC
- connected with the 470kΩ resistor or GND,
- connected with the 680Ω resistor to VCC
- connected with the 680Ω resistor to VCC

Expanded that gives 2x2x5x6 = 120 states ?

?? Is negative=GND and positive=VCC a legal/safe/useful state?
Can maybe be removed

For 2000 ms test time, gives approx 16.66 ms per state

In each state, 6 digital pins that need tristate changes. 3 bits trivial encoding
120*6*3 = 2160 bits
Typically all pins are on same 8-bit PORT, and two registers encode tristate.
120*8*2 = 1920 bits = 240 bytes
Whole table can possibly be stored in RAM


Voltage measurement for each of 3 measurement pins.
AtMega ADC has 10 bits resolution at 200kHz.
0.5 ms per sample
16 bit next standard representation

Measure 2-5 times per state?
On order of 1-10 ms.

Existing code uses 5ms delay before read in some places

Can median filter from sample rate, put into 16 bit value
Values will range 0-5V.
Maybe a bit higher than 5V in some cases, like up to 5.5V
Gives approx 76 uV resolution

## Raw data representation
Transmission.
3 values a 16 bit per 1 ms
3*16*1000 = 48000 bit/second
Doable with 192 kbaud serial
if using efficient binary transport / framing.
Maybe msgpack, with an array
Might want to also store a frame counter / timestamp

In memory.
With 2 second test time and 1 ms samplerate, requires 12 kB memory to store.
Too much for AtMega328p (2kB RAM total)
10 ms samplerate is doable, 0.6 kB RAM

## Measuring capacitance/inductance
? what is voltage change for large caps and inductors in 16 ms?
Assuming 680 + 20 ohm total resistance
Probably enough to classify at least??
Maybe not enough to reliably measure capacitance/inductance??

If not large enough, need to have some states be longer.
Could it be done non-conditionally?
Like wait for certain level of change, and measure time instead
AVR transistor tester uses a second phase, after finding cap measurement is needed.
Up to 8 seconds before timing out

## Model outputs

- PartType. Around 30 classes?
- Pinout. Around 3 per part type
- Measurements. Around 1-3 continious values. Different per part type

v0 would be only PartType / Pinout

One model per each?
Feature selection for measurements?

## Features
Assuming 3 features per pin per state, 120 states = 1080 features
with 16 bits, 2160 bytes
Too large for AtMega. Even if not keeping raw buffer
Reducing to 8 bits per each might be enough

Voltage at start,mid,end of state (per pin) might be a good base.
Perhaps transform to start,mid-start,end-start

## Data collection

Should try multiple instances of each component type
Should try all orientations of each instance


