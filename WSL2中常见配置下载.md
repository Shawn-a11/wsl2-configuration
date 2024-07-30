# WSL2中常见配置下载

## 代理配置：

- IP必须在windows下设ipconfig查看IPV4的IP地址 端口需要在设置中的代理查看or在vpn界面查看

- # 开启全局代理 关闭防火墙

### 先配置Proxychains4

- 使用apt进行安装

```
sudo apt install proxychains4
```

- 配置：对/etc/proxychains4.conf文件进行修改

```
sudo gedit /etc/proxychains4.conf

//打开文件
sudo nano  /etc/proxychains4.conf
```

<img src="https://pic.imgdb.cn/item/66a3b553d9c307b7e9beff2f.png" style="zoom:80%;" /><img src="https://pic.imgdb.cn/item/66a3b5c9d9c307b7e9bf71f6.png" style="zoom:80%;" />

- 代理核心原则见上文

### 一键脚本配置命令

```
#!/bin/bash
host_ip=$(cat /etc/resolv.conf |grep "nameserver" |cut -f 2 -d " ")
export ALL_PROXY="http://$host_ip:duan"
```

# 安装前添加命令proxychains4即可

### 设置环境变量

- 打开 `~/.bashrc` 或 `~/.zshrc` 文件：

```
nano ~/.bashrc
nano ~/.zshrc
```

- 添加以下行以配置 HTTP 和 HTTPS 代理

```
export http_proxy=http://192.168.1.100:8080
export https_proxy=https://192.168.1.100:8080
export no_proxy="localhost,127.0.0.1,::1"
//假设你的地址为192.168.1.100 vpn的端口为8080
//其中127.0.0。1为IPv4 的环回地址
//其中：：1为IPv6的还回地址
可以使用 ifconfig io命令来查看网络接口的详细信息
```

- 保存并退出（在 nano 中，按 `Ctrl+X`，然后按 `Y` 保存并退出）

- 重新加载配置文件

```
source ~/.bashrc
source ~/.zshrc
```

### 配置 Apt 包管理器

- 创建或编辑 apt 配置文件 `/etc/apt/apt.conf.d/proxy.conf`

```
sudo nano /etc/apt/apt.conf.d/proxy.conf
```

- 添加内容

```
Acquire::http::Proxy "http://your_proxy_address:your_proxy_port";
Acquire::https::Proxy "https://your_proxy_address:your_proxy_port";
```

- 保存并退出（在 nano 中，按 `Ctrl+X`，然后按 `Y` 保存并退出or Ctrl+o 和 ctrl X）

### 配置代理

- 配置 Git 的 HTTP 和 HTTPS 代理

```
git config --global http.proxy http://your_proxy_address:your_proxy_port
git config --global https.proxy https://your_proxy_address:your_proxy_port
```

### 验证代理配置

```
//验证环境变量
echo $http_proxy
echo $https_proxy

//验证 apt 代理配置
sudo apt update

//验证git代理配置
git config --global --get http.proxy
git config --global --get https.proxy
```

### 取消代理配置

- 删除或注释掉 `~/.bashrc` 或 `~/.zshrc` 中的代理设置行，然后重新加载配置文件

```
source ~/.bashrc
source ~/.zshrc
```

- 删除 `/etc/apt/apt.conf.d/proxy.conf` 文件中的代理配置
- 取消 Git 代理配置

```
git config --global --unset http.proxy
git config --global --unset https.proxy
```

## NVM下载：

- **下载并安装 nvm**:

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash // 下载nvm
```

- **加载 nvm**：

```
source ~/.bashrc
```

- **验证 nvm 安装**：

```
nvm --version
node -v
```

- **使用 nvm 安装 Node.js**：

```
nvm install 14
```

## Mamba安装：

- **使用 Conda 安装 Mamba**：

```
conda install mamba -n base -c conda-forge
```

- **使用 Mambaforge 安装 Mamba：**

Mambaforge 是一个包含 Mamba 的 Conda 发行版

```
wget https://github.com/conda-forge/miniforge/releases/latest/download/Mambaforge-Linux-x86_64.sh

