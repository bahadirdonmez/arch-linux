# How to Prepare for a New Arch Linux Installation

## Acquire an Installation Image and Verify It

To get started, you will need to download the Arch Linux installation image and
verify its authenticity. Follow the steps below to download and verify the
installation image:

1. Change directory to `Downloads`:

    ```bash
    $ cd /home/Bahadir/Downloads
    ```

2. Go to the [Arch Linux Downloads](https://www.archlinux.org/download/) page.

    - Choose a HTTP mirror site for the list, for instance:

        - `geo.mirror.pkgbuild.com`

    - Download the `.iso` file and the respective `.iso.sig` file, for instance:

        - `archlinux-2023.04.01-x86_64.iso`

        - `archlinux-2023.04.01-x86_64.iso.sig`

    - Alternatively, you can use the `curl` command to download these files:

        ```bash
        $ curl -O https://geo.mirror.pkgbuild.com/iso/2023.04.01/archlinux-2023.04.01-x86_64.iso
        ```

        ```bash
        $ curl -O https://geo.mirror.pkgbuild.com/iso/2023.04.01/archlinux-2023.04.01-x86_64.iso.sig
        ```

3. Run the following command to verify the authenticity of the installation
image:

    ```bash
    $ gpg --keyserver-options auto-key-retrieve --verify archlinux-2023.04.01-x86_64.iso.sig
    ```

    The output should contain a message indicating that the signature is valid
    and that the key used to sign the file is trusted. It should be similar to
    the following output:

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

    Make sure fingerprint in the output matches
    [Master Key Signatures](https://archlinux.org/master-keys/#master-sigs).

## Prepare USB Flash Installation Medium

To install Arch Linux, you will need to create a bootable USB drive.
Follow the steps below to create an Arch Linux Installer USB drive:

1. Find out the name of the USB flash drive with `lsblk`. Make sure that it
is not mounted.

    ```bash
    $ lsblk
    ```

    The output should show the name of the USB drive, similar to the
    output below:

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

    For me, the USB drive name is `/dev/sdb`.

2. Use the following command to write the installation image to the USB drive:

    ```bash
    $ sudo dd bs=4M if=archlinux-2023.04.01-x86_64.iso of=/dev/sdb conv=fsync oflag=direct status=progress
    ```

    > **Note**\
    > This command will irrevocably destroy all data on `/dev/sdb` and write
    the installation image into it. Make sure you have entered the correct
    drive name.

## Table of Contents

### [1. Home](./README.md)

### [2. Installation Guide](./INSTALLATION.md)

### [3. First Steps](./FIRSTSTEPS.md)

### [4. Graphical User Interface](./GUI.md)

### [5. Optional Tools and Configurations](./OPTIONAL.md)

### [6. Additional Information](./APPENDIX.md)

### [7. License](./LICENSE)
