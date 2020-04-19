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
- Edit /etc/hosts

- Enable dhcpcd

- Enable wpa_supplicant (laptops)

- Optimize pacman mirrors

## Install KDE Plasma desktop environment
