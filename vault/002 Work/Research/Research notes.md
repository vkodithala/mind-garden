#### Socket-based vs. shared memory approaches to Hw reconfigurations
- Paper develops a remote sensing instrument for atmospheric temperature measurements
	- With in-flight reconfiguration, which is of interest to us
- Scientific long-term measurements with remote sensing/imaging instruments have become important for climate system modeling
- In their design, OBC handles all comms with GCS (routed via direct network connection)![[Screenshot 2025-04-22 at 8.58.26 AM.png]]
	- Forwards config packages, commands, and controls to each payload using physical layer technologies like CAN bus, serial
- Xilinx SRAM-based SoM is analogous to an FPGA
	- First-stage bootloader (FSBL) stored in on-chip memory
	- PL (programmable logic) bitstream contains configuration data, loaded into SRAM for reconfiguration
	- Processor executable/ELF, containing kernel or firmware
- Size of FSBL is just a few KB, but size of PL/PS ELF are application dependent
	- Modified FSBL implementation has BOOT.bin with only FSBL rather than all 3 components
	- Configuration bit stream/application code are placed in separate files
	- But I'm not sure how this relates to our work?
- Sockets result in 2 copies of the data: from sending process to kernel buffer, and then kernel buffer to receiving process
	- W/a in shared memory, we only make 1 copy of the data: from the process to the shared memory
	- Shared memory = more involved because you need to worry ab synchronization, though
	-  But I think in terms of speed, using shared memory to communicate reconfiguration data is a clear winner
	- And you also need a signaling mechanism, which can be slow
- Exchanges of small chunks of data (1MB/sec) showed no clear advantage for shared memory, but with larger amounts of data, CPU consumption scaled
	- `read`/`write` syscalls aren't cheap, and we use these profusely with sockets
- Why do sockets incur higher CPU consumption overhead?
	- Data is copied multiple times: sender user space to kernel space
		- Then kernel space to user space of receiving proc
	- Leads to >= 2 memory copies/message
		- W/a in shared memory, both sender/receiver access the same memory region, so no copies are needed
	- Also every socket send/recv involves several system calls, whereas with shared memory, processes access memory directly without kernel involvement/context switches
- We want to make sure that shared memory isn't flushed to disk on every write
	- We don't need persistence, and this is super costly
- What actually needs to be shared between OBC <> sensors
	- Full bitstreams (multiple MB) vs. partial reconfiguration bitstreams (smaller, more efficient)
		- Useful for FPGAs
	- Or just sensor parameter updates (frame rates, exposure times) - < 1KB
	- Some data is small, like cmd packets, and some large, like FPGA bitstreams
- Shared memory requires shared physical RAM region, which is only possible when both devices are part of the same SoC complex (configuration 1)
- 2 main sources of overhead in sensor reconfiguration:
	- Time needed to transmit configuration commands from controller to sensor
	- Time needed for the sensors to process those commands
- High level integration combines SoC, memory, and peripherals on a SoM (System-on-Module)
	- OBC handles communications with GCS, forwards configuration packages/commands/controls to each payload
	- BIT file used to implement real-time sensor control, image processing, or DMA
	- ELF file reconfigures the CPU software
	- Configuration package (boot image) consists of FSBL, BIT, and ELF in a BOOT.sim file
	- Firmware stored across 3 memory devices: QSPI Flash, SD card, eMMC
- Initialization, upload phase, validation phase, and programming phase
	- Initialization phase informs instrument about reconfiguration and sends total length of new firmware image
	- In upload, firmware image transferred to internal upload buffer, each data block being secured with a checksum for retransmission
	- In validation, MD5 sent to instrument, which further validates (but not sure what this is?)
		- Validation of transferred firmware
	- Then, programming message now initiates transfer of internal upload buffer to config memory
- In-flight firmware updates
	- New configuration with associated MD5 hash is transferred from OBC to eMMC (eMMC = transfer memory)
		- Verified against locally calculated MD5 for good transfer
	- Then, duplicated in QSPI memory and then on SD card
- Boot image stored in 3 different configuration memories
	- QSPI = primary, SD-Card = secondary, eMMC = Golden image
	- For reliability