bash Mambaforge-Linux-x86_64.sh
```

- **使用 Micromamba 安装 Mamba**：

Micromamba 是 Mamba 的一个轻量级版本，适合快速安装和使用

```
curl micro.mamba.pm/install.sh | bash
export PATH=~/.local/bin:$PATH//在.bashrc文件中进行编辑 添加到末尾
source ~/.bashrc//使更改生效
```

- **验证安装**

```
mamba --version
```

- **使用Mamba**

```
mamba create -n name(可换) python=3.8
mamba activate name(可换)
mamba install xxx
```

### Rust安装

- 终端输入

```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

- 编辑相应的配置文件

```
nano ~/.bashrc
nano ~/.zshrc
```

- 添加环境变量

```
export PATH="$HOME/.cargo/bin:$PATH"
```

- 重新加载配置文件

```
source ~/.bashrc
source ~/.zshrc
```

- 验证

```
rustc --version
```

### 安装curl和Hombrew命令

### Curl：

- 更新你的软件包列表

```
sudo apt update
```

- 安装 `curl`

```
sudo apt install curl
```

- 检验版本

```
curl --version
```

### 安装homebrew

- 安装必要的依赖

```
sudo apt-get install build-essential procps curl file git
```

- 安装 Homebrew

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

- 添加 Homebrew 到你的 PATH

```
//配置环境变量
eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"

//为使更改立即生效，运行
source ~/.bashrc
# 或者如果你使用的是zsh
source ~/.zshrc

//验证 Homebrew 是否安装成功
brew --version
```

## Zsh+Ohmyzsh+Fzf editor

- **安装Zsh**

```
sudo apt update
sudo apt install zsh //在 Debian/Ubuntu 系统
sudo dnf install zsh //在 Fedora 系统
brew install zsh //在 macOS 系统上（使用 Homebrew）
```

- **将 `zsh` 设置为默认的 Shell**

```
chsh -s $(which zsh)
//你需要重新登录终端以应用更改
```

- **安装 `oh-my-zsh`**

```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

- **安装 `fzf`**

```
sudo apt install fzf //在 Debian/Ubuntu 系统
sudo dnf install fzf //在 Fedora 系统
brew install fzf //在 macOS 系统上（使用 Homebrew）
```

- **配置 `oh-my-zsh` 和 `fzf`**

```
nano ~/.zshrc
//在文件末尾添加
# 启用 fzf
[ -f ~/.fzf.zsh ] && source ~/.fzf.zsh

# 设置 oh-my-zsh 的主题和插件
ZSH_THEME="agnoster" # 选择你喜欢的主题
plugins=(git fzf)    # 添加 fzf 插件

source $ZSH/oh-my-zsh.sh
//保存并退出编辑器，然后重新加载 .zshrc 文件以应用更改：
source ~/.zshrc
```

### FZF editor：

- **查找文件**

```
fzf //启动fzf editor
```

- **查找命令历史记录**

```
history | fzf
或者你可以使用 Ctrl+R 直接调用 fzf 查找命令历史记录，如果你已经在 ~/.zshrc 中启用了 fzf 插件：
```

- **查找并打开文件**

```
//使用 find 命令和 fzf 结合，可以查找并打开文件
find . -type f | fzf | xargs -r less
```

- **查找 Git 提交记录**

```
git log --oneline | fzf
```

- **查找并切换 Git 分支**

```
git branch | fzf | xargs -r git checkout
```

- **查找并打开目录**

```
fd -t d | fzf | xargs -r cd
```

- **在 Vim 中使用 fzf**

```
:FZF
```

- **快捷键**
  - `Ctrl+J` / `Ctrl+N`: 向下移动
  - `Ctrl+J` / `Ctrl+N`: 向下移动
  - `Enter`: 选择
  - `Esc` / `Ctrl+C`: 取消

## Zsh安装之后base环境不见了：

- **初始化 Conda**

运行以下命令来初始化 Conda：

```
conda init zsh
```

<img src="https://pic.imgdb.cn/item/66a21669d9c307b7e949febb.png" style="zoom:80%;" />

- **重新加载配置文件**

初始化完成后，你需要重新加载 Zsh 配置文件以使更改生效：

```
source ~/.zshrc
```

- **激活 Conda 环境**

```
conda activate base
```

## 安装Zsh之后无法找到原有Bash配置 换回zsh的操作同上

- **查找原来 Shell 的路径**

```
which bash //拿到bash的路径
```

- **使用 `chsh` 命令更改默认 Shell**

```
chsh -s /bin/bash //使用 chsh 命令并提供 Bash 的路径
chsh -s /usr/bin/bash
```

- **重新登陆即可**

## 删除现有的Wsl2环境

- 打开 PowerShell 以管理员身份运行
- 注销并删除 Ubuntu 发行版

```
wsl --unregister Ubuntu
```

- 确认删除

```
wsl --list --verbose
```

# 重新配置WSL2

## 在管理员终端上运行指令 下载环境

```
wsl --list --online //查看所支持的wsl系统
wsl --install -d name //指定安装的wsl版本(默认是ubuntu)
```

## 在管理员终端上更新包

```
sudo apt upgrade & sudo apt upgrade
```

## 安装wget

```
sudo apt update
sudo apt install wget -y
```

## 安装Zsh和ohmyzsh

```
//下载zsh
sudo apt install zsh -y

