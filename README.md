smartos-platform-upgrade
========================

A script to simplify upgrades of USB booted SmartOS installations. It's using the
SkyLime mirror with the [arekinath smartos-live](https://github.com/arekinath/smartos-live) image.

Installation
------------

```
[root@acro ~]# curl -OkL https://raw.githubusercontent.com/drscream/smartos-platform-upgrade/master/platform-upgrade
[root@acro ~]# chmod 755 platform-upgrade
```

Optionally edit the script to select the desired mirror/download site at the top.

Usage
-----

Run the script. It'll find the USB stick, download the latest platform
from Joyent, do the upgrade and report its status. The download and
update platform stages will probably take a few minutes each. Kernel and
boot archive checksums are verified to make sure the files can be read
correctly from the USB stick.

```
[root@acro ~]# platform-upgrade
Checking current boot device... detected /dev/dsk/c0t0d0p1, mounted, OK
Downloading latest platform... OK
Verifying checksum... OK
Extracting latest platform... OK
Updating platform on boot device... OK
Remounting boot device...  OK
Verifying kernel checksum on boot device... OK
Verifying boot_archive checksum on boot device... OK
Activating new platform on /dev/dsk/c0t0d0p1... OK

Boot device upgraded. To do:

 1) Sanity check the contents of /tmp/tmp.Tsas6L/usb
 2) umount /dev/dsk/c0t0d0p1
 3) reboot
[root@acro ~]#
```

Once the upgrade is done, verify that the contents of the USB stick looks sane,
unmount it and reboot to the new platform.

```
[root@acro ~]# ls -l /tmp/tmp.KCaawL/usb
total 24
drwxrwxrwx   1 root     root        4096 Jan  3  2011 boot
drwxrwxrwx   1 root     root        4096 Dec 28 16:03 platform
drwxrwxrwx   1 root     root        4096 Dec 28 15:24 platform.old
[root@acro ~]# ls -l /tmp/tmp.KCaawL/usb/platform
total 9
drwxrwxrwx   1 root     root        4096 Dec 28 16:03 i86pc
-rwxrwxrwx   1 root     root          17 Dec 28 02:40 root.password
[root@acro ~]# umount /dev/dsk/c0t0d0p1
[root@acro ~]# reboot
```

By default, the first removable device found is assumed to be the boot USB
stick. If this is not the case, the correct device name can be given on the
command line:

```
[root@acro ~]# platform-upgrade /dev/dsk/c1t0d0p1
Checking current boot device... using /dev/dsk/c1t0d0p1, mounted, OK
Downloading latest platform...
```

License
-------

MIT
