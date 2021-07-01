.. _AYAB_serial-communication-specification-apiv6:

AYAB Serial Communication Specification, API v6
===============================================

This document specifies the serial communication protocol used by the AYAB desktop
software (host) to communicate with the firmware on the AYAB shield (device), starting
with version 1.0.0 of both `software <https://github.com/AllYarnsAreBeautiful/ayab-desktop>`_ and `firmware <https://github.com/AllYarnsAreBeautiful/ayab-firmware>`_.

.. _generic-configuration:

Generic configuration
---------------------

Asynchronous serial communication at 115200 baud, 8N1 without hardware flow control,
encoded by `SLIP protocol <https://tools.ietf.org/html/rfc1055.html>`_. SLIP packets are
parsed sequentially. Checksums are calculated using the CRC8 algorithm as implemented in
Dallas/Maxim application note 27.

.. _API-v6:

Protocol for API v6
-------------------

Initial handshake
~~~~~~~~~~~~~~~~~

The host sends a **reqInfo** message to the device, which responds with **cnfInfo**
indicating its API and firmware versions.

Work request
~~~~~~~~~~~~

The host waits for a **indState(true)** message before requesting work. On startup,
the device continuously checks for the initialization of the machine (carriage passed left
hall sensor). When this happens, it sends an **indState(true)** to tell the host that the
machine is ready. After receiving this message, the host can send either a **reqStart**
message to begin knitting, or a **reqTest** message to begin testing the hardware.
The device confirms receipt of **reqStart** and **reqTest** messages by returning a
**cnfStart** or **cnfTest** message, respectively.

Knitting operation
~~~~~~~~~~~~~~~~~~

After a successful **reqStart**, and when it is ready to receive a further signal,
the device begins to poll the host for line data with **reqLine**. (The device becomes
ready after the carriage moves past the Hall sensor marking the beginning of the row.)
The host answers with a **cnfLine** message containing information for the next row
of knitting. After the row has been completed, the device sends another **reqLine** message
to request the next line of data. When the host does not have any morelines to send,
it sets the *lastLine* flag in its final **cnfLine** message.

Hardware test operation
~~~~~~~~~~~~~~~~~~~~~~~

After a successful **reqTest**, the device begins to poll the host for commands. The host
polls the device for test results in a similar fashion. The device sends test results
asynchronously, as zero-terminated text strings with an initial **testRes** id.
The hardware test is terminated when the host sends **quitCmd**.

.. _message-identifier-format:

Message identifier format
-------------------------

Messages start with a byte that identifies their type. This byte is called
"id" or "message id" in the following document. This table lists all the bits
of this byte and assigns their purpose:

+-----+-------+--------------------+------------------------------------------+
| Bit | Value |        Name        |         Description and Values           |
+=====+=======+====================+==========================================+
|     |       |                    | - 0 = the message is from the host       |
|  7  |  128  | message source     | - 1 = the message is from the controller |
|     |       |                    |                                          |
+-----+-------+--------------------+------------------------------------------+
|     |       |                    | - 0 = the message is unprompted          |
|  6  |   64  | message type       | - 1 = the message is confirmation        |
|     |       |                    |   of a request                           |
+-----+-------+--------------------+------------------------------------------+
|     |       |                    | - 0 = normal operation                   |
|  5  |   32  | test flag          | - 1 = hardware test mode                 |
|     |       |                    |                                          |
+-----+-------+--------------------+------------------------------------------+
|     |       |                    | - 0 = not a debug message                |
|  4  |   16  | debug flag         | - 1 = debug message                      |
|     |       |                    |                                          |
+-----+-------+--------------------+------------------------------------------+
|  3  |    8  |                    |                                          |
+-----+-------+                    | These are the values that identify the   |
|  2  |    4  |                    | message.                                 |
+-----+-------+ message identifier |                                          |
|  1  |    2  |                    |                                          |
+-----+-------+                    |                                          |
|  0  |    1  |                    |                                          |
+-----+-------+--------------------+------------------------------------------+

.. _message-definitions-apiv6:

Message definitions (API v6)
----------------------------

The length is the total length with :ref:`id <message-identifier-format>`
and parameters. Note that the initial and terminal  ``0xc0`` bytes required
by the SLIP protocol are not included in the message length.

========== ============ ==== ====== ==============================================================
  source      name       id  length        parameters
