# Creating a KASM server to containerize applications


1. Setup Ubuntu 20.04 server to host the application, kasm.local

2. setup swap partition for stability

  dd if=/dev/zero bs=1M count=1024 of=/mnt/1GiB.swap
  chmod 600 /mnt/1GiB.swap
  mkswap /mnt/1GiB.swap
  swapon /mnt/1GiB.swap
  
3. Make swap survive a reboot

  echo '/mnt/1GiB.swap swap swap 0 0' | tee -a /etc/fstab
  
4. Download kasm software

  wget https://kasm-static-content.s3.amazonaws.com/kasm_release_9.99.9.999999.tar.gz
  tar -xf ./kasm_release.tar.gz
  kasm_release/install.sh
  
**A few hours later**
  
5. Voila, kasm is ready and free to use for personal use.
  
  
