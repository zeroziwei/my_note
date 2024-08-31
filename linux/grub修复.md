
Use the following steps to restore the GRUB 2 boot loader:

1. Boot from Live media such as GParted Live or your GNU/Linux distribution image. Open a terminal window.
    
2. Determine which partition contains the / file system for your GNU/Linux distribution.
    
    Use GParted to list the partitions on your disk device. Look for a partition that contains your GNU/Linux / file system. This Linux partition will likely use a file system such as ext2, ext3, ext4, or btrfs.
    
    If the / partition is on LVM then the Logical Volume Manager must be active. LVM can be started with the command:
    
    ```
    # vgchange -a y
    ```
    
    With LVM, the equivalent of a disk partition is a Logical Volume. Logical Volumes can be listed with the command:
    
    ```
    # lvscan
    ```
    
    If the / partition is on RAID, then the RAID must be active. Linux Software RAID can be started with the command:
    
    ```
    # mdadm --assemble --scan
    ```
    
3. Create a mount point directory by entering (as root):
    
    ```
    # mkdir /tmp/mydir
    ```
    
4. Mount the / partition on the mount point directory. For example assume the / file system is contained in the /dev/sda5 partition. Enter (as root):
    
    ```
    # mount /dev/sda5 /tmp/mydir
    ```
    
5. If you have a separate /boot partition, for example at /dev/sda3, then an extra step is required. Mount the /boot partition at /tmp/mydir/boot by entering (as root):
    
    ```
    # mount /dev/sda3 /tmp/mydir/boot
    ```
    
    If you do not know whether you have a separate boot partition then you probably do not and can ignore this step.
    
6. Prepare to change the root environment by entering (as root):
    
    ```
    # mount --bind /dev /tmp/mydir/dev
    ```
    
    ```
    # mount --bind /proc /tmp/mydir/proc
    ```
    
    ```
    # mount --bind /sys /tmp/mydir/sys
    ```
    
7. Change the root environment by entering (as root):
    
    ```
    # chroot /tmp/mydir
    ```
    
8. Reinstall GRUB 2 on the boot device. Note that the device name is used and not the partition name. For example, if the / partition is /dev/sda5 then the device is /dev/sda.
    
    For Debian, Ubuntu, and other offshoot GNU/Linux distributions, enter the command (as root):
    
    ```
    # grub-install /dev/sda
    ```
    
    For CentOS, Fedora, openSUSE and other offshoot GNU/Linux distributions, enter the command (as root):
    
    ```
    # grub2-install /dev/sda
    ```
    
9. Exit the chroot environment by entering (as root):
    
    ```
    # exit
    ```
    
10. Reboot your computer.