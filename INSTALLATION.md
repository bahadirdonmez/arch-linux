# Comprehensive Guide for Installing Arch Linux

This guide delivers a concise, step-by-step procedure for installing Arch Linux. It
covers crucial tasks from setting up the live environment, partitioning disks,
installing the base system, and reaching the user interface promptly, along with
additional useful tool setup.

## Boot the Live Environment

Insert the USB flash drive containing the Arch Linux installation medium and follow the
steps below:

1. Restart or power on your computer. During the power-on self-test (POST) phase,
access the firmware (BIOS) settings by pressing either the <kbd>DEL</kbd> or
<kbd>F2</kbd> key.

2. Once on the BIOS screen, open the **Boot Menu** by pressing its dedicated button or
hitting the <kbd>F8</kbd> key.

3. A Boot Menu popup window will appear. Select the USB flash drive containing the Arch
Linux installation medium, and the computer will reboot automatically.

4. Upon reboot, the installation medium's boot loader menu will appear. Choose the
**Arch Linux install medium (x86_64, UEFI)** option, and press <kbd>Enter</kbd> to
proceed to the installation environment.

5. You'll then be logged into the first virtual console as the root user, greeted by a
Zsh shell prompt.

## Set the Console Keyboard Layout

The default console keymap is US. To set a Swiss German keyboard layout, use the
following command:

```bash
# loadkeys de_CH-latin1
```

## Connect to the Internet for Installation

To install the Arch Linux `base` and `linux` packages, ensure that you are connected to
the internet. First, check the names of your network interfaces using the following
command:

```bash
# ip link
```

You should see output similar to this:

```bash
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: eno3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP mode DEFAULT group default qlen 1000
    link/ether aa:bb:cc:dd:ee:ff brd ff:ff:ff:ff:ff:ff
    altname enp8s0
3: enp18s0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc mq state DOWN mode DEFAULT group default qlen 1000
    link/ether aa:bb:cc:dd:ee:ff brd ff:ff:ff:ff:ff:ff
4: wlo1: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN mode DORMANT group default qlen 1000
    link/ether aa:bb:cc:dd:ee:ff brd ff:ff:ff:ff:ff:ff permaddr aa:bb:cc:dd:ee:ff
    altname wlp0s20f3
```

- `eno3` and `enp18s0` are the wired interfaces

- `wlo1` is the wireless interface

### Wired Connection During Installation

If you are using a wired connection, you should already be connected to the internet.

### Wireless Connection During Installation

You can connect to a wireless access point using the `iwctl` command from `iwd`.

1. To scan for networks, use the following command:

    ```bash
    # iwctl station wlo1 scan
    ```

2. To obtain a list of scanned networks, use the following command:

    ```bash
    # iwctl station wlo1 get-networks
    ```

3. To connect to your network, use the following command:

    ```bash
    # iwctl station wlo1 connect yallo_3D04C9
    ```

    Enter the router password when prompted.

### Verify Connection for Installation

Ping `google.com` to confirm that you are online:

```bash
# ping google.com
```

If you receive an "Unknown host" or "Destination host unreachable" response, it means
you are not online yet. Review your network configuration and repeat the steps above.

## Partition the Disks

Before installing Arch Linux, you need to partition the disks.

When recognized by the live system, disks are assigned to a block device such as
`/dev/sda` or `/dev/nvme0n1`. To identify these devices, use `fdisk`.

```bash
# fdisk -l
```

The output should be similar to the following:

```bash
Disk /dev/nvme0n1: 1.82 TiB, 2000398934016 bytes, 3907029168 sectors
Disk model: Samsung SSD 990 PRO 2TB                 
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 57A387AC-9145-406F-B7ED-88F282ADE694

Device             Start        End    Sectors  Size Type
/dev/nvme0n1p1      2048    4196351    4194304    2G EFI System
/dev/nvme0n1p2   4196352  138414079  134217728   64G Linux swap
/dev/nvme0n1p3 138414080 3907028991 3768614912  1.8T Linux filesystem
```

`/dev/nvme0n1` is the main drive for me.

### Clean Up the Disk and Create New Partitions

Let's clean up our main drive before we create new partitions for our installation.

1. Use the `fdisk` command with the main drive:

    ```bash
    # fdisk /dev/nvme0n1
    ```

2. Press <kbd>g</kbd> and then <kbd>Enter</kbd> to
**create a new empty GPT partition table**.

3. Create the `boot` partition:

    - Press <kbd>n</kbd> and then <kbd>Enter</kbd> to **add a new partition**.

    - Press <kbd>Enter</kbd> to select the default option for the partition number.

    - Press <kbd>Enter</kbd> to select the default option for the first sector.

    - Type `+2G` and press <kbd>Enter</kbd> when it asks you for the last sector. This
    will determine the boot partition size. The Arch wiki recommends at least
    **300 MB** for the boot size. We'll make it **2GB** in case we need to add more OS
    to our machine later.

    - Press <kbd>Y</kbd> and then <kbd>Enter</kbd> if it warns you that the partition
    contains a `vfat` signature. This will remove it.

    - Press <kbd>t</kbd> and then <kbd>Enter</kbd> to **change a partition type**.

    - Type `1` and then <kbd>Enter</kbd> to set the partition type to **EFI System**.

