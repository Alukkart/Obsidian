sudo fallocate -l 1G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile

/etc/fstab
/swapfile swap swap defaults 0 0



sudo swapoff -v /swapfile
1. Remove the swap file entry `/swapfile swap swap defaults 0 0` from the `/etc/fstab` file.
2. sudo rm /swapfile