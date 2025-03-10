# AYAB Serial Communication Specification, API v6

This document specifies the serial communication protocol used by the AYAB desktop software (host)
to communicate with the firmware on the AYAB shield (device), starting with version 1.0.0 of both
[software](https://github.com/AllYarnsAreBeautiful/ayab-desktop) and
[firmware](https://github.com/AllYarnsAreBeautiful/ayab-firmware).

## Generic configuration

Asynchronous serial communication at 115200 baud, 8N1 without hardware flow control, encoded by
[SLIP protocol](https://tools.ietf.org/html/rfc1055.html). SLIP packets are parsed sequentially.
Checksums are calculated using the CRC8 algorithm as implemented in Dallas/Maxim application note
27.

Messages are limited in size to **64** bytes, not including the SLIP packet markers (`0xC0`
bytes), but taking into account the fact that under SLIP, data bytes equal to `0xC0` or `0xDB`
are escaped into two bytes, the maximum safe payload size is **32** bytes.
This value is defined in the MAX_MSG_BUFFER_LEN constant in AYAB firmware und should be adapted
accordingly in case of API message length changes.

## Protocol for API v6

### API version check

The host sends a **reqInfo** message to the device, which responds with **cnfInfo** indicating the
API version used by its firmware.

### Specification of knitting machine type

The host sends a **reqInit** message to the device specifying the knitting machine type in use.
The device responds with **cnfInit** indicating the success or otherwise of the initialization.

### Work request

The host waits for a **indState** message before requesting work. On startup, the device
continuously checks for the initialization of the machine (carriage passed left hall sensor). When
this happens, it sends an **indState** to tell the host that the machine is ready. After receiving
this message, the host can send either a **reqStart** message to begin knitting, or a **reqTest**
message to begin testing the hardware. The device confirms receipt of **reqStart** and **reqTest**
messages by returning a **cnfStart** or **cnfTest** message, respectively.

### Knitting operation

After a successful **reqStart**, and when it is ready to receive a further signal, the device
begins to poll the host for line data with **reqLine**. (The device becomes ready after the
carriage moves past the Hall sensor marking the beginning of the row.) The host answers with a
**cnfLine** message containing information for the next row of knitting. After the row has been
completed, the device sends another **reqLine** message to request the next line of data. When the
host does not have any more lines to send, it sets the *lastLine* flag in its final **cnfLine**
message.

### Hardware test operation

After a successful **reqTest**, the device begins to poll the host for commands. The host polls
the device for test results in a similar fashion. The device sends test results asynchronously, as
zero-terminated text strings with an initial **testRes** id. The hardware test is terminated when
the host sends **quitCmd**.

## Message identifier format

Messages start with a byte that identifies their type. This byte is called "id" or "message id" in
the following document. This table lists all the bits of this byte and assigns their purpose:

| Bit | Value |        Name        |         Description and Values                       |
|-----|-------|--------------------|------------------------------------------------------|
|  7  |  128  | message source     | 0 = the message is from the host                     |
|     |       |                    | 1 = the message is from the controller               |  
|  6  |   64  | message type       | 0 = the message is unprompted                        |
|     |       |                    | 1 = the message is confirmation of a request         |
|  5  |   32  | test flag          | 0 = normal operation                                 |
|     |       |                    | 1 = hardware test mode                               |
|  4  |   16  | debug flag         | 0 = not a debug message                              |
|     |       |                    | 1 = debug message                                    |
|  3  |    8  | message identifier | These are the values that identify the message.      | 
|  2  |    4  | message identifier |                                                      |
|  1  |    2  | message identifier |                                                      |
|  0  |    1  | message identifier |                                                      |

## Message definitions (API v6)

The length is the total length with id and parameters. Note that the initial and terminal 0xc0
bytes are required by the SLIP protocol are not included in the message length.

| Source   | Name         | ID   | Length | Parameters                                                   |
|----------|--------------|------|--------|--------------------------------------------------------------|
| host     | **reqStart** | 0x01 | 5      | *0xaa 0xbb 0xcc 0xdd*                                        |
|          |              |      |        | 0xaa = start needle (Range: 0-198)                           |
|          |              |      |        | 0xbb = stop needle (Range: 0-199)                            |
|          |              |      |        | 0xcc = flags (bit 0: continuous reporting)                   |
|          |              |      |        |              (bit 1: hardware beep on/off)                   |
|          |              |      |        | 0xdd = CRC8 checksum                                         |
| device   | **cnfStart** | 0xC1 | 2      | *0xaa*                                                       |
|          |              |      |        | 0xaa = success (0 = success, other values = error)           |
| device   | **reqLine**  | 0x82 | 2      | *0xaa*                                                       |
|          |              |      |        | 0xaa = line number (Range: 0 - 255)                          |
| host     | **cnfLine**  | 0x42 | 25/30  | *0xaa 0xbb 0xcc 0xdd[] 0xee*                                 |
|          |              |      |        | 0xaa = line number (Range: 0 - 255)                          |
|          |              |      |        | 0xbb = flags (bit 0: lastLine)                               |
|          |              |      |        | 0xcc = color information                                     |
|          |              |      |        | 0xdd[] = binary pixel data (15 or 25 bytes)                  |
|          |              |      |        | 0xee = CRC8 checksum                                         |
| host     | **reqInfo**  | 0x03 | 1      | Request firmware API version                                 |
| device   | **cnfInfo**  | 0xC3 | 22     | *0xaa 0xbb 0xcc 0xdd 0xee[17]*                               |
|          |              |      |        | 0xaa = API Version Identifier                                |
|          |              |      |        | 0xbb = Firmware Major Version                                |
|          |              |      |        | 0xcc = Firmware Minor Version                                |
|          |              |      |        | 0xdd = Firmware Patch Version                                |
|          |              |      |        | 0xee[17] = Firmware Suffix (max. 16 chars + \0)              |
| device   | **indState** | 0x84 | 10     | *0xaa 0xbb 0xCC 0xcc 0xDD 0xdd 0xee 0xff 0xgg*               |
|          |              |      |        | 0xaa = ready (0 = ready, other values = not ready)           |
|          |              |      |        | 0xbb = Finite State Machine state                            |
|          |              |      |        | 0xCCcc = `int` left hall sensor value                        |
|          |              |      |        | 0xDDdd = `int` right hall sensor value                       |
|          |              |      |        | 0xee = carriage type:                                        |
|          |              |      |        |     FF = no carriage detected                                 |
|          |              |      |        |     0 = Knit carriage                                        |
|          |              |      |        |     1 = Lace carriage                                        |
|          |              |      |        |     2 = Garter carriage                                      |
|          |              |      |        | 0xff = carriage position (needle number)                     |
|          |              |      |        | 0xgg = carriage direction:                                   |
|          |              |      |        |     0 = Left                                                 |
|          |              |      |        |     1 = Right                                                |
|          |              |      |        |     other = direction not known                              |
| host     | **reqTest**  | 0x04 | 1      | Request hardware test operation                              |
| device   | **cnfTest**  | 0xC4 | 2      | *0xaa*                                                       |
|          |              |      |        | 0xaa = success (0 = success, other values = error)           |
| host     | **reqInit**  | 0x05 | 3      | *0xaa 0xbb*                                                  |
|          |              |      |        | 0xaa = machine type:                                         |
|          |              |      |        |     0 = KH-910 or KH-950                                     |
|          |              |      |        |     1 = KH-930, KH-940, or KH-965                            |
|          |              |      |        |     2 = KH-270                                               |
|          |              |      |        | 0xbb = CRC8 checksum                                         |
| device   | **cnfInit**  | 0xC5 | 2      | *0xaa*                                                       |
|          |              |      |        | 0xaa = success (0 = success, other values = error)           |
| host     | **reqQuit**  | 0x0C | 1      | Request end of knitting operation                            |
| device   | **cnfQuit**  | 0xCC | 2      | *0xaa*                                                       |
|          |              |      |        | 0xaa = success (0 = success, other values = error)           |
| host     | **helpCmd**  | 0x26 | 1      | Hardware test command requesting help on available commands. |
| host     | **sendCmd**  | 0x27 | 1      | Hardware test command requesting that the device send a      |
|          |              |      |        | test packet consisting of three bytes, 0x31 0x32 0x33.       |
| host     | **beepCmd**  | 0x28 | 1      | Hardware test command requesting that the device beep.       |
| host     | **readCmd**  | 0x29 | 1      | Hardware test command requesting that the device read the    |
|          |              |      |        | EOL (end of line) Hall sensors and the position encoders.    |
| host     | **autoCmd**  | 0x2A | 1      | Hardware test command requesting that the device read the    |
|          |              |      |        | EOL sensors and position encoders once per second, sending   |
|          |              |      |        | a **testRes** message reporting the information each time.   |
| host     | **testCmd**  | 0x2B | 1      | Hardware test command requesting that the device test the    |
|          |              |      |        | solenoids by activating odd and even sensors alternately,    |
|          |              |      |        | once per second.                                             |
| host     | **quitCmd**  | 0x2C | 1      | Hardware test command requesting that the device quit        |
|          |              |      |        | hardware test mode and return to normal operation.           |
| host     | **setCmd**   | 0x2D | 3      | *0xaa 0xbb*                                                  |
|          |              |      |        | 0xaa = index of solenoid to set                              |
|          |              |      |        | 0xbb = solenoid value (0 = unset, 1 = set)                   |
| device   | **testRes**  | 0xEE | var    | A string containing hardware test information.               |
|          |              |      |        | The length is variable. The string terminates with 0.        |
| device   | **debug**    | 0x9F | var    | A debug string, of variable length, terminating in 0.        |

## Error codes

As of APIv6, the only important distinction is between *Success* (0x00) and any other value.
Informative error codes are provided for diagnostic purposes (that is, for debugging). Non-zero
error codes are subject to change: such changes will be considered non-breaking.

| Value | Meaning |
|-------|---------|
| 0x00  | Success |
|       | Message not understood: |
| 0x01  | Expected longer message |
| 0x02  | Unrecognized MsgId |
| 0x03  | Unexpected MsgId |
| 0x04  | Checksum error |
|       | Invalid arguments: |
| 0x10  | Machine type invalid |
| 0x11  | Needle value invalid |
| 0x12  | Null pointer argument |
| 0x13  | Argument(s) invalid or incompatible |
|       | Device not initialized: |
| 0x20  | Machine type not initialized |
| 0x21  | Carriage not initialized |
| 0x22  | Direction not initialized |
| 0x23  | Beltshift not initialized |
|       | Machine in wrong FSM state: |
| 0xE0  | MACHINE_STATE_INIT |
| 0xE1  | MACHINE_STATE_READY |
| 0xE2  | MACHINE_STATE_KNIT |
| 0xE3  | MACHINE_STATE_TEST |
| 0xEF  | WRONG_MACHINE_STATE |
|       | Generic error codes: |
| 0xF0  | Warning (ignorable error) |
| 0xF1  | Recoverable error |
| 0xF2  | Critical error |
| 0xF3  | Fatal error |
| 0xFF  | Unspecified failure |