========== ============ ==== ====== ==============================================================
host       .. _m6-01:   0x01 6      ``0xaa 0xbb 0xcc 0xdd 0xee``      
                                  
           reqStart_                - ``0xaa`` = machine type:

                                      - ``0`` = KH-910 or KH-950
                                      - ``1`` = KH-930, KH-940, or KH-965
                                      - ``2`` = KH-270
                                    - ``0xbb`` = start needle (Range: 0-198)
                                    - ``0xcc`` = stop needle (Range: 0-199)
                                    - ``0xdd`` = flags (bit 0: continuousReporting)
                                    - ``0xee`` = CRC8 checksum
device     .. _m6-C1:   0xC1 2      ``0xaa``

           cnfStart_                - ``aa`` = success (0 = true, other values = false)
device     .. _m6-82:   0x82 2      ``0xaa``

           reqLine_                 - ``aa`` = line number (Range: 0..255)
host       .. _m6-42:   0x42 25/30  ``0xaa 0xbb 0xcc 0xdd[] 0xee``

           cnfLine_                 - ``aa`` = line number (Range: 0..255)
                                    - ``bb`` = flags (bit 0: lastLine)
                                    - ``cc`` = color information
                                    - ``dd[]`` = binary pixel data (15 or 25 bytes)
                                    - ``ee`` = CRC8 checksum
host       .. _m6-03:   0x03 1

           reqInfo_
device     .. _m6-C3:   0xC3 4      ``0xaa 0xbb 0xcc``

           cnfInfo_                 - ``aa`` = API Version Identifier
                                    - ``bb`` = Firmware Major Version
                                    - ``cc`` = Firmware Minor Version
device     .. _m6-84:   0x84 9      ``0xaa 0xBB 0xbb 0xCC 0xcc 0xdd 0xee 0xff``

           indState_                - ``aa`` = ready (0 = true, other values = false)
                                    - ``BBbb`` = `int` left hall sensor value
                                    - ``CCcc`` = `int` right hall sensor value
                                    - ``dd`` = carriage type:

                                      - ``0`` = no carriage detected
                                      - ``1`` = Knit carriage
                                      - ``2`` = Lace carriage
                                      - ``3`` = Garter carriage
                                    - ``ee`` = carriage position (needle number)
                                    - ``ff`` = carriage direction:

                                      - ``0`` = direction not known
                                      - ``1`` = Left
                                      - ``2`` = Right
host       .. _m6-04:   0x04 1      Request hardware test operation

           reqTest_
device     .. _m6-C4:   0xC4 2      ``0xaa``

           cnfTest_                 - ``aa`` = success (0 = true, other values = false)
host       .. _m6-26:   0x26 1      Hardware test command requesting help on available commands.
                                  
           helpCmd_               
host       .. _m6-27:   0x27 1      Hardware test command requesting that the device 
                                    send a test packet consisting of three bytes, 0x01 0x02 0x03.
           sendCmd_               
host       .. _m6-28:   0x28 1      Hardware test command requesting that the device beep. 
                                  
           beepCmd_               
host       .. _m6-29:   0x29 1      Hardware test command requesting that the device read the 
                                    EOL (end of line) Hall sensors and the position encoders.
           readCmd_               
host       .. _m6-2A:   0x2A 1      Hardware test command requesting that the device read the 
                                    EOL sensors and position encoders once per second, sending
           autoCmd_                 a testRes_ message reporting the information each time.
host       .. _m6-2B:   0x2B 1      Hardware test command requesting that the device test the 
                                    solenoids by activating odd and even sensors alternately,
           testCmd_                 once per second.
host       .. _m6-2C:   0x2C 1      Hardware test command requesting that the device quit 
                                    hardware test mode and return to normal operation.
           quitCmd_               
host       .. _m6-2D:   0x2D 3      ``0xaa 0x0b``

           setCmd_                  - ``aa`` = index of solenoid to set
                                    - ``b``  = solenoid value (0 = unset, 1 = set   
device     .. _m6-EE:   0xEE var    A string containing hardware test information.
                                  
           testRes_                 The length is variable. The string terminates with 0.
device     .. _m6-9F:   0x9F var    A debug string.
                                  
           debug_                   The length is variable. The string terminates with 0.
========== ============ ==== ====== ==============================================================
