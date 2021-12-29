
# Component tester
Transistor tester

## Status
Idea/planning

## Goals
Be a fun and (somewhat) practical demonstrator for [emlearn](https://github.com/emlearn/emlearn) project.

### Run on standard hardware
Able to run on a modern subset of hardware as used for "AVR Transistor Tester".
AtMega 328p primarily.
Support for LCD displays not prioritized initially.
Primary output will be over serial/USB.

### Data-first approach
- Using Machine Learning for classifying the components.
- Keeping software for test sequence as simple and standard as possible.
- Building up a good sized dataset of measured components

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
