# Arch Linux + KDE 配置&美化（持续更新~）

这篇文章着重记录archlinux + KDE的一个基本的配置过程。不包括安装过程（使用[archInstall.sh](https://github.com/klelee/arch_install/blob/master/archInstall.sh)）。内容大概有以下几点：

当前美化进度

![SDDM](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220827081936876.png)

![Desktop](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220827082110852.png)

![Dolphin](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220827082143676.png)

#### 配置阶段项目

1. archlinuxcn源配置
2. fcitx + fcitx-sogoupinyin 输入法配置

#### 美化阶段项目

1. 全局主题
2. 状态栏
3. 虚拟桌面
4. 图标
5. SDDM
6. Dock

那我们开始吧！

### Start

这是刚刚安装完成的样子，左上角会显示Plasma(X11)，人像下方会显示你安装过程中设置的用户名。

![image-20220827060906689](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220827060906689.png)

![image-20220827061843695](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220827061843695.png)

### 显示设置（分辨率&缩放）

System Setting -> Display and Monitor -> Display Configuration

![image-20220827061507080](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220827061507080.png)

设置为自己需要的分辨率，下面的Global scale 就是缩放，这个设置需要reboot。根据需要设置。

## 配置

在开始之前先介绍一个全局的快捷键：Alt + Space （程序启动器），类似于Windows端我们用到utools

![image-20220827062303398](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220827062303398.png)

### 配置archlinuxcn源

该仓库是由archlinux中文社区驱动的一个非官方的软件仓库。编辑/etc/pacman.conf：

```
sudo vim /etc/pacman.conf
-------------------------------------------
# 在最后添加
[archlinuxcn]
Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch   
```

需要注意的是，这里的两个highlight点处都是archlinuxcn而非archlinux

![image-20220827063241209](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220827063241209.png)

然后更新GPG密钥

```
sudo pacman -Sy archlinuxcn-keyring
```

**注** ： 如果以上更新密钥步骤出现错误，就是那种连着一串ERROR的情况，请执行以下步骤

```
sudo rm -rf /etc/pacman.d/gnupg
sudo pacman-key --init
sudo pacman-key --populate archlinux archlinuxcn
sudo pacman -Syy
```

至此，archlinuxcn源配置完成，现在就可以安装一些在archlinuxcn源的软件了。

```
sudo pacman -S yay
```

笑死，不会有人会去考虑某个包是在archlinuxcn源还是在AUR里面，我到底该使用pacman还是yay吧。当然是全部用yay呀！

### 输入法配置

输入法采用fcitx + Sogou的组合，安装需要的包：

```
yay -S fcitx fcitx-im fcitx-configtool fcitx-sogoupinyin
```

然后写一个fcitx的配置文件（温馨提示：能CV的就不要打字）

```
vim ~/.xprofile
-------------------------------------
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
```

这一步配置完成需要重启！`reboot`

重启之后看到小键盘没：

![image-20220827064752846](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220827064752846.png)

右击小键盘，选择Configure

![image-20220827064840862](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220827064840862.png)

点击左下角的加号

![image-20220827064916088](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220827064916088.png)

取消勾选only Show Current Language，之后拉到最下面选择：sogoupinyin

![image-20220827065223771](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220827065223771.png)

单击选中sogoupinyin之后点击向上的箭头将sogoupinyin移动到最上面

![image-20220827065350513](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220827065350513.png)

然后选中下面的Keyboard-English(US) 点击下面的减号，删掉它

![image-20220827065504792](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220827065504792.png)

这样，sogoupinyin就配置完成了

![image-20220827065632543](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220827065632543.png)

## 美化

由于众所周知的原因，在国内无法直接在设置中下载KDE的主题，所以我们选择从KDE Store网页版下载主题：

[KDE Store](https://store.kde.org/browse/)

所有的资源如果下不下来或者其他原因都可以加群，群文件应有尽有：219261838

### 全局主题

[Global themes](https://store.kde.org/browse?cat=121&ord=latest)

个人比较喜欢的全局主题是[future-dark](https://github.com/yeyushengfan258/Future-kde)和future。接下来我以future-dark包的下载安装记录：

这里不要直接下载，直接去GitHub：

![image-20220827070715003](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220827070715003.png)

下载方式自行选择，Download ZIP和git clone 哪个能搞下来就搞哪个，

![image-20220827071138712](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220827071138712.png)

这是我的选择

![image-20220827071447735](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220827071447735.png)

进入解压之后的文件夹，执行install.sh，因为涉及到`/usr`目录的操作，所以使用sudo执行

![image-20220827071812106](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220827071812106.png)

这样子就安装完成了，然后去设置里更换全局主题吧。

System Setting -> Appearance -> Global Theme 选择Future-dark 然后Apply

![image-20220827072251515](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220827072251515.png)

### 状态栏

右击状态栏 -> Enter Edit Mode 左键按住Drag to move那个按钮将状态栏拖到上方，Panel height 设置为25（自行选择）

![image-20220827072441087](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220827072441087.png)

这一步之后

![image-20220827072706875](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220827072706875.png)

状态栏中不需要的项目可以删掉，也可以添加需要的东西：

![image-20220827072821244](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220827072821244.png)

在这里选择需要的工具放到状态栏，比如Global Menu，按住它，拖到状态栏适当位置。

![image-20220827072933725](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220827072933725.png)

托上去之后显示不出来，可以打开Dolphin看看效果

![image-20220827073051277](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220827073051277.png)

剩下的项目可以自行摸索，看看需要什么。

### 虚拟桌面

Linux端的虚拟桌面是真的好用

System Setting -> Workspace behavior -> Virtual Desktop 

![image-20220827073433748](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220827073433748.png)

为了呈现的更加清晰，我换了个壁纸

![image-20220827073619221](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220827073619221.png)

之后，就可以在不用的桌面做不同的事情了。

### 图标

和找主题同样的原理去找图标

[Full Icon Themes](https://store.kde.org/browse?cat=132&ord=latest)

我最喜欢的图标主题是：[WhiteSur icon theme](https://store.kde.org/p/1405756/) 这个可以直接Download

![image-20220827074433144](C:\Users\klfairy\AppData\Roaming\Typora\typora-user-images\image-20220827074433144.png)

解压之后会有两个文件夹：

![image-20220827075205411](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220827075205411.png)

将这俩cp到`/usr/share/icons

![](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220827075319110.png)

然后打开设置更换图标即可

![image-20220827075711631](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220827075711631.png)

更换后的图标，果里果气有木有！

![image-20220827075833062](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220827075833062.png)

### SDDM

System Setting -> Startup and Shutdown -> Login Screen (SDDM)

玄学吧，这个可以直接下载

![image-20220827080027302](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220827080027302.png)

选评分最高的

![image-20220827080101127](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220827080101127.png)

注意点，这里需要输密码，超时就要重新下。下载完成之后不会立即显示，返回重进一下就可以看到你下载的项目。

### Dock栏

安装latte-dock

![image-20220827080946901](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220827080946901.png)

启动latte

![image-20220827081017162](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220827081017162.png)

初始效果

![image-20220827081039958](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220827081039958.png)

右击dock栏，选择Edit Dock

![image-20220827081111896](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220827081111896.png)

我的配置，根据自己喜好设置

![image-20220827081234820](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220827081234820.png)

就，，先到这里吧，有时间再写！