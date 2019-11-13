## these are instructions for hardware version 1!

### install dependencies
```
apt-get install raspberrypi-kernel-headers
```

### build module
```
make -C /lib/modules/$(uname -r)/build M=$PWD
```

### install module
```
sudo make -C /lib/modules/$(uname -r)/build M=$PWD modules_install
sudo depmod -a
```

# compile and install device tree overlay
```
sudo dtc -@ -I dts -O dtb -o /boot/overlays/mcp2517fd-can0.dtbo mcp2517fd-can0.dts
sudo dtc -@ -I dts -O dtb -o /boot/overlays/mcp2517fd-can1.dtbo mcp2517fd-can1.dts
```

# activate device tree overlay
```
sudo sh -c 'echo "dtparam=spi=on" >> /boot/config.txt'
sudo sh -c 'echo "dtoverlay=mcp2517fd-can0,oscillator=40000000,interrupt=25" >> /boot/config.txt'
sudo sh -c 'echo "dtoverlay=mcp2517fd-can1,oscillator=40000000,interrupt=5" >> /boot/config.txt'
sudo sh -c 'echo "dtoverlay=spi-bcm2835-overlay" >> /boot/config.txt'
```
