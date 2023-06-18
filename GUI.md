# Configuring the Graphical User Interface

## Qogir GTK Theme

Qogir is a flat design theme for GTK 3, GTK 2, and GNOME-Shell. It supports
various desktop environments that use GTK 3 and GTK 2, such as GNOME, Unity,
Budgie, Cinnamon, Pantheon, Xfce, Mate, and more.

### Install GTK2 Engines Requirements

Some applications require the installation of GTK2 engines. You can install
them by typing the following command in the terminal:

```bash
$ sudo pacman -S gtk-engine-murrine gtk-engines
```

### Installing the Qogir Theme

1. Change to the `git` directory and clone the `Qogir-theme` repository by
running the following command:

    ```bash
    $ cd ~/git && git clone git@github.com:vinceliuice/Qogir-theme.git
    ```

2. Change to the `Qogir-theme` directory:

    ```bash
    $ cd ~/git/Qogir-theme
    ```

3. Run the installation script by executing the following command:

    ```bash
    $ sudo ./install.sh --dest "/usr/share/themes" --theme default --color dark --libadwaita --tweaks round && git clean -dfX
    ```

## Qogir Icon and Cursors Pack

1. Change to the `git` directory and clone the `Qogir-icon-theme` repository by
running the following command:

    ```bash
    $ cd ~/git && git clone git@github.com:vinceliuice/Qogir-icon-theme.git
    ```

2. Change to the `Qogir-icon-theme` directory:

    ```bash
    $ cd ~/git/Qogir-icon-theme
    ```

3. Execute the installation script by running the following command:

    ```bash
    $ sudo ./install.sh --dest "/usr/share/icons" --theme default --color all && git clean -dfX
    ```

## Fonts and Emojis

### Install p7zip Archiver

p7zip is a command-line port of 7-Zip for POSIX systems, including Linux.  

```bash
$ sudo pacman -S p7zip
```

### Set Up Fonts Directory

Create a directory to store new fonts and change to the `fonts` directory:

```bash
$ sudo mkdir -p /usr/local/share/fonts/otf /usr/local/share/fonts/ttf && cd /usr/local/share/fonts
```

### Download and Extract the SF Pro Font

1. Download the **SF Pro** font:

    ```bash
    $ sudo curl -O https://devimages-cdn.apple.com/design/resources/download/SF-Pro.dmg
    ```

2. Extract the fonts from the `.dmg` file:

    ```bash
    $ sudo sh -c '7z x SF-Pro.dmg && cd SFProFonts && 7z x "SF Pro Fonts.pkg" && 7z x "Payload~"'
    ```

3. Move the fonts to the `SF Pro` folders inside the `otf` and `ttf`
directories:

    ```bash
    $ sudo sh -c 'mkdir -p /usr/local/share/fonts/otf/"SF Pro" && mv SFProFonts/Library/Fonts/*.otf "$_" ; mkdir -p /usr/local/share/fonts/ttf/"SF Pro" && mv SFProFonts/Library/Fonts/*.ttf "$_"'
    ```

### Download and Extract the SF Mono Font

1. Download the **SF Mono** font:

    ```bash
    $ sudo curl -O https://devimages-cdn.apple.com/design/resources/download/SF-Mono.dmg
    ```

2. Extract the fonts from the `.dmg` file:

    ```bash
    $ sudo sh -c '7z x SF-Mono.dmg && cd SFMonoFonts && 7z x "SF Mono Fonts.pkg" && 7z x "Payload~"'
    ```

3. Move the fonts to the `SF Mono` folders inside the `otf` and `ttf`
directories:

    ```bash
    $ sudo sh -c 'mkdir -p /usr/local/share/fonts/otf/"SF Mono" && mv SFMonoFonts/Library/Fonts/*.otf "$_" ; mkdir -p /usr/local/share/fonts/ttf/"SF Mono" && mv SFMonoFonts/Library/Fonts/*.ttf "$_"'
    ```

### Download and Extract the New York Font

1. Download the **New York** font:

    ```bash
    $ sudo curl -O https://devimages-cdn.apple.com/design/resources/download/NY.dmg
    ```

2. Extract the fonts from the `.dmg` file:

    ```bash
    $ sudo sh -c '7z x NY.dmg && cd NYFonts && 7z x "NY Fonts.pkg" && 7z x "Payload~"'
    ```

3. Move the fonts to the `New York` folders inside the `otf` and `ttf`
directories:

    ```bash
    $ sudo sh -c 'mkdir -p /usr/local/share/fonts/otf/"New York" && mv NYFonts/Library/Fonts/*.otf "$_" ; mkdir -p /usr/local/share/fonts/ttf/"New York" && mv NYFonts/Library/Fonts/*.ttf "$_"'
    ```

### Install Noto Fonts

Install Google's Noto font family with full Unicode coverage, including its
emoji and CJK optional dependencies:

```bash
$ sudo pacman -S noto-fonts-emoji noto-fonts-cjk noto-fonts-extra noto-fonts
```

### Install DejaVu Fonts

Install DejaVu Font family based on the Bitstream Vera Fonts with a wider range
of characters:

```bash
$ sudo pacman -S ttf-dejavu
```

### Complete Font Installation

1. Update the fontconfig cache:

    ```bash
    $ sudo fc-cache
    ```

2. Remove the temporary files:

    ```bash
    $ sudo sh -c 'rm -r *.dmg SFProFonts SFMonoFonts NYFonts ; find . -type d -empty -delete'
    ```

