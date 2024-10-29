# dlp-io20

The DLP-IO20 is a simple USB data acquisition module which permits to receive or send TTL signals. From a software point of view, it appears as a serial device which can be controlled by writing and reading characters.

A full description of the device is available at <https://www.dlpdesign.com/usb/dlp-io20-ds-v11.pdf>

It can be bought, for example, at [rs-online](https://co-en.rs-online.com/product/dlp-design/dlp-io20/70372088/)


## Installation

### Linux 


* To use it under Python, you need to install `pyserial`:

       pip install pyserial

* Add yourself to the `tty` and `dialup` groups:

       sudo usermod -a -G tty $USER
       sudo usermod -a -G dialout $USER 

* Load the following drivers:

       sudo modprobe ftdi_sio
       sudo modprobe usebserial

* run `sudo dmesg -w` and plug the DLP into a usb port of your computer. You should see a message such as:


      FTDI USB Serial Device converter now attached to ttyUSB0


This means that the DLP is accessible on `/dev/ttyUSB0` 




## Usage 

### Example 1


```python
    from serial import Serial
    from time import sleep
    
    dlp = Serial(port='/dev/ttyUSB0', baudrate=115200)  # open serial port

    # Check that the board is alive
    FLASH_LED = 0x28
    dlp.write(bytearray([2,FLASH_LED]))  # blink LED D1

    DIGITAL_IO = 0x35
    DIR_OUTPUT = 0
    DIR_INPUT = 1
    SET_LOW = 0
    SET_HIGH = 1

    # Program Channel AN2 as digital output
    dlp.write(bytearray([5, DIGITAL_IO, 2, DIR_OUPUT, SET_HIGH]))
    sleep(0.5)
    dlp.write(bytearray([5, DIGITAL_IO, 2, DIR_OUPUT, SET_LOW]))
    sleep(0.5)

    # Program Channel AN0 as digital input
    dlp.write(bytearray([5, DIGITAL_IO, 0, DIR_INPUT, 0]))
    print(dlp.read())
```    



