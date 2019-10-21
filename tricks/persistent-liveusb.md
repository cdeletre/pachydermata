## 


The easiest way to create USB Live CD with persistence is to manually form the partitions using GParted.

This is what I've done:

    Format your USB with GPT partition table (though, I believe MBR should work fine as well).
    Create two partitions. First is for ISO files and it should be FAT32 (~1100MB for Ubuntu 14.04). Second is for persistence and it should be ext4 with label casper-rw.
    Copy files from your ISO (including hidden) to USB FAT32 partition.
    Edit boot/grub/grub.cfg and just add the word persistent (This is the reason why persistence doesn't work for you in UEFI mode.):

    menuentry "Start Kubuntu" {

              set gfxpayload=keep

              linux   /casper/vmlinuz.efipersistentfile=/cdrom/preseed/kubuntu.seed boot=casper maybe-ubiquity quiet splash --

              initrd  /casper/initrd.lz

          }

It might be a little slow when you first start it especially if you use USB2.0.

