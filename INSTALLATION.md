# Comprehensive Guide for Installing Arch Linux

This guide will provide a step-by-step process for installing Arch Linux on
your computer. Our goal is to provide the most essential steps to get the
operating system installed and get to the user interface as quickly as
possible. We will cover the basic steps needed to install the operating system
from a live environment, partition the disks, install the base system, and
other useful tools that will be helpful in setting up your system.

## Boot the Live Environment

Insert the USB flash drive containing the Arch Linux installation medium and
follow the steps below:

1. Reboot or turn on your computer and access firmware (BIOS) settings by
pressing the <kbd>DEL</kbd> key during the power-on self-test (POST) phase.

2. In the **Boot** tab on the BIOS screen, set the
**FIXED BOOT ORDER Priorities** as follows:

    - Boot Option #1: [USB Hard Disk]

    - Boot Option #2: [Hard Disk]

    - Boot Option #3: [USB CD/DVD]

    - Boot Option #4: [USB Lan]

3. In the **Security** tab, go to **Secure Boot** options and set
**Secure Boot Support** to **[Disabled]**. This is necessary to be able to boot
the installation medium since Arch Linux installation images do not support
Secure Boot. You will set up Secure Boot after completing the installation.

4. In the **Save & Exit** tab, click on **Save Changes and Reset**. The
computer will restart automatically.

5. When the installation medium's boot loader menu appears, select
**Arch Linux install medium (x86_64, UEFI)** and press Enter to enter the
installation environment

6. You will be logged in to the first virtual console as the root user and
presented with a Zsh shell prompt.

## Set the Console Keyboard Layout

The default console keymap is US. To set a Swiss German keyboard layout, use
the following command:

```shell
# loadkeys de_CH-latin1
```

## Connect to Internet

To install the Arch Linux `base` and `linux` packages, we need to ensure that
we are connected to the internet. Let's check the names of our network
interfaces using the following command:

```shell
# ip link
```

You should see output similar to this:

```shell
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: enp8s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP mode DEFAULT group default qlen 1000
    link/ether 64:4b:65:ec:f1:62 brd ff:ff:ff:ff:ff:ff
3: wlan0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN mode DORMANT group default qlen 1000
    link/ether f2:cf:8e:f7:81:df brd ff:ff:ff:ff:ff:ff permaddr 00:42:c8:4b:d6:b2
    altname wlp0s20f3
```

- `enp8s0` is the wired interface

- `wlan0` is the wireless interface

### Wired Connection

If you are using a wired connection, you should already have an internet
connection.

### Wireless Connection

If you are using a laptop, you can connect to a wireless access point using the
`iwctl` command from `iwd`.

1. To scan for networks, use the following command:

    ```shell
    # iwctl station wlan0 scan
    ```

2. To get a list of the scanned networks, use the following command:

    ```shell
    # iwctl station wlan0 get-networks
    ```

3. To connect to your network, use the following command:

    ```shell
    # iwctl station wlan0 connect yallo_3D04C9
    ```

    Enter the router password when prompted.

### Verify Connection

Ping `google.com` to confirm that you are online:

```shell
# ping google.com
```

If you receive an "Unknown host" or "Destination host unreachable" response, it
means you are not online yet. Review your network configuration and repeat the
steps above.

## Partition the Disks

Before installing Arch Linux, we need to partition the disks.

When recognized by the live system, disks are assigned to a block device such
as `/dev/sda` or `/dev/nvme0n1`. To identify these devices, use `fdisk`.

```shell
# fdisk -l
```

The output should be similar to the following:

```shell
Festplatte /dev/nvme0n1: 953.87 GiB, 1024209543168 Bytes, 2000409264 Sektoren
Festplattenmodell: Phison 1TB SM2801T24GKBB4S-E162         
Einheiten: Sektoren von 1 * 512 = 512 Bytes
Sektorgröße (logisch/physikalisch): 512 Bytes / 512 Bytes
E/A-Größe (minimal/optimal): 512 Bytes / 512 Bytes
Festplattenbezeichnungstyp: gpt
Festplattenbezeichner: C08A1630-A720-924E-9099-7598CDE9F0E8

Gerät            Anfang       Ende   Sektoren  Größe Typ
/dev/nvme0n1p1     2048    2099199    2097152     1G EFI-System
/dev/nvme0n1p2  2099200   69208063   67108864    32G Linux Swap
/dev/nvme0n1p3 69208064 2000408575 1931200512 920.9G Linux-Dateisystem
```

