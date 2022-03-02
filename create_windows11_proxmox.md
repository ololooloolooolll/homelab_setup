# Creating a virtual machine running Windows 11 on kvm

This article guides you to install a windows 11 guest in proxmox.

1. Download two (2) .iso files:
   a. https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/latest-virtio/virtio-win.iso
   b. windows installer iso
   
2. upload to the hypervisor's datastore 
3. make sure proxmox is running the latest version.
   a. I was running 7.0-11 and windows 11 machines were not an option
   b. I proceed with the upgrade of proxmox by enabling the non-subscritpion repositories
   c. I upgraded proxmox to 7.1-10
   d. after the upgrade, windows 11 is an option when creating a vm.
   
4. Once the vm instance is created you must add a disk as ide0.
5. The boot order must be to boot from virtIO (hdd), then the windows iso, then the drivers iso.
6. Access the bios and disable secure boot.
7. Boot the installer
