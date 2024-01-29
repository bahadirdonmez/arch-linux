# Useful Tools for Arch Linux

## Graphics

### Image Editing: Pinta

Pinta is a lightweight, user-friendly raster image editing program similar to
Microsoft Paint and Paint.NET.

To install Pinta on Arch Linux, use the following command:

```bash
sudo pacman -S pinta
```

### Image Viewer and Organizer: Shotwell

Shotwell is a digital photo organizer designed for the GNOME desktop
environment. It is compatible with other desktop environments, including Xfce.

1. To install Shotwell on Arch Linux, use the following command:

    ```bash
    sudo pacman -S shotwell
    ```

2. After the installation is complete, launch Shotwell from the application
launcher.

3. Create a folder to save imported pictures by running the following command:

    ```bash
    mkdir -p ~/Pictures/Import
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
    cd ~/Downloads
    curl -O https://installers.privateinternetaccess.com/download/pia-linux-3.5.1-07760.run
    ```

2. Once the download is complete, run `chmod +x` on it to make it executable:

    ```bash
    chmod +x pia-linux-3.5.1-07760.run
    ```

3. Run the installer with the following command:

    ```bash
    ./pia-linux-3.5.1-07760.run
    ```

4. After the installation is complete, launch PIA VPN from the application
launcher and log in with your PIA VPN account credentials.

5. Configure your PIA VPN preferences by navigating to **PIA VPN > Settings**:

    - In the **General** tab:

        - Check **Launch on System Startup**.

        - Uncheck **Show Desktop Notifications**.

### ETH Zurich VPN Configuration

To establish an encrypted remote connection to the data network of ETH Zurich, we will
use OpenConnect, which is a client for Cisco's AnyConnect SSL VPN.

1. Install the `networkmanager-openconnect` package to enable OpenConnect support in
NetworkManager:

    ```bash
    sudo pacman -S networkmanager-openconnect
    ```

2. Install the gnome-keyring package to securely store your VPN credentials:

    ```bash
    sudo pacman -S gnome-keyring
    ```

3. Restart the `NetworkManager.service` to apply the changes:

    ```bash
    systemctl restart NetworkManager.service
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

    - When the GNOME Keyring asks for a password, enter your Arch Linux password.

    - For future connections, check the
    **Automatically start connection next time** box.

Once connected, a lock icon should appear on the NetworkManager applet,
indicating that the VPN connection is active.

### Importing OpenVPN Config (.ovpn) Files

1. Install the necessary packages for OpenVPN support:

    ```bash
    sudo pacman -S networkmanager-openvpn
    ```

2. Restart the `NetworkManager.service` to apply the changes:

    ```bash
    systemctl restart NetworkManager.service
    ```

3. Download the `.ovpn` file  from the source.

4. Left-click on the NetworkManager applet icon in the system tray to open the  
NetworkManager menu.

    - Click on **VPN Connections** and then **Configure VPN...**.

    - Click on the **+** button to add a new connection.

    - Choose **Import a saved VPN configuration...** as the connection type and click
    on **Create...**.

    - Browse to the location where you saved the `.ovpn` file, select it, and click
    **Open**.

5. The settings from the `.ovpn` file should be automatically populated. You can review
and modify these settings if necessary.

    - Connection name: You can rename it to something descriptive if desired.

    - Fill **Username** and **Password** fields, leave the **Gateway** field as it is.

    - Click on **Save** to store the configuration.

6. To connect to the VPN, click on the NetworkManager applet icon again, navigate to
**VPN Connections**, and select the VPN connection you just created.

Once connected, a lock icon should appear on the NetworkManager applet, indicating that
the VPN connection is active.

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
    sudo pacman -S avahi nss-mdns
    ```

2. Edit the `/etc/nsswitch.conf` file and modify the `hosts` line to include
`mdns_minimal [NOTFOUND=return]` before `resolve` and `dns`:

    ```bash
    sudo sed -i '/^hosts:/ s/\(mymachines \)\(resolve \[!UNAVAIL=return\]\)/\1mdns_minimal [NOTFOUND=return] \2/' \
    /etc/nsswitch.conf
    ```

3. Verify the contents of the `/etc/nsswitch.conf` file and correct any errors:

    ```bash
    sudo vim /etc/nsswitch.conf
    ```

    Ensure that the `hosts` line has the correct content as shown below:

    ```properties
    hosts: mymachines mdns_minimal [NOTFOUND=return] resolve [!UNAVAIL=return] files myhostname dns
    ```

4. Enable and start the Avahi service:

    ```bash
    systemctl enable --now avahi-daemon.service
    ```