4. Create the `swap` partition

    - Press <kbd>n</kbd> and then <kbd>Enter</kbd> to **add a new partition**.

    - Press <kbd>Enter</kbd> to select the default option for the partition number.

    - Press <kbd>Enter</kbd> to select the default option for the first sector.

    - Type `+64G` and press <kbd>Enter</kbd> when it asks you for the last sector. This
    will determine the swap partition size. The Arch wiki recommends at least
    **512 MB** for the swap size. We'll make it **64GB** in case we need to use it for
    hibernation.

    - Press <kbd>Y</kbd> and then <kbd>Enter</kbd> if it warns you that the partition
    contains a `swap` signature. This will remove it.

    - Press <kbd>t</kbd> and then <kbd>Enter</kbd> to **change a partition type**.

    - Type `2` and then <kbd>Enter</kbd> to select the swap partition.

    - Type `19` and then <kbd>Enter</kbd> to set the partition type to **Linux swap**.

5. Create the `root` partition

    - Press <kbd>n</kbd> and then <kbd>Enter</kbd> to **add a new partition**.

    - Press <kbd>Enter</kbd> to select the default option for the partition number.

    - Press <kbd>Enter</kbd> to select the default option for the first sector.

    - Press <kbd>Enter</kbd> to select the default option for the last sector. This
    will allocate the rest of the disk to the root partition.

    - Press <kbd>Y</kbd> and then <kbd>Enter</kbd> if it warns you that the partition
    contains an `ext4` signature. This will remove it.

    - Press <kbd>t</kbd> and then <kbd>Enter</kbd> to **change a partition type**.

    - Type `3` and then <kbd>Enter</kbd> to select the root partition.

    - Type `20` and then <kbd>Enter</kbd> to set the partition type to
    **Linux filesystem**.

6. Press <kbd>w</kbd> and then <kbd>Enter</kbd> to **write table to disk and exit**.
Now we are done partitioning the disk.

### Verifying the Partitions

To check the partitions we created, use `fdisk`.

```bash
# fdisk -l
```

The output should be similar to the following:

```bash
Disk /dev/nvme0n1: 1.82 TiB, 2000398934016 bytes, 3907029168 sectors
Disk model: Samsung SSD 990 PRO 2TB                 
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 57A387AC-9145-406F-B7ED-88F282ADE694

Device             Start        End    Sectors  Size Type
/dev/nvme0n1p1      2048    4196351    4194304    2G EFI System
/dev/nvme0n1p2   4196352  138414079  134217728   64G Linux swap
/dev/nvme0n1p3 138414080 3907028991 3768614912  1.8T Linux filesystem
```

**`nvme0n1`** is the main disk

**`nvme0n1p1`** is the boot partition

**`nvme0n1p2`** is the swap partition

**`nvme0n1p3`** is the root partition

### Format the Partitions

After creating the partitions, we need to format them with a file system.

1. Format `/dev/nvme0n1p3` partition as `EXT4`:

    ```bash
    # mkfs.ext4 /dev/nvme0n1p3
    ```

    This will be our `root` partition.

2. Create `swap` on the `/dev/nvme0n1p2` partition:

    ```bash
    # mkswap /dev/nvme0n1p2
    ```

3. Format `/dev/nvme0n1p1` partition as `FAT32`:

    ```bash
    # mkfs.fat -F 32 /dev/nvme0n1p1
    ```

    This will be our `/boot`.

### Mount the Filesystems

1. Mount the `/dev/nvme0n1p3` partition to `/mnt`. This will be our `/`:

    ```bash
    # mount /dev/nvme0n1p3 /mnt
    ```

2. Create a `/boot` mountpoint and mount the `/dev/nvme0n1p1` partition to `/mnt/boot`.
This will be our `/boot`:

    ```bash
    # mount --mkdir /dev/nvme0n1p1 /mnt/boot
    ```

3. Enable swap on `/dev/nvme0n1p3` partition using `swapon`:

    ```bash
    # swapon /dev/nvme0n1p2
    ```

## Install the Base System

Now we are ready to install the Arch Linux base system.

Use the `pacstrap` command to install the `base` package and other necessary packages
such as `linux` and `linux-firmware`:

```bash
# pacstrap -K /mnt base linux linux-firmware
```

This will install the basic packages needed for a functional system.

## Generating the fstab

The fstab (file system table) is a configuration file that contains information about
the file systems mounted at boot time. Run the following command to generate the fstab:

```bash
# genfstab -U /mnt >> /mnt/etc/fstab
```

This command generates the fstab based on the current disk configuration and writes it
to `/mnt/etc/fstab`.

## Chroot

Now, change the root directory of the current shell to the newly installed system using
the `arch-chroot` command:

```bash
# arch-chroot /mnt
```

