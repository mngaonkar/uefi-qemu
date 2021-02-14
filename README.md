# UEFI with QEMU

# QEMU UEFI
qemu-system-x86_64 --bios ovmf-efi-bios.bin
ifconfig -l eth0

# Install Linux on QEMU
qemu-img create -f qcow2 alpine.qcow2 0.5G
qemu-system-x86_64 -m 512 -nic user -boot d -cdrom ~/Downloads/alpine-standard-3.13.1-x86_64.iso -hda alpine.qcow2 --bios ovmf-efi-bios.bin
setup-alpine
qemu-system-x86_64 -m 512 -nic user -hda alpine.qcow2


qemu-system-x86_64 \
-smp 2 \
-vnc :5 \
-m 1024 \
-drive file=alpine.qcow2,if=virtio \
-bios ovmf-efi-bios.bin \
-device virtio-balloon \
-boot c \
-net nic,model=virtio,macaddr=54:54:00:55:55:55 \
-net tap,script=./scripts/tap-up,downscript=./scripts/tap-down \
-daemonize


brctl addbr br0
