
# Component tester
Project to implement an electronic component tester, using Machine Learning.
Inspired by the existing [AVR transistortester](https://www.mikrocontroller.net/articles/AVR_Transistortester) project.

## Status
Idea/planning stage. See [TODO](./TODO.md)

## Goals
Be a fun and (somewhat) practical demonstrator for [emlearn](https://github.com/emlearn/emlearn) project.
Not intended to be better than existing devices out there.
So if you aware looking for a practical tool, just buy one of the existing devices from Banggood/Aliexpress etc.
They can usually be updated with new [open source firmware](https://github.com/madires/Transistortester-Warehouse).

### Run on standard hardware
Able to run on a modern subset of hardware as used for "AVR Transistor Tester".
AtMega 328p primarily.
Support for LCD displays not prioritized initially.
Primary output will be over serial/USB.

### Data-first approach
- Using Machine Learning for classifying the components.
- Keeping software for test sequence as simple and standard as possible.
- Building up a good sized dataset of measured components

### Be a good example
As a demonstrator for emlearn, it is important that the code is understandable and accessible.
That means simple and clear code is favored whenver possible.
Potentially to the detriment of flexibility, features, performance and usability.
Separating and hiding away underlying details about the hardware.

## Scope
Keep the great user interaction of original Transistor Tester

- Automatically work regardless of how component is placed
- Test time 2-3 seconds

### Classifications primarily
Classify components is the primary objective of the project.
Measurements/characterization of the devices is secondary.
Welcome for contributions for that.

### Automatic component detection
Would be nice to trigger test automatically
when a component is detected.
No need to press a "test" button.
