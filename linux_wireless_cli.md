# Setting up a wireless network

## Generate a config file

1. Make sure a file `/etc/wpa_supplicant/wpa_supplicant.conf` has been created and is valid. A template is available under `/etc/wpa_supplicant/minimal.conf`.

2. Start the `wpa_supplicant` daemon by running the command

        sudo wpa_supplicant -B -i wlan0 -c /etc/wpa_supplicant/wpa_supplicant.conf

3. Start the `wpa_supplicant` command line interface by running `sudo wpa_cli`.

4. In the shell, you can now scan, add networks, manage existing networks etc. To add a new network, first scan with the commands

        > scan
        OK
        <3>CTRL-EVENT-SCAN-STARTED
        <3>CTRL-EVENT-SCAN-RESULTS
        > scan_results
        ...

   Now add a new network by typing

        > add_network
        0

   The returned number is the ID of the network. You can get a list of existing IDs and further properties by running

        > list_networks
        ...

   Next, set an SSID and a passphrase for the network with

        > set_network 0 ssid "MYSSID"
        OK
        > set_network 0 psk "passphrase"
        OK

   If the network does not use a passphrase, replace the second command with

        > set_network 0 key_mgmt NONE
        OK

   To enable the network, run

        > enable_network 0
        <2>CTRL-EVENT-CONNECTED  ...

   Save the configuration with

        > save_config
        OK

   and exit the CLI with

        > quit

5. Acquire an IP address by running

        sudo dhcpcd

Now an internet connection should be established!

## Fine-tune the configuration

To improve security and not expose passphrases in the configuation file, replace the passphrase strings in `wpa_supplicant.conf` by hashes. To get the hash for a network with given SSID and passphrase, run

    wpa_passphrase "MY_SSID" "MY_SECRET_PASSPHRASE"

This command returns a section like

    network={
            ssid="MY_SSID"
            #psk="MY_SECRET_PASSPHRASE"
            psk=db69facc73f72e37f7c029ff43642f7a695da5718c772c88bcc7da43335cf41a
    }

for the config file that can be copied and pasted there. Remove the commented-out explicit passphrase when done.

To improve liability of pairing with the right network, additional options should be set in the network sections. The most important ones seem to be

      scan_ssid=1
      key_mgmt=WPA-PSK
      pairwise=CCMP
      group=CCMP

In particular, the `scan_ssid` option seems to be important when multiple network variants with the same SSID are present. The other options can be used to narrow down the key exchange and pairing variants.

## Sources
- https://wiki.archlinux.org/index.php/WPA_supplicant (great resource for the general setup)
- https://www.pahoehoe.net/tag/wpa_supplicant-conf/ (nice explanation of config file options)
- `/usr/share/doc/wpa_supplicant-<version>/wpa_supplicant.conf.bz2` (example config with explanation of all options)
