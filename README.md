MOS6502.js
==========
This is an emulator for the MOS Technology 6502 microprocessor and a couple of its derivatives, written in JavaScript. It's been developed to serve as a component of an emulator for a larger system which incorporates a 6502 as its CPU.

The emulation is highly complete. Specifically, the emulator passes [Klaus Dormann's functional test](http://2m5.de/6502_Emu/index.htm).

Supported derivatives of the 6502 are the Ricoh 2A03 CPU (used in the Nintendo Famicom/NES) and technically the 6507. The 2A03 mode must be enabled; to use the 6507, you simply have the host system never attempt any interrupts or use any addresses higher than 0x1FFF.


Usage
-----
Using the emulator is fairly simple; just call the MOS6502 constructor, passing it an argument which is an object I call the emulator core as the first argument, and a collection of options as the second argument. The core object should contain the following functions:

    mem_read(address)

This function should return the byte at the specified memory address.

    mem_write(address, value)

This function should write the specified byte to the specified address.

There are no requirements placed on the core object except that those functions exist.

The second parameter to the constructor is an options object. The simulator will recognize the following properties on this object:

    nes_mode

This parameter enables emulation of the Ricoh 2A03 CPU. This means that the ADC and SBC instructions will ignore the decimal mode flag, always operating in binary mode, but the flag may still be set and cleared as normal.

The constructor will return an object containing the following three functions, which are the entire public interface to the Z80:

    reset()

Resets the processor. This must be called on power up, and may be called again later to perform a reset.

    run_instruction()

Runs the instruction the program counter is currently pointing at, advances the program counter to the next instruction, then returns the number of cycles that instruction took to execute. If an interrupt was triggered during this instruction, the cycles used to handle the interrupt will be included.

    interrupt(non_maskable)

Triggers an interrupt. non_maskable should be true if the interrupt is a non-maskable interrupt or false if it is a "normal" IRQ.

Known Issues
------------
The undocumented opcodes are not well tested, and a few of the particularly unstable ones are not implemented at all and will simply print messages to the console.

Several variants of the 6502, notably including the 65C02, are not supported.

Those are the only problems I'm aware of; let me know if you find any others.

License
-------
This code is copyright 2014 Matthew J. Howell and is made available under the MIT license. The text of the MIT license can be found in the LICENSE file in this repository.
