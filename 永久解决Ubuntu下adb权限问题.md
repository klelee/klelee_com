在Linux环境下使用adb链接手机，经常会遇到全下问题，如下：

```shell
adb: unable to connect for root: insufficient permissions for device:...
```

很多帖子都说在`/etc/udev/rules.d/`目录下，新建`51-android.rules` 但这个方法仅仅对当前设备有效，如果换个设备还是存在权限问题。

因此这里建议直接提升adb命令的权限。

首先，查看adb命令所在位置`whereis adb`

```shell
klelee@klubuntu:~$ whereis adb
adb: /usr/bin/adb /usr/share/man/man1/adb.1.gz
```

给adb命令增加权限

1. 只给当前用户增加权限

   `sudo chmod u+s /usr/bin/adb`

2. 给所有人增加权限

   `sudo chmod a+s /usr/bin/adb`