5. Avahi includes several utilities that help you discover the services running
on a network. To discover the printer, for example, run the following command:

    ```bash
    avahi-browse --all --ignore-local --resolve --terminate
    ```

    Note the printer's hostname, such as: `BRN001BA95D5DB6.local`.

### Printer Installation and Configuration

CUPS, the standards-based open-source printing system developed by
OpenPrinting, is designed for Linux and other Unix-like operating systems. Arch
Linux packages the OpenPrinting CUPS fork instead of the Apple CUPS fork.

Follow these steps to install and configure the Brother MFC-J6710DW printer:

1. Install CUPS:

    ```bash
    sudo pacman -S cups
    ```

2. Install Ghostscript, which is required for non-PDF printers:

    ```bash
    sudo pacman -S ghostscript
    ```

3. Enable and start the CUPS service using the following command:

    ```bash
    systemctl enable --now cups.service
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
    `Brother MFC-J6710DW, driverless, cups-filters 2.0.0 (en)` and click on
    **Add Printer**.

    - In **Default Settings**:

        - Set **Media Size** to **A4**.

        - Set **Print Color Mode** to **Monochrome**.

        - Set **2-Sided Printing** to **On (Portrait)**.

    Now you should be able to print with your Brother MFC-J6710DW printer.

5. Install the GTK printer configuration tool and status applet:

    ```bash
    sudo pacman -S system-config-printer
    ```

### Scanner Installation and Configuration

SANE (Scanner Access Now Easy) provides a library and a command-line tool for
using scanners under GNU/Linux.

Follow these steps to install and configure the scanner:

1. Install the `sane` package:

    ```bash
    sudo pacman -S sane
    ```

2. Navigate to the `aur` directory and clone the `brscan4` package from the AUR
using the `git clone` command:

    ```bash
    cd ~/.aur
    git clone https://aur.archlinux.org/brscan4.git
    ```

3. Change to the `brscan4` directory, build, and install the package using the
`makepkg` command:

    ```bash
    cd ~/.aur/brscan4
    makepkg -sirc
    git clean -dfX
    ```

4. For network scanners, Brother provides a configuration tool. Replace the
part `BRN001BA95D5DB6.local` with your scanner's hostname found using the
`avahi-browse` command:

    ```bash
    sudo brsaneconfig4 -a name=Brother_MFC_J6710DW model=MFC-J6710DW nodename=BRN001BA95D5DB6.local
    ```

5. Now, the scanner should be recognized by SANE. To check, run:

    ```bash
    scanimage -L
    ```

6. Install the Simple Scan frontend for SANE:

    ```bash
    sudo pacman -S simple-scan
    ```

7. Create a folder to save scanned documents by running the following command:

    ```bash
    mkdir -p ~/Pictures/Scanned
    ```

## Communication and Chat Applications

### WhatsApp

There is no official WhatsApp desktop client for Linux, and Meta has strictly attempted
to ban third-party clients and plugins using their protocol.

You can use WhatsApp for Linux to use WhatsApp in Arch. It is an unofficial WhatsApp
desktop application written in C++:

1. Navigate to the `aur` directory and clone the `whatsapp-for-linux` package from the
AUR using the `git clone` command:

    ```bash
    cd ~/.aur
    git clone https://aur.archlinux.org/whatsapp-for-linux.git
    ```

2. Navigate to the `whatsapp-for-linux` directory, then build and install the package
using the `makepkg` command:

    ```bash
    cd ~/.aur/whatsapp-for-linux
    makepkg -sirc
    git clean -dfX
    ```

3. Configure your Whatsapp preferences by navigating to
**Whatsapp for Linux > Settings**:

    - In the **General** tab:

        - Uncheck **Enable Notification Sounds**.

### Matrix Client: Element

Matrix is an ambitious new ecosystem for open federated instant messaging.

1. Element is a Glossy Matrix client with an emphasis on performance and usability.
Install the desktop application based on the Electron platform using the following
command:

    ```bash
    sudo pacman -S element-desktop
    ```

2. Log in to Element using:

    - **Homeserver:** `https://matrix.phys.ethz.ch`

    - **Username:** `@username:phys.ethz.ch`

    - **Password:** D-PHYS password

3. Configure your Element preferences by navigating to **Element > All settings**:

    - In the **Preferences** tab:

        - Uncheck **Show tray icon and minimise window to it on close**.

### Zoom

Zoom, a popular video conferencing application, can be installed on Arch Linux through
the Arch User Repository (AUR).

1. Navigate to the `aur` directory and clone the `zoom` package from the AUR:

    ```bash
    cd ~/.aur
    git clone https://aur.archlinux.org/zoom.git
    ```

