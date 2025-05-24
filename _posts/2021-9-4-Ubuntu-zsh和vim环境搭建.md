---
layout:     post
title:      Ubuntu zsh 和 vim 自用环境搭建
subtitle:   Ubuntu自用环境搭
date:       2025-05-24
author:     snow
header-img: https://pic2.zhimg.com/v2-77b1f0ae60ade65252babc452ac0ad71_r.jpg
catalog: true
tags:
    - Blog utils
---

[toc]

## zsh 终端环境

#### 安装 `zsh`
```sh
# 安装zsh
sudo apt install zsh

# 切换终端到 zsh，此步通常不需要，oh-my-zsh 安装后会提示切换默认终端
chsh -s /bin/zsh
```

#### 安装 `oh-my-zsh`
`zsh` 安装完毕之后安装 `oh-my-zsh`
`oh-my-zsh` 有三种安装方式

`oh-my-zsh` github 地址 [omz](https://github.com/ohmyzsh/ohmyzsh)

| Method    | Command                                                                                           |
| --------- | ------------------------------------------------------------------------------------------------- |
| **curl**  | `sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"` |
| **wget**  | `sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`   |
| **fetch** | `sh -c "$(fetch -o - https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"` |

如果github访问不顺利，可以使用如下命令： 

| Method | Command                                           |
| ------ | ------------------------------------------------- |
| curl   | `sh -c "$(curl -fsSL https://install.ohmyz.sh/)"` |
| wget   | `sh -c "$(wget -O- https://install.ohmyz.sh/)"`   |
| fetch  | `sh -c "$(fetch -o - https://install.ohmyz.sh/)"` |


安装完成之后在提示界面把默认终端切换为 `zsh`

#### 替换配置文件
从蓝奏云下载 `zsh` 配置文件替换默认配置文件
[下载地址](https://xuezhong.lanzoue.com/ixk7d0fy71ad)

#### 配置 gitconfig 
```sh
git config --global url."https://ghfast.top/https://github.com/".insteadOf https://github.com/
```

#### 常用 `oh-my-zsh` 插件
1. **zsh-syntax-highlighting**  [地址](https://github.com/zsh-users/zsh-syntax-highlighting)
```sh
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```
2. **zsh-autosuggestions**  [地址](https://github.com/zsh-users/zsh-autosuggestions)
```sh
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

3. 可选的 `pokemonsay`

[pokemonsay github 主页](https://github.com/possatti/pokemonsay)


`zsh` 环境到此安装完毕

## vim 环境安装
#### 下载 `neovim`
首先下载 `neovim`，以下是 `neovim` 主页下载链接
[neovim github page](https://github.com/neovim/neovim)
[neovim github release page](https://github.com/neovim/neovim/releases)

Linux (x64)
1. Download nvim.appimage
2. Run `chmod u+x nvim.appimage && ./nvim.appimage`
    - If your system does not have FUSE you can extract the appimage:
        ```sh
        ./nvim.appimage --appimage-extract
        ./squashfs-root/usr/bin/nvim
        ```

#### 安装 `neovim`
`neovim` 不需要额外安装，
1. 给予可执行权限
```sh
chmod u+x nvim.appimage
```
2. 重命名以及移动到系统路径
```sh
mv nvim.appimage /usr/local/bin/nvim
```
3. 设置 `vi`, `vim` 的别名软链接
注意此处 `target` 也就是 `/usr/local/bin/nvim` 必须为完整绝对路径，否则可能查找失败 
```sh
sudo ln -s /usr/local/bin/nvim /usr/bin/vi
sudo ln -s /usr/local/bin/nvim /usr/bin/vim
```

#### vim 配置文件设置
1. 下载 vim 配置文件并移动到 `nvim` 配置文件位置

```sh
git clone https://gitee.com/light_snow/vim-relation.git

cd vim-relation

mv nvim ~/.config/nvim
```

2. 安装 `vim-plug`  
    - **vim** 环境
        ```sh
        curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
        ```
    - `neovim` 环境
        ```sh
        sh -c 'curl -fLo "${XDG_DATA_HOME:-$HOME/.local/share}"/nvim/site/autoload/plug.vim --create-dirs \
       https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim'
        ```
其他环境安装请访问 `vim-plug` 主页，
[vim-plug github](https://github.com/junegunn/vim-plug)

3. 安装 `nerd font`，支持完整图标显示，下载完成后记得切换 `terminal` 字体
一般下载 `JetBrainsMono Nerd Font`  
[nerd font download page](https://www.nerdfonts.com/font-downloads)  
[蓝奏云地址](https://xuezhong.lanzoui.com/inBQEvnhroh)

4. 安装 `vim` 插件  
```sh
vim ~/.config/nvim/init.vim
```
然后执行vim指令
```sh
:PlugInstall
```
进行 `init.vim` 文件中已定义插件的安装

## 补充
1. 如果安装 `cgdb`，需要安装 `texinfo`  
```sh
sudo apt install texinfo
```

2. 安装 `ag`  
```sh
sudo apt install silversearcher-ag
```

3. `neovim` 添加 `python3` 支持  
```sh
sudo apt install python3-pip
pip3 install pynvim
```

3.1 pip3 设置源 
```sh
mkdir ~/.pip
vim ~/.pip/pip.conf
# 内容如下:

[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
[install]
trusted-host = https://pypi.tuna.tsinghua.edu.cn
```

4. nodejs 安装
nodejs 软件源，github 地址 https://github.com/nodesource/distributions
以 `14.x` 为例，
```sh
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo bash - 
sudo apt-get install -y nodejs
```

nodejs 设置淘宝源
```sh
npm config set registry https://registry.npm.taobao.org
```
更多方式请参考： [https://developer.aliyun.com/article/760687](https://developer.aliyun.com/article/760687)


