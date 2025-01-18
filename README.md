# Postgresql
Instalasi Postgresql on Oracle Linux 8

```
https://www.postgresql.org/download/linux/redhat/
```
```
[root@node01 ~]# hostnamectl
   Static hostname: node01
         Icon name: computer-vm
           Chassis: vm
        Machine ID: 9b488e669f9a407c974df87ba9433126
           Boot ID: 37d1d3f1bab044c195fb2ad8ebf980cf
    Virtualization: vmware
  Operating System: Oracle Linux Server 8.8
       CPE OS Name: cpe:/o:oracle:linux:8:8:server
            Kernel: Linux 5.15.0-101.103.2.1.el8uek.x86_64
      Architecture: x86-64
[root@node01 ~]#
![alt text](https://github.com/abduumr/Postgresql/blob/main/postgres/2.png?raw=true)

```

```
[root@node01 ~]# lsblk
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda           8:0    0   150G  0 disk
├─sda1        8:1    0   600M  0 part /boot/efi
├─sda2        8:2    0     1G  0 part /boot
└─sda3        8:3    0 148.4G  0 part
  ├─ol-root 252:0    0    70G  0 lvm  /
  ├─ol-swap 252:1    0   7.9G  0 lvm  [SWAP]
  └─ol-home 252:2    0  70.6G  0 lvm  /home
sdb           8:16   0    50G  0 disk
sr0          11:0    1  11.6G  0 rom
[root@node01 ~]#
```

```
[root@node01 ~]# df -h
Filesystem           Size  Used Avail Use% Mounted on
devtmpfs             3.7G     0  3.7G   0% /dev
tmpfs                3.8G   16K  3.8G   1% /dev/shm
tmpfs                3.8G   17M  3.7G   1% /run
tmpfs                3.8G     0  3.8G   0% /sys/fs/cgroup
/dev/mapper/ol-root   70G   17G   54G  24% /
/dev/sda2           1014M  276M  739M  28% /boot
/dev/sda1            599M  5.1M  594M   1% /boot/efi
/dev/mapper/ol-home   71G  536M   70G   1% /home
tmpfs                762M     0  762M   0% /run/user/0
[root@node01 ~]#
```

```
[root@node01 ~]# free -h
              total        used        free      shared  buff/cache   available
Mem:          7.4Gi       500Mi       6.0Gi        28Mi       986Mi       6.8Gi
Swap:         7.9Gi          0B       7.9Gi
[root@node01 ~]#
```

```
[root@node01 ~]# fdisk /dev/sdb

Welcome to fdisk (util-linux 2.32.1).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Device does not contain a recognized partition table.
Created a new DOS disklabel with disk identifier 0xb6da0b3d.

Command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-104857599, default 2048):
Last sector, +sectors or +size{K,M,G,T,P} (2048-104857599, default 104857599):

Created a new partition 1 of type 'Linux' and of size 50 GiB.

Command (m for help): p
Disk /dev/sdb: 50 GiB, 53687091200 bytes, 104857600 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0xb6da0b3d

Device     Boot Start       End   Sectors Size Id Type
/dev/sdb1        2048 104857599 104855552  50G 83 Linux

Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.
```

```
[root@node01 ~]# pvcreate /dev/sdb1
  Physical volume "/dev/sdb1" successfully created.
```
```
[root@node01 ~]# vgcreate database_vg /dev/sdb1
  Volume group "database_vg" successfully created
```
```
[root@node01 ~]# lvcreate -l 100%FREE -n database_lv database_vg
  Logical volume "database_lv" created.
```
```
[root@node01 ~]# mkfs.ext4 /dev/database_vg/database_lv

mke2fs 1.45.6 (20-Mar-2020)
Discarding device blocks: done
Creating filesystem with 13106176 4k blocks and 3276800 inodes
Filesystem UUID: a2473092-77ca-45c5-8dfe-693e5531de89
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208,
        4096000, 7962624, 11239424

Allocating group tables: done
Writing inode tables: done
Creating journal (65536 blocks): done
Writing superblocks and filesystem accounting information: done
```
```
[root@node01 ~]# mkdir /database/
[root@node01 ~]# mount /dev/database_vg/database_lv /database
```
```
[root@node01 ~]# vi /etc/fstab

```
```
[root@node01 ~]# lsblk
NAME                        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda                           8:0    0   150G  0 disk
├─sda1                        8:1    0   600M  0 part /boot/efi
├─sda2                        8:2    0     1G  0 part /boot
└─sda3                        8:3    0 148.4G  0 part
  ├─ol-root                 252:0    0    70G  0 lvm  /
  ├─ol-swap                 252:1    0   7.9G  0 lvm  [SWAP]
  └─ol-home                 252:2    0  70.6G  0 lvm  /home
sdb                           8:16   0    50G  0 disk
└─sdb1                        8:17   0    50G  0 part
  └─database_vg-database_lv 252:3    0    50G  0 lvm  /database
sr0                          11:0    1  11.6G  0 rom
```
```
isi
```
```
isi
```
```
isi
```
```
isi
```
```
isi
```
```
isi
```
