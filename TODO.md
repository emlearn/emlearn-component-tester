
# TODO

Hardware v0

- Receive AVR Transistor Tester hardware.
ETA January 2021
- Get working with stock AVR Transistor Tester firmware

Dataset v0

- Prepare a set of devices to test
- With latest firmware, including debug output
- Write a parser for the output. Including debug
- Hook up an oscilloscope with storage
- Measure all devices in set with AVR Transistor tester,
capturing output and oscilloscope traces
- Parse output into structured form
- Build initial dataset, with structured data and raw waveforms

Data capture v0

- Setup initial firmware
- Implement the test sequence
- Implement streaming over serial/USB
- Capture raw data for each component in dataset

Model v0

- Set up ML pipeline. Dataset loading, model evaluation
- Make first classification model

Tester v0

- Integrate ML model in firmware
- Output classifications over serial/USB
- Test with a selected set of components
- Make a demo video
