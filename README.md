# Pico Timing Box

This the repository for (almost) all things related to the Raspberry Pi Pico powered timing boxes that are used by the [SPIM and spimGUI](https://github.com/jmtayloruk/spim-interface.git) to schedule the triggering of cameras and lasers etc for prospective optical gating. These pico based timing boxes mimic the existing xmos-xs1 based timing boxes in the software (timing box type 3/4) in the spimGUI config files albeit they only feature 4 BNC outputs. Two of the outputs are permanently wired to a $10k\Omega$ impedance, and two are switchable between the high impedance ($10k\Omega$) mode and a low impedance mode ($50\Omega$) via a pair of switches in side the timing box. We originally had 10 units produced by European circuits however due to some mis-communications they need to be modified to be full operational (see the relevant file)

## The Pianola Concept
To synchronise the triggering of different cameras, lasers, etc this means trigger pulse width and delay is dependent on what needs to be triggered. The timing box can be used like an old player piano (pianola) where sets of triggers can be programmed and played back to ensure we trigger a laser and cameras in synchrony.

## Communication Protocol

The timing box communicates with a host PC using a standard 8n1 serial protocol using a FTDI USB-to-serial cable and a series of commands at a baud rate of 115200. Each command sent to the timing box consists of a 32bits and is composed of a opcode, of 8 bits, and 24 bits of other data. Some commands give responses and where appropriate below these are denoted as a call and a response to keep to the musical theme.

## Command Set 
- SET_Pianola => 0x01
- SET_PianolaFinalPos => 0x02
- SET_PianolaRepeatFrom => 0x03
- SET_PianolaRepeating => 0x04
- RUN_Pianola => 0x05
- SET_PianolaFireTime => 0x06
- IRQ_StopAndReset => 0x7
- GET_CurrentPianolaTime => 0x08
- SET_PinSource => 0x09
- GET_PinSource => 0xA
- SET_CameraClk => 0xB
- SET_PIVParams => 0xC
- SET_ClockDivisor => 0xAB
- GET_FirmwareVersion => 0xFD
- IRQ_DumpLog => 0xFE
- IRQ_HARDRESET => 0xFF

Below the action of each command in the protocol is defined as well ad the format of call and the response.

### SET_Pianola => 0x01

Set the address of the pianola with the mask of outputs to drive high (0) or (1) and the duration to drive high for. Call : CMD, pianola address, pianola pin mask, pianola duration (3bytes) = 6 bytes Response : none

### SET_PianolaFinalPos => 0x02

Sets the address of the final instruction in the pianola. Call : CMD, pianola address = 2 bytes Response : none

### SET_PianolaRepeatFrom => 0x03

Sets pianola repeating from given address. Repeating can be stopped using the SET_PPianolaRepeating command. Call : CMD, repreat address = 2 bytes Response : none

### SET_PianolaRepeating => 0x04

Sets flag to repeat pianola. (0x01) to repeat until heat death of the universe and (0x00) to stop repeating. Call : CMD, repeat flag = 2 bytes

### RUN_Pianola => 0x05

Sets pianola running and runs once. Call : CMD = 1 byte Response : pianola clock time = 3 bytes

### SET_PianolaFireTime => 0x06

Configure pianola to run once at a future time. Returns pianola time and a flag to signify if pianola will fire in the future. Call : CMD, future fire time = 4 bytes Response : fire flag, pianola time = 4 bytes

### IRQ_StopAndReset => 0x7

Stops pianola running if it is and cancels any scheduled fire times. Pianola instructions remain intact. Call : CMD Response : none

### GET_CurrentPianolaTime => 0x08

Gets the current clock time from the timing box. Call : CMD Response : current pianola time = 3 bytes

### SET_PinSource => 0x09

Sets which pins are outputs and intputs. This is mainly for older version of the timing box with configurable inputs and outputs but is implemented for posterity. Call: CMD, pin index to set, bit index that pin is mapped to, flag if that bit is to be inverted = 3 bytes Respnse : none

### GET_PinSource => 0xA

Get the configuraion for a queried pin. Call : CMD, pin index to query = 2 bytes Response : Bit index its mapped to, flag indicating if the output should be inverted = 2 bytes

### SET_CameraClk => 0xB

Sets the clock rate for the additional clocks in terms of the half period in pianola ticks (2.56us). Call : index of clock, clock period (3 bytes) = 4 bytes Response : none

### SET_PIVParams => 0xC

Configures the various parameters for PIV. Call: camera index, interval in ticks betweeen laser pulse pairs (four bytes), duration in ticks of laser pulse pairs (four bytes), interval in ticks between the end of the first laser pulse and end of first camera exposure (four bytes), duration of camera exposures (four bytes), Bit index of the camera exposuer mask to be montiored as part of the PIV pulse sequencing = 18 bytes Response : none

### SET_ClockDivisor => 0xAB

Sets the tick rate of the triggering clock by dividing the cpu clock speed (125MHz) by a fixed-point integer with 16 integral bits and 8 fractional bits. The pianola clock divisor is set as this value divided by 10 - clock takes 10 cpu ticks to decrement. There is a miniscule loss of precision on certain value but this should be fine. TO DO - this should maybe be rolled into the SET_CameraCLK command? Call : CMD, clock divisor integral (2 bytes), clock divisor fraction = 4 bytes Response : none

### GET_FirmwareVersion => 0xFD

Returns the current firmware version and the earliest firmware version that is backwards compatible. Call : CMD = 1 byte Response : current firmware version, earliest compatible version = 2 bytes

### IRQ_DumpLog => 0xFE

Returns the contents of the debug buffer. This is of variable length and dependent on what it is you want to debug. TO DO: maybe instead of returning CHAR arrays it should instead print to the serial monitor?

Call : CMD = 1 byte Response : how longs a piece of string?

### IRQ_HARDRESET => 0xFF

Like the IRQ_StopAndReset command but the nuclear option. Clears any pending fires, the pianola instructions, and resets other flags / values to their defaults

# To Do
A short list of things that don't really interfere with the ability for the pianola to operate but are features present on other timing boxes that haven't been implemented.
1. I never actually implemented the pin remapping function. As such, the various drop-down menues on the electronics setup do nothing for remapping or inverting the BNC's.
2. Related to number 1 but in the spim-interface, I had intended to implement by own timing box type (and in fact there is a branch for this) that would present the correct output list in the electronics set up etc. This isn't a massive job - I just ran out of time.
