# Arch Linux Installation

This section offers a detailed, step-by-step walkthrough for installing Arch Linux,

## Boot the Live Environment

1. Insert the USB flash drive containing the Arch Linux installation medium and restart
your computer.

2. Access the BIOS/UEFI settings (usually by pressing <kbd>DEL</kbd> or <kbd>F2</kbd>
during boot).

3. Navigate to the **Boot Menu** (often accessible via <kbd>F8</kbd>) and select the
USB flash drive containing the Arch Linux installation medium. The computer will reboot
automatically.

4. Upon reboot, the installation medium's boot loader menu will appear. Choose the
**Arch Linux install medium (x86_64, UEFI)** option, and press <kbd>Enter</kbd> to
proceed to the installation environment.

5. You'll then be logged into the first virtual console as the root user, greeted by a
Zsh shell prompt.

## Set the Console Keyboard Layout

The live environment defaults to a US keyboard layout. Change it to Swiss German:

```bash
# loadkeys de_CH-latin1
```

## Establishing Internet Connection

Ensure that you are connected to the internet for downloading necessary packages.

### Wired Connection During Installation

If you are using a wired connection, you should already be connected to the internet.

### Wireless Connection During Installation

You can connect to a wireless access point using the `iwctl` command from `iwd`.

1. Check the name of your wireless interface:

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

    - `eno3` and `enp18s0` are the wired interfaces.

    - `wlo1` is the wireless interface.

2. Scan for networks:

    ```bash
    # iwctl station wlo1 scan
    ```

3. Obtain a list of scanned networks:

    ```bash
    # iwctl station wlo1 get-networks
    ```

4. Connect to your network:

    ```bash
    # iwctl station wlo1 connect [SSID]
    ```

    Replace `[SSID]` with your network's SSID. Enter the router password when prompted.

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
`/dev/sda` or `/dev/nvme0n1`. To identify these devices, use `lsblk` or `fdisk`:

```bash
# lsblk -p
```

The output should be similar to the following:

```bash
NAME             MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
/dev/sda           8:0    0 115.2G  0 disk
└─/dev/sda1        8:1    0 115.2G  0 part
/dev/nvme0n1     259:0    0   1.8T  0 disk
├─/dev/nvme0n1p1 259:1    0     2G  0 part
├─/dev/nvme0n1p2 259:2    0    64G  0 part
└─/dev/nvme0n1p3 259:3    0   1.8T  0 part
```

`/dev/nvme0n1` is the main drive for me.

### Clean Up the Disk and Create New Partitions

Let's clean up our main drive before we create new partitions for our installation.

1. **Launch Partition Tool:**

    ```bash
    fdisk /dev/nvme0n1
    ```

2. **Initialize GPT:**

    Press <kbd>g</kbd> and then <kbd>Enter</kbd> to
    **create a new empty GPT partition table**.

3. **Create Boot Partition:**

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

4. **Create Swap Partition:**

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

5. **Create Root Partition:**

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

6. **Write Changes and Exit:**

Press <kbd>w</kbd> and then <kbd>Enter</kbd> to **write table to disk and exit**.
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

### Formatting Partitions

After creating the partitions, we need to format them with a file system.

1. **EFI System Partition:**

    Format the boot partition `/dev/nvme0n1p1` to FAT32:

    ```bash
    # mkfs.fat -F 32 /dev/nvme0n1p1
    ```

    This will be our `/boot`.

2. **Swap Partition :**

    Initialize the swap partition `/dev/nvme0n1p2` to provide additional virtual
    memory:

    ```bash
    # mkswap /dev/nvme0n1p2
    ```

3. **Root Partition:**

    Format the root partition `/dev/nvme0n1p3` as EXT4.

    ```bash
    # mkfs.ext4 /dev/nvme0n1p3
    ```

    This will be our `/`.

### Mounting Filesystems

Before installation, you must mount the newly formatted partitions.

1. **Root Partition:**

    Mount the root partition `/dev/nvme0n1p3` to `/mnt`:

    ```bash
    # mount /dev/nvme0n1p3 /mnt
    ```

2. **EFI System Partition:**

    Create a mount point for the boot partition `/dev/nvme0n1p1` at `/mnt/boot` and
    mount it:

    ```bash
    # mount --mkdir /dev/nvme0n1p1 /mnt/boot
    ```

3. **Swap Partition :**

    Enable swap for the partition `/dev/nvme0n1p2`:

    ```bash
    # swapon /dev/nvme0n1p2
    ```

