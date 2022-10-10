本着学习C的态度来了解dwm，本身作为一个i3wm的追崇者，与dwm会擦出怎么样的火花呢？

## 下载安装dwm

### archlinuxcn源配置

编辑`/etc/pacman.conf`文件，添加bfsu的archlinuxcn源

```bash
sudo vim /etc/pacman.conf
---------------------------------------------
[archlinuxcn]
Server = https://mirrors.bfsu.edu.cn/archlinuxcn/$arch
sudo pacman -Sy archlinuxcn-keyring
```

### 安装dwm所需要的基本包

```bash
sudo pacman -S xorg xorg-xinit feh pcmanfm compton xfce4-terminal
```

### 下载dwm

可以建立单独的目录用于管理dwm相关的配置，也可以像我一样直接把相关的仓库放在家目录下

```bash
git clone https://git.suckless.org/dwm
git clone https://git.suckless.org/st
git clone https://git.suckless.org/dmenu
```

### 首次编译dwm并配置启动dwm

分别进入到自己克隆的这三个仓库中执行：

```bash
sudo make clean install
# 需要注意的是在这个仓库下操作需要使用sudo权限
```

配置xinit来启动dwm：

首先将xinit的配置文件拷贝一份到家目录下：

```bash
cp /etc/X11/xinit/xinitrc .xinitrc
```

然后编辑`.xinitrc`文件：

删除掉最后五行，这五行我们不会用到，直接删除即可，然后在最后加上`exec dwm`，上图：

![image-20221010062136112](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20221010062136112.png)

## 配置dwm

### 快捷键更改

尤其对于i3wm的老用户来说，这个太必要了。比如我更习惯用win+q去关闭窗口、习惯用alt+enter来打开终端等等，这些都是需要配置的。

首先进入dwm仓库，编辑config.h文件：

```bash
cd dwm
sudo vim config.h
-------------------------------------------
// 首先找到 key definitions ，我个人比较习惯设置两个mod键，
// win键更趋向于一些窗口管理
// alt键更趋向于一些动作
/* key definitions */
#define MODKEY1 Mod1Mask    // alt键
#define MODKEY Mod4Mask    //win键
#define TAGKEYS(KEY,TAG) \
	{ MODKEY,                       KEY,      view,           {.ui = 1 << TAG} }, \
	{ MODKEY|ControlMask,           KEY,      toggleview,     {.ui = 1 << TAG} }, \
	{ MODKEY|ShiftMask,             KEY,      tag,            {.ui = 1 << TAG} }, \
	{ MODKEY|ControlMask|ShiftMask, KEY,      toggletag,      {.ui = 1 << TAG} },
```

上图：

![image-20221010063134475](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20221010063134475.png)

更改几个常用的快捷键，涉及关闭窗口、打开终端、退出dwm

```C
static const Key keys[] = {
	/* modifier                     key        function        argument */
    /* 打开dmenu win + p */
	{ MODKEY,                       XK_p,      spawn,          {.v = dmenucmd } },
    /* 打开终端 Alt + Enter */
	{ MODKEY1,       	        XK_Return, spawn,          {.v = termcmd } },
	{ MODKEY,                       XK_b,      togglebar,      {0} },
	{ MODKEY,                       XK_j,      focusstack,     {.i = +1 } },
	{ MODKEY,                       XK_k,      focusstack,     {.i = -1 } },
	{ MODKEY,                       XK_i,      incnmaster,     {.i = +1 } },
	{ MODKEY,                       XK_d,      incnmaster,     {.i = -1 } },
	{ MODKEY,                       XK_h,      setmfact,       {.f = -0.05} },
	{ MODKEY,                       XK_l,      setmfact,       {.f = +0.05} },
	{ MODKEY,                       XK_Return, zoom,           {0} },
	{ MODKEY,                       XK_Tab,    view,           {0} },
    /* 关闭当前聚焦的窗口 win + p */
	{ MODKEY, 	                XK_q,      killclient,     {0} },
	{ MODKEY,                       XK_t,      setlayout,      {.v = &layouts[0]} },
	{ MODKEY,                       XK_f,      setlayout,      {.v = &layouts[1]} },
	{ MODKEY,                       XK_m,      setlayout,      {.v = &layouts[2]} },
	{ MODKEY,                       XK_space,  setlayout,      {0} },
	{ MODKEY|ShiftMask,             XK_space,  togglefloating, {0} },
	{ MODKEY,                       XK_0,      view,           {.ui = ~0 } },
	{ MODKEY|ShiftMask,             XK_0,      tag,            {.ui = ~0 } },
	{ MODKEY,                       XK_comma,  focusmon,       {.i = -1 } },
	{ MODKEY,                       XK_period, focusmon,       {.i = +1 } },
	{ MODKEY|ShiftMask,             XK_comma,  tagmon,         {.i = -1 } },
	{ MODKEY|ShiftMask,             XK_period, tagmon,         {.i = +1 } },
	TAGKEYS(                        XK_1,                      0)
	TAGKEYS(                        XK_2,                      1)
	TAGKEYS(                        XK_3,                      2)
	TAGKEYS(                        XK_4,                      3)
	TAGKEYS(                        XK_5,                      4)
	TAGKEYS(                        XK_6,                      5)
	TAGKEYS(                        XK_7,                      6)
	TAGKEYS(                        XK_8,                      7)
	TAGKEYS(                        XK_9,                      8)
    /* 退出dwm win + shfit + c */
	{ MODKEY|ShiftMask,             XK_c,      quit,           {0} },
	/* klelee's volume config */
	{ MODKEY,			XK_F1,	   spawn,	SHCMD("amixer sset Master toggle") },
	{ MODKEY,			XK_F2,	   spawn,	SHCMD("amixer sset Master 5%- ") },
	{ MODKEY,			XK_F3,	   spawn,	SHCMD("amixer sset Master 5%+ ") },
    /* 截屏  需要安装flameshot */
	{ MODKEY1,			XK_p,	   spawn,	SHCMD("flameshot gui") },
	{ MODKEY,                       XK_minus,  setgaps,        {.i = -1 } },
	{ MODKEY,                       XK_equal,  setgaps,        {.i = +1 } },
	{ MODKEY|ShiftMask,             XK_equal,  setgaps,        {.i = 0  } },
};

```

