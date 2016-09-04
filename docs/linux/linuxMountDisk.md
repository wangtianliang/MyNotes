#Linux下挂载硬盘的方法
* 查看当前分区情况
	
		df -h

* 添加磁盘，查看磁盘状况
    
		[root@db1 /]# fdisk -l
		Disk /dev/sda: 10.7 GB, 10737418240 bytes
		255 heads, 63 sectors/track, 1305 cylinders
		Units = cylinders of 16065 * 512 = 8225280 bytes
		   Device Boot  Start End  Blocks   Id  System
		/dev/sda1   * 1511305 9277537+  83  Linux
		/dev/sda2   1 150 1204843+  82  Linux swap
		Partition table entries are not in disk order
		Disk /dev/sdb: 5368 MB, 5368709120 bytes
		255 heads, 63 sectors/track, 652 cylinders
		Units = cylinders of 16065 * 512 = 8225280 bytes
		   Device Boot  Start End  Blocks   Id  System
 
	从查询结果看出，多了一个/dev/sdb的盘
 
* 用fdisk 对/dev/sdb 进行分区
 
		[root@db1 /]# fdisk /dev/sdb
		Command (m for help): n //创建新分区
		Command action
		   e   extended
		   p   primary partition (1-4)
		p //创建主分区
		Partition number (1-4): 1 //新建主分区序号
		First cylinder (1-652, default 1):
		Using default value 1
		Last cylinder or +size or +sizeM or +sizeK (1-652, default 652):
		Using default value 652
		 
		Command (m for help): w //w 表示保存
		The partition table has been altered!
		 
		Calling ioctl() to re-read partition table.
		Syncing disks.
 
	* 再次查看分区情况，多出来一个/dev/sdb1 的区，这个1是我们在前面指定的，如果我们指定2，就变成 sdb2了。
 
			[root@db1 /]# fdisk -l
			Disk /dev/sda: 10.7 GB, 10737418240 bytes
			255 heads, 63 sectors/track, 1305 cylinders
			Units = cylinders of 16065 * 512 = 8225280 bytes
			   Device Boot      Start         End      Blocks   Id  System
			/dev/sda1   *         151        1305     9277537+  83  Linux
			/dev/sda2               1         150     1204843+  82  Linux swap
			Partition table entries are not in disk order
			 
			Disk /dev/sdb: 5368 MB, 5368709120 bytes
			255 heads, 63 sectors/track, 652 cylinders
			Units = cylinders of 16065 * 512 = 8225280 bytes
			 
			   Device Boot      Start         End      Blocks   Id  System
			/dev/sdb1               1         652     5237158+  83  Linux
			[root@db1 /]#
 
	* 如果创建完之后，/proc/partitions 查看不到对应的分区，使用parprobe 命令刷新一下就可以了：
		
			[root@web1 ~]# cat /proc/partitions 
			major minor  #blocks  name
			   8     0  175825944 sda
			   8     1    1020096 sda1
			   8     2   30716280 sda2
			   8     3    8193150 sda3
			[root@web1 ~]# partprobe /dev/sda
			[root@web1 ~]# cat /proc/partitions 
			major minor  #blocks  name
			
			
			   8     0  175825944 sda
			   8     1    1020096 sda1
			   8     2   30716280 sda2
			   8     3    8193150 sda3
			   8     4  135893835 sda4
			[root@web1 ~]# 


 
* 格式化 /dev/sdb1 分区
 
		[root@db1 /]# mkfs -t ext3 /dev/sdb1
		mke2fs 1.35 (28-Feb-2004)
		Filesystem label=
		OS type: Linux
		Block size=4096 (log=2)
		Fragment size=4096 (log=2)
		655360 inodes, 1309289 blocks
		65464 blocks (5.00%) reserved for the super user
		First data block=0
		Maximum filesystem blocks=1342177280
		40 block groups
		32768 blocks per group, 32768 fragments per group
		16384 inodes per group
		Superblock backups stored on blocks:
		        32768, 98304, 163840, 229376, 294912, 819200, 884736
		 
		Writing inode tables: done
		Creating journal (8192 blocks): done
		Writing superblocks and filesystem accounting information: done
		 
		This filesystem will be automatically checked every 30 mounts or
		180 days, whichever comes first.  Use tune2fs -c or -i to override.
 
* 创建目录 并将 /dev/sdb1 挂在到该目录下
 
		[root@db1 /]# ls
		backup  dev   initrd      media  opt   sbin     sys       usr
		bin     etc   lib         misc   proc  selinux  tftpboot  var
		boot    home  lost+found  mnt    root  srv      tmp
		[root@db1 /]# mkdir /u01
		[root@db1 /]# ls
		backup  dev   initrd      media  opt   sbin     sys       u01
		bin     etc   lib         misc   proc  selinux  tftpboot  usr
		boot    home  lost+found  mnt    root  srv      tmp       var
		[root@db1 /]# mount /dev/sdb1 /u01
 
* 验证挂载是否成功

		[root@db1 /]# df -k
		Filesystem           1K-blocks      Used Available Use% Mounted on
		/dev/sda1              9131772   7066884   1601012  82% /
		none                    454256         0    454256   0% /dev/shm
		/dev/sdb1              5154852     43040   4849956   1% /backup
 
* 设置开机自动挂载
 
		[root@db1 /]# vi /etc/fstab
		# This file is edited by fstab-sync - see 'man fstab-sync' for details
		LABEL=/                 /                       ext3    defaults        1 1
		none                    /dev/pts                devpts  gid=5,mode=620  0 0
		none                    /dev/shm                tmpfs   defaults        0 0
		none                    /proc                   proc    defaults        0 0
		none                    /sys                    sysfs   defaults        0 0
		LABEL=SWAP-sda2         swap                    swap    defaults        0 0
		/dev/sdb1               /u01                 ext3    defaults        0 0
		/dev/hdc                /media/cdrom            auto    pamconsole,exec,noauto,m
		anaged 0 0
		/dev/fd0                /media/floppy           auto    pamconsole,exec,noauto,m
		anaged 0 0

* 阿里云分区可参考
		
	[https://help.aliyun.com/document_detail/25426.html?spm=5176.775974154.0.0.7Lnp5C](https://help.aliyun.com/document_detail/25426.html?spm=5176.775974154.0.0.7Lnp5C "Linux格式化和挂载数据盘")