Acer Cloudbook BunsenLabs Install Guide
=======================================

This is a great little budget laptop that boots Debian like champ, if you can 
get it configured properly. After some headaches I am very happy with how this
system turned out considering I got mine on Amazon for only $157.

## Installing BunsenLabs

After creating a bootable USB, press F2 when the system starts up to access the
BIOS. In the boot menu change the boot mode from UEFI to Legacy, and move your
USB to the first position in the boot order list. Restarting the machine should
take you to the BunsenLabs LiveUSB menu.

Without a magic incantation of kernel parameters, the installer will not even
begin to boot. Press TAB while highlighting "install" to edit the install 
command. You will need to add the following parameters to the end of the 
install command:

```
edd=off noapic modprobe.blacklist=pinctrl_cherryview
```

The installer will run with only the `noapic` parameter, but the system will 
not boot after install unless you include all of them. Once past this point you
can follow the instructions to install the system as normal.

## Booting into BunsenLabs

When booting for the first time you will need to speak the incantation once
again. When the GRUB menu comes up, press `e` to edit the boot command. Go to
the line starting with `linux` and add the same three parameters to the end of
that line. Press F10 to boot into the system.

To avoid having to do this every time, we want to make these parameters part
of the boot sequence by default. Edit the file `/etc/default/grub` and change
the line with the `GRUB_CMDLINE_LINUX_DEFAULT` variable to look like this:

```
GRUB_CMDLINE_LINUX_DEFAULT="quiet edd=off noapic"
```

Then run `sudo update-grub`. Then create the file 
`/etc/modprobe.d/pinctrl_cherryview.conf` with the following contents:

```
blacklist pinctrl_cherryview
```

Reboot the system to confirm that it boots normally with no editing of the 
boot commands.

