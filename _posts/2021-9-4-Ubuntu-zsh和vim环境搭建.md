---
layout:     post
title:      Ubuntu zsh 和 vim 自用环境搭建
subtitle:   Ubuntu自用环境搭
date:       2021-9-4
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

| Method | Command                                                                                           |
| ------ | ------------------------------------------------------------------------------------------------- |
| **curl**   | `sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`  |
| **wget**   | `sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`   |
| **fetch**  | `sh -c "$(fetch -o - https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"` |

安装完成之后在提示界面把默认终端切换为 `zsh`

#### 替换配置文件
从蓝奏云下载 `zsh` 配置文件替换默认配置文件
[下载地址](https://xuezhong.lanzoue.com/ixk7d0fy71ad)

#### 配置 gitconfig 
```sh
git config --global url."https://ghproxy.com/https://github.com/".insteadOf https://github.com/
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
> 注: 在执行操作之前最好设置一下 `git`
> `git` 默认是 `https` 的下载方式，但是 `https` 经常会下载失败，
> 所以把 `git` 配置为默认使用 `git` 的下载方式
```sh
git config --global url."git://github.com".insteadOf https://github.com
# 上面的应该够用了，还是别把全部的 git 地址都换成 git 比较好，要是没权限就下不了了
# git config --global url."git://".insteadOf https://
```

```sh
vim ~/.config/nvim/init.vim
```
在打开的 `vim` 界面中，冒号+`PlugInstall`
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

4. 安装 `ctags` 和 `gtags`  
- `ctags`
```sh
sudo apt install exuberant-ctags
```

- `gtags`
    - 依赖环境
        ```sh
        sudo apt build-dep global
        sudo apt install libncurses5-dev libncursesw5-dev
        ```
    - 下载 `tar.gz` 包
    - `wget https://ftp.gnu.org/pub/gnu/global/global-6.6.7.tar.gz`
    - https://ftp.gnu.org/pub/gnu/global/

    - 编译 & 安装
        ```sh
        ./configure --with-sqlite3   # gtags可以使用Sqlite3作为数据库, 可   加可不加
        make
        make install # 权限不够的话加 sudo
        ```

5. nodejs 安装
nodejs 软件源
NodeSource 软件源提供了以下版本：
- v14.x - 最新稳定版
- v13.x
- v12.x - 最新长期版本
- v10.x - 前一个长期版本  
以 `14.x` 为例，
```sh
curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
```
更多方式请参考： [https://developer.aliyun.com/article/760687](https://developer.aliyun.com/article/760687)


