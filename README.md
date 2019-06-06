# esp32-rotary-encoder

## Introduction

This ESP32 component uses a debouncing state machine to track the position of an [incremental](https://en.wikipedia.org/wiki/Rotary_encoder#Incremental) rotary encoder such as the [EC11](https://www.alps.com/prod/info/E/HTML/Encoder/Incremental/EC11/EC11_list.html) or [LPD3806](https://www.codrey.com/electronic-circuits/paupers-rotary-encoder/).

These encoders provide two outputs (typically "A" and "B") that have a quadrature relationship - that is, they oscillate between high and low as the shaft rotates, but with a 90 degree phase offset. Therefore the outputs `AB` may go through the following pattern: `00`, `10`, `11`, `01`, `00` ... The direction can be determined by observing the order of this pattern. This component uses a state machine to decode the sequence of quadrature states, and therefore provides a fairly glitch-free and accurate measurement of the encoder's movement.

Interrupts are registered on the A and B pins to detect edges. This component assumes that `gpio_install_isr_service()` has been called prior.

The direction event is placed into a FreeRTOS queue and can be used by a task to increment or decrement a counter that represents the encoder's absolute position.

Encoders that provide a push button are supported, however this component does not provide direct support for the button. Typically, the button is normally open and pushing it closes the contacts, which can be used to pull a GPIO pin high or low depending on arrangement. This can be detected with a normal GPIO poll or interrupt.

## Dependencies

It is written and tested for the [ESP-IDF](https://github.com/espressif/esp-idf) environment, using the xtensa-esp32-elf toolchain (gcc version 5.2.0).

## Example

An example application that uses this component is available: [esp32-rotary-encoder-example](https://github.com/DavidAntliff/esp32-rotary-encoder-example)

## Features

* Publication of an event into a user-supplied queue when the encoder moves to either a full or half step:
** Full step mode, where the encoder must move through an entire sequence of four steps before an event is published.
** Half step mode, where the encoder must move through half an entire sequence (two steps) before an event is published.

## Documentation

Automatically generated API documentation is available [here](https://davidantliff.github.io/esp32-rotary-encoder/index.html).

## Source Code

The source is available from [GitHub](https://www.github.com/DavidAntliff/esp32-rotary-encoder).

## License

The code in this project is licensed under the GPLv3 license - see LICENSE for details.

## Links

 * [esp32-rotary-encoder-example](https://github.com/DavidAntliff/esp32-rotary-encoder-example)
 * [Espressif IoT Development Framework for ESP32](https://github.com/espressif/esp-idf)

## Acknowledgements

Thank you to [Ben Buxton](mailto://bb@cactii.net) who provided the [original](https://github.com/buxtronix/arduino/tree/master/libraries/Rotary) Arduino code that this component is based upon.



