# First Steps After Installing Arch Linux

## Set up Keyboard and Display Settings

1. Set up display scaling:

    - Go to **Settings > Appearance** and switch to the **Settings** tab.

    - Under **Window Scaling**, select **2x** option.

2. Set Swiss German keyboard layout:

    - Go to **Settings > Keyboard** and switch to the **Layout** tab.

    - Disable the **Use system defaults** option.

    - Click on **Add** and select **German (Switzerland)** from the list.

    - Select any other keyboards in the list and click on **Remove** to keep
    only the **German (Switzerland)** layout.

## Connect to Internet Using NetworkManager

To be able to do anything on Arch Linux, we need to ensure that we are
connected to the internet.

Enable and start the NetworkManager service using the following command:

```shell
$ systemctl enable --now NetworkManager.service
```

### Wired Connection

If you are using a wired connection, you should already have an internet
connection.

### Wireless Connection

If you are using a laptop, you can connect to a wireless access point using the
`nmcli` command.

1. List nearby Wi-Fi networks using the following command:

    ```shell
    $ nmcli device wifi list
    ```

2. Connect to your Wi-Fi network using the following command:

    ```shell
    $ nmcli device wifi connect yallo_3D04C9 password "password"
    ```

    Replace "password" with your Wi-Fi password.

### Verify Connection

Ping `google.com` to confirm that you are online:

```shell
$ ping google.com
```

## Install Git

Git is a popular version control system used to track changes to files and
directories. It is often used by software developers to manage source code.

1. To install Git, run the following command:

    ```shell
    $ sudo pacman -S git
    ```

2. You can create a directory to store your Git repositories. To create a
directory named `git` in your home directory, run the following command:

    ```shell
    $ mkdir ~/git
    ```

## Enable Arch User Repository (AUR)

The Arch User Repository (AUR) is a community-driven repository for Arch Linux
users. It contains many useful packages that are not available in the official
Arch Linux repositories.

1. Install the `base-devel` package group, which contains the tools needed to
build and install packages from the AUR:

    ```shell
    $ sudo pacman -S base-devel
    ```

2. Create a directory to store AUR packages. For example, you can create a
directory named `aur` in your home directory:

    ```shell
    $ mkdir ~/aur
    ```

## Install Google Chrome