## Single and Dual Monitor Wallpapers

You can customize your desktop wallpapers in Xfce.

1. Create a directory to store single-monitor wallpapers and copy them to this
new directory by running the following command:

    ```bash
    $ cp -r ~/"Google Drive"/Resources/Wallpapers/16x9 ~/Downloads && sudo sh -c 'mkdir -p /usr/share/backgrounds/single-monitor && cp /home/Bahadir/Downloads/16x9/* $_' && rm -r ~/Downloads/16x9
    ```

2. Create a directory to store dual-monitor wallpapers and copy them to this
new directory by running the following command:

    ```bash
    $ cp -r ~/"Google Drive"/Resources/Wallpapers/32x9 ~/Downloads && sudo sh -c 'mkdir -p /usr/share/backgrounds/dual-monitor && cp /home/Bahadir/Downloads/32x9/* $_' && rm -r ~/Downloads/32x9
    ```

## HiDPI Scaling

1. To scale GDK 3 (GTK 3) for HiDPI, you can add environment variables to the
`/etc/environment` file.

    ```bash
    $ echo -e "\
    #\n\
    # This file is parsed by pam_env module\n\
    #\n\
    # Syntax: simple "KEY=VAL" pairs on separate lines\n\
    #\n\
    \n\
    # Scale GDK 3 (GTK 3) for HiDPI\n\
    # Scale UI elements by a factor of 2\n\
    GDK_SCALE=2\n\
    # Undo scaling of text\n\
    GDK_DPI_SCALE=0.5\n\
    " | sudo tee /etc/environment > /dev/null
    ```

    The `GDK_SCALE` variable scales the UI elements by a factor of `2`, while
    the `GDK_DPI_SCALE` variable undoes the scaling of text.

2. Confirm the changes to the `/etc/environment` file:

    ```bash
    $ sudo vim /etc/environment
    ```

    The file should contain the following entries:

    ```properties
    # Scale GDK 3 (GTK 3) for HiDPI
    # Scale UI elements by a factor of 2
    GDK_SCALE=2
    # Undo scaling of text
    GDK_DPI_SCALE=0.5
    ```

3. To adjust the GRUB font size, convert the `DejaVuSansMono.ttf` font to a
format that GRUB can utilize:

    ```bash
    $ sudo grub-mkfont -s 40 -o /boot/grub/fonts/grubfont.pf2 /usr/share/fonts/TTF/DejaVuSansMono.ttf
    ```

4. Modify the `/etc/default/grub` file to set the new font:

    ```bash
    $ sudo sed -i -e '/^GRUB_FONT=/d' \
    -e '/^#\?GRUB_THEME=/a\GRUB_FONT="/boot/grub/fonts/grubfont.pf2"' \
    /etc/default/grub
    ```

5. Check the contents of the `/etc/default/grub` file and correct any errors:

    ```bash
    $ sudo vim /etc/default/grub
    ```

    The `GRUB_FONT` line in the file should look like this:

    ```properties
    # Uncomment one of them for the gfx desired, a image background or a gfxtheme
    #GRUB_BACKGROUND="/path/to/wallpaper"
    #GRUB_THEME="/path/to/gfxtheme"
    GRUB_FONT="/boot/grub/fonts/grubfont.pf2"
    ```

6. Regenerate the GRUB configuration file:

    ```bash
    $ sudo grub-mkconfig -o /boot/grub/grub.cfg
    ```

7. Configure HiDPI settings for your desktop environment:

    - Open **Settings > Appearance**.

    - Under the **Fonts** section:

        - Enable the **Custom DPI** setting and set the value to `192`.

    - Under the **Settings** section:

        - Reset **Window Scaling** to `1` (No scaling).

8. Restart the computer to apply the changes.

## Configure LightDM Login Manager

### Configure the LightDM Background

1. Set the default background image path:

    ```bash
    $ sudo sed -i -e '/^$/N;/^\[greeter]/,/^#\?\(\[[^greeter]\|\s*$\)/ {
    s/^#background[[:blank:]]*=.*/background = \/usr\/share\/pixmaps\/background.jpg/;
    /^background[[:blank:]]*=/ { s/=.*/= \/usr\/share\/pixmaps\/background.jpg/; h; d }
    /^\[greeter]/ { H; x; /^background[[:blank:]]*=/!s/\(\[greeter\]\)/\1\nbackground = \/usr\/share\/pixmaps\/background.jpg/; s/\n// }
    }' /etc/lightdm/lightdm-gtk-greeter.conf
    ```

2. Set user background to `false`:

    ```bash
    $ sudo sed -i -e '/^$/N;/^\[greeter]/,/^#\?\(\[[^greeter]\|\s*$\)/ {
    s/^#user-background[[:blank:]]*=.*/user-background = false/;
    /^user-background[[:blank:]]*=/ { s/=.*/= false/; h; d }
    /^\[greeter]/ { H; x; /^user-background[[:blank:]]*=/!s/\(\[greeter\]\)/\1\nuser-background = false/; s/\n// }
    }' /etc/lightdm/lightdm-gtk-greeter.conf
    ```

