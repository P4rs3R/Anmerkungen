# Install (and configure) a MISP Virtual Machine
Download a VM (test only!) here: 
(Suggested: upgrade compatibility to at least Workstation 12.x, in order to have access to more functionalities).
Then:
## Increase your HD
- Expand HD (via VMware)
- Boot VM and increase partition this way:

- `sudo fdisk /dev/sda`
- `p` to list the partitions. Make note of the start cylinder of /dev/sda1 [e.g.: 2048]
 `d` to delete first the partition
- `n` to create a new primary partition 
- `p` (default) for primary partition type
- `1` (default) as partition number
- [now check that the number of the start cylinder is the same as previously shown then press enter, otherwise type that number]
- [For the last sector, you can leave the default number, tipically the last one possible]
- Now the Linux partition is created
-`N` -important: do NOT remove the LVM2_member signature, otherwise next boot will load the grub recovery utility in order to find the bootable partition
- `a` to toggle the bootable flag on the new /dev/sda1
- `w` to write on disk
- `sudo reboot`
- `sudo resize2fs -f /dev/sda1` <-- this may not work, so:
- `sudo pvs` to check PSize and PFree
- `sudo pvresize /dev/sda1`   ----resize the physical volume
- `sudo lvextend -r -l +100%FREE /dev/mapper/misp--vg-root`
- `sudo fdisk -l` to check that everything is fine

## Export OVF and deploy it on ESXi

## Configure network interface
- `ifconfig` so, if there's no network interface listed, add network interface:
- `dmesg | grep -i network` to check the name of the interface [e.g.: eth0]

Add these lines in /etc/network/interfaces:
```
auto eth1
iface eth1 inet dhcp
```
---------restart networking (e.g.: sudo /etc/init.d/networking restart)
## Update MISP
After updating via web GUI (or git), maybe also necessary:
```
root@host:/var/www/MISP# git submodule update --init --force
```
