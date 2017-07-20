---
layout: post
title: "Configuring i3 for my setup"
categories: linux
tags: tricks info
image: https://raw.githubusercontent.com/tunnelshade/awesome-dots/44c51685ac446ffdd2f6ec87252ed1c4be652026/screenshots/dirty2.png
---

**I later moved to bspwm, maybe you might want to look at that as well --> [bspwm]({% post_url 2017-07-20-configuring-bspwm %})**

Well, in this article, I will follow step by step procedure of setting up i3. My [dotfiles](http://github.com/tunnelshade/awesome-dots)

+ First of all, install i3, i3lock & i3status. Generally, installing i3 will pull these as dependencies.
+ Edit your **.xinitrc** to launch i3.

{% highlight bash %}
exec i3

# If you wish to log to a file
exec i3 -V >> ~/.i3/i3log 2>&1
{% endhighlight %}

+ During the first run, i3 will ask for your default keybinding. Some help on keyboard layout is [here](http://i3wm.org/docs/userguide.html#_default_keybindings).
+ Now, lets start editing the configuration files. The main config file is present at **~/.i3/config**. First, one would prefer to have named workspaces.
So the following has to be done for number of named workspaces you want. Check [here](https://github.com/tunnelshade/awesome-dots/blob/44c51685ac446ffdd2f6ec87252ed1c4be652026/.i3/config#L90).

{% highlight bash %}
# Name the workspaces
set $tag1 "1: www"

# switch to workspace
bindsym $mod+1 workspace $tag1

# move focused container to workspace
bindsym $mod+Shift+1 move container to workspace $tag1
{% endhighlight %}

+ Wow, so now we have named workspaces (for which you have to login again) let us change some settings for i3bar in **~/.i3/config**. Colors can be set here along
with some position settings. Check for **bar** in config file as there are some defaults present. Help on these settings is available [here](http://i3wm.org/docs/userguide.html#_configuring_i3bar).

{% highlight conf %}
# Start i3bar to display a workspace bar (plus the system information i3status
# finds out, if available)
bar {
	colors {
		# Whole color settings
		background #000000
		statusline #ffffff
		separator  #666666

		# Type             border  background font
		focused_workspace  #008fff #007fff #ffffff
		active_workspace   #333333 #5f676a #ffffff
		inactive_workspace #333333 #222222 #888888
		urgent_workspace   #aa0000 #990000 #ffffff
	}
	# i3bar position
	position top
	# Using custom i3status.conf
	status_command i3status -c ~/.i3/i3status.conf
}
{% endhighlight %}

+ Edit the i3status config file if you want to tweak it. Don't forget to copy it to your home directory. Check out my
[i3status config](https://github.com/tunnelshade/awesome-dots/blob/master/.i3/i3status.conf). Official wiki is [here](http://i3wm.org/i3status/manpage.html).

{% highlight bash %}
cp /etc/i3status.conf ~/.i3/i3status.conf
{% endhighlight %}

+ Some applications have to launched at startup, like setting wallpaper, network manager applet etc.. This can be done from config file. Official wiki
[here](http://i3wm.org/docs/userguide.html#exec).

{% highlight conf %}
# Startup programs
exec --no-startup-id nm-applet
exec --no-startup-id feh --bg-fill ~/Pictures/Dark-pattern.jpg
{% endhighlight %}

+ Next, we have to set up our screenlock, for which I use **i3lock**. I have a [small shell script](https://github.com/tunnelshade/awesome-dots/blob/master/.i3/i3lock.sh) which does some magic.
After that, some keybindings have to be done. Official wiki [here](http://i3wm.org/docs/userguide.html#keybindings).

{% highlight conf %}
# Custom KeyBinds
bindsym Control+mod1+l exec sh ~/.i3/i3lock.sh
bindsym Print exec scrot '%Y-%m-%d-%T_$wx$h_scrot.png' -e 'mv $f ~/Pictures/screenshots/'
{% endhighlight %}

+ Now, we have a screen looking something like this (Don't forget to get some cool wallpaper):

<img src="https://raw.githubusercontent.com/tunnelshade/awesome-dots/44c51685ac446ffdd2f6ec87252ed1c4be652026/screenshots/clean.png" class="image-center" alt="Clean Screen"/>

#### **Beautification**

+ So, now let us transform our ugly looking setup :P, fonts first. Use a good font and change the setting in **~/.i3/config**.

{% highlight conf %}
font pango: Monospace 8
{% endhighlight %}

+ Next up are out ugliest component. GTK Apps :P. Instead of editing configuration files and saving them, I highly recommend **lxappearance**, which has
absolutely no dependencies! Use it set your GTK, Icon & Cursor theme. GTK themes are picked from **~/.themes** & remaining from **~/.icons**. My
generated **~/.config/gtk-3.0/settings.ini** looked like this

{% highlight conf %}
[Settings]
gtk-theme-name=Zukitwo
gtk-icon-theme-name=Faenza-Dark
gtk-font-name=Ubuntu 10
gtk-cursor-theme-name=Ecliz
gtk-cursor-theme-size=0
gtk-toolbar-style=GTK_TOOLBAR_ICONS
gtk-toolbar-icon-size=GTK_ICON_SIZE_LARGE_TOOLBAR
gtk-button-images=0
gtk-menu-images=1
gtk-enable-event-sounds=1
gtk-enable-input-feedback-sounds=1
gtk-xft-antialias=1
gtk-xft-hinting=1
gtk-xft-hintstyle=hintfull
gtk-xft-rgba=rgb
{% endhighlight %}

+ Use some cool theme and even now, if the buttons are ugly make sure that you have the required **gtk-engine** installed. For instance, I use **Zukwito**,
so I need **gtk-engine-murrine**. Now, your gtk apps must be cool as in any other DE.

#### **Screenshots**

<img src="https://raw.githubusercontent.com/tunnelshade/awesome-dots/44c51685ac446ffdd2f6ec87252ed1c4be652026/screenshots/dirty2.png" class="image-center" alt="Dirty Screen"/>

#### **Other Apps**

+ A list of applications that I use

	 Type                | : Application
	---------------------|------------
	 **Window Manager**  | : i3
	 **Editor**          | : GVim
	 **Terminal**        | : Terminator
	 **IRC Client**      | : Weechat
	 **GTK Theme**       | : Zukwito
	 **Icon Theme**      | : Faenza
	 **Cursor Theme**    | : Ecliz
	 **Music Player**    | : Cmus
	 **Video Player**    | : Mplayer


+ My **.vimrc** along with **molokai** colorscheme is present [here](https://github.com/tunnelshade/awesome-dots).
+ **WeeChat** configuration:
	+ Install and enable autoload of some plugins ( **beep.pl** , **buffers.pl** etc..).
	+ Make sure, your terminal emulator responds to urgent bell. In terminator, set **Terminal bell** to **Window List Flash**. To simulate bell

{% highlight bash %}
echo -ne '\e'

# Sleep & change workspace to see the effect
sleep 5; echo -ne '\e'
{% endhighlight %}

<img src="https://raw.githubusercontent.com/tunnelshade/awesome-dots/22f8edae157e4a1dac6548fb004673d90ce6bf42/screenshots/dirty1.png" class="image-center" alt="IRC Screen"/>


**PS**: Don't forget to push your dotfiles to github so that anyone else can use those.