3. Add a script to replace `background.jpg` in `/usr/share/pixmaps/` with a
random background:

    - Create the script:

        ```bash
        $ echo -e \
        '#!/bin/bash

        # Define the directory where the backgrounds are stored
        backgrounds_dir="/usr/share/backgrounds/single-monitor"

        # Check if the backgrounds directory exists
        if [[ -d "$backgrounds_dir" ]]; then
            # If the directory exists, randomly select one of the background files
            background=$(find "$backgrounds_dir" -type f | shuf -n 1)

            # Check if a file was selected and if it exists
            if [[ -n "$background" && -f "$background" ]]; then
                # If a valid file was selected, copy it to replace the current LightDM greeter background
                sudo cp "$background" /usr/share/pixmaps/background.jpg
            else
                # If no valid file was found, output an error message
                echo "No suitable background file found in $backgrounds_dir."
            fi
        else
            # If the backgrounds directory does not exist, output an error message
            echo "Directory $backgrounds_dir does not exist."
        fi
        ' | sudo tee /usr/local/bin/random_lightdm_background.sh > /dev/null
        ```

    - Run `chmod +x` on the script to make it executable:

        ```bash
        $ sudo chmod +x /usr/local/bin/random_lightdm_background.sh
        ```

    - Review the contents of the `/usr/local/bin/random_lightdm_background.sh`
    file and correct any errors:

        ```bash
        $ sudo vim /usr/local/bin/random_lightdm_background.sh
        ```

4. To configure the script to launch at startup using systemd, enter the
following command:

    ```bash
    $ echo -e "\
    [Unit]\n\
    Description=Service to Randomize LightDM Background at Startup\n\
    \n\
    [Service]\n\
    Type=oneshot\n\
    ExecStart=/usr/local/bin/random_lightdm_background.sh \n\
    Restart=on-failure\n\
    RestartSec=5\n\
    \n\
    [Install]\n\
    WantedBy=default.target\n\
    " | sudo tee /etc/systemd/system/random-lightdm-background.service > /dev/null
    ```

5. Review the contents of the
`/etc/systemd/system/random-lightdm-background.service` file and correct any
errors:

    ```bash
    $ sudo vim /etc/systemd/system/random-lightdm-background.service 
    ```

6. Enable the newly created service to initiate at startup:

    ```bash
    $ systemctl enable --now random-lightdm-background.service 
    ```

### Configure the LightDM Theme, Icons, Cursor and Fonts

1. Set default greeter theme name to `Qogir-Dark`:

    ```bash
    $ sudo sed -i -e '/^$/N;/^\[greeter]/,/^#\?\(\[[^greeter]\|\s*$\)/ {
    s/^#theme-name[[:blank:]]*=.*/theme-name = Qogir-Dark/;
    /^theme-name[[:blank:]]*=/ { s/=.*/= Qogir-Dark/; h; d }
    /^\[greeter]/ { H; x; /^theme-name[[:blank:]]*=/!s/\(\[greeter\]\)/\1\ntheme-name = Qogir-Dark/; s/\n// }
    }' /etc/lightdm/lightdm-gtk-greeter.conf
    ```

2. Set default greeter icon theme name to `Qogir-dark`:

    ```bash
    $ sudo sed -i -e '/^$/N;/^\[greeter]/,/^#\?\(\[[^greeter]\|\s*$\)/ {
    s/^#icon-theme-name[[:blank:]]*=.*/icon-theme-name = Qogir-dark/;
    /^icon-theme-name[[:blank:]]*=/ { s/=.*/= Qogir-dark/; h; d }
    /^\[greeter]/ { H; x; /^icon-theme-name[[:blank:]]*=/!s/\(\[greeter\]\)/\1\nicon-theme-name = Qogir-dark/; s/\n// }
    }' /etc/lightdm/lightdm-gtk-greeter.conf
    ```

3. Set default greeter cursor theme name to `Qogir-dark`:

    ```bash
    $ sudo sed -i -e '/^$/N;/^\[greeter]/,/^#\?\(\[[^greeter]\|\s*$\)/ {
    s/^#cursor-theme-name[[:blank:]]*=.*/cursor-theme-name = Qogir-dark/;
    /^cursor-theme-name[[:blank:]]*=/ { s/=.*/= Qogir-dark/; h; d }
    /^\[greeter]/ { H; x; /^cursor-theme-name[[:blank:]]*=/!s/\(\[greeter\]\)/\1\ncursor-theme-name = Qogir-dark/; s/\n// }
    }' /etc/lightdm/lightdm-gtk-greeter.conf
    ```

4. Set default greeter cursor theme size to `48`:

    ```bash
    $ sudo sed -i -e '/^$/N;/^\[greeter]/,/^#\?\(\[[^greeter]\|\s*$\)/ {
    s/^#cursor-theme-size[[:blank:]]*=.*/cursor-theme-size = 48/;
    /^cursor-theme-size[[:blank:]]*=/ { s/=.*/= 48/; h; d }
    /^\[greeter]/ { H; x; /^cursor-theme-size[[:blank:]]*=/!s/\(\[greeter\]\)/\1\ncursor-theme-size = 48/; s/\n// }
    }' /etc/lightdm/lightdm-gtk-greeter.conf
    ```

5. Set default greeter font name to `SF Pro Regular 10`:

    ```bash
    $ sudo sed -i -e '/^$/N;/^\[greeter]/,/^#\?\(\[[^greeter]\|\s*$\)/ {
    s/^#font-name[[:blank:]]*=.*/font-name = SF Pro Regular 10/;
    /^font-name[[:blank:]]*=/ { s/=.*/= SF Pro Regular 10/; h; d }
    /^\[greeter]/ { H; x; /^font-name[[:blank:]]*=/!s/\(\[greeter\]\)/\1\nfont-name = SF Pro Regular 10/; s/\n// }
    }' /etc/lightdm/lightdm-gtk-greeter.conf
    ```

