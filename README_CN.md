## QEMU-CDP 

```
#编译qemu代码需要预安装对应的依赖库如glib等
#编译环境 ubuntu 18.06
apt install glib*
apt install libglib2.0-dev
apt install libpixman-1-dev

#目前基于qemu stable-2.11版本修改，因此clone对应版本代码
git clone -b stable-2.11 https://github.com/OpenCDP/QEMU-CDP.git
cd QEMU-CDP
#进入qemu-cdp目录创建编译目录build
mkdir build
cd build
#配置编译参数并编译，启用kvm虚拟化，只编译x86 64版本，启用debug调试信息
#启用trace event为syslog，这样可以方便观察我们cdp日志，/var/log/syslog
#不把warring当成error，否则无法编译通过
export CFLAGS="-Wno-error"
../configure --enable-kvm --target-list=x86_64-softmmu --enable-debug --enable-trace-backend=syslog
make
#一切顺利后，在build目录将会看到x86_64-softmmu/qemu-system-x86_64，里面存放了编译好的二进制文件

#下载虚拟机测试镜像，这里我们选择cirros
#需要注意的是，默认cirros镜像是qcow2格式的，我们需要转换成raw格式
#这样的话cdp模块合并数据就是跟磁盘布局一致
mkdir /opt/vm
cd /opt/vm
wget "http://download.cirros-cloud.net/0.6.1/cirros-0.6.1-x86_64-disk.img"
#qcow2转换成raw格式
qemu-img convert -f qcow2 -O raw cirros-0.6.1-x86_64-disk.img cirros-0.6.1-x86_64-disk.raw

#启动虚拟机，然后查看/var/log/syslog文件观测cdp模块日志
/opt/QEMU-CDP/build/x86_64-softmmu/qemu-system-x86_64 -m 128 -enable-kvm \
-drive file=/opt/vm/cirros-0.6.1-x86_64-disk.raw,format=raw,if=virtio,id=drive-virtio-disk0 \
-vnc 0.0.0.0:0 --trace "cdp_*"

```
