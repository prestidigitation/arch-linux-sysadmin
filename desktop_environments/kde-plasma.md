# KDE Plasma tweaks

## Enable Network Manager
- sudo systemctl start NetworkManager.service

## How to change specific app's display theme
- In the GUI, go to System Settings > Global Theme (in Appearance section)
    - Choose a global theme (this automatically creates .colors file in color-schemes)
- Nagivate to `~/.kde4/share/apps/color-schemes/` and copy desired color's `.colors` file contents
- Edit relevant rc file in `~/.config/`, appending .colors file contents
- If rc file doesn't already exist for the program, create one:
    - `touch ~/config/programnamerc`
    - e.g. for VLC, `touch ~/config/vlcrc`
- Can achieve all of this with a one-liner, for instance:
  - `cat ~/.kde4/share/apps/color-schemes/Breeze.colors >> ~/.config/Ankirc`

## Locale
KDE Plasma can sometimes override Arch's locale settings. If the `locale` command prints a different value for `LANG` than expected after you start KDE:
- Check if the file `~/.config/plasma-locale-settings.sh` exists. If so, delete it and restart your session.

## Avoid screen tearing in KDE (KWin) when using Nvidia GPU

* TribleBuffering solution:
Check to see if `/etc/X11/xorg.conf.d/20-nvidia.conf` exists.
If not, create an Xorg configuration file by running `nvidia-xconfig`, then move it from `/etc/X11/xorg.conf` to `/etc/X11/xorg.conf.d/20-nvidia.conf`.

* Create the `/etc/profile.d/kwin.sh` file with the following content:
  - `export KWIN_TRIPLE_BUFFER=1`
