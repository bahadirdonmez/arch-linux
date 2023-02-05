# Configure XFCE Desktop Graphical User Interface

## Qogir GTK Theme

Qogir is a flat Design theme for GTK 3, GTK 2 and Gnome-Shell which supports
GTK 3 and GTK 2 based desktop environments like Gnome, Unity, Budgie, Cinnamon
Pantheon, XFCE, Mate, etc.

### Install GTK2 Engines Requirements

Some applications require GTK2 engines to be installed. You can install them by
typing the following command in the terminal:

```shell
$ sudo pacman -S gtk-engine-murrine gtk-engines
```

### Install Theme

1. Change to the `git` directory and clone the `Qogir-theme` repository using
the `git clone` command:

    ```shell
    $ cd ~/git && git clone git@github.com:vinceliuice/Qogir-theme.git
    ```

2. Change to `Qogir-theme` directory:

    ```shell
    $ cd ~/git/Qogir-theme
    ```

3. Run the `install` script:

    ```shell
    $ sudo ./install.sh --theme default --tweaks round
    ```

## Qogir Icon and Cursors Pack

1. Change to the `git` directory and clone the `Qogir-icon-theme` repository
using the `git clone` command:

    ```shell
    $ cd ~/git && git clone git@github.com:vinceliuice/Qogir-icon-theme.git
    ```

2. Change to `Qogir-icon-theme` directory:

    ```shell
    $ cd ~/git/Qogir-icon-theme
    ```

3. Run the `install` script:

    ```shell
    $ sudo ./install.sh --theme default
    ```

## Apple Fonts

### Install p7zip Archiver

p7zip is command line port of 7-Zip for POSIX systems, including Linux.

```shell
$ sudo pacman -S p7zip
```

### Download and Extract the SF Pro Font

1. Create a directory to store new fonts:

    ```shell
    $ sudo mkdir -p /usr/local/share/fonts
    ```

2. Change the directory to `fonts` and download **SF Pro** font:

    ```shell
    $ cd /usr/local/share/fonts && sudo curl -O https://devimages-cdn.apple.com/design/resources/download/SF-Pro.dmg
    ```

3. Extract the fonts in the `.dmg` file:

    ```shell
    $ sudo sh -c '7z x SF-Pro.dmg && cd SFProFonts && 7z x "SF Pro Fonts.pkg" && 7z x "Payload~"'
    ```

4. Move the fonts to `SF Pro` folder:

    ```shell
    $ sudo sh -c 'mkdir -p /usr/local/share/fonts/"SF Pro" && mv SFProFonts/Library/Fonts/* "$_"'
    ```

5. Remove the temporary files:

    ```shell
    $ sudo rm -r *.dmg SFProFonts
    ```

### Download and Extract the SF Mono Font

1. Change the directory to `fonts` and download **SF Mono** font:

    ```shell
    $ cd /usr/local/share/fonts && sudo curl -O https://devimages-cdn.apple.com/design/resources/download/SF-Mono.dmg
    ```

2. Extract the fonts in the `.dmg` file:

    ```shell
    $ sudo sh -c '7z x SF-Mono.dmg && cd SFMonoFonts && 7z x "SF Mono Fonts.pkg" && 7z x "Payload~"'
    ```

3. Move the fonts to `SF Mono` folder:

    ```shell
    $ sudo sh -c 'mkdir -p /usr/local/share/fonts/"SF Mono" && mv SFMonoFonts/Library/Fonts/* "$_"'
    ```

4. Remove the temporary files:

    ```shell
    $ sudo rm -r *.dmg SFMonoFonts
    ```

## Desktop and Login Screen Wallpapers

You can add your own desktop wallpapers to Xfce.

1. Create a directory to store multi monitor wallpapers and copy them to this
new directory by typing the following command:

    ```shell
    $ cp -r ~/"Google Drive"/Resources/Wallpapers/32x9 ~/Downloads && sudo sh -c 'mkdir -p /usr/share/backgrounds/multihead && cp /home/Bahadir/Downloads/32x9/* $_' && rm -r ~/Downloads/32x9
    ```

