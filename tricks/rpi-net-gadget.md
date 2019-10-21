
You can also pass the kernel parameters for static MAC addresses to the g_ether module in the cmdline.txt
I prefer this method because it's on the FAT boot partition and easily added when enabling gadget mode.

Code: Select all

modules-load=dwc2,g_ether g_ether.host_addr=00:22:82:ff:ff:20 g_ether.dev_addr=00:22:82:ff:ff:22

g_ether.dev_addr is the Pi Zero interface.
g_ether.host_addr is the host PC interface.

