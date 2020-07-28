# AYAB Serial Communication Protocol, API version 5

The following is a description of the serial communication protocol used by the
AYAB desktop software (host) to communicate with the firmware on the AYAB shield
(device), starting with version 1.0.0 of both software and firmware.

## Generic configuration

Asynchronous 115200 8N1 without hardware flow control, encoded by SLIP protocol
(https://tools.ietf.org/html/rfc1055.html). SLIP packets are parsed sequentially.
Checksums are calculated using **[FIXME: insert description of CRC8 algorithm.]**

## Initialization sequence

The host sends a single byte token as a request for information (ReqInfo = 0x03).

In return, it expects a four-byte message consisting of a token (CnfInfo = 0xC3),
a single-byte parameter (API version = 0x05), the firmware major version, and the
firmware minor version.

**[FIXME: Going forward, the host could adapt to whatever value of the API is used
by the device, thereby ensuring backwards compatibility.]**

## Start sequence

When the device is ready to receive a start signal, it sends a state indicator 
token (IndState = 0x84) followed by a parameter value of 0x01. Other parameter 
values indicate an error state in the device.

In response, the host sends a four-byte message consisting of a token byte
(ReqStart = 0x01) followed by the start needle, stop needle, and a flag byte.
Start needle and stop needle are both zero-indexed. The least significant bit
of the flag byte indicates whether the device should continuously report carriage
information.

**[FIXME: For the next version of the API, the host also needs to report the
machine that is in use. Maybe also a CRC8 checksum.]**

The device confirms receipt of the start request by responding with a two-byte
message consisting of a token (CnfStart = 0xC1) followed by a parameter value.
A parameter value of 0x01 indicates that the device is ready to start the knitting
operation. Other parameter values indicate that the device is not ready.

## Knitting sequence

When the device is ready to knit it sends a two-byte message consisting of a
token (reqLine = 0x82) followed by a parameter indicating the requested line.
Parameter value for the first line is zero and is incremented for each successive
line that is requested.

The host responds with a 28-byte message consisting of a token (CnfLine = 0x42),
the requested line number, 25 bytes of needle data encoding 200 bits left-to-right
in little-endian format, one byte of flag information, and a CRC8 checksum. The
least significant bit of the flag byte indicates whether the requested line is the
last line of the pattern.

**[FIXME: For the next version of the API, the length of the needle data could be
reduced to 15 bytes when knitting on a KH-270 machine, which has 114 needles
instead of 200.]**

**[FIXME: Perhaps add one byte to report the yarn color (palette index) so that
in future it might be possible to support the KRC1000e automatic color changer.]**