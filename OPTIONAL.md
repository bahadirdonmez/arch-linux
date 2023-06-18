# Useful Tools for Arch Linux

## Graphics

### Image Editing: Pinta

Pinta is a lightweight, user-friendly raster image editing program similar to
Microsoft Paint and Paint.NET.

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

    - In **Default Settings**:

        - Set **Media Size** to **A4**.

        - Set **Print Color Mode** to **Monochrome**.

        - Set **2-Sided Printing** to **On (Portrait)**.

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

## Messaging Apps

Among all messaging apps, WhatsApp and Signal stand out due to their popularity
and unique features. This guide details how to utilize these apps on Arch Linux.

### WhatsApp

WhatsApp Web offers a seamless way to enjoy the functionality of WhatsApp
directly on your computer:

1. Launch Chrome and navigate to the profile icon situated at the top right
corner, then click on **Add**.

2. In the subsequent dialog box, click on **Continue without an account**.

3. Input a preferred name for your new profile, such as `WhatsApp Web`. Select
an icon of your choice, and finalize by clicking on **Add**.

4. Now in the newly created profile window, direct your browser to the
[WhatsApp Web](<https://web.whatsapp.com/>).

5. On the right side of the address bar, locate the **Install WhatsApp Web**
button and click on it. This action will generate a desktop shortcut for
WhatsApp Web on your machine.

6. To ensure that the WhatsApp Web application always launches in the newly
created profile and all other links are opened in your default profile, modify
the Chrome configuration file with the `--profile-directory=Default` flag:

    ```bash
    $ echo -e "\
    --profile-directory=Default\n\
    " | tee ~/.config/chrome-flags.conf > /dev/null
    ```

    Validate the changes to the `~/.config/chrome-flags.conf` file:

    ```bash
    $ vim ~/.config/chrome-flags.conf
    ```

    The contents of the file should be as follows:

    ```properties
    --profile-directory=Default
    ```

7. Finally, to personalize the appearance of WhatsApp Web in your new profile,
adhere to the following steps:

    - Navigate to **Settings > Appearance**.

    - Under the **Themes** section, click on **Use GTK**.

### Signal

Signal is a highly secure messaging platform renowned for its robust encryption
standards. To install the Signal Desktop client on Arch Linux, execute the
following command in the terminal:

```bash
$ sudo pacman -S signal-desktop
```

## Running Windows Applications on Linux

There may be circumstances where Linux users need to run software exclusive to
Windows. One effective solution is to utilize virtualization technology, such
as VMware, which allows a fully-functional Windows operating system to run
within the Linux environment.

### Setting up VMware Workstation Player

VMware offers a virtual platform, specifically the free-to-use VMware
Workstation Player, that enables running Windows applications on a Linux
machine.

1. VMware Keymaps is necessary for certain VMware packages. To install it,
navigate to the `aur` directory and clone the `vmware-keymaps` package from the
AUR (Arch User Repository) using the `git clone` command:

    ```bash
    $ cd ~/aur && git clone https://aur.archlinux.org/vmware-keymaps.git
    ```

2. Navigate to the `vmware-keymaps` directory, then build and install the
package using the `makepkg` command:

    ```bash
    $ cd ~/aur/vmware-keymaps && makepkg -sirc && git clean -dfX
    ```

3. You need to install the appropriate kernel headers package for your
installed kernel. Use the following command to do so:

    ```bash
    $ sudo pacman -S linux-headers
    ```

4. Go back to the `aur` directory and clone the `vmware-workstation` package
from the AUR using the `git clone` command:

    ```bash
    $ cd ~/aur && git clone https://aur.archlinux.org/vmware-workstation.git
    ```

5. Navigate to the `vmware-workstation` directory, then build and install the
package using the `makepkg` command:

    ```bash
    $ cd ~/aur/vmware-workstation && makepkg -sirc && git clean -dfX
    ```

6. Start the `vmware-networks-configuration.service` to generate
`/etc/vmware/networking`:

    ```bash
    $ systemctl start vmware-networks-configuration.service
    ```

7. Enable the `vmware-networks.service` for guest network access:

    ```bash
    $ systemctl enable vmware-networks.service
    ```

8. Enable the `vmware-usbarbitrator` to connect USB devices to the guest system:

    ```bash
    $ systemctl enable vmware-usbarbitrator
    ```

9. Finally, load the VMware modules with this command:

    ```bash
    $ sudo modprobe -a vmw_vmci vmmon
    ```

After following these steps, VMware Workstation Player should be fully
installed and ready for use on your Arch Linux system.

--------------------------------------------------------------------------------

### Creating a Virtual Machine with VMware Workstation Player

After successfully installing VMware Workstation Player, you can create a virtual machine (VM) to run Windows:

1. Open VMware Workstation Player and select **Create a New Virtual Machine**.

2. Opt for the **Installer disc image file (iso)** option, then browse and select your Windows ISO file. Click **Next**.

3. Choose your Windows version from the dropdown menu, then click **Next**.

4. Define the maximum disk size and specify whether the virtual disk should be split into multiple files or stored as a single file. Click **Next**.

5. Review your settings. Click **Finish** to create the VM.

With your VM ready, you can install Windows applications as you normally would on a native Windows system. Keep in mind, running Windows applications on Linux via VMware might have some performance compromises due to the virtualization overhead.

--------------------------------------------------------------------------------

### Installing VirtualBox

Oracle's VirtualBox is a robust virtualization software compatible with x86 and
AMD64/Intel64 architectures. Its intuitive interface simplifies the procedure
of creating and managing virtual machines.

1. To install VirtualBox, run the following command in the terminal:

    ```bash
    $ sudo pacman -S virtualbox
    ```

    During installation, you'll be asked to choose a package that supplies the
    host modules, crucial for the virtualization process. If you're using the
    standard Linux kernel, select `virtualbox-host-modules-arch`.

2. Install the `virtualbox-guest-iso` package on your host machine:

    ```bash
    $ sudo pacman -S virtualbox-guest-iso
    ```

    This package includes a disk image (.iso file) required for installing
    VirtualBox Guest Additions on guest operating systems.

3. Download the Windows 11 Disk Image (ISO) for x64 devices from the
[Microsoft Software Download](
<https://www.microsoft.com/software-download/windows11>) page.

4. Edit the `/etc/default/grub` file:

    ```bash
    $ sudo vim /etc/default/grub
    ```

    Insert your kernel options between the quotes in the
    `GRUB_CMDLINE_LINUX_DEFAULT` line:

    ```properties
    GRUB_CMDLINE_LINUX_DEFAULT="loglevel=3 quiet ibt=off"
    ```

    Then, automatically regenerate the `grub.cfg` file with:

    ```bash
    $ sudo grub-mkconfig -o /boot/grub/grub.cfg
    ```

### Setting up a Virtual Machine in VirtualBox

1. Launch VirtualBox. Click on the **New** button to start the creation of a
new virtual machine.

2. Switch to **Expert Mode** for more configuration options.

3. In the **Name and Operating System** section:

    - Assign a unique name to your virtual machine, such as `Windows11`.

    - For the **ISO Image**, locate and select the Windows 11 Disk Image (ISO)
    you downloaded earlier, then click **Open**.

    - Set the machine type to `Microsoft Windows` and select
    `Windows 11 (64-bit)` for the version.

    - To manually control the installation process, enable
    **Skip Unattended Installation**.

4. In the **Hardware** section:

    - Allocate `16384` (1/4 of the available memory) to the base memory.

    - Assign `8` CPUs to the virtual machine (1/4 of the total available CPUs).

5. In the **Hard Disk** section:

    - Allocate `512 GB` (1/4 of the available storage) to the hard disk.

6. Click **Finish**.

## Customize the Virtual Machine

After creating your virtual machine, you can modify its settings to match your
specific requirements.

1. Select your virtual machine and click on the **Settings** button.

2. Navigate to the **General** section and select the **Advanced** tab:

    - Set the **Shared Clipboard** and **Drag'n'Drop** to `Bidirectional`.

3. Navigate to the **System** section:

    - Select the **Motherboard** tab:

        - Check the **Hardware Clock in UTC Time** option.

    - Select the **Processor** tab:

        - Check the **Enable PAE/NX** option.

        - Check the **Enable VT-x/AMD-V** option.

    - Select the **Acceleration** tab:

        - Choose the paravirtualization interface **Hyper-V** from the drop
        down menu.

        - Check the **Enable Nested Paging** option.

4. Navigate to the **Display** section and select the **Screen** tab:

    - Check the **Enable 3D Acceleration** option.

    - Increase the **Video Memory** to the maximum.

5. After customizing your virtual machine, click **OK** to save and close the
settings dialog.

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

## Table of Contents

### [1. Home](./README.md)

### [2. Installation Guide](./INSTALLATION.md)

### [3. First Steps](./FIRSTSTEPS.md)

### [4. Graphical User Interface](./GUI.md)

### [5. Optional Tools and Configurations](./OPTIONAL.md)

### [6. Additional Information](./APPENDIX.md)

### [7. License](./LICENSE)