2. Create a directory to store login screen wallpapers and copy them to this new
directory by typing the following command:

    ```shell
    $ cp -r ~/"Google Drive"/Resources/Wallpapers/16x9 ~/Downloads && sudo sh -c 'mkdir -p /usr/share/pixmaps/login && cp /home/Bahadir/Downloads/16x9/* $_' && rm -r ~/Downloads/16x9
    ```

3. Go to **Settings Editor** with the following command:

    ```shell
    $ xfce4-settings-editor
    ```

4. In the Settings Editor window, select `xfce4-desktop` from the left-hand side
pane and create a new property with the following settings:

    ```text
    Property: /backdrop/screen0/xinerama-stretch
    Type: Boolean
    Value: TRUE
    ```

    This will allow one desktop wallpaper across multiple monitors.

## Configure Environment Variables for Scaling

1. Scale GDK 3 (GTK 3) for HiDPI:

    ```shell
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
    " | sudo tee /etc/environment
    ```

2. Scale Qt 5 Applications for HiDPI:

    ```shell
    $ echo -e "\
    # Scale Qt 5 Applications for HiDPI\n\
    # Manually set global scaling for Qt 5 applications\n\
    QT_SCALE_FACTOR=1\n\
    # Avoid double-scaling of fonts in Qt applications\n\
    QT_FONT_DPI=96\n\
    " | sudo tee -a /etc/environment
    ```

3. Check the contents of `/etc/environment` file and correct any errors:

    ```shell
    $ sudo vim /etc/environment
    ```

    The new environment variables should look like this:

    ```text
    # Scale GDK 3 (GTK 3) for HiDPI
    # Scale UI elements by a factor of 2
    GDK_SCALE=2
    # Undo scaling of text
    GDK_DPI_SCALE=0.5

    # Scale Qt 5 Applications for HiDPI
    # Manually set global scaling for Qt 5 applications
    QT_SCALE_FACTOR=1
    # Avoid double-scaling of fonts in Qt applications
    QT_FONT_DPI=96
    ```

## Applying Theme, Icons, Cursors, Fonts and Wallpapers, Scaling

1. Go to **Settings > Window Manager**:

    - Under **Style**.

        - Choose **Qogir-Dark-xhdpi**.

        - Choose **SF Pro Bold, 9** as **Title font**.

        - Drag the **Menu** and **Shade** controls and drop them to **Hidden**.

2. Go to **Settings > Mouse and Touchpad**:

    - Under **Thema**.

        - Choose **Qogir-dark**.

        - Set **Cursor size** to **48**.

3. Go to **Settings > Desktop > Background**:

    - Set folder to `/usr/share/background/multihead/`

    - Click on one of the background images.

    - Check **Change the background**, set it to **60** minutes and allow
    **Random Order**.

    - Set **Style** to **Span monitors**.

4. Go to **Settings > Apperance**:

    - Under **Style**.

        - Choose **Qogir-Dark**.

    - Under **Icons**.

        - Choose **Qogir-dark**.

    - Under **Fonts**.

        - Choose **SF Pro Regular, 10** as **Default Font**.

        - Choose **SF Mono Regular, 10** as **Default Monospace Font**.

        - Enable **Custom DPI** setting and change from `96` to `192`.

5. Restart the computer to apply the changes.

## Ulauncher Application Launcher for Linux

Ulauncher is a fast application launcher for Linux. It's written in Python using
GTK+, and features: App Search (fuzzy matching), Calculator, Extensions,
Shortcuts, File browser mode and Custom Color Themes.

### Installing Ulauncher