From this point, any command you run will affect the new system.

## Install Essential Packages

Now we are ready to install the essential packages for Arch Linux.

1. Install the `networkmanager` package, which allows you to configure and manage
network connections:

    ```bash
    # pacman -S networkmanager
    ```

    You can use it to connect to Wi-Fi or Ethernet networks after completing the Arch
    Linux installation.

2. Install the `gvim` text editor:

    ```bash
    # pacman -S gvim
    ```

    This is a powerful text editor that you can use to modify configuration files or
    write scripts.

3. Install `sudo` to allow granting administrator privileges to regular users:

    ```bash
    # pacman -S sudo
    ```

    With sudo, you can run commands as another user, such as the root user, without
    having to switch to that user account.

After completing these steps, you have installed the essential packages required to run
Arch Linux.

## Users and Groups

1. Set the password for the `root` account using the following command:

    ```bash
    # passwd
    ```

2. Add a new user account using the `useradd` command. In this example, we'll simply
use `Bahadir` as the username for the new user account.

    ```bash
    # useradd -m Bahadir
    ```

    This command creates a new user account and its home folder.

3. Set the password for the `Bahadir` account:

    ```bash
    # passwd Bahadir
    ```

4. Add the `Bahadir` account to the `wheel` group using the `gpasswd` command:

    ```bash
    # gpasswd -a Bahadir wheel
    ```

    The wheel group is the administration group and is commonly used to grant
    administrative privileges.

5. Grant the `Bahadir` account the ability to use the `sudo` command by editing the
`/etc/sudoers` file:

    ```bash
    # EDITOR=vim visudo
    ```

    Uncomment the line (remove #):

    ```properties
    # %wheel ALL=(ALL:ALL) ALL
    ```

    Save the file and exit the editor.

## Install the GRUB Boot Loader

The GRUB boot loader is used to load the operating system at boot time.

1. Install the `grub` and `efibootmgr` packages using the following command:

    ```bash
    # pacman -S grub efibootmgr
    ```

2. Install the GRUB EFI application `grubx64.efi` to `/boot/EFI/GRUB/` and its modules
to `/boot/grub/x86_64-efi/` using the following command:

    ```bash
    # grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB --modules="tpm" --disable-shim-lock
    ```

3. Generate the main configuration file using the `grub-mkconfig` tool:

    ```bash
    # grub-mkconfig -o /boot/grub/grub.cfg
    ```

4. Install the intel-ucode package using the following command:

    ```bash
    # pacman -S intel-ucode
    ```

    The microcode update provides bug fixes and enhancements for the CPU.

5. `grub-mkconfig` will automatically detect the microcode update and configure GRUB
appropriately. After installing the `microcode` package, regenerate the GRUB
configuration to activate loading the microcode update by running:

    ```bash
    # grub-mkconfig -o /boot/grub/grub.cfg
    ```

## User Interface Installation

### Display Server: Xorg

Xorg is the most popular display server among Linux users and is required for running
GUI applications.

Install the `xorg-server` package using the following command:

```bash
# pacman -S xorg-server
```

### Desktop Environment: Xfce

Xfce is a lightweight and modular desktop environment that includes a window manager, a
file manager, a desktop, and a panel.

Install the `xfce4` group using the following command:

```bash
# pacman -S xfce4
```

Press <kbd>Enter</kbd> to install all packages in the group.

### Display Manager: LightDM

LightDM is a cross-desktop display manager.

1. Install the `lightdm` package using the following command:

    ```bash
    # pacman -S lightdm
    ```

2. Install a greeter that prompts the user for credentials:

    ```bash
    # pacman -S lightdm-gtk-greeter
    ```

3. Ensure that `lightdm.service` is enabled so LightDM starts at boot:

    ```bash
    # systemctl enable lightdm.service
    ```

## Mozilla Firefox

Mozilla Firefox is a widely-used open-source web browser developed by Mozilla
Corporation.

1. Install the `firefox` package using the following command:

    ```bash
    # pacman -S firefox
    ```

2. During the installation process, if prompted to select between `jack2` and
`pipewire-jack`, choose `pipewire-jack`.

3. If prompted to select between `pipewire-media-session` and `wireplumber`, choose
`wireplumber`.

## Exit `chroot` and Reboot

To exit the chroot environment, type `exit` or press <kbd>Ctrl</kbd> + <kbd>d</kbd>.
Finally, type `reboot` to restart the computer.

### Boot into Arch Linux

1. When the GRUB bootloader menu appears, select **Arch Linux** and press
<kbd>Enter</kbd> to boot into Arch Linux.

2. When prompted, log in with the new user account you created.

## Table of Contents

### [1. Home](./README.md)

### [2. Installation Guide](./INSTALLATION.md)

### [3. First Steps](./FIRSTSTEPS.md)

### [4. Graphical User Interface](./GUI.md)

### [5. Optional Tools and Configurations](./OPTIONAL.md)

### [6. Additional Information](./APPENDIX.md)

### [7. License](./LICENSE)
