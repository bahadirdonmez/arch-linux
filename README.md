# Bahadir's Personal Arch Linux Installation Guide

This guide is a personalized, comprehensive Arch Linux installation tutorial for myself,
Bahadir, to guide me through the steps needed to build a customized Arch Linux system.

Arch Linux is a lightweight Linux distribution, providing a bare minimum base system.
This minimalist design allows users to include only the components they need, making it
remarkably fast, reliable, and ideal for those who want an extensive control over their
operating environment.

With clear step-by-step instructions, you'll be able to navigate through the
installation with ease and confidence.

## System Specifications

- **Processor:** Intel Core I9-13900K (LGA 1700, 3 GHz, 24-Core)

- **Graphics Card:** Sapphire Radeon RX 7900 XTX Pulse (24 GB)

- **Storage Drive:** Samsung 990 Pro (2000 GB, M.2 2280)

- **Memory Kit:** G.Skill Trident Z5 RGB (2 X 32GB, 6400 MHz, DDR5 RAM, DIMM)

- **Motherboard:** ASUS ROG Strix Z790-I Gaming WIFI (LGA 1700, Intel Z790, Mini ITX)

- **CPU Cooler:** MasterLiquid ML280 Mirror CPU Liquid Cooler

- **Power Supply:** V850 SFX Gold 850 Watt

- **Case:** Cooler Master PC Case MasterBox NR200P (Mini ITX)

## Obtain and Verify the Installation Image

To start, follow these steps to obtain and authenticate the installation image:

1. Change the directory to `Downloads`:

    ```bash
    cd ~/Downloads
    ```

2. Visit the [Arch Linux Downloads](https://www.archlinux.org/download/) page.

    - Pick an HTTP mirror site from the list. You will download both the `.iso` file
    and the corresponding `.iso.sig` file from this mirror site.

    - Set the environment variables for the mirror site and the files:

        ```bash
        export MIRROR_SITE="https://geo.mirror.pkgbuild.com/iso/latest"
        export ISO_FILE="archlinux-2024.01.01-x86_64.iso"
        export SIG_FILE="$ISO_FILE.sig"
        ```

    - Download these files using the `curl` command:

        ```bash
        curl -O $MIRROR_SITE/$ISO_FILE
        curl -O $MIRROR_SITE/$SIG_FILE
        ```

3. Verify the authenticity of the installation image using the following command:

    ```bash
    gpg --keyserver-options auto-key-retrieve --verify $SIG_FILE
    ```

    The output should include a message indicating that the signature is valid and that
    the key used to sign the file is trusted. It should resemble the following output:

    ```bash
    gpg: assuming signed data in 'archlinux-2024.01.01-x86_64.iso'
    gpg: Signature made Mon 01 Jan 2024 05:48:11 PM CET
    gpg:                using EDDSA key 3E80CA1A8B89F69CBA57D98A76A5EF9054449A5C
    gpg:                issuer "pierre@archlinux.org"
    gpg: key 76A5EF9054449A5C: public key "Pierre Schmitz <pierre@archlinux.org>" imported
    gpg: key 7F2D434B9741E8AC: public key "Pierre Schmitz <pierre@archlinux.org>" imported
    gpg: Total number processed: 2
    gpg:               imported: 2
    gpg: no ultimately trusted keys found
    gpg: Good signature from "Pierre Schmitz <pierre@archlinux.org>" [unknown]
    gpg: WARNING: The key's User ID is not certified with a trusted signature!
    gpg:          There is no indication that the signature belongs to the owner.
    Primary key fingerprint: 3E80 CA1A 8B89 F69C BA57  D98A 76A5 EF90 5444 9A5C
    ```

    Ensure the fingerprint in the output matches the [Master Key Signatures](
    https://archlinux.org/master-keys/#master-sigs).

## Create a USB Flash Installation Medium

Creating a bootable USB drive is crucial for the Arch Linux installation process.
To do so:

1. Use `lsblk` to identify the name of the USB flash drive, and ensure it is not
mounted:

    ```bash
    lsblk
    ```

    The output should display the name of the USB drive, similar to the example below:

    ```bash
    NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
    sda           8:0    0 931.5G  0 disk 
    └─sda1        8:1    0 931.5G  0 part
    sdb           8:16   1  58.6G  0 disk 
    ├─sdb1        8:17   1   798M  0 part 
    └─sdb2        8:18   1    15M  0 part 
    nvme0n1     259:0    0   1.8T  0 disk 
    ├─nvme0n1p1 259:1    0     2G  0 part /boot
    ├─nvme0n1p2 259:2    0    64G  0 part [SWAP]
    └─nvme0n1p3 259:3    0   1.8T  0 part /
    ```

    In my case, the USB drive name is `/dev/sdb`.

2. Execute the following command to write the installation image to the USB drive:

    ```bash
    export USB_DEV="/dev/sda"
    sudo dd bs=4M if=$ISO_FILE of=$USB_DEV conv=fsync oflag=direct status=progress
    sudo sync
    ```

    > :warning: **Note**\
    > This command will permanently erase all data on `/dev/sdb` and write the
    > installation image onto it. Ensure you have entered the correct drive name.

## Next Steps

Proceed with the subsequent steps in this guide to install Arch Linux on your computer
and tailor it to your preferences.

### [1. Home](./README.md)

### [2. Installation Guide](./INSTALLATION.md)

### [3. First Steps](./FIRSTSTEPS.md)

### [4. Graphical User Interface](./GUI.md)

### [5. Optional Tools and Configurations](./OPTIONAL.md)

### [6. Additional Information](./APPENDIX.md)

### [7. License](./LICENSE)