You can install Ulauncher from the [Arch User Repository (AUR)](
https://aur.archlinux.org/packages/ulauncher).

1. Change to the `aur` directory and clone the `ulauncher` package from the
AUR using the `git clone` command:

    ```shell
    $ cd ~/aur && git clone https://aur.archlinux.org/ulauncher.git
    ```

2. Change to the `ulauncher` directory, build and install the package using
the `makepkg` command:

    ```shell
    $ cd ~/aur/ulauncher && makepkg -sirc
    ```

3. Clean up the build directory to free up space:

    ```shell
    $ git clean -dfX
    ```

### Configuring Ulauncher

1. Download Qogir theme for Ulauncher:

    - Change to the `git` directory and clone the `Qogir-ulauncher-theme`
    repository using the `git clone` command:

        ```shell
        $ cd ~/git && git clone git@github.com:bahadirdonmez/Qogir-ulauncher-theme.git
        ```

    - Create folder for Ulauncher user themes and copy the Qogir theme files to
    this directory:

        ```shell
        $ mkdir -p ~/.config/ulauncher/user-themes && cp -r ~/git/Qogir-ulauncher-theme/ $_
        ```

2. Start Ulauncher:

    ```shell
    $ ulauncher
    ```

3. Click on the cog wheel symbol to open preferences:

    - Select **Qogir Ulauncher** as **Color Theme**.

    - Select **Launch at Login**.

## Configure LightDM Login Manager

### Configure the LightDM Theme, Icons, Cursor and Fonts

1. Set default greeter theme name to `Qogir-Dark`:

    ```shell
    $ sudo sed -i -e '/^$/N;/^\[greeter]/,/^#\?\(\[[^greeter]\|\s*$\)/ {
    s/^#theme-name[[:blank:]]*=.*/theme-name = Qogir-Dark/;
    /^theme-name[[:blank:]]*=/ { s/=.*/= Qogir-Dark/; h; d }
    /^\[greeter]/ { H; x; /^theme-name[[:blank:]]*=/!s/\(\[greeter\]\)/\1\ntheme-name = Qogir-Dark/; s/\n// }
    }' /etc/lightdm/lightdm-gtk-greeter.conf
    ```

2. Set default greeter icon theme name to `Qogir-dark`:

    ```shell
    $ sudo sed -i -e '/^$/N;/^\[greeter]/,/^#\?\(\[[^greeter]\|\s*$\)/ {
    s/^#icon-theme-name[[:blank:]]*=.*/icon-theme-name = Qogir-dark/;
    /^icon-theme-name[[:blank:]]*=/ { s/=.*/= Qogir-dark/; h; d }
    /^\[greeter]/ { H; x; /^icon-theme-name[[:blank:]]*=/!s/\(\[greeter\]\)/\1\nicon-theme-name = Qogir-dark/; s/\n// }
    }' /etc/lightdm/lightdm-gtk-greeter.conf
    ```

3. Set default greeter cursor theme name to `Qogir-dark`:

    ```shell
    $ sudo sed -i -e '/^$/N;/^\[greeter]/,/^#\?\(\[[^greeter]\|\s*$\)/ {
    s/^#cursor-theme-name[[:blank:]]*=.*/cursor-theme-name = Qogir-dark/;
    /^cursor-theme-name[[:blank:]]*=/ { s/=.*/= Qogir-dark/; h; d }
    /^\[greeter]/ { H; x; /^cursor-theme-name[[:blank:]]*=/!s/\(\[greeter\]\)/\1\ncursor-theme-name = Qogir-dark/; s/\n// }
    }' /etc/lightdm/lightdm-gtk-greeter.conf
    ```

4. Set default greeter cursor theme size to `48`:

    ```shell
    $ sudo sed -i -e '/^$/N;/^\[greeter]/,/^#\?\(\[[^greeter]\|\s*$\)/ {
    s/^#cursor-theme-size[[:blank:]]*=.*/cursor-theme-size = 48/;
    /^cursor-theme-size[[:blank:]]*=/ { s/=.*/= 48/; h; d }
    /^\[greeter]/ { H; x; /^cursor-theme-size[[:blank:]]*=/!s/\(\[greeter\]\)/\1\ncursor-theme-size = 48/; s/\n// }
    }' /etc/lightdm/lightdm-gtk-greeter.conf
    ```

5. Set default greeter font name to `SF Pro Regular 10`:

    ```shell
    $ sudo sed -i -e '/^$/N;/^\[greeter]/,/^#\?\(\[[^greeter]\|\s*$\)/ {
    s/^#font-name[[:blank:]]*=.*/font-name = SF Pro Regular 10/;
    /^font-name[[:blank:]]*=/ { s/=.*/= SF Pro Regular 10/; h; d }
    /^\[greeter]/ { H; x; /^font-name[[:blank:]]*=/!s/\(\[greeter\]\)/\1\nfont-name = SF Pro Regular 10/; s/\n// }
    }' /etc/lightdm/lightdm-gtk-greeter.conf
    ```

6. Set resolution for Xft in dots per inch to `192`:

    ```shell
    $ sudo sed -i -e '/^$/N;/^\[greeter]/,/^#\?\(\[[^greeter]\|\s*$\)/ {
    s/^#xft-dpi[[:blank:]]*=.*/xft-dpi = 192/;
    /^xft-dpi[[:blank:]]*=/ { s/=.*/= 192/; h; d }
    /^\[greeter]/ { H; x; /^xft-dpi[[:blank:]]*=/!s/\(\[greeter\]\)/\1\nxft-dpi = 192/; s/\n// }
    }' /etc/lightdm/lightdm-gtk-greeter.conf
    ```

7. Set degree of hinting to use to `hintfull`:

    ```shell
    $ sudo sed -i -e '/^$/N;/^\[greeter]/,/^#\?\(\[[^greeter]\|\s*$\)/ {
    s/^#xft-hintstyle[[:blank:]]*=.*/xft-hintstyle = hintfull/;
    /^xft-hintstyle[[:blank:]]*=/ { s/=.*/= hintfull/; h; d }
    /^\[greeter]/ { H; x; /^xft-hintstyle[[:blank:]]*=/!s/\(\[greeter\]\)/\1\nxft-dpi = hintfull/; s/\n// }
    }' /etc/lightdm/lightdm-gtk-greeter.conf
    ```

8. Set type of subpixel antialiasing to `none`:

    ```shell
    $ sudo sed -i -e '/^$/N;/^\[greeter]/,/^#\?\(\[[^greeter]\|\s*$\)/ {
    s/^#xft-rgba[[:blank:]]*=.*/xft-rgba = none/;
    /^xft-rgba[[:blank:]]*=/ { s/=.*/= none/; h; d }
    /^\[greeter]/ { H; x; /^xft-rgba[[:blank:]]*=/!s/\(\[greeter\]\)/\1\nxft-rgba = none/; s/\n// }
    }' /etc/lightdm/lightdm-gtk-greeter.conf
    ```

### Configure the LightDM Panel

1. Set greeter panel indicators:

    ```shell
    $ sudo sed -i -e '/^$/N;/^\[greeter]/,/^#\?\(\[[^greeter]\|\s*$\)/ {
    s/^#indicators[[:blank:]]*=.*/indicators = ~host;~spacer;~clock;~spacer;~layout;~session;~power/;
    /^indicators[[:blank:]]*=/ { s/=.*/= ~host;~spacer;~clock;~spacer;~layout;~session;~power/; h; d }
    /^\[greeter]/ { H; x; /^indicators[[:blank:]]*=/!s/\(\[greeter\]\)/\1\nindicators = ~host;~spacer;~clock;~spacer;~layout;~session;~power/; s/\n// }
    }' /etc/lightdm/lightdm-gtk-greeter.conf
    ```

2. Use `localectl` to set keyboard layout to `ch`:

    ```shell
    $ localectl --no-convert set-x11-keymap ch pc105
    ```

### Configure the LightDM Background

1. Set the default background image path:

    ```shell
    $ sudo sed -i -e '/^$/N;/^\[greeter]/,/^#\?\(\[[^greeter]\|\s*$\)/ {
    s/^#background[[:blank:]]*=.*/background = \/usr\/share\/pixmaps\/background.jpg/;
    /^background[[:blank:]]*=/ { s/=.*/= \/usr\/share\/pixmaps\/background.jpg/; h; d }
    /^\[greeter]/ { H; x; /^background[[:blank:]]*=/!s/\(\[greeter\]\)/\1\nbackground = \/usr\/share\/pixmaps\/background.jpg/; s/\n// }
    }' /etc/lightdm/lightdm-gtk-greeter.conf
    ```

2. Set user background to `false`:

    ```shell
    $ sudo sed -i -e '/^$/N;/^\[greeter]/,/^#\?\(\[[^greeter]\|\s*$\)/ {
    s/^#user-background[[:blank:]]*=.*/user-background = false/;
    /^user-background[[:blank:]]*=/ { s/=.*/= false/; h; d }
    /^\[greeter]/ { H; x; /^user-background[[:blank:]]*=/!s/\(\[greeter\]\)/\1\nuser-background = false/; s/\n// }
    }' /etc/lightdm/lightdm-gtk-greeter.conf
    ```

3. Add a script to replace `background.jpg` in `/usr/share/pixmaps/` with a
random background:

    - Create the script:

        ```shell
        $ echo -e \
        '#!/bin/bash
        
        cd /usr/share/pixmaps
        
        background_home=/usr/share/pixmaps/login
        
        # Shuffle backgrounds and pick one
        background=$(ls $background_home | shuf -n 1)
        
        # Replace current LightDM greeter background
        cp $background_home/$background background.jpg
        ' | sudo tee /usr/share/pixmaps/login/randomize.sh > /dev/null
        ```

    - Run `chmod +x` on the script to make it executable:

        ```shell
        $ sudo chmod +x /usr/share/pixmaps/login/randomize.sh
        ```

    - Run the script:

        ```shell
        $ sudo /usr/share/pixmaps/login/randomize.sh
        ```

4. Configure LightDM to run the script at start:

    ```shell
    $ sudo sed -i -e '/^$/N;/^\[Seat:\*]/,/^#\?\(\[[^Seat:\*]\|\s*$\)/ {
    s/^#greeter-setup-script[[:blank:]]*=.*/greeter-setup-script = \/usr\/share\/pixmaps\/lightdm\/randomize.sh/;
    /^greeter-setup-script[[:blank:]]*=/ { s/=.*/= \/usr\/share\/pixmaps\/lightdm\/randomize.sh/; h; d }
    /^\[Seat:\*]/ { H; x; /^greeter-setup-script[[:blank:]]*=/!s/\(\[Seat:\*\]\)/\1\ngreeter-setup-script = \/usr\/share\/pixmaps\/login\/randomize.sh/; s/\n// }
    }' /etc/lightdm/lightdm.conf
    ```

### Configure the LightDM Active Monitor

```shell
$ sudo sed -i -e '/^$/N;/^\[greeter]/,/^#\?\(\[[^greeter]\|\s*$\)/ {
s/^#active-monitor[[:blank:]]*=.*/active-monitor = 0/;
/^active-monitor[[:blank:]]*=/ { s/=.*/= 0/; h; d }
/^\[greeter]/ { H; x; /^active-monitor[[:blank:]]*=/!s/\(\[greeter\]\)/\1\nactive-monitor = 0/; s/\n// }
}' /etc/lightdm/lightdm-gtk-greeter.conf
```

### Configure the LightDM Time and Date Format

```shell
$ sudo sed -i -e '/^$/N;/^\[greeter]/,/^#\?\(\[[^greeter]\|\s*$\)/ {
s/^#clock-format[[:blank:]]*=.*/clock-format = %a %b %d   %H:%M/;
/^clock-format[[:blank:]]*=/ { s/=.*/= %a %b %d   %H:%M/; h; d }
/^\[greeter]/ { H; x; /^clock-format[[:blank:]]*=/!s/\(\[greeter\]\)/\1\nclock-format = %a %b %d   %H:%M/; s/\n// }
}' /etc/lightdm/lightdm-gtk-greeter.conf
```

### Check Configuration Files for Errros

1. Check the contents of `/etc/lightdm/lightdm.conf` file and correct any
errors:

    ```shell
    $ sudo vim /etc/lightdm/lightdm.conf
    ```

2. Check the contents of `/etc/lightdm/lightdm-gtk-greeter.conf` file and
correct any errors:

    ```shell
    $ sudo vim /etc/lightdm/lightdm-gtk-greeter.conf
    ```

## Change User Avatar

1. Install `accountsservice` package:

    ```shell
    $ sudo pacman -S accountsservice
    ```

2. Move the user avatar to `/var/lib/AccountsService/icons` by typing the
following command:

    ```shell
    $ cp -r ~/"Google Drive"/Personal/"Profile Pictures"/bahadir_profile_1_1_192px.png ~/Downloads && sudo mv /home/Bahadir/Downloads/bahadir_profile_1_1_192px.png /var/lib/AccountsService/icons/Bahadir.png
    ```

3. Edit the account settings file by typing the following command:

    ```shell
    $ echo -e "\
    [User]\n\
    Icon=/var/lib/AccountsService/icons/Bahadir.png\n\
    " | sudo tee /var/lib/AccountsService/users/Bahadir
    ```

4. Make sure that both created files have 644 permissions:

    ```shell
    $ sudo sh -c 'chmod 664 /var/lib/AccountsService/icons/Bahadir.png && chmod 664 /var/lib/AccountsService/users/Bahadir'
    ```

5. Check the contents of `/var/lib/AccountsService/users/Bahadir` file and
correct any errors:

    ```shell
    $ sudo vim /var/lib/AccountsService/users/Bahadir
    ```

    The file should look like this:

    ```text
    [User]
    Icon=/var/lib/AccountsService/icons/Bahadir.jpg
    ```

OLDOLDOLDOLDOLDOLDOLDOLDOLDOLDOLDOLDOLDOLDOLDOLDOLDOLDOLDOLDOLDOLDOLDOLDOLDOLD

## Configure Xfce Desktop

Xfce is a lightweight desktop environment for Linux that is known for its
simplicity and customization options. Follow the steps below to configure the
Xfce desktop on your Arch Linux system.

### Initial Configuration

1. Disable desktop icons:

    - Go to **Settings > Desktop > Icons**.
    - Set **Icon type** to **None**.

3. Configure **Window Manager Tweaks**:

    - Go to **Settings > Window Manager Tweaks**.
    - Under **Cycling**:
        - Uncheck **Draw frame around selected window while cycling**.
    - Under **Placement**:
        - Select **By default, place windows: At the center of the screen**.
    - Under **Compositor**:
        - Uncheck **Show shadows under dock windows**.

### Configure Side Panel

You can add frequently accessed folders to the side panel in your home folder
by following these steps:

1. Open your home folder by clicking on the home icon in the file manager.

2. Right-click on the folder you want to add and select **Add to Side Pane**.

### Install **Whisker Menu**

Whisker menu is an alternative application launcher. It shows a list of
favorites, browses through all installed applications through category
buttons, and supports fuzzy searching.

1. Install the `xfce4-whiskermenu-plugin` package with the following command:

    ```shell
    $ sudo pacman -S xfce4-whiskermenu-plugin
    ```

2. Enable `xfce4-popup-whiskermenu` by setting a keyboard shortcut:

    - Go to **Settings > Keyboard > Application Shortcuts**.
    - Click **Add** and type `xfce4-popup-whiskermenu`.
    - Set <kbd>Super</kbd> + <kbd>Space</kbd> or <kbd>Super</kbd> +
        <kbd>A</kbd>  as the shortcut.

### Configure **Xfce Panel**

1. Go to **Settings > Panel > Panel 1**:

    - Under **Display**:
        - Check **Span monitors** box.
        - Set **Row size (pixels)** to **40**.

    - Under **Appearance**:
        - Disable **Dark mode** box.
        - Set **Background: Style** to **None (use system style)**.
        - Set **Fixed icon size (pixels)** to **24**.
        - Set **Opacity: Enter** to **80**.
        - Set **Opacity: Leave** to **80**.

    - Under **Items**:
        - Click **Add** and select **Whisker Menu**.
        - Move **Whisker Menu** to the top of the list.
        - Remove **Applications Menu**, **Window Buttons**,
        **Seperator** and **Workspace Switcher** from the top three.
        - Enable **Expand** option for the other **Seperator**  at the top.
        - Click on **Whisker Menu** options:
            - Under **Appearance**:
                - Choose **Show as list**.
                - Uncheck **Show category names**.
                - Uncheck **Show application tooltips**.
                - Uncheck **Show application descriptions**.
                - Set **Background opacity** to **80**.
            - Under **Panel Button**:
                - Set `preferences-system-search` as icon.
                - Check **Use single panel row**.
            - Under **Behavior**:
                - Set **Default Category** as **All Applications**.
                - Check **Switch categories by hovering**.
            - Under **Commands**:
                - Uncheck everything.
            - Open the **Whisker Menu** and resize it from corner until pull
                until the bottom of the screen.
        - Click on **Clock** options:
            - Set **Appearance: Layout** to **Digital**.
            - Set **Tooltip format** to **Custom Format**: `%A, %d %B %Y`.
            - Set **Clock Options: Layout** to **Only Time**.
            - Set **Font** to **SF Pro Regular, 10**.
            - Choose **Format**: `11:27`.
        - Click on **Action Buttons** options:
            - Set **Appearance** to **Action Buttons**.
            - Uncheck all actions except **Logout...**.
        - Click on **Power Manager Plugin** options:
            - Set **Show label** to **None**.
        - Click on **Stray Items** options:
            - Set **Fixed icon size (pixels)** to **18**.

2. Go to **Settings > Panel > Panel 2**:
    - Remove the panel.

### Install and Configure **Plank Dock**

Plank is a lightweight and minimal dock that allows you to launch your favorite
applications with ease.

1. Install the `plank` package with the following command:

    ```shell
    $ sudo pacman -S plank
    ```

2. Once you have installed Plank, you can configure it for your needs.

    - First, run Plank with the following command in the terminal:

        ```shell
        $ XDG_SESSION_TYPE=x11 plank
        ```

    - Right-click on any unnecessary symbols in the dock and uncheck the box
    labeled **Keep in Dock** to remove them.

    - Open **File Manager** and **Terminal** applications and right-click on the
    symbols in the dock and check the box labeled **Keep in Dock** to pin them
    to dock.

3. Download Azeny theme for Plank:

    - Change to the `git` directory and clone the `plank-themes` repository
        using the `git clone` command:

        ```shell
        $ cd ~/git && git clone git@github.com:Monsene/plank-themes.git
        ```

    - Copy the Azeny theme files to the Plank themes folder using the following
    command:

        ```shell
        $ cp -r ~/git/plank-themes/Azeny/Azeny/ ~/.local/share/plank/themes/
        ```

4. Configure Plank:

    - Open the Plank preferences with the following command:

        ```shell
        $ plank --preferences
        ```

    - Under **Appearance**:
        - Choose **Azeny** as **Thema**.
        - Set **Icon Size** to **40**.
        - Enable **Icon zoom** and set it to **140**.

    - Under **Docklets**:
        - Double click on **Desktop**.

5. Enable autostart of Plank in XFCE4 settings by doing the following:

    - Go to **Settings > Session and Startup** and switch to the
        **Application Autostart** tab.
    - Click on the **Add** button.
    - In the **Name** field, enter **"Plank Dock"**.
    - In the **Command** field, enter `XDG_SESSION_TYPE=x11 plank`.
    - Click on **OK** to save the autostart entry.

## Install Printer

```shell
$ sudo pacman -S cups cups-pdf
```

1. Enable the CUPS service using the following command:

    ```shell
    $ systemctl enable cups.service
    ```

2. Start the CUPS service using the following command:

    ```shell
    $ systemctl start cups.service
    ```

3. Install the `avahi` package.

3. Install Brother MFC-J6710DW drivers:

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
5. Select **Key Management** and Enter. In the page that open, set **FActory Default Key Provisioning** to **\[Disabled]**
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
