# Useful Tools for Arch Linux

## Graphics

### Image Editing: Pinta

Pinta is a lightweight, user-friendly raster image editing program similar to
Microsoft Paint and Paint.NET. It is suitable for basic image editing tasks such
as cropping, resizing, drawing, and working with layers.

To install Pinta on Arch Linux, use the following command:

```bash
$ sudo pacman -S pinta
```

### Image Viewer and Organizer: Shotwell

Shotwell is a digital photo organizer designed for the GNOME desktop
environment. It is compatible with other desktop environments, including Xfce.

1. To install Shotwell on Arch Linux, use the following command:

    ```bash
    $ sudo pacman -S shotwell
    ```

2. After the installation is complete, launch Shotwell from the application
launcher.

3. Create a folder to save imported pictures by running the following command:

    ```bash
    $ mkdir -p ~/Pictures/Import
    ```

4. Configure Shotwell preferences by navigating to **Edit > Settings**:

    - In the **Library** tab:

        - Set **Import photos** to `~/Pictures/Import`.

## Network Connectivity

### PIA VPN Installation and Configuration

The desktop version of PIA VPN for Linux is available on the
[PIA Download Page](<https://www.privateinternetaccess.com/download/linux-vpn>).
For more details, refer to the <!--markdownlint-disable-next-line MD013-->
[PIA Support Portal](<https://helpdesk.privateinternetaccess.com/guides/linux/install-2/linux-installing-the-pia-app#linux-installing-the-pia-app_step-1-download>).

1. Navigate to the `Downloads` directory and download the official installer for
PIA VPN:

    ```bash
    $ cd ~/Downloads && curl -O https://installers.privateinternetaccess.com/download/pia-linux-3.3.1-06924.run
    ```

2. Once the download is complete, run the installer with the following command:

    ```bash
    $ sh pia-linux-3.3.1-06924.run
    ```

3. After the installation is complete, launch PIA VPN from the application
launcher and log in with your PIA VPN account credentials.

4. Configure your PIA VPN preferences by navigating to **PIA VPN > Settings**:

    - In the **General** tab:

        - Check **Launch on System Startup**.

        - Uncheck **Show Desktop Notifications**.

### ETH Zurich VPN Configuration

To establish an encrypted remote connection to the data network of ETH Zurich,
the ITS ID-Infrastructure section operates a VPN Gateway. This VPN connection
serves as the foundation for PWLAN. In order to connect to the ETH Zurich VPN,
we will use OpenConnect, which is a client for Cisco's AnyConnect SSL VPN.

1. Install the `networkmanager-openconnect` package to enable OpenConnect
support in NetworkManager:

    ```bash
    $ sudo pacman -S networkmanager-openconnect
    ```

2. Install the gnome-keyring package to securely store your VPN credentials:

    ```bash
    $ sudo pacman -S gnome-keyring
    ```

3. Restart the `NetworkManager.service` to apply the changes:

    ```bash
    $ systemctl restart NetworkManager.service
    ```

4. Add a new VPN connection by following these steps:

    - Left-click on the NetworkManager applet icon in the system tray to open
    the NetworkManager menu.

    - Click on **VPN Connections** and then **Add a new VPN connection...**.

    - Choose **Cisco AnyConnect or OpenConnect (OpenConnect)** as the
    connection type and click **Create...**.

5. Configure the new VPN connection with the following settings:

    - Connection name: `ETH Zurich VPN`

    - Gateway: `sslvpn.ethz.ch`

    - Click on **Save** to store the configuration.

6. To connect to the ETH Zurich VPN, click on the NetworkManager applet icon
again, navigate to **VPN Connections**, and select **ETH Zurich VPN**:

    - Set **GROUP** to **staff-net**

    - Enter your ETH Zurich VPN username and password.

    - Check **Save passwords** box.

    - Click on **Login**.

    - When the GNOME Keyring asks for a password, enter your Arch Linux
    password.

    - For future connections, check the
    **Automatically start connection next time** box.

Once connected, a lock icon should appear on the NetworkManager applet,
indicating that the VPN connection is active.

## Brother MFC-J6710DW Printer and Scanner

### Avahi Installation

Avahi is a free Zero-configuration networking (zeroconf) implementation that
allows programs to publish and discover services and hosts running on a local
network. For instance, you can plug into a network and instantly find printers
available for printing.

Follow these steps to set up DNS-SD/mDNS with Avahi for printer discovery and
sharing:

1. Install the `avahi` and `nss-mdns` packages:

    ```bash
    $ sudo pacman -S avahi nss-mdns
    ```

2. Edit the `/etc/nsswitch.conf` file and modify the `hosts` line to include
`mdns_minimal [NOTFOUND=return]` before `resolve` and `dns`:

    ```bash
    $ sudo sed -i '/^hosts:/ s/\(mymachines \)\(resolve \[!UNAVAIL=return\]\)/\1mdns_minimal [NOTFOUND=return] \2/' \
    /etc/nsswitch.conf
    ```

3. Verify the contents of the `/etc/nsswitch.conf` file and correct any errors:

    ```bash
    $ sudo vim /etc/nsswitch.conf
    ```

    Ensure that the `hosts` line has the correct content as shown below:

    ```properties
    hosts: mymachines mdns_minimal [NOTFOUND=return] resolve [!UNAVAIL=return] files myhostname dns
    ```

4. Enable and start the Avahi service:

    ```bash
    $ systemctl enable --now avahi-daemon.service
    ```

5. Avahi includes several utilities that help you discover the services running
on a network. To discover the printer, for example, run the following command:

    ```bash
    $ avahi-browse --all --ignore-local --resolve --terminate
    ```

    Note the printer's hostname, such as: `BRN001BA95D5DB6.local`.

### Printer Installation and Configuration

CUPS, the standards-based open-source printing system developed by
OpenPrinting, is designed for Linux and other Unix-like operating systems. Arch
Linux packages the OpenPrinting CUPS fork instead of the Apple CUPS fork.

Follow these steps to install and configure the Brother MFC-J6710DW printer:

1. Install CUPS:

    ```bash
    $ sudo pacman -S cups
    ```

2. Install Ghostscript, which is required for non-PDF printers:

    ```bash
    $ sudo pacman -S ghostscript
    ```

3. Enable and start the CUPS service using the following command:

    ```bash
    $ systemctl enable --now cups.service
    ```

4. Add the Brother MFC-J6710DW printer by visiting <http://localhost:631/>:

    - Click on **Administration**.

    - Click on **Add Printer** and enter your Arch Linux username and password
    when prompted.

    - Choose **Internet Printing Protocol (ipp)** and click on **Next**.

    - Set **Connection** to `ipp://BRN001BA95D5DB6.local:631/ipp/port1`. Replace
    the part `BRN001BA95D5DB6.local` with the hostname of your printer.

    - Set **Name** to `Brother_MFC_J6710DW`.

    - Set **Description** to `Brother MFC-J6710DW`.

    - Specify a **Location**, such as `Office Zurich`, and click on **Next**.

    - Select `Brother` as the manufacturer and click on **Next**.

    - Choose **Model** as
    `Brother MFC-J6710DW, driverless, cups-filters 1.28.17 (en)` and click on
    **Add Printer**.

    - In **Default Settings**, set **Print Color Mode** to **Monochrome** and
    **2-Sided Printing** to **On (Portrait)**.

    Now you should be able to print with your Brother MFC-J6710DW printer.

5. Install the GTK printer configuration tool and status applet:

    ```bash
    $ sudo pacman -S system-config-printer
    ```

### Scanner Installation and Configuration

SANE (Scanner Access Now Easy) provides a library and a command-line tool for
using scanners under GNU/Linux.

Follow these steps to install and configure the scanner:

1. Install the `sane` package:

    ```bash
    $ sudo pacman -S sane
    ```

2. Navigate to the `aur` directory and clone the `brscan4` package from the AUR
using the `git clone` command:

    ```bash
    $ cd ~/aur && git clone https://aur.archlinux.org/brscan4.git
    ```

3. Change to the `brscan4` directory, build, and install the package using the
`makepkg` command:

    ```bash
    $ cd ~/aur/brscan4 && makepkg -sirc && git clean -dfX
    ```

4. For network scanners, Brother provides a configuration tool. Replace the
part `BRN001BA95D5DB6.local` with your scanner's hostname found using the
`avahi-browse` command:

    ```bash
    $ sudo brsaneconfig4 -a name=Brother_MFC_J6710DW model=MFC-J6710DW nodename=BRN001BA95D5DB6.local
    ```

5. Now, the scanner should be recognized by SANE. To check, run:

    ```bash
    $ scanimage -L
    ```

6. Install the Simple Scan frontend for SANE:

    ```bash
    $ sudo pacman -S simple-scan
    ```

7. Create a folder to save scanned documents by running the following command:

    ```bash
    $ mkdir -p ~/Pictures/Scanned
    ```

## Running Windows Apps on Linux

You can run Windows apps such as Microsoft Office and Adobe in Arch Linux using
XFCE4. These apps will integrate into your native OS, allowing seamless
interaction with your files and applications.

### Installation of Virt-Manager

Virt-Manager, a user-friendly graphical interface for the `libvirt` library,
simplifies virtual machine management, allowing you to effortlessly create,
delete, and manipulate virtual machines.

1. Install the `Virt-Manager` package using the following command:

    ```bash
    $ sudo pacman -S virt-manager
    ```

2. Install the `qemu-desktop` package using the following command:

    ```bash
    $ sudo pacman -S qemu-desktop
    ```

3. Install the `dnsmasq` package using the following command:

    ```bash
    $ sudo pacman -S dnsmasq
    ```

4. Enable and start the `libvirtd.service` service to run at startup by
executing the following command:

    ```bash
    $ systemctl enable --now libvirtd.service
    ```

5. Running domains can be automatically suspended/shutdown at host shutdown
using the `libvirt-guests.service` systemd service. This same service will
resume/startup the suspended/shutdown domain automatically at host startup:

    ```bash
    $ systemctl enable --now libvirt-guests.service
    ```

### Configuring Virt-Manager for Non-Root Users

To use Virt-Manager as a non-root user, you'll need to configure KVM and enable
`libvirt` networking components. This involves adjusting socket permissions,
adding your user to the right groups, and setting up a user session in
Virt-Manager.

1. Set the UNIX domain socket ownership to `libvirt` and the UNIX socket
permission to read and write:

    ```bash
    $ sudo sed -i \
    -e 's/# *\(unix_sock_group = "libvirt"\)/\1/' \
    -e 's/#*\(unix_sock_rw_perms = "0770"\)/\1/' \
    /etc/libvirt/libvirtd.conf
    ```

2. Add your user to the `libvirt` group:

    ```bash
    $ sudo gpasswd -a ${USER} libvirt
    ```

3. Once these changes are made, restart the libvirt daemon for the changes to
take effect:

    ```bash
    $ systemctl restart libvirtd.service
    ```

4. Right-click on **QEMU/KVM** connection and select **Details**. On the
**Virtual Network** tab, check **Autostart** and then click **Apply**.

### Creating a Virtual Hard Disk Image and Preparing the Installation Media

1. To create a virtual hard disk for the virtual machine, run the following
command in a terminal:

    ```bash
    $ cd /var/lib/libvirt/images/ && \
    sudo qemu-img create -f qcow2 -o preallocation=off Windows11 512G
    ```

    This command will create a hard disk file named `Windows11` in the
    `/var/lib/libvirt/images/` directory.

2. Navigate to the [Microsoft Software Download](
<https://www.microsoft.com/software-download/windows11>) page and download the
Download Windows 11 Disk Image (ISO) for x64 devices.

3. You will also require the Virtio drivers for Windows. Visit the dedicated
[GitHub page](
<https://github.com/virtio-win/virtio-win-pkg-scripts/blob/master/README.md>)
for these drivers and download the stable ISO file.

### Setting Up a Virtual Machine

1. Open `virt-manager`. To start the process of creating a new virtual machine,
select **File** from the menu bar, followed by **New Virtual Machine**.

2. Choose **Import existing disk image** as your preferred method of installing
the operating system. Proceed by clicking **Forward**.

3. You're now prompted to identify the disk image. Navigate to
`/var/lib/libvirt/images/` and choose the disk image. As your operating system,
select `Microsoft Windows 11`. Continue by clicking **Forward**.

4. Next, define the resources dedicated to the virtual machine:

    - Allocate the memory size to `16384` (1/4 of the available memory).

    - Assign `8` CPUs to the virtual machine (/4 of the total available CPUs).

5. Assign a name to your virtual machine and verify that all other VM details
are correct:

    - Set your virtual machine name as `Windows11`.

    - Choose **Customize configuration before install**.

6. Click **Finish** to create the virtual machine.

## Customize the Virtual Machine

1. In the **Memory** tab set the **Current allocation** to `4096` (1/4 of the
allocated memory).

2. In the **SATA Disk 1** tab set the **Disk bus** to `VirtIO`.

3. In the **NIC** tab:

    - Set the **Device model** to `virtio`.

    - Run the following command:

    ```bash
    $ sudo virsh net-update default add-last ip-dhcp-host \
    '<host mac="52:54:00:a8:19:ac" ip="192.168.122.111"/>' \
    --live --config --parent-index 0
    ```

    Replace the MAC address with the MAC address of your Virtual Machine. For
    the IP address, choose an address between `192.168.122.2` and
    `192.168.122.254`.

4. In the **Display Spice** tab:

    - Set **Listen Type** to `None`

    - Check the **OpenGL** option.

    - Set `Radeon RX 7900 XTX` as the graphics card.

5. In the **Video QXL** tab:

    - Set the **Model** to `Virtio`.

    - Check the **3D acceleration** option.

6. QEMU can emulate Trusted Platform Module, which is required by some systems
such as Windows 11. To add TPM:

    - Install the `swtpm` package:

        ```bash
        $ sudo pacman -S swtpm
        ```

    - Click on **Add Hardware**, then select **TPM** from the list of options.

    - Click on **Finish**.

7. Add installation media:

    - Click on **Add Hardware**, then select **Storage** from the list of
    options.

    - Choose **Select or Create custom storage**. Click on **Manage**, this will
    open a file browser.

    - In the file browser, navigate to the location of the downloaded
    `Win11.iso` ISO file, select it, and confirm your selection.

    - After the ISO file is selected, set the device type to **CDROM**.

    - Click on **Finish**.

8. Add Virtio drivers:

    - Click on **Add Hardware**, then select **Storage** from the list of
    options.

    - Choose **Select or Create custom storage**. Click on **Manage**, this will
    open a file browser.

    - In the file browser, navigate to the location of the downloaded
    `virtio-win.iso` driver ISO file, select it, and confirm your selection.

    - After the ISO file is selected, set the device type to **CDROM**.

    - Click on **Finish**.

9. In the **Boot Options** tab:

    - Check **Start the virtual machine on host bootup**.

    - Check **SATA CDROM 1** containing the `Win11.iso` ISO file.

10. Click **Begin Installation** on top left.

## Seamlessly Run Windows Apps

1. Install `freerdp` package by running following command on terminal:

    ```bash
    $ sudo pacman -S freerdp
    ```

2. Establish the remote desktop connection to your virtual machine with
`xfreerdp` command:

    - Start fullscreen on multi monitor:

        ```bash
        $ sudo xfreerdp -f /scale:180 /scale-desktop:200 /multimon /bpp:32 /rfx /gfx:avc444 /u:<user> /p:<password> /v:192.168.122.111 +fonts +clipboard +menu-anims +window-drag /drive:home,/home/Bahadir
        ```

    - Start with 95% height on single monitor:

        ```bash
        $ sudo xfreerdp /size:95%h /scale:180 /scale-desktop:200 /bpp:32 /rfx /gfx:avc444 /u:<user> /p:<password> /v:192.168.122.111 +fonts +clipboard +menu-anims +window-drag /drive:home,/home/Bahadir
        ```

    Replace `<user>` with your username and `<password>` with your password.

3. To run a specific Windows application, like `paint.exe`, without having to
navigate through the entire Windows desktop interface, you can utilize
xfreerdp's seamless mode:

    ```bash
    $ sudo xfreerdp /scale:180 /scale-desktop:200 /bpp:32 /rfx /gfx:avc444 /u:<user> /p:<password> /v:192.168.122.111 +fonts +clipboard +menu-anims +window-drag /drive:home,/home/Bahadir /app:"||<app-path>"
    ```

    Replace `<app-path>` with the path to your application, and again replace
    `<user>` and `<password>` with your username and password respectively.

## Setup Secure Boot

### Enable Setup Mode in BIOS settings

1. After reboot, go to BIOS screen.
2. In the **Boot** tab in the BIOS screen, set the **FIXED BOOT ORDER Priorities** as follows:
    - Boot Option #1: \[Hard Disk: GRUB]
    - Boot Option #2: \[USB Hard Disk]
    - Boot Option #3: \[USB CD/DVD]
    - Boot Option #4: \[USB Lan]
3. In **Security** tab, go to **Secure Boot** options and set **Secure Boot Mode** to **\[Custom]**.
4. Select **Reset to Setup Mode** and Enter. This will **delete all Secure Boot key databases from NVRAM**.
    - Hit Yes when it asks you if you want to proceed.
    - Hit No when it asks you reset without saving.
5. Select **Key Management** and Enter. In the page that open, set **Factory Default Key Provisioning** to **\[Disabled]**
6. In the **Save & Exit** tab click on **Save Changes and Reset**. The computer will restart automatically.

### Checking Secure Boot status

Check the secure boot status:

    ```
    $ sbctl status
    ```

You should see that sbctl is not installed, setup mode is enabled and secure boot is disabled.

### Creating and enrolling keys

Then create your custom secure boot keys:
`
 $ sudo sbctl create-keys
 `
Enroll your keys, with Microsoft\`s keys, to the UEFI:
`
 $ sudo sbctl enroll-keys -m
 `
Check the secure boot status again:

    ```
    $ sbctl status
    ```

sbctl should be installed now, but secure boot will not work until the boot files have been signed with the keys you just created.

### Signing files

Check what files need to be signed for secure boot to work:

    ```
    $ sudo sbctl verify
    ```

Now sign all the unsigned files. Usually the kernel and the boot loader need to be signed:

    ```
    $ sudo sbctl sign -s /boot/EFI/GRUB/grubx64.efi
    $ sudo sbctl sign -s /boot/grub/x86_64-efi/core.efi
    $ sudo sbctl sign -s /boot/grub/x86_64-efi/grub.efi
    $ sudo sbctl sign -s /boot/vmlinuz-linux

    ```

The files that need to be signed will depend on your system\`s layout, kernel and boot loader.

Now you are done! Reboot your system.

1. After reboot, go to BIOS screen.
2. In **Security** tab, go to **Secure Boot** options and set **Secure Boot Support** to **\[Enabled]**.
3. In the **Save & Exit** tab click on **Save Changes and Reset**. The computer will restart automatically.

If the boot loader and OS load, secure boot should be working. Check with:

    ```
    $ sbctl status
    ```

## Other Essential Packages

4\.  System administration

    *   `sbctl`: User-friendly way of setting up secure boot and signing files

6. Other useful tools:

    - `which`: Used to identify the location of a given executable

These tools will be useful later.

## Initramfs

Creating a new initramfs is usually not required, because mkinitcpio was run on installation of the kernel package with pacstrap. When you modify `mkinitcpio.conf`, run:

    ```
    # mkinitcpio -P
    ```

## Table of Contents

### [1. Home](./README.md)

### [2. Installation Guide](./INSTALLATION.md)

### [3. First Steps](./FIRSTSTEPS.md)

### [4. Graphical User Interface](./GUI.md)

### [5. Optional Tools and Configurations](./OPTIONAL.md)

### [6. Additional Information](./APPENDIX.md)

### [7. License](./LICENSE)