`/dev/nvme0n1` is the main drive for me.

### Clean Up the Disk

Let’s clean up our main drive before we create new partitions for our
installation.

1. Use `fdisk` command with the main drive:

    ```shell
    # fdisk /dev/nvme0n1
    ```

2. Press <kbd>p</kbd> and then <kbd>Enter</kbd> to
**print the partition table** we currently have.

3. Press <kbd>g</kbd> and then <kbd>Enter</kbd> to
**create a new empty GPT partition table**.

4. Press <kbd>w</kbd> and then <kbd>Enter</kbd> to **to write table to disk**.

### Create New Partitions

Now we start partitioning our filesystem.

1. Once again, enter:

    ```shell
    # fdisk /dev/nvme0n1
    ```

2. Create the `boot` partition

    - Press <kbd>n</kbd> and then <kbd>Enter</kbd> to **add a new partition**.

    - Press <kbd>Enter</kbd> to select the default option for the partition
    number.

    - Press <kbd>Enter</kbd> to select the default option for the first sector.

    - Type `+1024M` and press <kbd>Enter</kbd> when it asks you the last sector.
    This will determine the boot partition size. Arch wiki recommends atleast
    **300 MB** for the boot size. We make it **1GB** in case we need to add more
    OS to our machine later.

    - Press <kbd>Y</kbd> and then <kbd>Enter</kbd> if it warns you that the
    partition contains a `vfat` signature. This will remove it.

    - Press <kbd>t</kbd> and then <kbd>Enter</kbd> to
    **change a partition type**.

    - Type `1` and then <kbd>Enter</kbd> to set the partition type to
    **EFI System**.

3. Create the `swap` partition

    - Press <kbd>n</kbd> and then <kbd>Enter</kbd> to **add a new partition**.

    - Press <kbd>Enter</kbd> to select the default option for the partition
    number.

    - Press <kbd>Enter</kbd> to select the default option for the first sector.

    - Type `+32G` and press <kbd>Enter</kbd> when it asks you the last sector.
    This will determine the swap partition size. Arch wiki recommends atleast
    **512 MB** for the swap size. We make it **32GB** in case we need to use it
    for hybernation.

    - Press <kbd>Y</kbd> and then <kbd>Enter</kbd> if it warns you that the
    partition contains a `swap` signature. This will remove it.

    - Press <kbd>t</kbd> and then <kbd>Enter</kbd> to
    **change a partition type**.

    - Type `2` and then <kbd>Enter</kbd> to select the swap partition.

    - Type `19` and then <kbd>Enter</kbd> to set the partition type to
    **Linux swap**.

4. Create the `root` partition

    - Press <kbd>n</kbd> and then <kbd>Enter</kbd> to **add a new partition**.

    - Press <kbd>Enter</kbd> to select the default option for the partition
    number.

    - Press <kbd>Enter</kbd> to select the default option for the first sector.

    - Press <kbd>Enter</kbd> to select the default option for the last sector.
    This will allocate rest of the disk to the root partition.

    - Press <kbd>Y</kbd> and then <kbd>Enter</kbd> if it warns you that the
    partition contains a `ext4` signature. This will remove it.

    - Press <kbd>t</kbd> and then <kbd>Enter</kbd> to
    **change a partition type**.

    - Type `3` and then <kbd>Enter</kbd> to select the root partition.

    - Type `20` and then <kbd>Enter</kbd> to set the partition type to
    **Linux filesystem**.

5. Press <kbd>w</kbd> and then <kbd>Enter</kbd> to
**write table to disk and exit** to the disk. Now we are done partitioning the
disk.