6. Set Xft fonts antialiasing to `true`:

    ```bash
    $ sudo sed -i -e '/^$/N;/^\[greeter]/,/^#\?\(\[[^greeter]\|\s*$\)/ {
    s/^#xft-antialias[[:blank:]]*=.*/xft-antialias = true/;
    /^xft-antialias[[:blank:]]*=/ { s/=.*/= true/; h; d }
    /^\[greeter]/ { H; x; /^xft-antialias[[:blank:]]*=/!s/\(\[greeter\]\)/\1\nxft-antialias = true/; s/\n// }
    }' /etc/lightdm/lightdm-gtk-greeter.conf
    ```

7. Set resolution for Xft in dots per inch to `192`:

    ```bash
    $ sudo sed -i -e '/^$/N;/^\[greeter]/,/^#\?\(\[[^greeter]\|\s*$\)/ {
    s/^#xft-dpi[[:blank:]]*=.*/xft-dpi = 192/;
    /^xft-dpi[[:blank:]]*=/ { s/=.*/= 192/; h; d }
    /^\[greeter]/ { H; x; /^xft-dpi[[:blank:]]*=/!s/\(\[greeter\]\)/\1\nxft-dpi = 192/; s/\n// }
    }' /etc/lightdm/lightdm-gtk-greeter.conf
    ```

8. Set degree of hinting to use to `hintfull`:

    ```bash
    $ sudo sed -i -e '/^$/N;/^\[greeter]/,/^#\?\(\[[^greeter]\|\s*$\)/ {
    s/^#xft-hintstyle[[:blank:]]*=.*/xft-hintstyle = hintfull/;
    /^xft-hintstyle[[:blank:]]*=/ { s/=.*/= hintfull/; h; d }
    /^\[greeter]/ { H; x; /^xft-hintstyle[[:blank:]]*=/!s/\(\[greeter\]\)/\1\nxft-hintstyle = hintfull/; s/\n// }
    }' /etc/lightdm/lightdm-gtk-greeter.conf
    ```

9. Set type of subpixel antialiasing to `none`:

    ```bash
    $ sudo sed -i -e '/^$/N;/^\[greeter]/,/^#\?\(\[[^greeter]\|\s*$\)/ {
    s/^#xft-rgba[[:blank:]]*=.*/xft-rgba = none/;
    /^xft-rgba[[:blank:]]*=/ { s/=.*/= none/; h; d }
    /^\[greeter]/ { H; x; /^xft-rgba[[:blank:]]*=/!s/\(\[greeter\]\)/\1\nxft-rgba = none/; s/\n// }
    }' /etc/lightdm/lightdm-gtk-greeter.conf
    ```

### Configure the LightDM Panel

1. Set greeter panel indicators:

    ```bash
    $ sudo sed -i -e '/^$/N;/^\[greeter]/,/^#\?\(\[[^greeter]\|\s*$\)/ {
    s/^#indicators[[:blank:]]*=.*/indicators = ~host;~spacer;~clock;~spacer;~session;~layout;~a11y;~power/;
    /^indicators[[:blank:]]*=/ { s/=.*/= ~host;~spacer;~clock;~spacer;~session;~layout;~a11y;~power/; h; d }
    /^\[greeter]/ { H; x; /^indicators[[:blank:]]*=/!s/\(\[greeter\]\)/\1\nindicators = ~host;~spacer;~clock;~spacer;~session;~layout;~a11y;~power/; s/\n// }
    }' /etc/lightdm/lightdm-gtk-greeter.conf
    ```

2. Use `localectl` to set keyboard layout to `ch`:

    ```bash
    $ localectl --no-convert set-x11-keymap ch pc105
    ```

3. Set the greeter time and date format:

    ```bash
    $ sudo sed -i -e '/^$/N;/^\[greeter]/,/^#\?\(\[[^greeter]\|\s*$\)/ {
    s/^#clock-format[[:blank:]]*=.*/clock-format = %a %b %d   %H:%M/;
    /^clock-format[[:blank:]]*=/ { s/=.*/= %a %b %d   %H:%M/; h; d }
    /^\[greeter]/ { H; x; /^clock-format[[:blank:]]*=/!s/\(\[greeter\]\)/\1\nclock-format = %a %b %d   %H:%M/; s/\n// }
    }' /etc/lightdm/lightdm-gtk-greeter.conf
    ```

### Configure the LightDM Active Monitor

```bash
$ sudo sed -i -e '/^$/N;/^\[greeter]/,/^#\?\(\[[^greeter]\|\s*$\)/ {
s/^#active-monitor[[:blank:]]*=.*/active-monitor = 0/;
/^active-monitor[[:blank:]]*=/ { s/=.*/= 0/; h; d }
/^\[greeter]/ { H; x; /^active-monitor[[:blank:]]*=/!s/\(\[greeter\]\)/\1\nactive-monitor = 0/; s/\n// }
}' /etc/lightdm/lightdm-gtk-greeter.conf
```

### Check Configuration Files for Errros

Check the contents of `/etc/lightdm/lightdm-gtk-greeter.conf` file and
correct any errors:

```bash
$ sudo vim /etc/lightdm/lightdm-gtk-greeter.conf
```

The file should look like this:

