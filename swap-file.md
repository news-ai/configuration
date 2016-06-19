Swap file

1. `sudo fallocate -l 4G /swapfile`
2. `sudo chmod 600 /swapfile`
3. `sudo mkswap /swapfile`
4. `sudo swapon /swapfile`
5. `sudo sh -c 'echo "/swapfile none swap sw 0 0" >> /etc/fstab'` (at boot)