### Verifying the Partitions

Use `lsblk` to check the partitions we created.

```shell
# lsblk
```

You should see an output similar to this:

| NAME | MAJ:MIN | RM | SIZE | RO | TYPE | MOUNTPOINT |
| --- | --- | --- | --- | --- | --- | --- |
| nvme0n1 | 259:0 | 0 | 953.9G | 0 |   |   |
| nvme0n1p1 | 259:1 | 0 | 1G | 0 | part |   |
| nvme0n1p2 | 259:2 | 0 | 32G | 0 | part |   |
| nvme0n1p3 | 259:3 | 0 | 920.9G | 0 | part |   |

**`nvme0n1`** is the main disk

**`nvme0n1p1`** is the boot partition

**`nvme0n1p2`** is the swap partition

**`nvme0n1p3`** is the root partition

### Format the Partitions

After creating the partitions, we need to format them with a file system.

1. Format `/dev/nvme0n1p3` partition as `EXT4`. This will be our `root`
 partition.

    ```shell
    # mkfs.ext4 /dev/nvme0n1p3
    ```

2. Create `swap` under the `/dev/nvme0n1p2` partition.

    ```shell
    # mkswap /dev/nvme0n1p2
    ```

3. Format `/dev/nvme0n1p1` partition as `FAT32`. This will be our `/boot`.

    ```shell
    # mkfs.fat -F 32 /dev/nvme0n1p1
    ```

### Mount the Filesystems

1. Mount the `/dev/nvme0n1p3` partition to `/mnt`. This is our `/`:

    ```shell
    # mount /dev/nvme0n1p3 /mnt
    ```

2. Create a `/boot` mountpoint and mount the `/dev/nvme0n1p1` to `/mnt/boot`
partition. This is will be our `/boot`:

    ```shell
    # mount --mkdir /dev/nvme0n1p1 /mnt/boot
    ```

3. Enable swap on `/dev/nvme0n1p3` partition using `swapon`

    ```shell
    # swapon /dev/nvme0n1p2
    ```

## Install the Base System

Now we are ready to install the Arch Linux base system.

Use the `pacstrap` command to install the `base` package and other necessary
packages such as `linux`, `linux-firmware`:

```shell
# pacstrap -K /mnt base linux linux-firmware
```

This will install the basic packages needed for a functional system.

## Generating the fstab

The fstab (file system table) is a configuration file that contains information
about the file systems mounted at boot time. Run the following command to
generate the fstab:

```shell
# genfstab -U /mnt >> /mnt/etc/fstab
```

This command generates the fstab based on the current disk configuration and
writes it to `/mnt/etc/fstab`.

## Chroot

Now, change the root directory of the current shell to the newly installed
system using the `arch-chroot` command:

```shell
# arch-chroot /mnt
```

From this point, any command you run will affect the new system.

## Install Essential Packages

Now we are ready to install the essential pacakges for Arch Linux.

1. Install the `networkmanager` package which allows you to configure and
manage network connections:

    ```shell
    # pacman -S networkmanager
    ```

    You can use it to connect to Wi-Fi or Ethernet networks after completing
    the Arch Linux installation.

2. Install the `gvim` text editor:

    ```shell
    # pacman -S gvim
    ```

    This is a powerful text editor that you can use to modify configuration
    files or write scripts.

3. Install the `sudo` to allow granting administrator privileges to regular
users:

    ```shell
    # pacman -S sudo
    ```

    With sudo, you can run commands as another user, such as the root user,
    without having to switch to that user account.

After completing these steps, you have installed the essential packages required
to run Arch Linux.

## Users and Groups

1. Set the password for the `root` account using the following command:

    ```shell
    # passwd
    ```

2. Add a new user account using the `useradd` command. In this example, I'll
simply use `Bahadir` as the username for the new user account.

    ```shell
    # useradd -m Bahadir
    ```

    This command creates a new user account and its home folder.

3. Set the password for the Bahadir account:

    ```shell
    # passwd Bahadir
    ```

