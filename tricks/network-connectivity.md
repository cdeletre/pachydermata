# Check ethernet connectivity with a host in the same local @network link.
Sometime a host is unreachable for various reasons.
First it's a good start to check if we can see it at link layer level. To do so, we can try to resolve its IP address into MAC address:

~~~~
ping -c 2 -I *interface* *IP*
arp -n | grep *IP*
~~~~
