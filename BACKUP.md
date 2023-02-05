# System Backup and Restore

## Set Up System Backup

### Boot the Live Environment

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

3. In the **Save & Exit** tab, click on **Save Changes and Reset**. The
computer will restart automatically.

4. When the installation medium's boot loader menu appears, select
**Arch Linux install medium (x86_64, UEFI)** and press Enter to enter the
installation environment

5. You will be logged in to the first virtual console as the root user and
presented with a Zsh shell prompt.

### Clone the Disk Using Clonezilla

Cloning your disk with Clonezilla can help you backup your system in case of
hardware failure or other issues.

1. Open Clonezilla with the following command in the terminal:

    ```shell
    # clonezilla
    ```

2. In the Clonezilla interface, choose the **device-image** option for backup.

3. Choose the **local_dev** option to save the backup image to a local device.
Clonezilla will scan the disks on the machine every few seconds and display the
results. If you haven't inserted your backup disk already, insert it now. Press
<kbd>Ctrl</kbd> + <kbd>C</kbd> to finish scanning once you see your backup disk.

4. Choose the device partition to backup: `/dev/sda1`. Press <kbd>Enter</kbd>.
The partition type must be `ext4`. If necessary, quit Clonezilla and format the
disk properly using the `fdisk` command before running Clonezilla again.

5. Choose a directory on the target device where you want to store the backup
image. The default directory `/` is fine.

6. Select the option **Beginner Mode**.

7. Select **savedisk** mode.

8. Enter the image name. Clonezilla will give an image name based on date and
time, but you can feel free to change it.

9. Choose the source disk that you want to backup: `/dev/nvme0n1`

10. Skip the file system check.

11. Say **Yes** to the option **Check the saved image** to ensure that the image
is valid.

12. Do not encrypt the image.

13. Select **choose** as the action you want to perform after the image saving
is done.

14. The pocess of cloning your disk can take a significant amount of time
depending on the size of your disk and the speed of your computer. Be patient
and wait for Clonezilla to finish creating the backup image.

15. Once the backup process is complete, you will return to the console as the
root user and presented with a Zsh shell prompt. Safely eject the backup disk
using the appropriate commands:

    ```shell
    $ udisksctl unmount -b /dev/sda1
    ```

    ```shell
    $ udisksctl power-off -b /dev/sda
    ```

16. The backup process is now complete. Keep the backup disk in a safe place
and label it properly to avoid any confusion in the future.

17. Finally, type `reboot` to restart the computer.

### Boot Back into Arch Linux

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

## Restore Disk From Backup Image

Once you have created a backup of your disk using Clonezilla, you can use it to
restore your system in case of a failure or data loss.

### Boot into the Live Environment

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

3. In the **Save & Exit** tab, click on **Save Changes and Reset**. The
computer will restart automatically.

4. When the installation medium's boot loader menu appears, select
**Arch Linux install medium (x86_64, UEFI)** and press Enter to enter the
installation environment

5. You will be logged in to the first virtual console as the root user and
presented with a Zsh shell prompt.

### Restore the Disk from Backup Image Using Clonezilla

1. Open Clonezilla with the following command in the terminal:

    ```shell
    # clonezilla
    ```

2. In the Clonezilla interface, choose the **device-image** option for backup.

3. Choose the **local_dev** option to restore the backup image from the local
device. Clonezilla will scan the disks on the machine every few seconds and
display the results. If you haven't inserted your backup disk already, insert it
now. Press <kbd>Ctrl</kbd> + <kbd>C</kbd> to finish scanning once you see your
backup disk.

4. Choose the device partition to backup: `/dev/sda1`. Press <kbd>Enter</kbd>.
The partition type must be `ext4`. If necessary, quit Clonezilla and format the
disk properly using the `fdisk` command before running Clonezilla again.

5. Choose the directory on the local device where the backup image is stored.
For instance: `/`.

6. Select the option **Beginner Mode**.

7. Select **restoredisk** mode.

8. Select the image to restore.

9. Choose the source disk to restore the backup to: `/dev/nvme0n1`

10. Say **Yes** to the option **Check the image before restoring** to know if
the image is broken or not.

11. Select **choose** as the action you want to perform after the image saving
is done.

12. The process of restoring your disk can take a significant amount of time
depending on the size of your disk and the speed of your computer. Be patient
and wait for Clonezilla to finish restoring the backup image.

13. Once the restoration process is complete, you will return to the console as
the root user and presented with a Zsh shell prompt. Safely eject the backup
disk using the appropriate commands:

    ```shell
    $ udisksctl unmount -b /dev/sda1
    ```

    ```shell
    $ udisksctl power-off -b /dev/sda
    ```

14. The restore process is now complete.

15. Finally, type `reboot` to restart the computer.

### Boot Back into Operating System

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
