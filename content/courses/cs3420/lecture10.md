---
date: '2025-02-25T13:29:47-05:00'
title: 'Lecture 10: Communication Protocols'
draft: false
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

## I2C Bus

In CAN, we worried about clock synchronization. This is not an issue with I2C because I2C has separate data (SDA) and clock (SCL) lines. The data set set when SCL is low. Conventionally, when SCL is high, the data should not change.

The master signals the start of a message by changing SDA from high to low while SCL is high. Conversely, a stop is indicated by changing SDA from low to high while SCL is high.

The the first thing sent after a start is the address of the peripheral device, and multiple starts can be sent without a stop. The address is the first 7 bits, and the 8th bit is whether you should be reading (1) or writing (0). Every 9th bit is an acknowledgement, which should be driven low first by the peripheral then by the data receiver.

## SPI

The *serial peripheral interface* is technically not a bus protocol because there is technically only one device on each side. Instead, there is a select line for multiple devices. SPI is indicated by the present of MISO and MOSI.

The data is latched according to the clock, either on the rising or falling edge. There are different conventions, but the controller should match the peripheral device convention.

## UART

*Universal Asynchronous Receive Transmit* is what you would use to communicate between a computer and an external device instead between two chips. It is also a little slower, and it was originally sent through telephone lines.

Similar to SPI, it is not a bus protocol, as there is one dedicated line from the host to talk to the peripheral and one from the peripheral to the host. These are RX/TX lines. Notably, RX should go to TX and vice versa instead of, for example, MISO to MISO or SDA to SDA.

UART is very common when using devices connected to computers using USB.

## Error Checking

UART has a parity bit for basic error checking, and CAN has a whole process for error correction. I2C and SPI do not have that, but since SPI is between two chips on the same board, there is usually not that much noise.