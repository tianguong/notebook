#### 1. 基础
> 语言选英语，键盘选Chinese
``` bash
sudo apt update
sudo apt upgrade
sudo snap refresh
```
设置项 choice language chinese
``` bash
如果是虚拟机，全屏和与主机共享剪切板
sudo apt install open-vm-tools-desktop
```
#### 2. zsh
``` bash
sudo apt install wget curl git -y
sudo apt install zsh
sudo usermod -s $(which zsh) 用户名
chsh -s $(which zsh)
```
这时，终端没什么变化，注销当前用户重新登入，打开终端，选择2，风格变为zsh

``` bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
# 或者
sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
#外观再次发生变化

ls -1 $HOME/.oh-my-zsh/themes/

cd ~/.oh-my-zsh/custom/plugins
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git
git clone https://github.com/zsh-users/zsh-autosuggestions 
vim $HOME/.zshrc
```
``` ini
plugins=(
git
zsh-autosuggestions
zsh-syntax-highlighting
) 
```
- 应用
``` bash
source ~/.zshrc
```
#### 3. 挂盘
虚拟机共享目录挂盘
``` bash
查看共享目录：vmware-hgfsclient
查看目录：ls /mnt
创建目录：sudo mkdir /mnt/hgfs 
sudo vi /etc/fstab
.host:/   /mnt/hgfs/  fuse.vmhgfs-fuse   allow_other  0   0
systemctl daemon-reload
ls /mnt/hgfs/
```
