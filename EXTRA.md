# Extra Steps After Installing Arch Linux

## Remove USB Flash Drive Installation Media

Since you have successfully installed Arch Linux on your computer, you can
safely remove the USB flash drive installation media.

1. Verify that the USB flash drive is not mounted. Run the following command to
display a list of all connected disks and their partitions:

    ```shell
    $ lsblk
    ```

    Look for the USB flash drive and its partitions in the output:

    ```shell
    NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
    sda           8:0    0 931.5G  0 disk 
    └─sda1        8:1    0 931.5G  0 part 
    sdb           8:16   1  58.6G  0 disk 
    ├─sdb1        8:17   1   809M  0 part 
    └─sdb2        8:18   1    15M  0 part 
    nvme0n1     259:0    0 953.9G  0 disk 
    ├─nvme0n1p1 259:1    0     1G  0 part /boot
    ├─nvme0n1p2 259:2    0    32G  0 part [SWAP]
    └─nvme0n1p3 259:3    0 920.9G  0 part /
    ```

    For example, if the USB flash drive is `/dev/sdb`, you should see
    `/dev/sdb1` and `/dev/sdb2`.

2. Install the `udisks2` package, which provides a tool to query and manipulate
storage devices:

    ```shell
    $ sudo pacman -S udisks2
    ```

3. If any partitions on the USB flash drive are mounted, unmount them with the
`udisksctl unmount` command:

    ```shell
    $ udisksctl unmount -b /dev/sdb1
    ```

    ```shell
    $ udisksctl unmount -b /dev/sdb2
    ```

    This will ensure that the partitions on the USB flash drive are not in use.

4. Optionally, you can use the `fdisk` command to erase the partition table and
create a new one. Be careful with this command, as it will erase all data on the
USB flash drive. Run the following command to start `fdisk`:

    ```shell
    $ sudo fdisk /dev/sdb
    ```

    - Press <kbd>o</kbd> and then <kbd>Enter</kbd> to create a new empty DOS
    partition table.

    - Press <kbd>n</kbd> and then <kbd>Enter</kbd> to add a new partition.

    - Follow the prompts to create new partitions.

    - Finally, press <kbd>w</kbd> and then <kbd>Enter</kbd> to write the changes
    to the USB flash drive and exit.

5. Power off the USB flash drive and remove it from the computer using the
`udisksctl` command:

    ```shell
    $ udisksctl power-off -b /dev/sdb
    ```

    This will safely power off the USB flash drive and allow you to remove it
    from the computer.

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
