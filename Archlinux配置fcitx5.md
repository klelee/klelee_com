fcitx5——Linux中最好用的中文输入法

# ArchLinux配置fcitx5 输入法

本文基于archlinux + dwm。其他的桌面环境以及窗口管理器，配置选项差不多。

## 安装基础包

- fcitx5-im

  首先是`fcitx5-im`包组，根据官网介绍，这个包组包含了：fcitx5本体、fcitx5-configtool和必要的输入法模块

- fcitx5-chinese-addons

  包含与中文相关的 addon，例如拼音、双拼和五笔。

- fcitx5-material-color

  一个fcitx5 的主题样式

```bash
sudo pacman -S fcitx5-im fcitx5-chinese-addons fcitx5-material-color
```

## fcitx5 配置

### 配置环境变量

```bash
sudo vim /etc/environment
----------------------------
# 在最后追加以下行
GTK_IM_MODULE=fcitx
QT_IM_MODULE=fcitx
XMODIFIERS=@im=fcitx
```

![image-20221003152552663](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20221003152552663.png)

### 配置fcitx5开机自启

```bash
vim ~/.xinitrc
------------------------------
# 在最后的exec dwm之前添加一句
fcitx5 &
```

![image-20221003152903997](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20221003152903997.png)

重启电脑或者注销

### 使用fcitx5-configtool

重新进入桌面之后，打开fcitx5-configtool。会进入下面的窗口，这里点击取消选中“仅显示当前语言”的复选框

![image-20221003153316266](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20221003153316266.png)

取消选中之后会出现一些中文常用的输入法，我们选择拼音，然后移到左边

![image-20221003153436453](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20221003153436453.png)

之后就可以看到pinyin已经加到左边我们在使用的输入法的框里面了，这里我们点击apply

![image-20221003153620653](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20221003153620653.png)

### Global Options

这里需要注意的就是中英文切换的方法是ctrl+space，然后勾选下面的复选框可以将中文设置为默认输入法，这个Linux用户不建议勾选。

![image-20221003153815540](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20221003153815540.png)

这里改完之后重启一下设备。

### 基本使用

那么现在重启之后，实际上fcitx5已经可以使用了，在输入界面按ctrl+space可以看到一个“拼”字，现在就可以输入中文了。

![](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20221003154243288.png)

其实在这里就能看出来现在还存在的一些弊端，这个联想能力太差了，这个明显是我们不能接受的，那我们继续我们的配置

![image-20221003154346873](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20221003154346873.png)

## 高级配置

### 美化

继续使用fcitx5-configtool 工具进行配置

进入addons选项卡，可以看到UI类下面有一个classic User interface ，也就是用户接口的意思，点击这一项后面的configure

![image-20221003154630447](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20221003154630447.png)

点击这里选择一个喜欢的主题，然后点击OK

![image-20221003154832576](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20221003154832576.png)

看看效果，好像确实好了那么一丢丢，但仅仅是一丢丢

![image-20221003154914476](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20221003154914476.png)

还有其他的一些主题可以安装，可以去AUR找找。

### 词汇匹配

#### 云拼音

下拉找到输入法类，点击pinyin后面的配置按钮

![image-20221003155528646](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20221003155528646.png)

1. 首先enable cloud pinyin

   ![image-20221003155636011](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20221003155636011.png)

2. 然后配置字库

   ![image-20221003155710186](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20221003155710186.png)

   将backend选项改成baidu

   ![image-20221003155820565](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20221003155820565.png)

   然后点ok返回

#### 离线字库

安装相关离线字库包：位于archlinuxcn

```bash
sudo pacman -S fcitx5-pinyin-moegirl fcitx5-pinyin-zhwiki
```

![image-20221003160432567](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20221003160432567.png)

可以在fcitx5-configtool -> addons -> input Method(pinyin) -> Dictionaries -> configure 查看安装的离线字库。

到这里archlinux 安装配置 fcitx5 就完成了。