// 安装ohmyzsh
wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | zsh || true
#也可以换清华源
git clone https://mirrors.tuna.tsinghua.edu.cn/git/ohmyzsh.git
cd ohmyzsh/tools
REMOTE=https://mirrors.tuna.tsinghua.edu.cn/git/ohmyzsh.git sh install.sh

git -C $ZSH remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/ohmyzsh.git
git -C $ZSH pull

#安装命令补全和高亮插件
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ~/.oh-my-zsh/plugins/zsh-syntax-highlighting

git clone https://github.com/zsh-users/zsh-autosuggestions ~/.oh-my-zsh/plugins/zsh-autosuggestions
sed -i 's/plugins=(git)/plugins=(git zsh-autosuggestions zsh-syntax-highlighting)/g' ~/.zshrc

# 将 zsh 设置为默认的shell
chsh -s /bin/zsh
```

## 配置Git

```
//启动大小写敏感
git config --global core.ignorecase false

# 配置用户名和密码
git config --global user.name "Your Name"
git config --global user.email "youremail@domain.com"


```

#### 生成ssh-key

```
ssh-keygen

# 用 Github 的话，可以拷贝生成的公钥到 https://github.com/settings/keys
```

<img src="https://pic.imgdb.cn/item/66a863eed9c307b7e9c0b84f.png" style="zoom:80%;" />

- 验证SSH Key是否有效

```
ssh -T git@github.com

