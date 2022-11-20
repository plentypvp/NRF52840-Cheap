# NRF52840-Cheap

A cheap way for NRF52840 development.
Uses inexpensive ST-Link V2 USB in-circuit debugger.

0. Buy this board:
https://wiki.makerdiary.com/nrf52840-mdk-usb-dongle/   
Or make another yourself according to reference circuitry on Nordic Semiconductor website.
1. Buy ST-Link V2 USB debugger.
2. Connect SWDIO, SWDCLK and GND pins.
Power up the target.
3. Install all from:
https://wiki.makerdiary.com/nrf52840-mdk/nrf5-sdk/
4. OpenOCD from:
https://4k2.de/microcontroller/openocd-flashing-nrf52/   

git clone https://git.code.sf.net/p/openocd/code openocd    
cd openocd    
./bootstrap    
mkdir build; cd build    
../configure --enable-cmsis-dap --enable-openjtag --prefix=/opt/openocd    
make    
sudo make install    

5. We are NRF52840 (PCA10059 S140), use that hex in examples.
6. Type:    
openocd -f interface/stlink-dap.cfg -f target/nrf52.cfg -c "telnet_port pipe;tcl_port disabled;gdb_port disabled;log_output /dev/null"    

And use following commands:    
nrf52.dap apreg 1 0x04 1    
init    
reset halt    
nrf52.dap apreg 1 0x04 1    
nrf5 mass_erase    
program /home/plentypvp/nRF5_SDK_17.1.0_ddde560/examples/ble_peripheral/ble_app_blinky/hex/ble_app_blinky_pca10059_s140.hex verify    
reset    

7. Test again with:    
openocd -f interface/stlink-dap.cfg -f target/nrf52.cfg -c "init" -c "nrf52_recover" -c "reset halt" -c "nrf5 mass_erase" -c "program /home/plentypvp/nRF5_SDK_17.1.0_ddde560/examples/ble_peripheral/ble_app_blinky/hex/ble_app_blinky_pca10059_s140.hex verify" -c "reset" -c "exit"    

8. Brick it with:    
openocd -f interface/stlink-dap.cfg -f target/nrf52.cfg  -c "init" -c "reset halt" -c "nrf5 mass_erase" -c "program /home/plentypvp/nRF5_SDK_17.1.0_ddde560/examples/ble_peripheral/ble_app_gatts_c/hex/ble_app_gatts_c_pca10056_s140.hex verify" -c "reset" -c "exit"    

9. Unbrick with step 7.    

10. Go to:    
nRF5_SDK_17.1.0_ddde560/examples/ble_peripheral/ble_app_blinky/pca10059/s140/armgcc    
And type:    
make    

11. Flash with:    
openocd -f interface/stlink-dap.cfg -f target/nrf52.cfg -c "init" -c "nrf52_recover" -c "reset halt" -c "nrf5 mass_erase" -c "program /home/plentypvp/nRF5_SDK_17.1.0_ddde560/components/softdevice/s140/hex/s140_nrf52_7.2.0_softdevice.hex" -c "program /home/plentypvp/nRF5_SDK_17.1.0_ddde560/examples/ble_peripheral/ble_app_blinky/pca10059/s140/armgcc/_build/nrf52840_xxaa.hex verify" -c "reset" -c "exit"

(For compiled app with softdevice)    

Tested on Ubuntu 22.04.1 LTS    