## Base System Installation

Install the essential base system along with the Linux kernel and firmware.

```bash
# pacstrap -K /mnt base linux linux-firmware
```

## Generating the fstab

Generate the fstab (file system table) configuration file to define how disk
partitions, block devices, or remote file systems are mounted into the filesystem:

```bash
# genfstab -U /mnt >> /mnt/etc/fstab
```

## Change Root (chroot)

Switch to the root environment of your newly installed system:

```bash
# arch-chroot /mnt
```

This step allows you to execute commands as if your new system were already running.

## Essential Package Installation

Now we are ready to install the essential packages for Arch Linux.

1. **NetworkManager:**

    `NetworkManager` allows you to configure and manage network connections.

    ```bash
    # pacman -Syu networkmanager
    ```

    You can use it to connect to Wi-Fi or Ethernet networks after completing the Arch
    Linux installation.

2. **Vim:**

    Vim is a powerful terminal text editor that you can use to modify text files.

    ```bash
    # pacman -Syu gvim
    ```

    GVim is essentially the same package as Vim with GTK/X support.

3. **Sudo:**

    Sudo allows specified users to execute commands as the root user or another user,
    as specified in the `sudoers` file, enhancing security and control.

    ```bash
    # pacman -Syu sudo
    ```

## Configuring Users and Groups

1. **Root Password:**

    Ensure the root account has a secure password:

    ```bash
    # passwd
    ```

2. **Creating a New User:**

    For everyday tasks, it's best to use a non-root user.

    - I use `bahadir` as the username for the new user account:

        ```bash
        # useradd -m bahadir
        ```

    - Set the password for the new user account:

        ```bash
        # passwd bahadir
        ```

    - Add the new user account to the `wheel` group:

        ```bash
        # gpasswd -a bahadir wheel
        ```

        The `wheel` group is the administration group and is commonly used to grant
        administrative privileges.

3. **Configuring Sudo:**

    Edit the `sudoers` file to grant `wheel` group members sudo privileges:

    ```bash
    # EDITOR=vim visudo
    ```

    Uncomment the line (remove #):

    ```properties
    # %wheel ALL=(ALL:ALL) ALL
    ```

    Save the file and exit the editor.

## Boot Loader Setup: GRUB

The GRUB boot loader is used to load the operating system at boot time.

1. **Package Installation:**

    Install the `grub` and `efibootmgr` packages:

    ```bash
    # pacman -Syu grub efibootmgr
    ```

2. **EFI Application Installation:**

    Install the GRUB EFI application:

    ```bash
    # grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB --modules="tpm" --disable-shim-lock
    ```

3. **Configuration:**

    Generate the main GRUB configuration file:

    ```bash
    # grub-mkconfig -o /boot/grub/grub.cfg
    ```

4. **Microcode:**

    - For Intel CPUs, install the microcode updates for enhanced stability and security:

        ```bash
        # pacman -Syu intel-ucode
        ```

    - Regenerate the GRUB configuration to activate loading the microcode update:

        ```bash
        # grub-mkconfig -o /boot/grub/grub.cfg
        ```

## User Interface Installation

### Desktop Environment: GNOME

GNOME is a desktop environment that is composed entirely of free and open-source
software. The default display is Wayland instead of Xorg.

Install the `gnome` group that contains the base GNOME desktop and the well-integrated
core applications:

```bash
# pacman -Syu gnome
```

During the installation process:

- If prompted to select the packages from the group, press <kbd>Enter</kbd> to install
all packages.

- If prompted to select a provider for emoji-font, choose `noto-fonts-emoji`.

- If prompted to select a provider for jack, choose `pipewire-jack`.

### Display Manager: GNOME Display Manager (GDM)

The display manager included in gnome is GDM. If you want GNOME to start automatically
on next boot, enable `gdm.service`:

```bash
# systemctl enable gdm.service
```

## Network Configuration

To be able to connect to the internet on next boot, enable NetworkManager service:

```bash
# systemctl enable NetworkManager.service
```

## Regenerating Initramfs

Refresh the initial ramdisk setup to incorporate all installed modules and
configurations:

```bash
# mkinitcpio -P
```

## Final Steps: Exiting chroot and Rebooting

1. Exit the chroot environment with `exit` or <kbd>Ctrl</kbd> + <kbd>d</kbd>.

2. Reboot your system with `reboot`.

Remember to remove the installation media to boot into your new Arch Linux setup.

### First Boot into Arch Linux

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
