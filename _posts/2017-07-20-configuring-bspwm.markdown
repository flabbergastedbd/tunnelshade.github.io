---
layout: post
title: "Configuring bspwm for my setup"
categories: linux
tags: tricks info
image: https://raw.githubusercontent.com/tunnelshade/awesome-dots/8af7e1fa947206aae6feb1a17d422b59c96c4bb0/screenshots/dirty1.png
---



After about few months with i3, I stumbled upon [i3gaps](https://github.com/Airblader/i3) in my quest for some space between the tiling
windows. If you haven't heard or used i3 like window manager, you might prefer [using it first]({% post_url 2014-05-12-making-i3-beautiful %}).
Soon, I started seeing awesome configurations of [herbstluftwm](https://www.herbstluftwm.org/index.html) and [bspwm](https://github.com/baskerville/bspwm).
Out of these two I ended up picking **bspwm** because of it's simplicity and configurability.

<img src="https://raw.githubusercontent.com/tunnelshade/awesome-dots/8af7e1fa947206aae6feb1a17d422b59c96c4bb0/screenshots/dirty1.png" class="image-center" alt="Dirty Screen"/>

Well, in this article, I will follow step by step procedure of setting up bspwm. My [dotfiles](http://github.com/tunnelshade/awesome-dots)

#### **Outline**

bspwm is another tiling window manager. It only responds to X events, and messages recevied on a dedicated socket. So we need another program to
listen for our keybindings and forward appropriate commands to this socket. Luckily **bspc** is a program that writes messages to bspwm's socket.
**sxhkd** (Simple X hotkey daemon) is an X daemon that can be made to execute commands as reaction to input events. Some configuration files have to be
created from scratch and that is what makes this experience rewarding. Using i3 or similar wm for sometime is recommended before trying bspwm.

**Packages to install**

+ bspwm
+ sxhkd

**Optional**

+ rofi (application launcher)
+ i3lock (screen lock)
+ gnome-keyring & libsecret (If you want avoid retyping ssh & gpg passwords in one session)
+ compton (compositor)
+ feh (set wallpaper)
+ yabar (status bar)
+ poweline fonts (fancy symbols)

#### **Steps**

+ Edit bspwm startup shell script which should be located at [**.config/bspwm/bspwmrc**](https://github.com/tunnelshade/awesome-dots/blob/50df998c78eca810916c71ea22ce5ccad7706fdb/.config/bspwm/bspwmrc)
to accomplish the following tasks.
  + Start sxhkd, compton, gnome-keyring, set wallpaper and any other startup app.
  + Run some bspc commands to define attributes and create workspaces. (`man bspc` or [bspc docs](https://github.com/baskerville/bspwm/wiki/Command-Syntax-Rewrite)).


+ Now as basic bspwm is setup, let us edit our [**.config/sxhkd/sxhkdrc**](https://github.com/tunnelshade/awesome-dots/blob/50df998c78eca810916c71ea22ce5ccad7706fdb/.config/sxhkd/sxhkdrc)
configuration file to define our keybindings for

  + Switching focus, swapping nodes, resizing etc... (Some examples here, rest in the configuration file is commented!!).

    ```bash
    # Close under focus node
    super + shift + q
        bspc node -c

    super + Return
        termite -e /usr/bin/fish

    # Resize
    ## expand the tiled space in the given direction
    super + alt + {h,j,k,l}
        bspc node {@west -r -10,@south -r +10,@north -r -10,@east -r +10}
    ```

  + Application Launcher.

    ```bash
    # I use rofi, see the screenshots for its beauty
    # Launcher
    super + d
        rofi -show run -font "Monospace Bold 16" -m -1 -fullscreen -padding 300 -hide-scrollbar
    ```

  + Multimedia keys.

    ```bash
    # Functional key bindings
    # I use mpd, so mpc can be used to switch music tracks
    XF86Audio{Stop,Prev,Next,Play}
        mpc {stop,prev,next,toggle}

    # xbacklight has to be installed
    XF86MonBrightness{Up,Down}
        xbacklight {-inc 10,-dec 10}

    # Using ALSA as it is present by default in most cases
    XF86Audio{Raise,Lower}Volume
        amixer sset Master {5%+,5%-}

    XF86AudioMute
        amixer sset Master toggle
    ```

+ Using a status bar helps, I use [yabar](https://github.com/geommer/yabar) and
[my config](https://github.com/tunnelshade/awesome-dots/blob/50df998c78eca810916c71ea22ce5ccad7706fdb/.config/yabar/yabar.conf). Helper scripts are present in `scripts`
directory.

+ Wondering when we will start ``nm-applet`` and other system tray items?? I don't! I switched to using ``nmtui``.

#### **Beautification**

+ The key to beauty is **font**, **colorscheme** and a complimenting **wallpaper**. I use ``Fira Mono for Powerline``, ``FontAwesome`` & ``gruvbox`` colorscheme. Use
[**terminal.sexy**](http://terminal.sexy/) to get some colorschemes that can be exported in suitable format for almost **all terminal emulators**.

+ Next up are out ugliest component i.e GTK Apps :P. Instead of editing configuration files directly, I highly recommend **lxappearance**, which has
absolutely no dependencies! Use it set your GTK, Icon & Cursor theme. GTK themes are picked from **~/.themes** & remaining from **~/.icons**.

+ Use some cool theme and even now, if the buttons are ugly make sure that you have the required **gtk-engine** installed. For instance, I use **Zukwito**,
so I need **gtk-engine-murrine**. Now, your gtk apps must be cool as in any other DE.

#### **Screenshots**

<img src="https://raw.githubusercontent.com/tunnelshade/awesome-dots/8af7e1fa947206aae6feb1a17d422b59c96c4bb0/screenshots/dirty2.png" class="image-center" alt="Dirty Screen"/>

[Multi Monitor Screenshot](https://raw.githubusercontent.com/tunnelshade/awesome-dots/8af7e1fa947206aae6feb1a17d422b59c96c4bb0/screenshots/multi_monitor.jpg)

#### **Other Apps**

+ A list of applications that I use

     Type                | : Application
    ---------------------|------------
     **Window Manager**  | : bspwm
     **Broswer**         | : firefox
     **Terminal**        | : termite
     **Shell**           | : fish
     **Editor**          | : vim
     **Music Player**    | : mpd/ncmpcpp
     **Video Player**    | : mpv
     **Launcher**        | : rofi
     **IRC Client**      | : weechat
     **GTK Theme**       | : zukwito
     **Icon Theme**      | : faenza

**PS**: Don't forget to push your dotfiles to github so that anyone else can use those.
