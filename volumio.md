# Setting up Volumio on a Raspberry Pi 4 #

## Prerequisites ##

  * Raspberry Pi 4
  * USB C power connector
  * Micro SD Card
  * (Optional) HDMI cable (**Caution:** The Pi output is HDMI Micro)
  * (Optional) External HDD as storage device
  
## Flashing the SD Card ##

  * Get the Volumio image: https://volumio.org/get-started/
  * Unzip
  * Flash to SD card:
    * **Make sure you know the device name:** The command `fdisk -l` should give enough info to figure it out.
      Usually, the name is `/dev/mmcblk0` if it's the first such device.
      **Note:** Do *not* use a partition on the device, e.g., `/dev/mmcblk0p1`; that will not work.
    * Run the command `dd if=<volumio image file> of=<SD card device>`.
      It may take a while to complete.
  * You now have a bootable Volumio SD card.

## Enable SSH ##

**Important:** This step may only work at this early stage.
I haven't seen a possibility to enable SSH through the web UI.

  * Mount the `boot` partition of the SD card.
    There should also be `volumio` and `volumio_data`.
  * On the `boot` partiion, create an empty file named `ssh`.
    This will let the startup script enable the SSH daemon.
  * The initial credentials are `user: volumio, password: volumio`.
    Don't forget to change them on first login, or even better, to disable password login and instead log in with a public SSH key.

## Set up HDD ##

  * If necessary, format the hard disk with a Linux filesystem, e.g., EXT4.
    The command for this is `sudo mkfs.ext4 /dev/<hard disk device>`.
    The device name is likely `/dev/sda1` or similar for a hard disk, and `/dev/nvme0n1p1` or similar for SSDs.

## First Start ##

  * Plug all devices into the Raspberry Pi that should be connected to the music server, e.g., DAC, hard disk, network cable, ...
  * Start the Pi.
    The first start will take a while due to some tasks that need to be performed only once.
  * Find out the Pi's IP address.
    This is most easily done by logging into the Router's admin interface and checking the logs for `volumio`.
    **Note:** It's a good idea to assign the Pi a fixed IP address.
  * Type the IP address into the browser's address bar.
    This should open a wizard for the first setup.
  * **Important:** For a USB DAC, keep the `I2C DAC` button switched *off*.
    I2C DACs use a different data format that is not compatible with the normal PCM data format.
  * Instead, the USB DAC (e.g. *irDAC II*) should be listed as one choice in the dropdown menu.
    Select it.
  * That's it.
    Now the Pi should be all set up to serve music!
    
## Miscellaneous ##

  * Copying data is most easily done via SSH.
    Just copy the music files to the storage disk and press the *Update* button in the *Sources* configuration menu.
    They should be discovered automatically.
  * At the time of writing this, there is no way to configure the time zone of the Pi on the web UI.
    The simplest way of changing the timezone is with the command `sudo dpkg-reconfigure tzdata`.
  * To check that the valuable high-definition music files are really handled well, one can turn on `logging="verbose"` in `/etc/mpd.conf` (might need a restart to take effect) and search for `alsa` in `/var/log/mpd.log`.
    The file will have information about input and output sample rates and bit depths.
    (Apparently, `/etc/mpd.conf` gets reset after reboot, but the changes still take effect...)
  * There's a volumio app for Android or iPhone.
    It can perform the same tasks as the web UI and works nicely (so far).
    Downside: It costs a bit of money (Android 2,39 Euro, iOS 2,29 Euro)
