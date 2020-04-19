# Arch General Recommendations (After Base Install)

## System administration
- Create non-root user account and add to wheel group
  - `useradd -m -G wheel username`

- Set password for newly created user account
  - `passwd username`

- Give wheel group members full root privileges with sudo
  - `EDITOR=nano visudo`
  - Uncomment line `%wheel      ALL=(ALL) ALL`

## Network configuration
- Set the hostname
  - edit `/etc/hostname` to include single line with _myhostname_
  - Add the following lines to `/etc/hosts`:
    ```
    127.0.0.1        localhost
    ::1              localhost
    127.0.1.1        myhostname.localdomaimyhostname
    ```

- Configure dhcpcd
  - display all network interfaces
    - `ip link`
  - turn on ethernet interface
    - `ip link set ethernetname up`
  - enable and start dhcpcd systemd unit
    - `systemctl enable --now dhcpcd@ethernetname.service`
    - this allows dhcpcd to automatically connect to ethernet when starting computer

- Enable wpa_supplicant (laptop)
  - Display wifi device
    - `iw dev`
    - must have `iw` installed already! (chicken and egg problem)

- Optimize pacman download speed
  - install `pacman-contrib` in order to use the rankmirrors Bash script
    - `pacman -S pacman-contrib`
  - back up existing mirror list
    - `cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.backup`
  - if the servers in the file are grouped by country, one can extract all the servers of a specific country by using:
    - `awk '/^## Country Name$/{f=1; next}f==0{next}/^$/{exit}{print substr($0, 1);}' /etc/pacman.d/mirrorlist.backup`
  - to uncomment every mirror:
    - `sed -i 's/^#Server/Server/' /etc/pacman.d/mirrorlist.backup`
  - to rank the mirrors and output only the 6 fastest:
    - `rankmirrors -n 6 /etc/pacman.d/mirrorlist.backup > /etc/pacman.d/mirrorlist`

## Install KDE Plasma desktop environment
