build with podman (as root)
run inside container 
>bootc install to-disk --filesystem=xfs --disable-selinux --target-transport=containers-storage  --root-ssh-authorized-keys=/ak.keys /dev/zvol/DATA1/dev1

then create disk to the block device 
>vboxmanage createmedium disk --filename dev1.vmdk --format=VMDK --variant RawDisk --property RawDrive=/dev/zvol/DATA1/dev1

add storage controller to vm 
>vboxmanage storagectl coreos --name "sata01" --add sata --controller IntelAHCI
then attach 
>vboxmanage storageattach coreos --storagectl "sata01" --port 0 --device 0 --type hdd --medium dev1.vmdk

also need to set --firmware to EFI
and add network controller
set up port forwarding 

>VBoxManage modifyvm coreos  --natpf1 "guestssh,tcp,,2222,,22"

then start and ssh to root


Use bootc switch to build inside the booted system and upgrade
>bootc switch --apply  --transport=containers-storage localhost/zfs:bootc-3

or instead use rpm-ostree rebase if you layered packages locally
>rpm-ostree rebase ostree-unverified-image:containers-storage:localhost/zfs:v2


