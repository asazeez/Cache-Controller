# Cache Controller
Cache Controller Project for COE758 Digital Systems Engineering.

## Abstract
The main goals of this project were to understand how a cache controller works and how it can
access data from the SRAM memory (used for the cache) and SDRAM controllers (main
memory). The project aimed to build skills in VHDL coding using the Xilinx ISE
CAD environment, with the Xilinx Spartan-3E FPGA as the hardware platform.

## Design
For this project, the cache can store 256 bytes of data. It is structured into 8 blocks, each holding 32 one-byte words. The CPU communicates with the cache controller by sending 16-bit
addresses, which are divided into the tag, index, and offset. The Cache Controller uses this
address information to compare the tags of the cache blocks, check the valid and dirty bits, and
determine the appropriate action for each read or write request.

The behavioral operation of the Cache Controller can be divided into four main cases:

1. Write a Word to Cache (Cache Hit): When the CPU issues a write request for data
already stored in the cache, it is categorized as a cache hit. The Cache Controller uses the
index and offsets from the CPU's address to locate the correct position in the SRAM. The
new data is then written to this location, and both the dirty and valid bits for the
corresponding block are set to 1. This indicates that the data has been modified and the
cache block has been used.

2. Read a Word from Cache (Cache Hit): If the CPU requests to read data that exists in
the cache, the Cache Controller identifies it as a cache hit. It retrieves the data using the
index and offset from the CPU's address and sends the requested data back to the CPU.

3. Read/Write from/to Cache (Cache Miss, Dirty Bit = 0): If the dirty bit is 0, the data in the cache has not been modified, thus the Cache Controller can replace the old block with the new block from the main memory. The CPU address is sent with the initial offset set to “00000” to read the entire 32-byte block. This block is then written to the SRAM, and the tag register is updated with the new tag from the CPU address. The valid bit is set to 1, and the procedure for a hit operation is performed.

4. Read/Write from/to Cache (Cache Miss, Dirty Bit = 1): When a cache miss occurs and
the dirty bit is set to 1, the Cache Controller must first write the modified data back to the
main memory. The data block from the SRAM is written to the following address: tag &
index & 00000. After writing back, the Cache Controller retrieves the new block from the
main memory using the same address format. The cache is then updated with the new
data block and the original CPU request is executed.

### State Machine Diagram
<div align="center"><img src="https://github.com/user-attachments/assets/5f5752c0-7da8-4a3d-863d-a68452af12dc" width="700"/></div>

## Qualitative Results
<div align="center"><img src="https://github.com/user-attachments/assets/ccbfc0d9-76bb-4da2-9790-35cdaafdccd7" width="500"/></div>