4. Add the `Bahadir` account to the `wheel` group using the `gpasswd` command:

    ```shell
    # gpasswd -a Bahadir wheel
    ```

    The wheel group is the administration group and is commonly used to grant
    administrative privileges.

5. Grant the `Bahadir` account the ability to use the `sudo` command by editing
the `/etc/sudoers` file:

    ```shell
    # EDITOR=vim visudo
    ```

    Uncomment the line (Remove #):

    ```shell
    # %wheel ALL=(ALL:ALL) ALL
    ```

    Save the file and exit the editor.

## Install the GRUB Boot Loader

The GRUB boot loader is used to load the operating system at boot time.

1. Install the `grub` and `efibootmgr` packages using the following command:

    ```shell
    # pacman -S grub efibootmgr
    ```

2. Install the GRUB EFI application `grubx64.efi` to `/boot/EFI/GRUB/` and its
modules to `/boot/grub/x86_64-efi/` using the following command:

    ```shell
    # grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB --modules="tpm" --disable-shim-lock
    ```

3. Generate the main configuration file using the `grub-mkconfig` tool:

    ```shell
    # grub-mkconfig -o /boot/grub/grub.cfg
    ```

4. Install the intel-ucode package using the following command:

    ```shell
    # pacman -S intel-ucode
    ```

    The microcode update provides bug fixes and enhancements for the CPU.

5. `grub-mkconfig` will automatically detect the microcode update and configure
GRUB appropriately. After installing the microcode package, regenerate the GRUB
configuration to activate loading the microcode update by running:

    ```shell
    # grub-mkconfig -o /boot/grub/grub.cfg
    ```

## User Interface Installation

### Display Server: Xorg

Xorg is the most popular display server among Linux users and is required for
running GUI applications.

Install the `xorg-server` package using the following command:

```shell
# pacman -S xorg-server
```

### Desktop Environment: Xfce

Xfce is a lightweight and modular desktop environment that includes a window
manager, a file manager, a desktop, and a panel.

Install the `xfce4` group using the following command:

```shell
# pacman -S xfce4
```

Press <kbd>Enter</kbd> to install all pakages in the group.

### Display Manager: LightDM

LightDM is a cross-desktop display manager.

1. Install the `lightdm` package using the following command:

    ```shell
    # pacman -S lightdm
    ```

2. Install a greeter that prompts the user for credentials:

    ```shell
    # pacman -S lightdm-gtk-greeter
    ```

3. Make sure to enable `lightdm.service` so LightDM will be started at boot:

    ```shell
    # systemctl enable lightdm.service
    ```

## Exit `chroot` and Reboot

Exit the chroot environment by typing `exit` or pressing <kbd>Ctrl</kbd> +
<kbd>d</kbd>. Finally, type `reboot` to restart the computer.

### Boot into Arch Linux

1. After reboot, access firmware (BIOS) settings by pressing the <kbd>DEL</kbd>
key during the power-on self-test (POST) phase.

2. In the **Boot** tab on the BIOS screen, set the
**FIXED BOOT ORDER Priorities** as follows:

    - Boot Option #1: [Hard Disk: GRUB]

    - Boot Option #2: [USB Hard Disk]

    - Boot Option #3: [USB CD/DVD]

    - Boot Option #4: [USB Lan]

3. In the **Save & Exit** tab, click on **Save Changes and Reset**. The
computer will restart automatically.

4. When the GRUB boot loader menu appears, select **Arch Linux** and press
<kbd>Enter</kbd> to boot into Arch Linux.

5. When prompted, log in with the new user account you created.

## Next Steps

### [1. Home](./README.md)

### [2. Preparation](./PREPERATION.md)

### [3. Installation Guide](./INSTALLATION.md)

### [4. First Steps](./FIRSTSTEPS.md)

### [5. Configure User Interface](./GUI.md)

### [6. Useful Tools](./TOOLS.md)

### [7. Extra Steps](./EXTRA.md)

### [8. System Backup and Restore](./BACKUP.md)

### [9. License](./LICENSE)