```properties
[greeter]
active-monitor = 0
clock-format = %a %b %d   %H:%M
indicators = ~host;~spacer;~clock;~spacer;~session;~layout;~a11y;~power
xft-rgba = none
xft-hintstyle = hintfull
xft-dpi = 192
xft-antialias = true
font-name = SF Pro Regular 10
cursor-theme-size = 48
cursor-theme-name = Qogir-dark
icon-theme-name = Qogir-dark
theme-name = Qogir-Dark
user-background = false
background = /usr/share/pixmaps/background.jpg
```

## Plugins and Addons

### GNOME Virtual File System

1. After installing GNOME Virtual File System (GVFS), Thunar will display the
trash can, removable media, and remote file systems (such as MTP/SMB). To
install GVFS, use the following command:

    ```bash
    $ sudo pacman -S gvfs
    ```

2. To access your Android device's storage via MTP in your file manager,
install the `gvfs-mtp` package:

    ```bash
    $ sudo pacman -S gvfs-mtp
    ```

3. For Picture Transfer Protocol (PTP) support, install the `gvfs-gphoto2`
package:

    ```bash
    $ sudo pacman -S gvfs-gphoto2
    ```

### Thunar Archive Plugin

This plugin allows you to create and extract archive files using context menu
items.

```bash
$ sudo pacman -S thunar-archive-plugin
```

### Screenshots

Flameshot is a Qt5-based software for interactive screenshot taking. Select the
desired area, draw with various tools, and enjoy the customization capabilities.

1. Install Flameshot using the following command:

    ```bash
    $ sudo pacman -S flameshot
    ```

2. Create a folder to save screenshots by running the following command:

    ```bash
    $ mkdir -p ~/Pictures/Screenshots
    ```

3. Open the Flameshot configuration menu:

    ```bash
    $ flameshot config
    ```

    - Under the **General** tab:

        - Uncheck **Show help message**.

        - Uncheck **Show desktop notifications**.

        - Uncheck **Show tray icon**.

        - Set **Save Path** to `~/Pictures/Screenshots`.

        - Check **Use fixed path for screenshots to save**.

        - Set **Preferred save file extension** to `png`.

    - Change the user interface color by editing the configuration file:

        ```bash
        $ sed -i -e '/^\[General\]/,/^$/{
            /^\(uiColor\|contrastUiColor\)=/{
                s/uiColor=.*/uiColor=#3294e2/;
                s/contrastUiColor=.*/contrastUiColor=#e0e5eb/;
                h;d;
            }
            /^\[General\]/{
                /uiColor/!s/\(\[General\]\)/\1\nuiColor=#3294e2/;
                /contrastUiColor/!s/\(\[General\]\)/\1\ncontrastUiColor=#e0e5eb/;
            }
        }' ~/.config/flameshot/flameshot.ini
        ```

    - Check the contents of the `~/.config/flameshot/flameshot.ini` file and
    correct any errors:

        ```bash
        $ vim ~/.config/flameshot/flameshot.ini
        ```

        The file should look like this:

        ```properties
        [General]
        contrastUiColor=#e0e5eb
        uiColor=#3294e2
        contrastOpacity=188
        disabledTrayIcon=true
        saveAsFileExtension=png
        savePath=~/Pictures/Screenshots
        savePathFixed=true
        showDesktopNotification=false
        showHelp=false
        ```

4. Go to **Settings > Keyboard > Application Shortcuts**:

    - Click **Add** and type `flameshot gui -d 500`.

        - Set <kbd>Print</kbd> as the shortcut.

        - If you get a conflict error, choose `flameshot gui -d 500`.

    - Click **Add** and type `flameshot gui -d 3000`.

        - Set <kbd>Shift</kbd> + <kbd>Print</kbd> as the shortcut.

        - If you get a conflict error, choose `flameshot gui -d 3000`.

    - Click **Add** and type
    `flameshot full -c -p ~/Pictures/Screenshots`.

        - Set <kbd>Alt</kbd> + <kbd>Print</kbd> as the shortcut.

        - If you get a conflict error, choose
        `flameshot full -c -p ~/Pictures/Screenshots`.

5. Edit the desktop entry for Flameshot to start the GUI when it is launched:

    - Use the following command to edit the `Flameshot.desktop` file:

        ```bash
        $ sudo sed -i -e '/^\[Desktop Entry\]/,/^\[/{s|^Exec=.*|Exec=/usr/bin/flameshot gui -d 500|}' /usr/share/applications/org.flameshot.Flameshot.desktop
        ```

    - Check the contents of the
    `/usr/share/applications/org.flameshot.Flameshot.desktop` file and correct
    any errors:

        ```bash
        $ sudo vim /usr/share/applications/org.flameshot.Flameshot.desktop
        ```

### Kill Window Shortcut

Xfce does not have a built-in shortcut to kill a window, which can be useful
when a program freezes.

1. Install `xorg-xkill`:

    ```bash
    $ sudo pacman -S xorg-xkill
    ```

2. To use `xkill` to interactively kill a window, use the keyboard shortcut
<kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>ESC</kbd>.

### Task Manager

1. `xfce4-taskmanager` is an easy-to-use task manager. Install it using the
following command:

    ```bash
    $ sudo pacman -S xfce4-taskmanager
    ```

2. Go to **Settings > Keyboard > Application Shortcuts**:

    - Select `xfce4-taskmanager` and click on **Edit**.

    - Set <kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>Del</kbd> as the shortcut.

    - If you get a conflict error, choose `xfce4-taskmanager`.

### Calculator

`galculator` is a GTK-based scientific calculator.

1. Install it with the following command:

    ```bash
    $ sudo pacman -S galculator
    ```

