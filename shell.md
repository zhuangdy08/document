# Shell

----------


## Find命令

`find ./ -name *.o | xargs rm -f`  //查找当前目录的所有.o文件，并删除

`find ./ -name "*.h" -o -name "*.c" -o -name "*.cpp"` //查找当前目录下的所有.h, .c, .cpp文件

`find . -type f -exec touch {} \; ` //将所有的普通文件touch一下

## Tmpfs
`mount -t tmpfs tmpfs /home/tmp -o size=3M` //挂载tmpfs
也可以直接修改/etc/fstab来实现自动挂载
增加一行
`tmpfs	/home/tmp	tmpfs	defaults,size=3M	0	0`



## Mount
`mount -t vfat /dev/sda1 /media/disk1`   //挂载FAT32
`mount -t ext2 /dev/sdb1 /media/disk2`   //挂载Ext2
挂载NFS，将NFS服务器192.168.1.18的目录/nfsdir，挂载到当前系统的/share目录下
`mount -t nfs -o nolock -o tcp 192.168.1.18:/nfsdir /share` 
`mount -t loop /dev/sr0 /run/media`


## Tar
`tar -cvzf 20150828/data.tgz  data/`  #将当前目录下的data目录压缩，并将压缩文件data.tgz放到20150828目录下
`tar -xvzf data.tgz -C 20150828`    #将当前目录下的压缩文件data.tgz解压到当前目录下的20150828目录下


## dd

`dd if=/dev/sdc1 of=disk bs=4096 count=1`  //拷贝出数据

`dd if=/dev/zero of=/data01/test bs=64k count=1024 conv=fdatasync` //测试磁盘写入速度(写入640MB，每块64K，写1024次)

`dd if=/dev/mtdblock4 of=mtdblock4-2.bin`

//文件拼接
`uboot:1M,kernel:2M,system:3M,app:10M
dd if=uboot.bin of=flash.bin 
dd if=kernel.bin of=flash.bin bs=1k seek=1024
dd if=rootfs.bin of=flash.bin bs=1k seek=3072
dd if=app.bin of=flash.bin bs=1k seek=6144`


## mkdosfs
`mkdosfs -F 32  -n logdisk /dev/sdc1` //将/dev/sdc1格式化成FAT32，label为logdisk

## sed
`sed 's/ /\n/g' ` //将空格改成回车

/proc/cmdline的内容：
mem=96M console=ttyAMA0,115200 root=/dev/mtdblock1 ro rootfstype=jffs2 mtdparts=hi_sfc:2240K(boot),6400K(rootfs),7680K(app),64K(para)
mode=`cat /proc/cmdline | sed 's/ /\n/g' | grep rootfstype | awk -F"=" '{print $2}'`
if [ $mode = jffs2 ]; then
    echo "jffs2......"
else
	echo "no Jffs2...."
fi



## lsof
查看当前正在写入的文件