Google Chrome is a popular web browser developed by Google. You can install
Google Chrome from the [Arch User Repository (AUR)](
https://aur.archlinux.org/packages/google-chrome).

1. Change to the `aur` directory and clone the `google-chrome` package from the
AUR using the `git clone` command:

    ```shell
    $ cd ~/aur && git clone https://aur.archlinux.org/google-chrome.git
    ```

2. Change to the `google-chrome` directory, build and install the package using
the `makepkg` command:

    ```shell
    $ cd ~/aur/google-chrome && makepkg -sirc
    ```

3. Clean up the build directory to free up space:

    ```shell
    $ git clean -dfX
    ```

## Install OpenSSH

OpenSSH is a free and open-source software that enables secure communication
between two systems using a cryptographic network protocol. Installing OpenSSH
on your Arch Linux system allows you to securely access and manage your
computer remotely.

1. Install OpenSSH by running the following command:

    ```shell
    $ sudo pacman -S openssh
    ```

2. Create a directory to store SSH configurations:

    ```shell
    $ mkdir ~/.ssh
    ```

3. Configure the SSH client to use the 1Password agent for authentication:

    ```shell
    $ echo -e "\
    # Configure the SSH client to use the 1Password agent for authentication \n\
    Host * \n\
    IdentityAgent ~/.1password/agent.sock \n\
    " >> ~/.ssh/config
    ```

4. Check the contents of `~/.ssh/config` file and correct any errors:

    ```shell
    $ vim ~/.ssh/config
    ```

## Install Visual Studio Code (VSCode)

Visual Studio Code is a popular code editor developed by Microsoft.

Install the `code` package using the following command:

```shell
$ sudo pacman -S code
```

This will install the latest version of VSCode on your system. Launch VSCode
from the application launcher or by running the `code` command in the terminal

## Configure Git

Git can be configured with the `git config` command. Here are the steps to
configure Git with your email address and name, as well as to set VSCode as the
default editor:

1. Set your email address for Git with the following command:

    ```shell
    $ git config --global user.email "doenmezb@ethz.ch"
    ```

2. Set your name for Git with the following command:

    ```shell
    $ git config --global user.name "Bahadır Dönmez"
    ```

3. Set VSCode as the default editor for Git with the following command:

    ```shell
    $ git config --global core.editor "code --wait"
    ```

4. You can check your Git configuration by running the following command:

    ```shell
    $ git config --list
    ```

## Set Up 1Password

1Password is a password manager that can help you securely store and manage
passwords.

### 1Password Installation

The desktop version of 1Password for Linux is available in
[Arch User Repository (AUR)](https://aur.archlinux.org/packages/1password). For
more details, refer to [1Password for Linux Download Page](
https://support.1password.com/install-linux/#arch-linux).

1. Get the 1Password signing key:

    ```shell
    $ curl -sS https://downloads.1password.com/linux/keys/1password.asc | gpg --import
    ```

2. Change to the `aur` directory and clone the `1password` package from the AUR
using the `git clone` command:

    ```shell
    $ cd ~/aur && git clone https://aur.archlinux.org/1password.git
    ```

3. Change to the `1password` directory, build and install the package using the
`makepkg` command:

    ```shell
    $ cd ~/aur/1password && makepkg -sirc
    ```

4. Clean up the build directory to free up space:

    ```shell
    $ git clean -dfX
    ```

### 1Password Configuration

1. Once the installation is complete, you can launch 1Password from the
application launcher or by running the `1password` command in the terminal.
Follow the on-screen instructions to set up your 1Password account and start
using the password manager.

2. Configure 1Password preferences:

    - In **1Password > Settings > General**:

        - Set **Click the icon to:** **Show the main window**.

        - Set **Autofill** keyboard shortcut to <kbd>Ctrl</kbd> + <kbd>.</kbd>.

### SSH Agent Settings

1. Go to **1Password > Settings > Developer**:

    - Click on **Set Up SSH Agent...**.

    - Select **Use Key Names**.

    This will allow you to use your SSH keys stored in 1Password with Git and
    other tools that require SSH authentication.

2. In 1Password, find your GitHub/GitLab key and open it:

    - Click on **Configure...** in the **Next Step: Sign Your Git Commits** box.
    This will open a window with a snippet that you can add to your `.gitconfig`
    file.

    - Select **Edit Automatically** and 1Password will update your
    `.gitconfig` file with a single click.

    - Check the contents of `~/.gitconfig` file and correct any errors:

        ```shell
        $ vim ~/.gitconfig
        ```

3. Add your GitHub/GitLab SSH key to the webpage:

    - Navigate to the SSH keys settings page and click on the **Add SSH key**
    button.

    - Paste the SSH key from 1Password into the Key field.

    - Repeat this both for **Authentication Keys** and **Signing Keys**.

### Systemd 1Password Service

1. Create a systemd folder to manage services:

    ```shell
    $ mkdir -p ~/.config/systemd/user/
    ```

2. To launch 1Password at start as a systemd service, run the following command:

    ```shell
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
    " | sudo tee ~/.config/systemd/user/1password.service
    ```

3. Check the contents of `~/.config/systemd/user/1password.service` file
and correct any errors:

    ```shell
    $ sudo vim ~/.config/systemd/user/1password.service
    ```

4. Enable the new service to run at startup:

    ```shell
    $ systemctl --user enable 1password.service
    ```

## Set up Google Drive

To manage Google Drive files on Arch Linux, we use Rclone.

### Rclone Installation and Configuration

1. Install Rclone by running the following command:

    ```shell
    $ sudo pacman -S rclone
    ```

2. Configure Rclone by running the following command:

    ```shell
    $ rclone config
    ```

    Follow the on-screen prompts to set up Rclone for use with your Google Drive
    account.

    - The remote name is: **"google-drive"**

    - Type of storage is: **"drive"**

    - For **Google Application Client Id** log into the [Google API Console](
    https://console.developers.google.com) with your Google account.

    For more detailed instructions on configuring Rclone for Google
    Drive, visit the [Rclone website](https://rclone.org/drive/).

### Systemd Service and Auto Mount

1. If `fuse` is not already installed on your system, install it by running:

    ```shell
    $ sudo pacman -S fuse2
    ```

2. To mount your Google Drive as a systemd service, run the following command:

    ```shell
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
    " | sudo tee ~/.config/systemd/user/google-drive.service
    ```

3. Check the contents of `~/.config/systemd/user/google-drive.service` file
and correct any errors:

    ```shell
    $ sudo vim ~/.config/systemd/user/google-drive.service
    ```

4. Enable the new service to run at startup:

    ```shell
    $ systemctl --user enable google-drive.service
    ```

5. Start the service:

    ```shell
    $ systemctl --user start google-drive.service
    ```

    Google Drive files will now be mounted at `~/Google Drive`

## Check the System Clock Synchronization

It's important to ensure that the system clock is accurately synchronized to
avoid issues with time-based applications and services.

1. Use `timedatectl` to check the status of the system clock synchronization:

    ```shell
    $ timedatectl status
    ```

    This will display the current status of the system clock synchronization.
    Look for the **"System clock synchronized"** line in the output. If it says
    **"yes"**, your clock is synchronized and accurate. If it says **"no"**,
    your clock    is not synchronized and will need to be manually configured.

2. To set your time zone:

    ```shell
    $ timedatectl set-timezone Europe/Zurich
    ```

3. Install the `ntp` package to synchronize the system clock:

    ```shell
    $ sudo pacman -S ntp
    ```

4. Start it from the console using the following command:

    ```shell
    $ sudo ntpd -u ntp:ntp
    ```

    Note that this command sets up a one-time synchronization.

5. To keep the synchronization running, enable the `ntpd.service` to start
automatically at boot:

    ```shell
    $ systemctl enable ntpd.service
    ```

6. Set the hardware clock from the system clock:

    ```shell
    $ sudo hwclock --systohc
    ```

7. Check the system clock synchronization status again using `timedatectl`.

    ```shell
    $ timedatectl status
    ```

    If it displays **"System clock synchronized: yes"**, your system clock is
    now synchronized and accurate.

    > **Note**\
    > It can take several minutes before `ntpd` selects a server to synchronize
    with. If the status still shows "no", try checking it again after 17
    minutes.

## Set Up Clipboard Manager

A clipboard manager is a helpful tool that allows you to keep track of the
items you've copied or cut to the clipboard, making it easy to access them
later.

1. Install the `xfce4-clipman-plugin` package with the following command:

    ```shell
    $ sudo pacman -S xfce4-clipman-plugin
    ```

2. Add the `xfce4-clipman-plugin` to the XFCE panel by following these steps:

    - Right-click on the XFCE panel and select **Panel > Add New Items...**.

    - In the **Add New Items** window, search for **Clipman** and click **Add**.

    - The clipboard manager icon should now appear in the panel.

3. In the Clipman Settings:

    - Under **Behavior**:

        - Uncheck **Sync mouse selections**.

        - Set **Paste instantly** option to <kbd>Shift</kbd> +
        <kbd>Insert</kbd> .

        - Check **Position menu at mouse pointer**.

        - Set **Maximum items** option to **15**.

4. Enable `xfce4-popup-clipman` by setting a keyboard shortcut:

    - Go to **Settings > Keyboard > Application Shortcuts**.

    - Click **Add** and type `xfce4-popup-clipman`.

    - Set <kbd>Super</kbd> + <kbd>V</kbd> as the shortcut.

## Vim Configuration

1. Configure Vim to use the `xfce4-clipman-plugin` clipboard manager:

    ```shell
    $ echo -e "\
    \"Configure Vim to use the \`xfce4-clipman-plugin\` clipboard manager\n\
    set clipboard=unnamed,unnamedplus\n\
    " | sudo tee -a ~/.vimrc ~root/.vimrc
    ```

2. Add custom Vim shortcuts for copy and paste operations with <kbd>Ctrl</kbd> +
<kbd>C</kbd>, <kbd>Ctrl</kbd> + <kbd>V</kbd> and <kbd>Ctrl</kbd> + <kbd>X</kbd>:

    ```shell
    $ echo -e "\
    \"Custom shortcuts for copy and paste operations\n\
    vmap <C-c> \"+yi\n\
    vmap <C-x> \"+c\n\
    vmap <C-v> c<ESC>\"+p\n\
    imap <C-v> <C-r><C-o>+\n\
    " | sudo tee -a ~/.vimrc ~root/.vimrc
    ```

3. Turn on color syntax highlighting in `vim`:

    ```shell
    $ echo -e "\
    \"Turn on color syntax highlighting\n\
    syntax on\n\
    " | sudo tee -a ~/.vimrc ~root/.vimrc
    ```

4. Check the contents of `.vimrc` files and correct any errors:

    ```shell
    $ sudo vim ~/.vimrc
    ```

    ```shell
    $ sudo vim ~root/.vimrc
    ```

## Localization

The `locale` defines which language the system uses, and other regional
considerations such as currency denomination, numerology, and character sets.
Possible values are listed in `/etc/locale.gen`.

1. Use the `sed` command to uncomment the necessary locales:

    ```shell
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

2. Check the contents of `/etc/locale.gen` file and correct any errors:

    ```shell
    $ sudo vim /etc/locale.gen
    ```

3. Generate the locales by running the following command:

    ```shell
    $ sudo locale-gen
    ```

4. Set the system-wide `LANG` variable to Swiss German with the following
command:

    ```shell
    $ echo -e "\
    # This is the fallback locale configuration provided by systemd.\n\
    LANG=\"C.UTF-8\"\n\n\
    # Set the system-wide \`LANG\` variable to Swiss German\n\
    LANG=de_CH.UTF-8\n\
    " | sudo tee /etc/locale.conf
    ```

5. Check the contents of `/etc/locale.conf` file and correct any errors:

    ```shell
    $ sudo vim /etc/locale.conf
    ```

6. Set the system-wide `KEYMAP` variable to Swiss German layout with the latin-1
character set using the following command:

    ```shell
    $ echo -e "\
    # Set system \`KEYMAP\` to Swiss German layout with the Latin-1\n\
    KEYMAP=de_CH-latin1\n\
    " | sudo tee -a /etc/vconsole.conf
    ```

7. Check the contents of `/etc/vconsole.conf` file and correct any errors:

    ```shell
    $ sudo vim /etc/vconsole.conf
    ```

    Note that the vconsole.conf file only affects the system console, and not
    the X server or any other graphical interface. If you want to change the
    keyboard layout for the X server or a graphical interface, you will need to
    configure it separately using the settings provided by the desktop
    environment or window manager.

## Network Configuration

1. Create the hostname file `/etc/hostname` and add your hostname to it. I use
Bahadir-MSI as the hostname, so I run the following command:

    ```shell
    $ echo "Bahadir-MSI" | sudo tee /etc/hostname
    ```

2. Check the contents of `/etc/hostname` file and correct any errors:

    ```shell
    $ sudo vim /etc/hostname
    ```

3. Add the following lines to `/etc/hosts`. This file maps hostnames to IP
addresses:

    ```shell
    $ echo -e "\
    # Static table lookup for hostnames.\n\
    # See hosts(5) for details.\n\n\
    # The following lines are desirable for IPv4 capable hosts\n\
    127.0.0.1       localhost\n\
    # 127.0.1.1 is often used for the FQDN of the machine\n\
    127.0.1.1       Bahadir-MSI\n\n\
    # The following lines are desirable for IPv6 capable hosts\n\
    ::1             localhost\n\
    " | sudo tee /etc/hosts
    ```

4. Check the contents of `/etc/hosts` file and correct any errors:

    ```shell
    $ sudo vim /etc/hosts
    ```

    The `127.0.1.1` entry maps the hostname to the loopback address. If you
    have a static IP address, use it instead of `127.0.1.1`.

## Change Visudo Editor Permanently

When you use the `visudo` command to edit the `/etc/sudoers` file, the default
editor is `vi`. We will change this to `vim`.

1. Establish `vim` as the `visudo` editor and call `visudo` to edit
`/etc/sudoers` file:

    ```shell
    $ sudo EDITOR=vim visudo
    ```

    This will open the `/etc/sudoers` file in the `vim` editor.

2. To change the `visudo` editor permanently system-wide add the following
to the bottom of the file:

    ```text
    # Set default EDITOR to restricted version of vim, 
    # and do not allow visudo to use EDITOR/VISUAL.
    Defaults    editor=/usr/bin/rvim, !env_editor
    ```

3. To test the changes, try to access and edit `/etc/sudoers` file with the
`vim` editor using the `visudo` command:

    ```shell
    $ sudo visudo
    ```

## Pacman configuration

Pacman is the default package manager for Arch Linux. It is used to install,
remove, and update software packages in the system. To get the most out of
pacman, it is essential to configure it properly. This section will guide you
on how to add the multilib repository, enable colors and parallel downloads,
and update repositories and packages.

### Adding Multilib Repository

1. Use the following command to uncomment `multilib` section in the
`/etc/pacman.conf` file:

    ```shell
    $ sudo sed -i -e '/^#\?\[multilib]/,/^#\?\(\[[^multilib]\|$\)/ {
    /^#\?\[[^multilib]/d
    s/^#//
    }' /etc/pacman.conf
    ```

2. Check the contents of `/etc/pacman.conf` file and correct any errors:

    ```shell
    $ sudo vim /etc/pacman.conf
    ```

    The `multilib` section should look like this:

    ```text
    [multilib]
    Include = /etc/pacman.d/mirrorlist
    ```

### Miscellaneous Options

1. Use the following command to enable colors and parallel downloads:

    ```shell
    $ sudo sed -i -e '/^# Misc options$/,/^$/ {
    /^#Color/s/^#//
    /^#ParallelDownloads = 5/s/^#//
    }' /etc/pacman.conf
    ```

2. Check the contents of `/etc/pacman.conf` file and correct any errors:

    ```shell
    $ sudo vim /etc/pacman.conf
    ```

    The `# Misc options` section should look like this:

    ```text
    # Misc options
    #UseSyslog
    Color
    #NoProgressBar
    CheckSpace
    #VerbosePkgLists
    ParallelDownloads = 5
    ```

### Update Repositories and Packages

To check if you successfully added the repositories and enabled the desired
options, run:

```shell
$ sudo pacman -Syu
```

If updating returns an error, open the `pacman.conf` again and check for errors.

### Clean Pacman Cache

Tt is necessary to deliberately clean up the cache periodically to prevent the
directory to grow indefinitely in size.

1. Install the `pacman-contrib` package:

    ```shell
    $ sudo pacman -S pacman-contrib
    ```

    The paccache(8) script, provided within the `pacman-contrib` package,
    deletes all cached versions of installed and uninstalled packages, except
    for the most recent three, by default.

2. Enable and start `paccache.timer` to discard unused packages weekly:

    ```shell
    $ systemctl enable --now paccache.timer
    ```

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
