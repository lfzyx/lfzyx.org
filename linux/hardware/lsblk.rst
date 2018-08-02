lsblk
=======

以树状图显示系统支持的设备

–m 显示设备的所有者和挂载模式
NAME – the device name
MAJ:MIN - Every device on a Linux operating system is represented by a file, for block (disk) devices, they describe the device using major and minor device numbers.
RM – removable device – shows 1 if this is a removable device and 0 if it’s not
TYPE – the device type
MOUNTPOINT - the location where the device is mounted
RO – it will display 1 for read-only filesystems and 0 for those that are not read-only
SIZE – the size of the device