# Arch Manual Updating and Troubleshooting Notes

## Fix swap file issue due to upgrade:
* Turn off and remove swap file
    - `sudo swapoff /swapfile`
    - `sudo rm -f /swapfile`
* Remove relevant entry from /etc/fstab
    - `sudo nano /etc/fstab`
* Create new swap file using dd
    - `sudo dd if=/dev/zero of=/swapfile count=2048 bs=1MiB`
    - `sudo mkswap /swapfile`
* Set permissions
    - `sudo chmod 600 /swapfile `
    - `sudo swapon /swapfile`
