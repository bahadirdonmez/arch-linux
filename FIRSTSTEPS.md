# First Steps After Installing Arch Linux

## Install OpenSSH

OpenSSH is a free and open-source software that facilitates secure communication
between two systems using a cryptographic network protocol. By installing
OpenSSH on your Arch Linux system, you can securely access and manage your
computer remotely.

1. Install OpenSSH by running the following command:

    ```bash
    $ sudo pacman -S openssh
    ```

2. Create a directory to store SSH configurations:

    ```bash
    $ mkdir ~/.ssh
    ```

3. Configure the SSH client to use the 1Password agent for authentication:

    ```bash
    $ echo -e "\
    # Configure the SSH client to use the 1Password agent for authentication \n\
    Host * \n\
      IdentityAgent ~/.1password/agent.sock \n\
    " >> ~/.ssh/config
    ```

4. Check the contents of the `~/.ssh/config` file and correct any errors:

    ```bash
    $ vim ~/.ssh/config
    ```

## Install Visual Studio Code (VSCode)

Visual Studio Code is a popular code editor developed by Microsoft.

Install the `code` package using the following command:

```bash
$ sudo pacman -S code
```

This command will install the latest version of VSCode on your system. Launch
VSCode from the application launcher or by running the `code` command in the
terminal.

## Configure Git

You can configure Git using the `git config` command. Follow these steps to
configure Git with your email address and name, as well as to set VSCode as the
default editor:

1. Set your email address for Git with the following command:

    ```bash
    $ git config --global user.email "doenmezb@ethz.ch"
    ```

2. Set your name for Git with the following command:

    ```bash
    $ git config --global user.name "Bahadır Dönmez"
    ```

3. Set VSCode as the default editor for Git with the following command:

    ```bash
    $ git config --global core.editor "code --wait"
    ```

4. You can check your Git configuration by running the following command:

    ```bash
    $ git config --list
    ```

## Set Up 1Password

1Password is a password manager that helps you securely store and manage
passwords.

### 1Password Installation