看到信息：Hi 用户名! You’ve successfully authenticated；说明配置成功
```

- 下载GitHub私有仓库里创建的项目至Linux服务器

```
git clone git@github.com:****/****.git
```

## 安装n/node/npm

```
// 安装n
# 安装 n
curl -fsSL https://raw.githubusercontent.com/tj/n/master/bin/n | sudo bash -s lts
sudo npm install -g n

//常用的 Node 工具，比如yarn、http-server(静态服务器)、figlet-cli
常用的 Node 工具，比如yarn、http-server(静态服务器)、figlet-clisudo npm install -g yarn http-server figlet-cli

vim .zshrc

# 编辑zsh配置文件，在最后添加：figlet "Hi, arthur"
source .zshrc
```

### 常见报错以及解决方法

- 清除缓存命令

```
npm cache clean -f
```

- NPM ERR！如何解决

```
关闭防火墙
npm config set registry https://registry.npmjs.org/
重新下载即可
```

## Windows11的WSL2上搭建Pytorch

### 安装GPU on wsl2驱动包 

https://developer.nvidia.com/cuda/wsl

<img src="https://pic.imgdb.cn/item/66a37925d9c307b7e98594fe.png" style="zoom:80%;" />

```
// 如果这一步你没有成功，请不要往下进行，直至成功为止。
(sd_dev) jianjianjianjian@DESKTOP-N2B3HNP:~$ nvcc -V
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2022 NVIDIA Corporation
Built on Tue_May__3_18:49:52_PDT_2022
Cuda compilation tools, release 11.7, V11.7.64
Build cuda_11.7.r11.7/compiler.31294372_0
```

### 安装Miniconda

- Miniconda installer = Python + conda （即内置了 Python 安装包，非常方便）
- Anaconda installer = Python + conda + meta package anaconda 大概 1500 个内置包，3G 空间
- 查看电脑显卡版本信息(驱动版本、记住CUDA版本)

<img src="https://pic.imgdb.cn/item/66a379a8d9c307b7e985f97a.png" style="zoom:80%;" />

```
//安装miniconda
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh

# 将 Miniconda 路径添加到 .zshrc
echo 'export PATH="$HOME/miniconda3/bin:$PATH"' >> ~/.zshrc

# 重新加载 .zshrc
source ~/.zshrc

//检查是否安装成功
conda -V
```

- 当你运行安装脚本时，脚本会要求你阅读并同意 Miniconda 的许可证。通常会显示一长段文本并提示你按 `ENTER` 键继续阅读，或者按 `q` 键退出阅读。

  - 在终端中，可能会看到类似以下的提示

  - ```
    Do you accept the license terms? [yes|no]
    ```

- 如果你希望直接跳过license

  - ```
    可以通过管道命令来自动输入 yes
    bash Miniconda3-latest-Linux-x86_64.sh -b
    -b 选项表示批处理模式，将自动接受许可证并使用默认设置进行安装
    ```

#### 创建虚拟环境

```
conda create -n name python=3.10
# -n 后面是虚拟环境的名称

conda activate name
# 激活你创建的环境
```

#### 安装框架、库及依赖

- 安装pytorch torchvision cudatoolkit

- 注意：cudatoolkit=11.7.0 安装时候需要切换到-c conda-forge这个目录，否则无法安装

  - cudatoolkit、pytorch、python这三个版本决定你能否使用GPU。因为conda这个依赖管理工具很呆，如果他找不到gpu版本，他就会自动帮你下载cpu版本~~~~~

  - ```
    conda install pytorch torchvision torchaudio cudatoolkit=11.7 -c pytorch -c conda-forge -c pytorch-lts -c nvidia
    
    
    conda install -c conda-forge cudatoolkit-dev
    ```

- 安装完成后进行检验

```
Python 3.10.9 (main, Mar  8 2023, 10:47:38) [GCC 11.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import torch
>>> torch.__version__
'1.13.1'
>>> torch.cuda.is_available()
True
>>>
如果返回的是false。可以使用 `conda list` 查看自己安装的是CPU版本的pytorch
```

#### 其他命令

```
# 查看已有虚拟环境
conda info --envs

# 在命令行中切换到想要的虚拟环境，我这里切换到sd_dev
conda activate sd_dev
```

### 安装Neovim+Astronvim

- 使用仓库更新

```
//卸载之前的包
sudo apt remove neovim

//安装最新的Neovim
wget https://github.com/neovim/neovim/releases/download/stable/nvim-linux64.tar.gz

//解压下载的文件
tar xzvf nvim-linux64.tar.gz

//得到初始路径
./nvim-linux64/bin/nvim

//清理压缩包文件
rm -rf nvim-linux64.tar.gz

//配置环境变量
nano ~/.zshrc
export PATH=$PATH:~/nvim-linux64/bin
source ~/.zshrc

//配置完成之后 启动nvim
nvim
```

### 下载tmux

- 更新你的包管理器

```
sudo apt update
```

- 安装 `tmux`

```
sudo apt install tmux
```

- 验证安装

```
tmux -V
```

### 安装Tree

- 安装指令

```
//Debian/Ubuntu
sudo apt-get install tree -y
//Mac
brew install tree
```

#### 使用参数

- **只罗列目录**

```
tree -d
```

- **包含隐藏文件（`.` 开头的文件）**

```
 tree -a
```

- **限制层级**

```
tree -L 2
```

- **更多操作请使用命令**：

```
man tree
```

### 配置Proxychains4

- 安装

```
sudo apt install proxychains4
```

- 配置：对/etc/proxychains4.conf文件进行修改

```
sudo gedit /etc/proxychains4.conf
```

- 打开文件：

```
sudo nano /etc/proxychains4.conf
```

<img src="https://pic.imgdb.cn/item/66a3b553d9c307b7e9beff2f.png" style="zoom:80%;" />

<img src="https://pic.imgdb.cn/item/66a3b5c9d9c307b7e9bf71f6.png" style="zoom:80%;" />

### 删除Conda环境

- 激活环境

```
conda activate your_environment_name
```

- 查看所有环境

```
conda env list
```

- 删除环境

```
conda env remove -n your_environment_name
mamba env remove -n your_environment_name
```

### 强力删除文件

- 使用 `rm` 命令与 `-f` 选项

```
rm -f filename
```

- 使用 `sudo` 来提升权限

```
sudo rm -f filename
```

### 删除WSL2环境

- 打开终端

```
wsl -l -v

wsl --list --verbose
```

- 注销并删除特定发行版

```
wsl --unregister Ubuntu
```

