# EtherCAT master at PSI
This is the EtherCAT master implementation that is currently used at PSI.
It includes the official EtherLab IgH Master 1.5.2 with addition of all 
community patches, and some PSI fixes.

### Hardware & OS
The master was tested on:

* Intel CPU (XEON)
* Intel I210 & I350 network cards
* Debian 10 with PREEMPT-RT patched kernel 4.19

### Building
Before building, ensure that all required software to build kernel modules and libraries is present.

    apt install build-essential dkms automake tree linux-headers-rt-amd64

After downloading the repo:

    cd ethercat
    ./bootstrap
    ./configure --enable-cycles=yes --enable-igb --enable-generic --disable-8139too --disable-e100 --disable-e1000 --disable-e1000e --disable-r8169 --disable-ccat --enable-static=yes --enable-shared=yes --enable-eoe=no --enable-hrtimer=yes --enable-regalias=no --enable-tool=yes --enable-userlib=yes --enable-sii-assign=yes --enable-rt-syslog=yes
    make all modules
    make modules_install install
    depmod

### Usage
      TODO

#### Use ec_igb on the specific port only
In case only one port should run the dedicated driver this can be done:

1. Find the BDF of the port we want to replace the driver (usual format XX:XX.X) using

        lspci

2. Unbind the current driver

        echo -n "BDF number" > /sys/bus/pci/drivers/igb/unbind

3. Add the ethercat master module

        modprobe ec_master main_devices=$(MAC_ADDRESS)

4. Add the dedicated network driver

        modprobe ec_igb

### Test
In order to test if the ethercat master is running type:

        ethercat master