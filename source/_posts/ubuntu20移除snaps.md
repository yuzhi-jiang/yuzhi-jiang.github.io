---
categories: []
---
### 移除现存snaps

在新安装的Ubuntu中，已经默认安装了部分snaps，可以通过以下命令查看:

```
snap list
```

例如，Raspberry Pi 4的Ubuntu Server 64位版本，输出显示:

```
Name    Version   Rev    Tracking       Publisher   Notes
core18  20200707  1883   latest/stable  canonical✓  base
lxd     4.0.2     16103  4.0/stable/…   canonical✓  -
snapd   2.45.2    8543   latest/stable  canonical✓  snapd
```

并且，如果你执行 df -h 命令可以看到已经挂载了snap的服务:

```
/dev/loop0       49M   49M     0 100% /snap/core18/1883
/dev/loop1       64M   64M     0 100% /snap/lxd/16103
/dev/loop2       26M   26M     0 100% /snap/snapd/8543
/dev/loop3       49M   49M     0 100% /snap/core18/1888
```

##### 注意 删除snap之前要先删除 包

使用 sudo snap remove <package> 命令移除:

```
sudo snap remove lxd
```

但是不能先移除 snapd ，如果你执行 sudo snap remove snapd 会提示错误:

```
error: cannot remove "snapd": snap "snapd" is not removable: remove all other snaps first

```

所以先移除 core18

```
sudo snap remove core18
```

最后移除 snapd

```
sudo snap remove snapd
```

<br/>

此时 snapd 服务依然在运行，需要卸载 snapd 软件包来清理

检查文件系统挂载 df -h 可以看到所有snaps相关挂载都已经消除:

```
Filesystem      Size  Used Avail Use% Mounted on
udev            876M     0  876M   0% /dev
tmpfs           185M  2.7M  183M   2% /run
/dev/mmcblk0p2  117G  5.0G  108G   5% /
tmpfs           925M     0  925M   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           925M     0  925M   0% /sys/fs/cgroup
/dev/mmcblk0p1  253M   97M  156M  39% /boot/firmware
tmpfs           185M     0  185M   0% /run/user/1000
```

删除和清理snapd软件包:

```
sudo apt purge snapd
```

执行 apt autoremove 清理所有无用软件包:

```
sudo apt autoremove
```

<br/>

删除snap目录:

```
rm -rf ~/snap
sudo rm -rf /snap
sudo rm -rf /var/snap
sudo rm -rf /var/lib/snapd
```

现在，所有snaps已经清理干净，