2. Navigate to the `zoom` directory, then build and install the package using the
`makepkg` command. Clean up the build files after installation using the `git clean`
command:

    ```bash
    cd ~/.aur/zoom
    makepkg -sirc
    git clean -dfX
    ```

## Running Windows Applications on Linux

There may be circumstances where Linux users need to run software exclusive to Windows.
One effective solution is to utilize virtualization technology, such as VMware, which
allows a fully-functional Windows operating system to run within the Linux environment.

### Setting up VMware Workstation Player

VMware offers a virtual platform, specifically the free-to-use VMware Workstation
Player, that enables running Windows applications on a Linux machine.

1. VMware Keymaps is necessary for certain VMware packages. To install it, navigate to
the `aur` directory and clone the `vmware-keymaps` package from the AUR (Arch User
Repository) using the `git clone` command:

    ```bash
    cd ~/.aur
    git clone https://aur.archlinux.org/vmware-keymaps.git
    ```

2. Navigate to the `vmware-keymaps` directory, then build and install the package using
the `makepkg` command:

    ```bash
    cd ~/.aur/vmware-keymaps
    makepkg -sirc
    git clean -dfX
    ```

3. You need to install the appropriate kernel headers package for your installed kernel.
Use the following command to do so:

    ```bash
    sudo pacman -S linux-headers
    ```

4. Go back to the `aur` directory and clone the `vmware-workstation` package from the
AUR using the `git clone` command:

    ```bash
    cd ~/.aur
    git clone https://aur.archlinux.org/vmware-workstation.git
    ```

5. Navigate to the `vmware-workstation` directory, then build and install the package
using the `makepkg` command:

    ```bash
    cd ~/.aur/vmware-workstation
    makepkg -sirc
    git clean -dfX
    ```

6. Start the `vmware-networks-configuration.service` to generate
`/etc/vmware/networking`:

    ```bash
    systemctl start vmware-networks-configuration.service
    ```

7. Enable the `vmware-networks.service` for guest network access:

    ```bash
    systemctl enable --now vmware-networks.service
    ```

8. Enable the `vmware-usbarbitrator` to connect USB devices to the guest system:

    ```bash
    systemctl enable --now vmware-usbarbitrator
    ```

9. Finally, load the VMware modules with this command:

    ```bash
    sudo modprobe -a vmw_vmci vmmon
    ```

After following these steps, VMware Workstation Player should be fully installed and
ready for use on your Arch Linux system.

### Creating a Virtual Machine with VMware Workstation Player

After successfully installing VMware Workstation Player, you can create a virtual
machine (VM) to run Windows:

1. Download the Windows 11 Disk Image (ISO) for x64 devices from the
[Microsoft Software Download](https://www.microsoft.com/software-download/windows11)
page.

2. Open VMware Workstation Player and select **Create a New Virtual Machine**.

3. Choose **Use ISO image** option, then browse and select your Windows 11 Disk Image
(ISO) file. Click **Next**.

4. When you are prompted about **Key in the Password for Encryption**:

    - Choose the **Only the files need to support a TPM are encrypted** to enable
    virtual TPM support without encrypting the entire virtual machine.

    - After selecting this option, provide a strong, unique password.

5. When you are prompted for **Disk Size**:

    - Allocate `256 GB` (1/8 of the available storage) to the hard disk.

    - Choose **Store virtual disk as a single file**.

6. Click on **Customize Hardware**:

    - **Memory:** Assign `8192 MB`.

    - **Processors:** Assign `4` processor cores.

    - **Network Adapter:** Choose **Bridged** to connect directly to the physical
    network and get a separate IP.

7. Click **Close** and then click **Finish**. This will begin the Windows 11
installation process.

## Customize the Virtual Machine

1. After Windows 11 has been successfully installed on the VM, you should install the
VMware Tools.

2. Some graphics drivers are blacklisted by default, due to poor and/or unstable 3D
acceleration. Set `mks.gl.allowBlacklistedDrivers = TRUE` in `~/.vmware/preferences`
file to override this.

    ```bash
    sed -i '/^mks\.gl\.allowBlacklistedDrivers =/d' ~/.vmware/preferences
    echo 'mks.gl.allowBlacklistedDrivers = "TRUE"' >> ~/.vmware/preferences
    ```

## Table of Contents

### [1. Home](./README.md)

### [2. Installation Guide](./INSTALLATION.md)

### [3. First Steps](./FIRSTSTEPS.md)

### [4. Graphical User Interface](./GUI.md)

### [5. Optional Tools and Configurations](./OPTIONAL.md)

### [6. Additional Information](./APPENDIX.md)

### [7. License](./LICENSE)