上图：可能还有一些配置项下面图片没有highlight，但是后面会说到

![image-20221010064126843](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20221010064126843.png)

设置默认终端为xfce4-terminal，这玩意儿是真的好用呀！！！

同样是编辑config.h，找到commands模块，修改termcmd的值为xfce4-terminal即可：

```C
/* commands */
static const char *dmenucmd[] = { "dmenu_run", "-fn", dmenufont, "-nb", col_gray1, "-nf", col_gray3, "-sb", col_cyan, "-sf", col_gray4, NULL };
static const char *termcmd[]  = { "xfce4-terminal", NULL };

```

上图：

![image-20221010064421018](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20221010064421018.png)

### 设置壁纸和透明度

可乐有一个background的仓库，直接克隆使用，可乐yyds

```bash
mkdir ~/Pictrues  # 新装的archlinux是没有home下对应的这些目录的，需要重新建立
cd ~/Pictures
git clone https://gitlink.org.cn/klelee/background.git
cd background && rm -rf README.md
```

那么第一步，好看的图片有了，接下来设置启动的时候随机选择一张好看的图片作为壁纸：

```shell
vim .xinitrc
-----------------------------------------
			......
# 在exec dwm之前添加
compton -b  # 这个是设置透明的
feh --bg-fill --randomize ~/Pictures/background/*  # 这个是设置随机壁纸的哦
			......
exec dwm
```

### 配置中文输入法

