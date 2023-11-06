---
title: 配置ubuntu22服务器
date: 2023-11-06 09:33:27
tags:
---

# 初始化配置

## 1. 设置ssh远程登陆

编辑服务器`/etc/ssh/sshd_config`文件
```bash
PermitRootLogin yes
PasswordAuthentication yes
```

服务器重启`sshd`
```bash
systemctl restart sshd
```

添加新用户并添加sudo权限
```bash
sudo adduser <username>
sudo usermod -aG sudo <username>
```

配置防火墙
```bash
sudo ufw enable
sudo ufw allow ssh
```

禁用root登陆
```bash
sudo passwd -l root
```

## 2. 设置ssh免密登陆

客户端生成公钥和密钥到`.ssh`目录下
```bash
ssh-keygen
```

方法一：使用命令行拷贝公钥
```
ssh-copy-id -i ~/.ssh/id_rsa.pub -f root@192.168.235.22
```

方法二：在服务器`/root/.ssh/authorized_keys`文件中手动拷贝公钥

## 3. 更新并安装软件包

更新包管理器，安装必要的软件
```bash
apt update
apt upgrade
apt install -y vim git wget curl zsh
apt install -y gdb
```

## 4. 配置软件

### 4.1 oh-my-zsh
```bash
# 安装oh-my-zsh，会将默认shell改为zsh
sh -c "$(wget https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh -O -)"
```

配置`/root/.zshrc`文件
```bash
plugins=(git z history zsh-autosuggestions
	web-search copyfile dirhistory colored-man-pages colorize
	rand-quote sudo cp)
```

安装插件(zsh-autosuggestions需要额外安装)
```bash
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```
### 4.2 python环境

下载miniconda
```bash
mkdir -p ~/miniconda3
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm -rf ~/miniconda3/miniconda.sh

~/miniconda3/bin/conda init bash
~/miniconda3/bin/conda init zsh

source ~/.zshrc

# 配置登陆不启动conda环境
conda config --set auto_activate_base false
# 恢复
conda config --set auto_activate_base true
```

下载jupyterlab
```bash
conda install jupyterlab
```

配置jupyterlab远程访问
```
# 生成配置文件
jupyter lab --generate-config
```

配置文件`.jupyter/jupyter_lab_config.py`
```
c.ServerApp.ip = '*'
c.ServerApp.allow_remote_access = True
c.ServerApp.port = 8888
```

配置密码登录
```bash
jupyter lab password
```

### 4.3 neovim

下载neovim（源码安装）
```bash
# 安装依赖
sudo apt update
sudo apt install -y git make cmake g++ libtool libtool-bin autoconf automake pkg-config unzip
# 下载源代码
git clone https://github.com/neovim/neovim.git
cd neovim
git checkout v0.9.0

# 编译和安装
make CMAKE_BUILD_TYPE=RelWithDebInfo 
sudo make install

# 移动可执行文件
cd neovim/build/bin/
sudo mv nvim /usr/local/bin/
```

下载nvchad
```bash
# 下载
git clone https://github.com/NvChad/NvChad ~/.config/nvim --depth 1 && nvim
# 更新
NvChadUpdate .
# 卸载
rm -rf ~/.config/nvim
rm -rf ~/.local/share/nvim
```
