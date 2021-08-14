# Arch General Recommendations (After Base Install)

## System administration
- Create non-root user account and add to wheel group
  - `useradd -m -G wheel username`

- Set password for newly created user account
  - `passwd username`

- Give wheel group members full root privileges with sudo
  - `EDITOR=nano visudo`
  - Uncomment line `%wheel      ALL=(ALL) ALL`

- Log out of root with `exit` and log in with non-root account.
- In general, use non-root account and `sudo` when elevated privileges are needed to run a command.

## MOST IMPORTANT SECTION
- Change pacman status bar from default to Pac-Man eating dots
  - edit `/etc/pacman.conf`
  - under "# Misc options" section, uncomment "Color" and add the line "ILoveCandy"
- Enable parallel downloads
  - Find the following line in `/etc/pacman.conf` and uncomment it:
    - `ParallelDownloads = 5`
  - Alter the number based on internet speed.

## Network configuration
- Set the hostname
  - edit `/etc/hostname` to include single line with _myhostname_
  - Add the following lines to `/etc/hosts` in order to create a minimal hosts file:
    ```
    127.0.0.1       localhost
    ::1             localhost
    ```
   - Aliases can be added to the right of the hostnames.

- Internet
  - Ethernet: configure dhcpcd
    - display all network interfaces
      - `ip link`
    - turn on ethernet interface
      - `ip link set ethernetname up`
    - enable and start dhcpcd systemd unit
      - `systemctl enable --now dhcpcd@ethernetname.service`
      - this allows dhcpcd to automatically connect to ethernet when starting computer, but it can cause timeouts when unplugged.
  - Wi-Fi: use iwd
  - Display wifi device
    - Use `iwctl` to open an interactive prompt
    - Get your wireless device's name:
      - `[iwd]# device list`
    - Scan for networks:
      - `[iwd]# station device scan`
    - List all available networks:
      - `[iwd]# station device get-networks`
    - To connect to a network:
      - `[iwd]# station device connect SSID`
    - If a passphrase is required, you will be prompted to enter it.

- Optimize pacman download speed
  - install `pacman-contrib` in order to use the rankmirrors Bash script
    - `pacman -S pacman-contrib`
  - back up existing mirror list
    - `cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.backup`
  - if the servers in the file are grouped by country, one can extract all the servers of a specific country by using:
    - `awk '/^## Country Name$/{f=1; next}f==0{next}/^$/{exit}{print substr($0, 1);}' /etc/pacman.d/mirrorlist.backup`
  - to uncomment every mirror:
    - `sed -i 's/^#Server/Server/' /etc/pacman.d/mirrorlist.backup`
  - to rank the mirrors and output only the 6 fastest, then write to mirrorlist:
    - `rankmirrors -n 6 /etc/pacman.d/mirrorlist.backup > /etc/pacman.d/mirrorlist`

## Install zsh
- Install `zsh`:
  - `sudo pacman -S zsh`
- Change default shell for non-root user to `zsh`:
  - `chsh -s /usr/bin/zsh`
- Can check /etc/passwd to see if user's default shell was successfully changed.
- Log out and back in for changes to take effect.
- My zsh config is in [arch-dotfiles](https://github.com/prestidigitation/arch-dotfiles)

## Desktop environment
- KDE Plasma (for high-spec computers)
  - Install KDE Plasma (`plasma-meta` includes SDDM config file)
    - `sudo pacman -S plasma-meta`
  - Install SDDM, KDE-recommended display manager
    - `sudo pacman -S sddm`
  - Enable SDDM
    - `sudo systemctl enable sddm`
  - Install all KDE applications
    - `sudo pacman -S kde-applications-meta`

- XFCE (for low-spec computers)
  - Install XFCE Desktop and LXDM display manager
    - `sudo pacman -S xfce4 xfce4-goodies lxdm`
  - Enable LXDM 
    - `sudo systemctl enable lxdm`
