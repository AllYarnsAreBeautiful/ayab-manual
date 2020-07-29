# AYAB Serial Communication Protocol, API version 6

The following is a description of the serial communication protocol used by the
AYAB desktop software (host) to communicate with the firmware on the AYAB shield
(device), starting with version 1.0.0 of both software and firmware.

## Generic configuration

Asynchronous 115200 8N1 without hardware flow control, encoded by SLIP protocol
(https://tools.ietf.org/html/rfc1055.html). SLIP packets are parsed sequentially.
Checksums are calculated using the CRC8 algorithm as implemented in Dallas/Maxim
application note 27.

## Initialization sequence

The host sends a single byte token as a request for information (ReqInfo = 0x03).

In return, it expects a four-byte message consisting of a token (CnfInfo = 0xC3),
a single-byte parameter (API version = 0x06), the firmware major version, and the
firmware minor version.

## Start sequence

When the device is ready to receive a start signal, it sends a state indicator 
token (IndState = 0x84) followed by a parameter value of 0x01. Other parameter 
values indicate an error state in the device.

In response, the host sends a six-byte message consisting of a token byte
(ReqStart = 0x01) followed by the machine type, start needle, stop needle, and a
flag byte. Start needle and stop needle are both zero-indexed. The least
significant bit of the flag byte indicates whether the device should continuously
report carriage information. The final byte is a CRC8 checksum of the first five
bytes.

The device confirms receipt of the start request by responding with a two-byte
message consisting of a token (CnfStart = 0xC1) followed by a parameter value.
A parameter value of 0x01 indicates that the device is ready to start the knitting
operation. Other parameter values indicate that the device is not ready.

## Knitting sequence

When the device is ready to knit it sends a two-byte message consisting of a
token (reqLine = 0x82) followed by a parameter indicating the requested line.
Parameter value for the first line is zero and is incremented for each successive
line that is requested.

The host responds with a message consisting of a token (CnfLine = 0x42), the
requested line number, one byte of flag information, one byte of color information,
either 15 bytes (KH-270) or 25 bytes (all other machines) of needle data encoded in
little-endian format, and a CRC8 checksum. The least significant bit of the flag
byte indicates whether the requested line is the last line of the pattern.
