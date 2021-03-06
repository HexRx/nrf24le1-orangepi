# nrf24le1-orangepi
A simple command-line interface with Nordic nRF24LE1 using 
a Orange Pi and the spidev driver.

# Pinout

![Orange Pi pinout](docs/500px-Opi_gpio.png)

Signal | Orange Pi | nRF24LE1 (QFN32)
--- | --- | ---
CS | SPI0_CS | P1.1
MOSI | SPI0_MOSI | P0.7
MISO | SPI0_MISO | P1.0
CLK | SPI0_CLK | P0.5
PROG | PA13 | PROG
RESET | PA14 | RESET

# Known issues

The spidev buffer size might be too small, fix with

`chmod u+w /sys/module/spidev/parameters/bufsiz`

`echo 65536 > /sys/module/spidev/parameters/bufsiz`


## Features
- Program memory read/write
- NVM memory read/write
- InfoPage handling

## Command Line Format

For further development, the tool should conform to the above protocol.

### Miscellaneous

`nrf24le1 show`

Show the FSR register and make sure we can modify it, this demonstrates proper
communication on the SPI bus to the nRF24LE1 module.

`nrf24le1 reset`

Reset the unit on the programmer, this resets the MCU to start the program afresh.

### Reading data from nRF24LE1

`nrf24le1 read infopage [filename]`

`nrf24le1 read firmware [filename]`

`nrf24le1 read nvm [filename]`

All read operations dump data to stdout by default, in Intel Hex format. 
It's possible to provide an optional filename as an argument.

When a filename is specified and the extension matches .hex or .ihx the 
dump will be saved in Intel Hex format, otherwise the binary format will 
be used.

### Writing data to nRF24LE1

`nrf24le1 write firmware [filename]`

`nrf24le1 write infopage [filename]`

`nrf24le1 write nvm [filename]`

All write operations expect data from stdin by default, in Intel Hex format.
It's possible to provide an optional filename as an argument.

When a filename is specified and the extension matches .hex or .ihx the 
dump will be saved in Intel Hex format, otherwise the binary format will 
be used.

### Additional Parameters:

(not yet implemented)

| Parameter          | Function                      |
| ------------------ | ----------------------------- |
| `--offset N_BYTES` | Skips N_BYTES bytes           |
| `--count N_BYTES`  | Read/Write only N_BYTES bytes |

# References

* Nordic nRF24LE1: <http://www.nordicsemi.com/eng/Products/2.4GHz-RF/nRF24LE1>