[Archlinux安装配置fcitx5](http://klelee.com/index.php/archives/11/) 可乐yyds

### 安装俩常用的软件

```bash
sudo pacman -S yay  # 不要问我为什么不用paru
# 个人习惯用edge，如果你不喜欢就装chrome
yay -S microsoft-edge-stable-bin visual-studio-code-bin
yay -S google-chrome
```

### Tag样式配置

也就是每一个workspace的标签样式，首先需要安装图标字体，让系统能够显示图标：

```bash
sudo pacman -S ttf-nerd-fonts-symbols-2048-em
```

然后刚刚安装的浏览器就派上用场了（那安装vscode是干嘛的？问就是怕你不会用vim），浏览器进入[nerd-fonts的官网](https://www.nerdfonts.com/) ，进来之后大概长这个样子，然后点击中间的icons：

![image-20221010070155441](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20221010070155441.png)

接下来在这里输入，你想要的图标，比如：terminal，然后选一个喜欢的图标，点击图标右上角弹出的Icon，就成功的将当前图标复制到剪贴板了

![image-20221010070302838](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20221010070302838.png)

然后，不管你用vim还是vscode打开dwm/config.h文件，把你的图标复制到这个位置哦：

![image-20221010070634947](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20221010070634947.png)

然后退出重新编译，再次进入dwm的时候，图标就变了呢

### 字体配置

上面配置完图标会不会觉得图标很虚，看着看着就觉的字体也很虚？没错，是你的字体太小了，哈哈哈。然后，不管你用vim还是vscode打开dwm/config.h文件，编辑下面的这个数组：

```C
static const char *fonts[]          = { "monospace:size=12",
				"WenQuanYi Micro Hei:size=12:type=Regular:antialias=true:autohint=true",
				"Symbols Nerd Font:pixelsize=16:type=2048-em:antialias=true:autohint=true"}; // 这里没有换行昂，如果你看到换行了，说明你屏幕和你一样小
```

### 状态栏配置

先安装这个模块需要的包：`sudo pacman -S alsa-utils alsa-firmware`

然后克隆suckless搞的状态栏：`git clone https://git.suckless.org/slstatus`

先进行一些简单的配置吧：

```C
cd slstatus
vim config.h
--------------------------------
static const struct arg args[] = {
/* function format          argument */
// { run_command, " %s|", "uname -r | awk -F \"-\" '{print $1}'" },
{ disk_free, "dsk %s|", "/" },
{ cpu_perc, "cpu %s%%|", NULL },
{ ram_perc, "mem %s%%|", NULL },
{ run_command, "vol %s|", "amixer sget Master | awk -F \"[][]\" '/Left:/{print $2}'" },
{ run_command, "spk %s| ", "amixer sget Capture | awk -F \"[][]\" '/Left:/{print $2}'" },
{ datetime, "%s",           "%b_%d %T" },
};
```

以上进行了一下简单的配置，也可以使用nerd fonts图标来替换上述提到的dsk、cpu、mem、vol等。

然后，重新编译`sudo make clean install` 

设置slstatus自动启动

```shell
vim .xinitrc
---------------------------
# 在exec dwm的上一行添加
exec slstatus
```

重新进入dwm之后生效

### 音量调节

上面状态栏只是能显示音量了（也有可能不能显示，哈哈哈），但是还不能调节音量，接下来进行配置

不管你用vim还是vscode打开dwm/config.h文件，在下面的数组中添加下面几行：然后就可以使用F1进行静音和取消静音了，分别用F2和F3来降低音量和升高音量

```C
static const Key keys[] = {
	/* modifier                     key        function        argument */
    ... ...
    /* 退出dwm win + shfit + c */
	{ MODKEY|ShiftMask,             XK_c,      quit,           {0} },
	/* klelee's volume config */
	{ MODKEY,			XK_F1,	   spawn,	SHCMD("amixer sset Master toggle") },
	{ MODKEY,			XK_F2,	   spawn,	SHCMD("amixer sset Master 5%- ") },
	{ MODKEY,			XK_F3,	   spawn,	SHCMD("amixer sset Master 5%+ ") },
    ... ...
};

```

这部分可能会遇到问题，比如这个时候你的默认音响不对，那么Master就不能使用。我就懒得写在这里面了，遇到问题要多百度，嘿嘿：[amixer: Unable to find simple control 'Master',0](https://blog.csdn.net/weixin_33726313/article/details/91753642)

### 电源管理

我的方案简单粗暴：直接安装xfce4-power-manager

安装：`sudo pacman -S xfce4-power-manager`

自动启动：

```shell
vim .xinitrc
--------------------------------
# 在exec slstatus的上一行添加：
xfce4-power-manager &
```

重启生效，后面可以使用xfce4-power-manager -c来打开它的配置UI，里面的配置大家都认识，不认识的查牛津字典

### 状态条颜色配置

不管你用vim还是vscode打开dwm/config.h文件，找到下面这几行，对着改你喜欢的颜色：[颜色表](https://tool.oschina.net/commons?type=3) <---- 点击挑色

```C
static const char col_gray1[]       = "#9b95c9";    // 状态条底色
static const char col_gray2[]       = "#444444";    // 当static const unsigned int borderpx不为0时，非活动窗口外边框颜色
static const char col_gray3[]       = "#bbbbbb";    // 当前非活动的title字体颜色
static const char col_gray4[]       = "#eeeeee";    // 当前活动的title字体颜色
static const char col_cyan[]        = "#f391a9";    // title底色
static const char *colors[][3]      = {
	/*               fg         bg         border   */
	[SchemeNorm] = { col_gray3, col_gray1, col_gray2 },
	[SchemeSel]  = { col_gray4, col_cyan,  col_cyan  },
};
```

### 截图

安装`flameshot` ，一款神级截图软件

```bash
sudo pacman  -S flameshot
```

不管你用vim还是vscode打开dwm/config.h文件，# 在static const Key keys[] 中添加下面这条，后面的截图使用alt+p
```C
static const Key keys[] = {
	/* modifier                     key        function        argument */
    ... ...
    /* 截屏  需要安装flameshot */
	{ MODKEY1,			XK_p,	   spawn,	SHCMD("flameshot gui") },
	... ...
};

```

### 系统托盘解决方案

dwm的系统托盘需要打补丁来解决，但是目前版本给的补丁直接合进去一般都会报错，不是和不进去就是合进去编译报错。因此你有两种选择：

1. 解决编译报错
2. 手动合入

我选择2

下载补丁：https://dwm.suckless.org/patches/systray/dwm-systray-6.3.diff

diff文件可以用vscode打开，方便复制。对比dwm/config.h 和 dwm/dwm.c 比较差异，然后复制粘贴就可以了。
