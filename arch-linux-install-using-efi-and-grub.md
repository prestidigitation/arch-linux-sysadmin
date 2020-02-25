# Installing Arch Linux with EFI

- Download the [Arch Linux ISO](https://www.archlinux.org/download) (torrent recommended).

- Create a live USB of Arch Linux.
  - In GNU/Linux:
    - Find out the name of your USB drive with `lsblk`. Make sure that it is **not** mounted.
    - Run the following command, replacing `/dev/**sdx**` with your drive, e.g. `/dev/sdb`. (Do **not** append a partition number, so do **not** use something like `/dev/sdb**1**`)
    - `dd bs=4M if=path/to/archlinux.iso of=/dev/sdx status=progress oflag=sync`
  
  - macOS:
    - First you need to identify the USB device. List all storage devices with the command:
    `diskutil list`
    - Your USB device will appear as something like `/dev/disk2 (external, physical)`. Verify that this is the device you want to erase by checking its name and size and then use its identifier for the commands below instead of /dev/diskX. A USB device is normally auto-mounted in macOS, and you have to unmount (not eject) it before block-writing to it with `dd`.
    `diskutil unmountDisk /dev/diskX`
    - Now copy the ISO image file to the device. The dd command is similar to its Linux counterpart, but notice the 'r' before 'disk' for raw mode which makes the transfer much faster:
    `dd if=path/to/arch.iso of=/dev/rdiskX bs=1m`
    - This command will run silently. To view progress, send SIGINFO by pressing Ctrl+t. Note diskX here should not include the s1 suffix, or else the USB device will only be bootable in UEFI mode and not legacy. After completion, macOS may complain that "The disk you inserted was not readable by this computer". Select 'Ignore'. The USB device will be bootable.

- Boot into UEFI settings (modern replacement for BIOS)
    - Check motherboard manual for specifics
    - Disable secure boot
    - Disable launch CSM or legacy support
      - CSM (Compatibility Support Module) is a component of UEFI that provides legacy BIOS compatibility by emulating a BIOS environment.
      - Windows 8 and later support UEFI but some graphics cards from as recently as 2013 don't.

- Press correct key to boot from USB drive on computer start (probably F2 or F11) and wait for Arch Linux to load live session.

- Verify boot mode:
    - `ls /sys/firmware/efi/efivars` (If the directory exists then computer supports EFI)

- Check internet connection with ping
  - `ping -c 6 archlinux.org`

- Update the system clock
  - `timedatectl set-ntp true`
  - `timedatectl set-timezone America/New_York`
  - Verify with `timedatectl status`

- Create EFI partition
  - `fdisk -l` to find the designation for the SSD (such as `/dev/sdb`)
  - `fdisk /dev/sdb`
    - g (to create a new partition table)
    - n (to create a new partition)
    - 1
    - enter
    - +512M
    - t
    - 1 (for EFI)
    - w

- Create root partition
  - `fdisk /dev/sdb`
    - n
    - 2
    - enter
    - enter (using the rest of the SSD)
    - w

> Home partition isn't really necessary, and can use swap file instead of swap partition.

- Create the filesystems:
  - `mkfs.fat -F32 /dev/sdb1`
  - `mkfs.etx4 /dev/sdb2`

- Mount root directory
  - `mount /dev/sdb2 /mnt`

- Use pacstrap script to install the base package, Linux kernel and firmware for common hardware:
  - `pacstrap /mnt base linux linux-firmware`

- Generate the `/etc/fstab` file:
  - `genfstab -U -p /mnt >> /mnt/etc/fstab`

- Chroot into installed system:
  - `arch-root /mnt`

- Set the time zone:
  - `ln -sf /usr/share/zoneinfo/America/New_York /etc/localtime`

- Update the hardware clock:
  - `hwclock --systohc`

- Set the root password
  - `passwd`

- Install boot manager and other helpful tools
  - `pacman -S grub efibootmgr sudo nano`

- Set locale:
  - `nano /etc/locale.gen`
  - Uncomment en_US.UTF-8
  - `locale-gen`

- Create EFI boot directory
  - `mkdir /efi`
  - `mount /dev/sdb1 /efi

- Install GRUB on EFI mode
  - `grub-install --efi-directory=/efi` (defaults to x86_64=efi platform)

- Set locale for GRUB:
  - `cp /usr/share/locale/en\@quot/LC_MESSAGES/grub.mo /boot/grub/locale/en.mo`

- Write GRUB config:
  - `grub-mkconfig -o /boot/grub/grub.cfg`

- Exit, unmount and reboot
  - `exit`
  - `umount -a`
  - `reboot`