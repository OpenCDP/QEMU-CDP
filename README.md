## QEMU-CDP 

```
#ubuntu 18.04 x86_64

apt install glib*
apt install libglib2.0-dev
apt install libpixman-1-dev

#clone the code
git clone -b stable-2.11 https://github.com/OpenCDP/QEMU-CDP.git
cd QEMU-CDP
mkdir build
cd build
#build qemu
export CFLAGS="-Wno-error"
../configure --enable-kvm --target-list=x86_64-softmmu --enable-debug --enable-trace-backend=syslog
make

#download cirros image ...
mkdir /opt/vm
cd /opt/vm
wget "http://download.cirros-cloud.net/0.6.1/cirros-0.6.1-x86_64-disk.img"
#convert qcow2 to raw format
qemu-img convert -f qcow2 -O raw cirros-0.6.1-x86_64-disk.img cirros-0.6.1-x86_64-disk.raw

#run the qemu
/opt/QEMU-CDP/build/x86_64-softmmu/qemu-system-x86_64 -m 128 -enable-kvm \
-drive file=/opt/vm/cirros-0.6.1-x86_64-disk.raw,format=raw,if=virtio,id=drive-virtio-disk0 \
-vnc 0.0.0.0:0 --trace "cdp_*"

```
