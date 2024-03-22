## QEMU-CDP 

```
#clone the code
git clone -b stable-2.11 https://github.com/OpenCDP/QEMU-CDP.git
cd QEMU-CDP
mkdir build
cd build
#build qemu
../configure --enable-kvm --target-list=x86_64-softmmu --enable-debug --enable-trace-backend=syslog

#create vm interface
tunctl
ip link set tap0 up
brctl addif virbr0 tap0

#download cirros image ...
#run the qemu
/opt/QEMU-CDP/build/x86_64-softmmu/qemu-system-x86_64 -m 128 -enable-kvm \
-drive file=/opt/vm/cirros-0.6.1-x86_64-disk.raw,format=raw,if=virtio,id=drive-virtio-disk0 \
-vnc 0.0.0.0:0 --trace "cdp_*"

```
