# How to import a virtualbox (or vmware) virtual machine to proxmox hypervisor

A great source for virtual machines is vulnhub.com. You can download OVA files from there.

## TLDR

    root@pve:/# tar -xvf vm.ova
    root@pve:/# qm importdisk TARGETVMID extracted.bmdk local -format qcow2
    GUI > VM > Hardware > Hard Disk > Add
    
## Detailed explaination

1. Once you have a working virtual machine on your workstation, from virtualbox, go to 

    file > export appliance > SELECT THE VM > continue > Format: OVF 1.0 > file: vm.ova > continue > export
    
**The process may take quite some time.**

2. Extract files from the archive

    user@host# tar xf vm.ova
    
**This process may also take quite some time**

3. Copy the virtual disk (vmdk) to proxmox datastore

    user@host# scp vm.vmdk root@pve:/tmp
    password:
    
**Depending on the size of the disk, the network etc, this step can be fast or slow**

4. Access the proxmox server and convert the vmdk file to qcow2

    root@pve# qemu-img convert -f vmdk vm.vmdk -O qcow2 vm.qcow2
    or
    root@pve# qemu-img convert -O qcow2 -c -f vmdk vm.vmdk vm.qcow2
    
**This step may take some time.**

5. Next step is proxmox's GUI

    Right-click on "pve" > Create VM > set vmid(ex: 105), vm_name > no media > next > Hard disk: temp > ram > network > confirm
    Select the VM, click on Hardware > select Hard Disk > detach > select Unused Disk 0 > Remove

6. Import disk

    root@pve:/tmp# qm importdisk 105 vm.qcow2 PathToDisks 
    
7. Return to proxmox GUI, there should be a new Unused Disk in the vm hardware

    double click > add
    
8. Go to virtual machine option

    Change boot order > device 1: Disk "ide0"
    
9. If the machine doesn't boot

    Check bios.
    Check display.
    Check Machine.
    Some bios settings won't work EFI/GPT.
    
