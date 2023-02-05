# Bahadir's Personal Arch Linux Installation Guide

This guide is a personalized Arch Linux installation tutorial created
specifically for me, Bahadir.

Arch Linux is a lightweight, flexible distribution that offers a minimal base
system to build upon. Designed for speed, reliability, and customization, it is
an ideal choice for users who want full control over their system. This guide
will walk you through the Arch Linux installation process step by step.

## Prerequisites

Before starting, ensure you have the following:

1. A computer with at least 512 MB of RAM, 2 GB of disk space, and a 64-bit CPU.

2. UEFI firmware and boot support.

3. An internet connection.

4. A USB drive with at least 2 GB of space.

5. Basic knowledge of the Linux command line.

## Obtain and Verify an Installation Image

First, download the Arch Linux installation image and verify its authenticity.
Follow these steps to obtain and verify the installation image:

1. Change the directory to `Downloads`:

    ```bash
    $ cd /home/Bahadir/Downloads
    ```

2. Visit the [Arch Linux Downloads](https://www.archlinux.org/download/) page.

    - Select an HTTP mirror site from the list, for example:

        - `geo.mirror.pkgbuild.com`

    - Download both the `.iso` file and the corresponding `.iso.sig` file, such
    as:

        - `archlinux-2023.04.01-x86_64.iso`

        - `archlinux-2023.04.01-x86_64.iso.sig`

    - Alternatively, use the `curl` command to download these files:

        ```bash
        $ curl -O https://geo.mirror.pkgbuild.com/iso/2023.04.01/archlinux-2023.04.01-x86_64.iso
        ```

        ```bash
        $ curl -O https://geo.mirror.pkgbuild.com/iso/2023.04.01/archlinux-2023.04.01-x86_64.iso.sig
        ```

3. Execute the following command to verify the authenticity of the installation
image:

    ```bash
    $ gpg --keyserver-options auto-key-retrieve --verify archlinux-2023.04.01-x86_64.iso.sig
    ```

    The output should include a message indicating that the signature is valid
    and that the key used to sign the file is trusted. It should resemble the
    following output:

    ```bash
    gpg: die unterzeichneten Daten sind wohl in 'archlinux-2023.04.01-x86_64.iso'
    gpg: Signatur vom Mi 01 Mär 2023 13:55:51 CET
    gpg:                mittels EDDSA-Schlüssel 3E80CA1A8B89F69CBA57D98A76A5EF9054449A5C
    gpg:                Aussteller "pierre@archlinux.org"
    gpg: Schlüssel 7F2D434B9741E8AC: Öffentlicher Schlüssel "Pierre Schmitz <pierre@archlinux.org>" importiert
    gpg: Schlüssel 76A5EF9054449A5C: Öffentlicher Schlüssel "Pierre Schmitz <pierre@archlinux.org>" importiert
    gpg: Anzahl insgesamt bearbeiteter Schlüssel: 2
    gpg:                              importiert: 2
    gpg: keine ultimativ vertrauenswürdigen Schlüssel gefunden
    gpg: Korrekte Signatur von "Pierre Schmitz <pierre@archlinux.org>" [unbekannt]
    gpg: WARNUNG: Dieser Schlüssel trägt keine vertrauenswürdige Signatur!
    gpg:          Es gibt keinen Hinweis, daß die Signatur wirklich dem vorgeblichen Besitzer gehört.
    Haupt-Fingerabdruck  = 3E80 CA1A 8B89 F69C BA57  D98A 76A5 EF90 5444 9A5C
    ```

    Ensure the fingerprint in the output matches the
    [Master Key Signatures](https://archlinux.org/master-keys/#master-sigs).

## Create a USB Flash Installation Medium

To install Arch Linux, you need to create a bootable USB drive. Follow the steps
below to create an Arch Linux Installer USB drive:

1. Use `lsblk` to determine the name of the USB flash drive, and ensure it is
not mounted:

    ```bash
    $ lsblk
    ```

    The output should display the name of the USB drive, similar to the example
    below:

    ```bash
    NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
    sda           8:0    0 931.5G  0 disk 
    └─sda1        8:1    0 931.5G  0 part 
    sdb           8:16   1  58.6G  0 disk 
    └─sdb1        8:17   1  58.6G  0 part 
    nvme0n1     259:0    0 953.9G  0 disk 
    ├─nvme0n1p1 259:1    0     1G  0 part /boot
    ├─nvme0n1p2 259:2    0    32G  0 part [SWAP]
    └─nvme0n1p3 259:3    0 920.9G  0 part /
    ```

    In my case, the USB drive name is `/dev/sdb`.

2. Execute the following command to write the installation image to the USB
drive:

    ```bash
    $ sudo dd bs=4M if=archlinux-2023.04.01-x86_64.iso of=/dev/sdb conv=fsync oflag=direct status=progress
    ```

    > **Note**\
    > This command will permanently erase all data on `/dev/sdb` and write the
    installation image onto it. Ensure you have entered the correct drive name.

## Next Steps

Proceed with the subsequent steps in this guide to install Arch Linux on your
computer and tailor it to your preferences.

### [1. Home](./README.md)

### [2. Installation Guide](./INSTALLATION.md)

### [3. First Steps](./FIRSTSTEPS.md)

### [4. Graphical User Interface](./GUI.md)

### [5. Optional Tools and Configurations](./OPTIONAL.md)

### [6. Additional Information](./APPENDIX.md)

### [7. License](./LICENSE)