The desktop version of 1Password for Linux is available in the
[Arch User Repository (AUR)](https://aur.archlinux.org/packages/1password). For
more details, refer to the [1Password for Linux Download Page](
<https://support.1password.com/install-linux/#arch-linux>).

1. Obtain the 1Password signing key:

    ```bash
    $ curl -sS https://downloads.1password.com/linux/keys/1password.asc | gpg --import
    ```

2. Navigate to the `aur` directory and clone the `1password` package from the
AUR using the `git clone` command:

    ```bash
    $ cd ~/aur && git clone https://aur.archlinux.org/1password.git
    ```

3. Navigate to the `1password` directory, build, and install the package using
the `makepkg` command:

    ```bash
    $ cd ~/aur/1password && makepkg -sirc && git clean -dfX
    ```

### 1Password Configuration

1. After the installation is complete, launch 1Password from the application
launcher or by running the `1password` command in the terminal. Follow the
on-screen instructions to set up your 1Password account and start using the
password manager.

2. Configure 1Password preferences:

    - In **1Password > Settings > General**:

        - Set **Click the icon to:** to **Show the main window**.

        - Set the **Autofill** keyboard shortcut to <kbd>Ctrl</kbd> +
        <kbd>.</kbd>.

### SSH Agent Settings

1. Go to **1Password > Settings > Developer**:

    - Click on **Set Up SSH Agent...**.

    - Select **Use Key Names**.

    This will enable you to use your SSH keys stored in 1Password with Git and
    other tools that require SSH authentication.

2. In 1Password, find your GitHub/GitLab key and open it:

    - Click on **Configure...** in the **Next Step: Sign Your Git Commits** box.
    This will open a window with a snippet that you can add to your `.gitconfig`
    file.

    - Select **Edit Automatically** and 1Password will update your `.gitconfig`
    file with a single click.

    - Check the contents of the `~/.gitconfig` file and correct any errors:

        ```bash
        $ vim ~/.gitconfig
        ```

3. Add your GitHub/GitLab SSH key to the webpage:

    - Navigate to the SSH keys settings page and click on the **Add SSH key**
    button.

    - Paste the SSH key from 1Password into the Key field.

    - Repeat this process for both **Authentication Keys** and **Signing Keys**.

### Systemd 1Password Service

1. Create a systemd folder to manage services:

    ```bash
    $ mkdir -p ~/.config/systemd/user/
    ```

2. To launch 1Password at startup as a systemd service, run the following
command:

    ```bash
    $ echo -e "\
    [Unit]\n\
    Description=1Password Service\n\
    \n\
    [Service]\n\
    Type=simple\n\
    RestartSec=5\n\
    ExecStart=\
    /usr/bin/1password --silent \n\
    ExecStop=killall 1password \n\
    Restart=on-failure\n\
    \n\
    [Install]\n\
    WantedBy=default.target\n\
    " | sudo tee ~/.config/systemd/user/1password.service > /dev/null
    ```

3. Check the contents of the `~/.config/systemd/user/1password.service` file and
correct any errors:

    ```bash
    $ sudo vim ~/.config/systemd/user/1password.service
    ```

4. Enable the new service to run at startup:

    ```bash
    $ systemctl --user enable 1password.service
    ```

## Set up Google Drive

To manage Google Drive files on Arch Linux, we use Rclone.

### Rclone Installation and Configuration

1. Install Rclone by running the following command:

    ```bash
    $ sudo pacman -S rclone
    ```

2. Configure Rclone by running the following command:

    ```bash
    $ rclone config
    ```

    Follow the on-screen prompts to set up Rclone for use with your Google Drive
    account.

    - The remote name is: `google-drive`

    - Type of storage is: `drive`

    - For **Google Application Client Id**, log into the [Google API Console](
    <https://console.developers.google.com>) with your Google account.

    - Option scope is: `drive`

    For more detailed instructions on configuring Rclone for Google Drive, visit
    the [Rclone website](https://rclone.org/drive/).

### Systemd Google Drive Service and Auto Mount

1. If `fuse` is not already installed on your system, you can install it by
running the following command:

    ```bash
    $ sudo pacman -S fuse3
    ```

2. To mount your Google Drive as a systemd service, execute the command below:

    ```bash
    $ echo -e "\
    [Unit]\n\
    Description=Google Drive (Rclone) Service\n\
    Wants=network-online.target\n\
    After=network-online.target\n\
    \n\
    [Service]\n\
    Type=notify\n\
    RestartSec=5\n\
    ExecStartPre=/usr/bin/mkdir -p %h/\"Google Drive\" \n\
    ExecStart=\
    /usr/bin/rclone mount \
    google-drive: %h/\"Google Drive\" \
    --config=%h/.config/rclone/rclone.conf \
    --vfs-cache-mode full \n\
    ExecStop=/bin/fusermount -uz %h/\"Google Drive\"\n\
    Restart=on-failure\n\
    \n\
    [Install]\n\
    WantedBy=default.target\n\
    " | sudo tee ~/.config/systemd/user/google-drive.service > /dev/null
    ```

3. Verify the contents of the `~/.config/systemd/user/google-drive.service` file
and correct any errors if needed:

    ```bash
    $ sudo vim ~/.config/systemd/user/google-drive.service
    ```

4. Enable and start new service to run at startup by executing the following
command:

    ```bash
    $ systemctl --user enable --now google-drive.service
    ```

    Your Google Drive files will now be mounted at `~/Google Drive`.

## Checking System Clock Synchronization

It's crucial to ensure that the system clock is accurately synchronized to avoid
issues with time-based applications and services.

1. Use `timedatectl` to check the status of the system clock synchronization:

    ```bash
    $ timedatectl status
    ```

    This command will display the current status of the system clock
    synchronization. Look for the `System clock synchronized` line in the
    output. If it says `yes`, your clock is synchronized and accurate. If it
    says `no`, your clock is not synchronized and needs manual configuration.

2. To set your time zone:

    ```bash
    $ timedatectl set-timezone Europe/Zurich
    ```

3. Install the `ntp` package to synchronize the system clock:

    ```bash
    $ sudo pacman -S ntp
    ```

4. Perform a one-time synchronization by starting `ntpd` from the console using
the following command:

    ```bash
    $ sudo ntpd -u ntp:ntp
    ```

5. To maintain synchronization, enable the `ntpd.service` to start automatically
at boot:

    ```bash
    $ systemctl enable ntpd.service
    ```

6. Set the hardware clock from the system clock:

    ```bash
    $ sudo hwclock --systohc
    ```

7. Check the system clock synchronization status again using `timedatectl`:

    ```bash
    $ timedatectl status
    ```

    If it displays `System clock synchronized: yes`, your system clock is now
    synchronized and accurate.

    > **Note**\
    > It can take several minutes before `ntpd` selects a server to synchronize
    with. If the status still shows `no`, try checking it again after 17
    minutes.

## Setting Up a Clipboard Manager

A clipboard manager is a useful tool that enables you to keep track of the items
you have copied or cut to the clipboard, making it easier to access them later.
Follow these steps to set up the `xfce4-clipman-plugin` clipboard manager on
your system.

1. Install the `xfce4-clipman-plugin` package by running the following command:

    ```bash
    $ sudo pacman -S xfce4-clipman-plugin
    ```

2. Launch the Clipman Clipboard Manager with this command:

    ```bash
    $ xfce4-clipman
    ```

3. Configure Clipman settings by right-clicking on the Clipman Clipboard Manager
icon in the Status Tray Items and selecting **Clipman settings...**. Then, do
the following:

    - Under **Behavior**:

        - Uncheck **Sync mouse selections**.

        - Set **Paste instantly** to <kbd>Shift</kbd> + <kbd>Insert</kbd>.

        - Check **Position menu at mouse pointer**.

        - Set **Maximum items** option to **15**.

4. Enable `xfce4-popup-clipman` by setting a keyboard shortcut:

    - Go to **Settings > Keyboard > Application Shortcuts**.

    - Click **Add** and type `xfce4-popup-clipman`.

    - Set <kbd>Super</kbd> + <kbd>V</kbd> as the shortcut.

5. Set up autostart:

    - Go to **Settings > Session and Startup** and switch to the
    **Application Autostart** tab.

    - Find **Clipman (Clipboard manager)** and check the box next to it.

## Configuring Vim

To configure Vim to use the `xfce4-clipman-plugin` clipboard manager, follow
these steps:

1. Run the following command:

    ```bash
    $ echo -e "\
    \"Configure Vim to use the \`xfce4-clipman-plugin\` clipboard manager\n\
    set clipboard=unnamed,unnamedplus\n\
    " | sudo tee -a ~/.vimrc ~root/.vimrc > /dev/null
    ```

2. Add custom Vim shortcuts for copy and paste operations with
<kbd>Ctrl</kbd> - <kbd>C</kbd>, <kbd>Ctrl</kbd> + <kbd>V</kbd> and
<kbd>Ctrl</kbd> + <kbd>X</kbd>:

    ```bash
    $ echo -e "\
    \"Custom shortcuts for copy and paste operations\n\
    vmap <C-c> \"+yi\n\
    vmap <C-x> \"+c\n\
    vmap <C-v> c<ESC>\"+p\n\
    imap <C-v> <C-r><C-o>+\n\
    " | sudo tee -a ~/.vimrc ~root/.vimrc > /dev/null
    ```

3. Turn on color syntax highlighting in Vim:

    ```bash
    $ echo -e "\
    \"Turn on color syntax highlighting\n\
    syntax on\n\
    " | sudo tee -a ~/.vimrc ~root/.vimrc > /dev/null
    ```

4. Check the contents of the `.vimrc` files and correct any errors by running
the following commands:

    ```bash
    $ sudo vim ~/.vimrc
    ```

    ```bash
    $ sudo vim ~root/.vimrc
    ```

## Localization

The `locale` defines the language used by the system, as well as other regional
considerations such as currency denomination, numerology, and character sets.
You can configure your system's locale settings by following these steps:

1. Use the `sed` command to uncomment the necessary locales:

    ```bash
    $ sudo sed -i -e 's/#\(de_CH\.UTF-8 UTF-8\)/\1/' \
    -e 's/#\(de_CH ISO-8859-1\)/\1/' \
    -e 's/#\(en_US\.UTF-8 UTF-8\)/\1/' \
    -e 's/#\(en_US ISO-8859-1\)/\1/' \
    -e 's/#\(fr_CH\.UTF-8 UTF-8\)/\1/' \
    -e 's/#\(fr_CH ISO-8859-1\)/\1/' \
    -e 's/#\(tr_TR\.UTF-8 UTF-8\)/\1/' \
    -e 's/#\(tr_TR ISO-8859-9\)/\1/' \
    -e 's/#\(vi_VN UTF-8\)/\1/' /etc/locale.gen
    ```

    This will enable support for the German, English, French, Turkish, and
    Vietnamese languages, as well as their respective character sets.

2. Check the contents of the `/etc/locale.gen` file and correct any errors by
running the following command:

    ```bash
    $ sudo vim /etc/locale.gen
    ```

3. Generate the locales by running the following command:

    ```bash
    $ sudo locale-gen
    ```

4. Set the system-wide `LANG` variable to Swiss German by running the following
command:

    ```bash
    $ echo -e "\
    # This is the fallback locale configuration provided by systemd.\n\
    LANG=\"C.UTF-8\"\n\n\
    # Set the system-wide \`LANG\` variable to Swiss German\n\
    LANG=de_CH.UTF-8\n\
    " | sudo tee /etc/locale.conf > /dev/null
    ```

5. Check the contents of the `/etc/locale.conf` file and correct any errors by
running the following command:

    ```bash
    $ sudo vim /etc/locale.conf
    ```

6. Set the system-wide `KEYMAP` variable to Swiss German layout with the latin-1
character set by running the following command:

    ```bash
    $ echo -e "\
    # Set system \`KEYMAP\` to Swiss German layout with the Latin-1\n\
    KEYMAP=de_CH-latin1\n\
    " | sudo tee -a /etc/vconsole.conf > /dev/null
    ```

7. Check the contents of the `/etc/vconsole.conf` file and correct any errors by
running the following command:

    ```bash
    $ sudo vim /etc/vconsole.conf
    ```

    Note that the `vconsole.conf` file only affects the system console, and not
    the X server or any other graphical interface. If you want to change the
    keyboard layout for the X server or a graphical interface, you will need to
    configure it separately using the settings provided by the desktop
    environment or window manager.

## Network Configuration

### How to Set the Hostname on Your System

To set the hostname on your system, please follow these steps:

1. Create the `/etc/hostname` file and add your desired hostname to it. For
instance, if you want to set the hostname as `Bahadir-MSI`, then run the
following command:

    ```bash
    $ echo "Bahadir-MSI" | sudo tee /etc/hostname > /dev/null
    ```

2. Verify that the `/etc/hostname` file contains the correct hostname by running
the following command:

    ```bash
    $ sudo vim /etc/hostname
    ```

    Ensure that the file contains only the desired hostname as shown below:

    ```properties
    Bahadir-MSI
    ```

### How to Resolve Local Network Hostnames

1. Run the following command to add the necessary lines to `/etc/hosts`. This
file maps hostnames to IP addresses:

    ```bash
    $ echo -e "\
    # Static table lookup for hostnames.\n\
    # See hosts(5) for details.\n\n\
    # The following lines are desirable for IPv4 capable hosts\n\
    127.0.0.1       localhost\n\
    # 127.0.1.1 is often used for the FQDN of the machine\n\
    127.0.1.1       Bahadir-MSI\n\n\
    # The following lines are desirable for IPv6 capable hosts\n\
    ::1             localhost\n\
    " | sudo tee /etc/hosts > /dev/null
    ```

2. Verify that the `/etc/hosts` file contains the correct entries by running the
following command:

    ```bash
    $ sudo vim /etc/hosts
    ```

    Ensure that the file contains the following entries:

    ```properties
    # The following lines are desirable for IPv4 capable hosts
    127.0.0.1       localhost
    # 127.0.1.1 is often used for the FQDN of the machine
    127.0.1.1       Bahadir-MSI

    # The following lines are desirable for IPv6 capable hosts
    ::1             localhost
    ```

    The `127.0.1.1` entry maps the hostname to the loopback address. If you have
    a static IP address, use it instead of `127.0.1.1`.

### Changing Interface Names

To change the name of a network interface, you can manually define the name
using a `systemd.link` file.

1. Find the MAC address of your network card by running ip link:

    ```bash
    $ ip link
    ```

2. Create the `/etc/systemd/network/10-eth25cd.link` file and specify the new
name of the network interface:

    ```bash
    $ echo -e "\
    [Match]\n\
    PermanentMACAddress=aa:bb:cc:dd:ee:ff\n\
    \n\
    [Link]\n\
    Name=eth25cd\n\
    " | sudo tee /etc/systemd/network/10-eth25cd.link > /dev/null
    ```

    In the above example, we're changing the name of the network interface with
    the MAC address `aa:bb:cc:dd:ee:ff` to `eth25cd`.

3. Verify the contents of the `/etc/systemd/network/10-eth25cd.link` file by
running the following command:

    ```bash
    $ sudo vim /etc/systemd/network/10-eth25cd.link
    ```

    Ensure that the file contains the correct content as shown below:

    ```properties
    [Match]
    PermanentMACAddress=aa:bb:cc:dd:ee:ff

    [Link]
    Name=eth25cd
    ```

4. Repeat these steps for any other network interfaces you want to rename,
specifying the appropriate MAC address and new name:

    - `eth1dl`

    - `wlan6wf`

5. Restart the computer to apply the changes.

### Network Manager System Tray Applet

1. Install the `network-manager-applet` for managing network connections:

    ```bash
    $ sudo pacman -S network-manager-applet
    ```

2. Launch the NetworkManager applet:

    ```bash
    $ nm-applet
    ```

3. Open the NetworkManager connection editor:

    ```bash
    $ nm-connection-editor
    ```

    - Remove all existing connections.

    - Click the **+** button to add a new connection.

    - Choose **Ethernet** and click **Create...**.

    - Set the **Connection Name** to `Yallo Ethernet`.

    - Set the **Device** to `eth25cd`.

    - Click **Save**.

4. Right-click the panel applet and deselect **Enable Notifications**.

## Changing the Default Editor for Visudo

By default, the `visudo` command uses the `vi` editor to edit the
`/etc/sudoers` file. However, you can change the default editor to `vim` by
following these steps:

1. Use the `EDITOR` environment variable to specify the `vim` editor and open
the `/etc/sudoers` file for editing:

    ```bash
    $ sudo EDITOR=vim visudo
    ```

    This will open the `/etc/sudoers` file in the `vim` editor.

2. To change the `visudo` editor permanently system-wide add the following
to the bottom of the file:

    ```properties
    # Set default EDITOR to restricted version of vim, 
    # and do not allow visudo to use EDITOR/VISUAL.
    Defaults    editor=/usr/bin/rvim, !env_editor
    ```

    This will permanently set the default editor for visudo to `vim`.

3. To test the changes, use the `visudo` command to edit the `/etc/sudoers` file
with the `vim` editor:

    ```bash
    $ sudo visudo
    ```

    This should open the `/etc/sudoers` file in the `vim` editor.

## Pacman Configuration

Pacman is the default package manager for Arch Linux, which is used to install,
remove, and update software packages in the system. Proper configuration of
Pacman is crucial to optimize its functionality. This section will guide you on
how to add the multilib repository, enable colors and parallel downloads, and
update repositories and packages.

### Adding Multilib Repository

To add the multilib repository, follow these steps:

1. Uncomment the `multilib` section in the `/etc/pacman.conf` file by using the
following command:

    ```bash
    $ sudo sed -i -e '/^#\?\[multilib]/,/^#\?\(\[[^multilib]\|$\)/ {
    /^#\?\[[^multilib]/d
    s/^#//
    }' /etc/pacman.conf
    ```

2. Check the contents of `/etc/pacman.conf` file for any errors by using the
following command:

    ```bash
    $ sudo vim /etc/pacman.conf
    ```

    The `multilib` section should look like this:

    ```properties
    [multilib]
    Include = /etc/pacman.d/mirrorlist
    ```

### Miscellaneous Options

To enable colors and parallel downloads, follow these steps:

1. Use the following command:

    ```bash
    $ sudo sed -i -e '/^# Misc options$/,/^$/ {
    /^#Color/s/^#//
    /^#ParallelDownloads = 5/s/^#//
    }' /etc/pacman.conf
    ```

2. Check the contents of `/etc/pacman.conf` file for any errors by using the
following command:

    ```bash
    $ sudo vim /etc/pacman.conf
    ```

    The # Misc options section should look like this:

    ```properties
    # Misc options
    #UseSyslog
    Color
    #NoProgressBar
    CheckSpace
    #VerbosePkgLists
    ParallelDownloads = 5
    ```

### Update Repositories and Packages

To update repositories and packages, use the following command:

```bash
$ sudo pacman -Syu
```

If updating returns an error, open the `pacman.conf` file again and check for
errors.

### Clean Pacman Cache

Periodically cleaning up the cache is necessary to prevent the directory from
growing indefinitely in size. To clean the cache, follow these steps:

1. Install the `pacman-contrib` package by using the following command:

    ```bash
    $ sudo pacman -S pacman-contrib
    ```

    The `pacman-contrib` package includes the `paccache` script, which deletes
    all cached versions of installed and uninstalled packages, except for the
    most recent three, by default.

2. Enable and start `paccache.timer` to discard unused packages weekly by using
the following command:

    ```bash
    $ systemctl enable --now paccache.timer
    ```

## Audio Configuration

The audio configuration section provides instructions on how to set up the
PulseAudio sound server and install a panel applet for volume control.

### PulseAudio Sound Server

PulseAudio is a sound server that provides advanced features, including mixing
of multiple audio streams, network transparency, and per-application volume
control. To set up the PulseAudio sound server, follow these steps:

1. Install the `pulseaudio` package by using the following command:

    ```bash
    $ sudo pacman -S pulseaudio
    ```

2. Install `pavucontrol`, a simple GTK volume control tool ("mixer") for
PulseAudio:

    ```bash
    $ sudo pacman -S pavucontrol
    ```

3. Install `xfce4-pulseaudio-plugin`, which provides a panel applet for volume
control with support for keyboard volume control and volume notifications:

    ```bash
    $ sudo pacman -S xfce4-pulseaudio-plugin
    ```

4. Restart the computer to apply the changes.

5. Add the `xfce4-pulseaudio-plugin` to the Xfce panel by following these steps:

    - Right-click on the Xfce panel and select **Panel > Panel Preferences**.

    - In the **Panel Preferences** window, click on the **Items** tab.

    - Click on the **Add** button at the bottom of the window.

    - In the **Add New Item** window, select **PulseAudio Plugin** from the list
    and click **Add**.

    - In the **Items** tab, move the **PulseAudio Plugin** up, below the
    **Status Tray Items**.

    The PulseAudio icon should now appear in the panel.

### Advanced Linux Sound Architecture

Advanced Linux Sound Architecture (ALSA) is a software framework and part of
the Linux kernel that provides an API for sound card drivers. To recognize the
internal audio system, which is required for some newer laptop models, follow
these steps:

1. Install sof-firmware:

    ```bash
    $ sudo pacman -S sof-firmware
    ```

2. Restart the computer to apply the changes.

## Bluetooth Configuration

In Arch Linux, the `bluez` package provides the Bluetooth protocol stack and
tools for managing Bluetooth devices. To install and configure Bluetooth,
follow these steps:

### Installing Packages for Bluetooth Protocol

1. Install the `bluez` package:

    ```bash
    $ sudo pacman -S bluez
    ```

2. Enable and start `bluetooth.service`:

    ```bash
    $ systemctl enable --now bluetooth.service
    ```

3. To use audio equipment like Bluetooth headphones or speakers, install the
additional `pulseaudio-bluetooth` package:

    ```bash
    $ sudo pacman -S pulseaudio-bluetooth
    ```

### Installing Graphical Interface to Customize Bluetooth

1. Install the `blueman` package, which provides a full-featured graphical
Bluetooth manager:

    ```bash
    $ sudo pacman -S blueman
    ```

2. Launch a graphical settings panel with `blueman-manager`:

    ```bash
    $ blueman-manager
    ```

    Click on **Yes** when asked if Bluetooth should be automatically activated.

### Intel Combined WiFi and Bluetooth Cards

If you're experiencing difficulties connecting a Bluetooth headset and
maintaining a strong downlink speed, consider disabling Bluetooth coexistence.
Follow these steps to do so:

1. Disable Bluetooth coexistence by executing the command below:

    ```bash
    $ echo -e "\
    # Disable Bluetooth coexistence\n\
    options iwlwifi bt_coex_active=0\n\
    " | sudo tee -a /etc/modprobe.d/iwlwifi.conf > /dev/null
    ```

2. Verify the contents of the `/etc/modprobe.d/iwlwifi.conf` file and correct
any errors:

    ```bash
    $ sudo vim /etc/modprobe.d/iwlwifi.conf
    ```

3. Restart your computer to apply the changes.

## Table of Contents

### [1. Home](./README.md)

### [2. Installation Guide](./INSTALLATION.md)

### [3. First Steps](./FIRSTSTEPS.md)

### [4. Graphical User Interface](./GUI.md)

### [5. Optional Tools and Configurations](./OPTIONAL.md)

### [6. Additional Information](./APPENDIX.md)

### [7. License](./LICENSE)
