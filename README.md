# Tbps_CRC
A SytemVerilog implementation of Cyclic Redundancy Check runs at up to Terabits per second. 

## Core HDL code for CRC calculation
The core code of Tbps CRC is located in the `core_src` directory. There are 3 files in the folder: `crc.sv` contains the SystemVerilog module, which computes the CRC without the byte-enabled feature; `crc_byteEn.sv` contains the byte-enabled version of the SystemVerilog module; `crc.svh` contains SystemVerilog constant functions that run at elaboration (pre-synthesis) time.

The byte-enabled version differs from the normal version in that the byte-enabled version allows the message length to be any length that is byte-aligned, while the normal version can only produce CRC results for messages whose size is a multiple of the selected input bus width.

## Correctness test
The correctness test is under directory `correctness_verification`. There are 4 make options for the correctness test: 

`make sim` and `make sim_byteEn` will generate and run simulations to verify the correctness of the normal version and the byte-enabled version respectively

`make gen_bitstream` and `make gen_bitstream_byteEn` will generate the bitstreams for the normal version and the byte-enabled version respectively

One should modify the `board_clk_source.xdc` and `config.mk` for different CRC configurations and different target boards.

Both simulation and on-board tests generate random messages of a given random size and feed the messages into the CRC module. The simulation compares the results generated by the hardware with the results computed by a software function and reports the mismatch (if any) between the software and hardware results. The on-board test uses a set of hardware random number generators to generate random messages and feed them into the CRC module. The message signal and the CRC result signal are probed by the ILA, allowing the results to be verified in hardware.

## Performance test
The performance test is under directory `performance_evaluation`. Same as the correctness test, there are 4 make options for the performance test:

`make find_fmax` and `make find_fmax_byteEn` find the Fmax for the non-pipelined CRC implementations for an assortment of commonly used CRC polynomials.

`make find_pipe_lvl_500mhz` and `make find_pipe_lvl_500mhz_byteEn` find the minimum pipeline level implementation that can run at 500 MHz for different CRC polynomials.

The summarized results can be found under /performance_evaluation/outputs.

**The default configuration of the test is based on Xilinx's Alveo U250. User should modify the `config.mk` and the files under `constraints` for a different target board.**

##References
To cite this work, reference the following paper:

Q. Clark Shen, J. C. Vega and P. Chow, "Parallel CRC On An FPGA At Terabit Speeds," 2022 International Conference on Field-Programmable Technology (ICFPT), Hong Kong, 2022, pp. 1-6, doi: 10.1109/ICFPT56656.2022.9974233.
