# KDE Plasma tweaks

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