2. Go to **Settings > Keyboard > Application Shortcuts**:

    - Click **Add** and type `galculator`.

        - Set <kbd>Calculator</kbd> as the shortcut.

### Screensaver and Lock Screen

1. `xflock4` is a reference Bash script used to lock an Xfce session. It locks
the screen with `xfce4-screensaver`. Install it using the following command:

    ```bash
    $ sudo pacman -S xfce4-screensaver
    ```

2. Go to **Settings > Keyboard > Application Shortcuts**:

    - Select `xflock4` and click on **Edit**.

    - Set <kbd>Super</kbd> + <kbd>L</kbd> as the shortcut.

3. Go to **Settings > Xfce Screensaver**:

    - Under **Screensaver**:

        - Choose **Pop art squares** as the theme.

        - Check **Inhibit screensaver for fullscreen applications**.

    - Under **Lock Screen**:

        - Set **Lock the screen after the screensaver is active for** to `1`
        minute.

## Ulauncher: Application Launcher for Linux

Ulauncher is an open-source, lightweight application launcher for Linux that
enables users to rapidly launch applications, search files, perform
calculations, and much more.

### Installing Ulauncher

You can install Ulauncher from the [Arch User Repository (AUR)](
<https://aur.archlinux.org/packages/ulauncher>).

1. Navigate to the `aur` directory and clone the `ulauncher` package from the
AUR using the `git clone` command:

    ```bash
    $ cd ~/aur && git clone https://aur.archlinux.org/ulauncher.git
    ```

2. Move to the `ulauncher` directory, then build and install the package using
the `makepkg` command:

    ```bash
    $ cd ~/aur/ulauncher && makepkg -sirc && git clean -dfX
    ```

### Configuring Ulauncher

Once Ulauncher is installed, you can configure it to suit your preferences.

1. Download the Qogir theme for Ulauncher:

    - Navigate to the `git` directory and clone the `Qogir-ulauncher-theme`
    repository using the `git clone` command:

        ```bash
        $ cd ~/git && git clone git@github.com:bahadirdonmez/Qogir-ulauncher-theme.git
        ```

    - Create a folder for Ulauncher user themes and copy the Qogir theme files
    to this directory:

        ```bash
        $ mkdir -p ~/.config/ulauncher/user-themes && cp -r ~/git/Qogir-ulauncher-theme/ $_
        ```

2. Launch ULauncher from the menu or with the following command to configure it
for your needs:

    ```bash
    $ ulauncher
    ```

3. Click on the cogwheel icon to open preferences:

    - Choose **Qogir Ulauncher** as the **Color Theme**.

    - Check **Launch at Login** to automatically launch Ulauncher when you log
    in.

    - Uncheck **Show Indicator Icon** under the **Advanced** section.

## Plank: Dock for Linux

Plank is a lightweight and minimalist dock that enables you to effortlessly
launch your favorite applications.

### Install Plank Dock

1. Install the `plank` package using the following command:

    ```bash
    $ sudo pacman -S plank
    ```

2. Go to **Settings > Panel > Panel 2**:

    - Click on the <kbd>-</kbd> button to remove the panel.

### Systemd Plank Service

1. To configure Plank to launch at startup using systemd, enter the following
command:

    ```bash
    $ echo -e "\
    [Unit]\n\
    Description=Plank Dock Service\n\
    \n\
    [Service]\n\
    Type=simple\n\
    ExecStart=/usr/bin/plank \n\
    ExecStop=/usr/bin/killall plank \n\
    Restart=on-failure\n\
    RestartSec=5\n\
    \n\
    [Install]\n\
    WantedBy=default.target\n\
    " | sudo tee /etc/systemd/user/plank.service > /dev/null
    ```

2. Review the contents of the `/etc/systemd/user/plank.service` file and correct
any errors:

    ```bash
    $ sudo vim /etc/systemd/user/plank.service
    ```

3. Enable the newly created service to initiate at startup:

    ```bash
    $ systemctl --user enable --now plank.service
    ```

### Configure Plank Dock

1. Configure Plank Dock according to your needs:

    - Right-click on any unnecessary icons in the dock and uncheck the box
    labeled **Keep in Dock** to remove them.

    - Open **Google Chrome**, **File Manager**, **Terminal**, **VS Code**, and
    **Flameshot** applications, right-click on the icons in the dock, and check
    the box labeled Keep in Dock to pin them to the dock.

2. Configure Plank preferences:

    - Open Plank preferences with the following command:

        ```bash
        $ plank --preferences
        ```

    - Under **Appearance**:

        - Choose **GTK+** as the **Theme**.

        - Set the **Icon size** to **40**.

        - Check **Icon zoom** and set it to **120**.

    - Under **Docklets**:

        - Double-click on **Desktop**.

## Mugshot User Configuration Utility

Mugshot is a lightweight user configuration utility for Linux, designed with
simplicity and ease of use in mind. It allows you to quickly update your
personal profile and synchronize the changes across applications.

### Installing Mugshot

You can install Mugshot from the [Arch User Repository (AUR)](
<https://aur.archlinux.org/packages/mugshot>).

1. Navigate to the `aur` directory and clone the mugshot package from the AUR
using the `git clone` command:

    ```bash
    $ cd ~/aur && git clone https://aur.archlinux.org/mugshot.git
    ```

2. Change to the `mugshot` directory, build, and install the package using the
`makepkg` command:

    ```bash
    $ cd ~/aur/mugshot && makepkg -sirc && git clean -dfX
    ```

### Configuring Mugshot

1. Move the user avatar to `/usr/share/pixmaps/users` by entering the following
command:

    ```bash
    $ cp -r ~/"Google Drive"/Personal/"Profile Pictures"/bahadir_profile_1_1_480px.png ~/Downloads && sudo sh -c 'mkdir -p /usr/share/pixmaps/users && mv /home/Bahadir/Downloads/bahadir_profile_1_1_480px.png /usr/share/pixmaps/users/Bahadir.png'
    ```

2. Launch Mugshot from the menu or with the following command to configure it
for your needs:

    ```bash
    $ mugshot
    ```

3. Update user details:

    - Click on the avatar and select `/usr/share/pixmaps/users/Bahadir.png`.

    - Fill in the other user details as needed.

## Whisker Menu

Whisker Menu is an alternative application launcher for the Xfce desktop
environment. It displays a list of favorite applications, allows browsing
through all installed applications by category, and supports fuzzy searching.

1. Install the `xfce4-whiskermenu-plugin` package using the following command:

    ```bash
    $ sudo pacman -S xfce4-whiskermenu-plugin
    ```

2. Go to **Settings > Panel > Panel 1**:

    - Under **Items**:

        - Click **Add** and select **Whisker Menu**.

        - Move **Whisker Menu** to the top of the list.

        - Remove **Applications Menu** from the list.

3. Go to **Settings > Keyboard > Application Shortcuts** to enable
`xfce4-popup-whiskermenu` by setting a keyboard shortcut:

    - Click **Add** and type `xfce4-popup-whiskermenu`.

    - Set <kbd>Alt</kbd> + <kbd>F1</kbd> as the shortcut.

    - If you encounter a conflict error, choose `xfce4-popup-whiskermenu`.

4. Use the following command to install `xcape`, which allows you to use a
modifier key as another key when pressed and released on its own:

    ```bash
    $ sudo pacman -S xcape
    ```

    - To configure `xcape` to execute the necessary command at startup using
    systemd, enter the following command:

        ```bash
        $ echo -e "\
        [Unit]\n\
        Description=Xcape Map Super Keys to Alt+F1\n\
        \n\
        [Service]\n\
        Type=forking\n\
        ExecStart=/usr/bin/xcape -e \"Super_L=Alt_L|F1;Super_R=Alt_L|F1\" \n\
        ExecStop=/usr/bin/killall xcape \n\
        Restart=on-failure\n\
        RestartSec=5\n\
        \n\
        [Install]\n\
        WantedBy=default.target\n\
        " | sudo tee /etc/systemd/user/xcape-super.service > /dev/null
        ```

        This will make the <kbd>Super</kbd> key generate <kbd>Alt</kbd> +
        <kbd>F1</kbd> when pressed and released on its own.

    - Review the contents of the `/etc/systemd/user/xcape-super.service` file
    and correct any errors:

        ```bash
        $ sudo vim /etc/systemd/user/xcape-super.service
        ```

    - Enable the newly created service to initiate at startup:

        ```bash
        $ systemctl --user enable --now xcape-super.service 
        ```

5. To customize the Whisker menu, right-click its icon on the panel and select
**Properties**:

    - In the **General** tab:

        - Uncheck **Show category names**.

        - Adjust the **Background opacity** slider to **95**.

    - In the **Appearance** tab:

        - Check **Position categories horizontally**.

        - Check **Position categories next to panel button**.

        - Check **Position search entry next to panel button**.

        - Set the **Profile** to **Square Picture**.

        - Check **Use single panel row**.

    - In the **Behavior** tab:

        - Select **Switch categories by hovering**.

    - In the **Commands** tab:

        - Deselect all options except for **Settings Manager** and
        **Lock Screen**.

6. To configure the **Whisker Menu**:

    - Open the menu and resize it by dragging the corner until it reaches the
    bottom of the screen.

    - In the **Favorites** tab, right-click on any unwanted icons and choose
    **Remove from favorites**.

    - Locate the **Google Chrome**, **File Manager**, **Terminal**,
    **VS Code**, and **Flameshot** applications. Right-click on each of them
    and select **Add to favorites**.

## Applying Themes, Icons, Cursors, Fonts, Wallpapers, and Scaling

### Xfce Desktop Environment Settings

1. Go to **Settings > Window Manager**:

    - Under **Style**:

        - Select **Qogir-Dark-xhdpi** as the window manager theme.

        - Choose **SF Pro Bold, 9** as the title font.

        - Drag the **Menu** and **Shade** controls and drop them to **Hidden**.

2. Go to **Settings > Mouse and Touchpad**:

    - Under **Thema**:

        - Choose **Qogir-dark** as the mouse theme.

        - Set **Cursor size** to **48**.

3. Go to **Settings > Desktop**:

    - Under **Background**:

        - Set the folder to `/usr/share/backgrounds/dual-monitor`

        - Click on one of the background images.

        - Check **Change the background**, set the interval to **60** minutes

        - Check **Random Order**.

        - Set **Style** to **Spanning screens**.

    - Under **Icons**:

        - Set **Icon type** to **None**.

4. Go to **Settings > Apperance**:

    - Under **Style**:

        - Choose **Qogir-Dark** as the widget theme.

    - Under **Icons**:

        - Choose **Qogir-dark** as the icon theme.

    - Under **Fonts**:

        - Choose **SF Pro Regular, 10** as the default font.

        - Choose **SF Mono Regular, 10** as the default monospace font.

5. Right-click Whisker menu icon on the panel and select **Properties**:

    - Under the **Appearance** tab:

        - Change the **Icon** to `hamburger-menu`.

### Google Chrome Settings

To customize the appearance of Google Chrome, follow these steps:

1. Navigate to **Settings > Appearance**:

2. Under **Themes**:

    - Click on **Use GTK**.

3. Check the **Use system title bar and borders** option.

4. Click on **Customize fonts** and make the following selections:

    - Set **SF Pro** as the standard font.

    - Set **New York** as the serif font.

    - Set **SF Pro** as the sans-serif font.

    - Set **SF Mono** as the fixed-width font.

    - Set **SF Pro** as the mathematics font.

### Terminal Settings

To modify the appearance of the Terminal, follow these steps:

1. Go to **Edit > Preferences**.

2. Under **Appearance**:

    - Set the font to **SF Mono Regular, 10**.

    - Choose the **Transparent background** option.

    - Adjust the **Opacity** to **0.95**.

    - Set **Default geometry** to `120` **columns** and `30` **rows**.

### Visual Studio Code Settings

To customize the font settings in Visual Studio Code, follow these steps:

1. Navigate to **File > Preferences > Settings**:

    - Under **Text Editor > Font**:

        - Set `'SF Mono', 'Droid Sans Mono', 'monospace', monospace` as the
        **Font Familiy**.

### Galculator Settings

To customize the appearance of Galculator, follow these steps:

1. Launch Galculator and navigate to **Edit > Preferences...**.

2. In the Preferences window, click on the **Dsiplay** tab.

3. Under **Appeearance**:

    - Set `SF Pro Bold, 26` as the **Result font**.

    - Set `SF Pro Bold, 11` as the **RPN stack font**.

    - Set `SF Pro Bold, 8` as the **Module font**.

    - Set `#32343D` as the **Background color**.

    - Set `#E6EBEF` as the **Result font color**.

    - Set `#E6EBEF` as the **RPN stack color**.

    - Set `#E6EBEF` as the **Active module color**.

    - Set `#80848B` as the **Inactive module color**.

### Desktop Icons and Side Pane Shortcuts

1. Hide unnecessary shortcuts in the Side Pane:

    To hide unwanted shortcuts in the Side Pane, right-click on an empty area
    of the pane, such as on the DEVICES section label. A pop-up menu will
    appear, allowing you to uncheck the items you wish to hide.

2. Add frequently accessed folders to the side panel in your home folder by
following these steps:

    - Places:

        - Computer

        - Bahadir

        - Trash

        - Downloads

    - Places:

        - File System

        - Google Drive

    - Network:

        - Browse Network

### Window Manager Tweaks

Navigate to **Settings > Window Manager Tweaks**:

1. Under **Cycling**:

    - Uncheck **Draw frame around selected window while cycling**.

2. Under **Accessibility**:

    - Set **Keys used to grab and move windows** to **None**.

    - Uncheck **Hide frame of windows when maximized**.

    - Uncheck **Use mouse wheel on title bar to roll up the window**.

3. Under **Workspaces**:

    - Uncheck **Use the mouse wheel on the desktop to switch workspaces**.

    - Uncheck **Wrap workspaces depending on the actual desktop layout**.

    - Uncheck
    **Wrap workspaces when the first or the last workspace is reached**.

4. Under **Compositor**:

    - Uncheck **Show shadows under dock windows**.

    - Uncheck **Zoom desktop with mouse wheel**.

    - Uncheck **Zoom pointer along with the desktop**.

### Xfce Panel Configuration

To configure the Xfce Panel, follow these steps:

1. Go to **Settings > Panel > Panel 1**:

    - Under **Display**:

        - Check the **Span monitors** box.

        - Set **Row size (pixels)** to **32**.

    - Under **Appearance**:

        - Set both **Opacity: Enter** and **Opacity: Leave** to **95**.

    - Under **Items**:

        - Remove **Window Buttons**, **Separator**, and **Workspace Switcher**
        from the list.

        - Enable the **Expand** option for the other **Separator** at the top.

        - Click on **Status Tray Plugin** options:

            - Set **Fixed icon size (pixels)** to **16**.

            - Check **Request symbolic icons**.

        - Click on **Power Manager Plugin** options:

            - Set **Show label** to **None**.

        - Left-click on the **Notifications** panel applet and click on
        **Notification options...**:

            - Under the **Appearance** tab:

                - Set **Theme** to **Qogir-Dark**.

                - Set **Opacity** to **95%**.

                - Set **Time/Date Format** to **Custom** and type
                `%a %b %d  %H:%M:%S`.

        - Click on **Clock** options:

            - Set **Appearance: Layout** to **Digital**.

            - Set **Tooltip format** to **Custom Format**:
            `%H:%M%n%A, %d %B %Y`.

            - Set **Clock Options: Layout** to **Time Only**.

            - Set **Font** to **SF Pro Bold, 9**.

            - Choose **Custom Format** as `%a %b %d  %H:%M`.

        - Click on **Action Buttons** options:

            - Set **Appearance** to **Action Buttons**.

            - Uncheck all actions except **Logout...**.

## Table of Contents

### [1. Home](./README.md)

### [2. Installation Guide](./INSTALLATION.md)

### [3. First Steps](./FIRSTSTEPS.md)

### [4. Graphical User Interface](./GUI.md)

### [5. Optional Tools and Configurations](./OPTIONAL.md)

### [6. Additional Information](./APPENDIX.md)

### [7. License](./LICENSE)
