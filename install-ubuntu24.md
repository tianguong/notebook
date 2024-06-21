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

#### 4. 安装neovim
``` bash
# 新版本好像不需要添加ppa了
sudo add-apt-repository ppa:neovim-ppa/stable
sudo apt update
sudo apt install neovim
```

#### 4. 安装nodejs
```bash
sudo apt-get install nodejs
sudo apt install npm
```

#### 5. 安装golang及相关环境
``` bash
wget https://golang.google.cn/dl/go1.20.14.linux-amd64.tar.gz
sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.20.14.linux-amd64.tar.gz
vim ~/.zshrc
```
- 添加golang路径
``` ini
export PATH=$PATH:/usr/local/go/bin
```
##### 5.1 golang多版本管理
- https://github.com/voidint/g
``` bash
curl -sSL https://raw.githubusercontent.com/voidint/g/master/install.sh | bash
echo "unalias g" >> ~/.zshrc # Optional. If other programs (such as `git`) have used `g` as an alias.
source "$HOME/.g/env"

# 列出所有可供安装的 Go 版本
g ls-remote

# 查询当前可用的稳定版
g ls-remote stable

# 安装特定版本的 Go
g install 1.14.7

# 查询已安装的 Go 版本列表
g ls

# 切换到另一个已安装的 Go 版本
g use 1.19.10

#　卸载特定已安装的 Go 版本：
g uninstall 1.19.10

＃ 清除 Go 安装的包文件缓存
g clean

g version
g self update
g self uninstall

```
- gopls多版本支持 [https://github.com/golang/tools/blob/master/gopls/README.md#supported-go-versions]
``` 
Go 1.12	gopls@v0.7.5
Go 1.15	gopls@v0.9.5
Go 1.17	gopls@v0.11.0
Go 1.18	gopls@v0.14.2
Go 1.20	gopls@v0.15.3
```
coc-setting.json gopls配置
- goplsPath需要指向按照地点，coc自身总是安装最新的，容易和golang版本不兼容
``` json
"languageserver": {    
    "golang": {    
        "command": "gopls",    
        "rootPatterns": [    
            "go.mod",    
            ".vim/",    
            ".git/",    
            ".hg/"    
        ],    
        "trace.server": "verbose",    
        "filetypes": [    
            "go"    
        ],    
        "initializationOptions": {    
            "usePlaceholders": true    
        }    
    }    
},    
"go.goplsOptions": {    
    "completeUnimported": true    
},    
"go.goplsPath":"/home/ng/go/bin/gopls",    
"go.delveConfig": {    
    "dlvLoadConfig": {    
        "maxStringLen": 1024,    
        "maxArrayValues": 1024    
    }    
},    
```

##### 5.2 安装gotests、gopls、delve
``` bash
go env -w GOPROXY=https://goproxy.cn,direct
go install github.com/cweill/gotests/gotests@latest
go install golang.org/x/tools/gopls@latest
# go1.21之前要按照下面版本
# go install golang.org/x/tools/gopls@v0.15.3

go install github.com/go-delve/delve/cmd/dlv@latest
# go1.17.13可以按照下面版本dlv
go install github.com/go-delve/delve/cmd/dlv@v1.20.2
```

##### 5.3 安装ctags
- ###### Plug `preservim/tagbar`
> 大纲视图
[源码安装ctags](http://ctags.sourceforge.net/) 或 [universal-ctags(推荐)](https://github.com/universal-ctags/ctags)

```bash
sudo snap install universal-ctags
/snap/bin/ctags
# mac
brew install ctags

# 或 universal-ctags(推荐)
git clone https://github.com/universal-ctags/ctags.git
cd ctags
./autogen.sh
# 若检测到没有相关库，则添加相关库： apt install autoconf automake pkg-config build-essential make
./configure --prefix=/usr/local/ctags  # defaults to /usr/local
make && make install # may require extra privileges depending on where to install
sudo vi /etc/bash.bashrc
# 最后添加
export PATH=/usr/local/ctags/bin:$PATH
```

[Tagbar](https://github.com/preservim/tagbar)
[TagbarWiki](https://github.com/preservim/tagbar/wiki)
[gotags](https://github.com/jstemmer/gotags)

``` ini
let g:tagbar_type_go = {
  \ 'ctagstype' : 'go',
  \ 'kinds'     : [
      \ 'p:package',
      \ 'i:imports:1',
      \ 'c:constants',
      \ 'v:variables',
      \ 't:types',
      \ 'n:interfaces',
      \ 'w:fields',
      \ 'e:embedded',
      \ 'm:methods',
      \ 'r:constructor',
      \ 'f:functions'
  \ ],
  \ 'sro' : '.',
  \ 'kind2scope' : {
      \ 't' : 'ctype',
      \ 'n' : 'ntype'
  \ },
  \ 'scope2kind' : {
      \ 'ctype' : 't',
      \ 'ntype' : 'n'
  \ },
  \ 'ctagsbin'  : 'gotags',
  \ 'ctagsargs' : '-sort -silent'
\ }
```

#### 6. 安装Ag、Rg
``` bash
# [What's so great about Ag?](https://github.com/ggreer/the_silver_searcher)
sudo apt install silversearcher-ag

# [fzf requires ripgrep (rg)](https://github.com/BurntSushi/ripgrep)
sudo apt install ripgrep
```

#### 7. 安装vim-plug及配置
``` bash
#　mkdir -p ~/.config/nvim
cd ~/.config
git clone https://github.com/tianguong/nvim_lua.git nvim
sh -c 'curl -fLo "${XDG_DATA_HOME:-$HOME/.local/share}"/nvim/site/autoload/plug.vim --create-dirs \
       https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim'
:PlugInstall
:PlugUpdate
:PlugUpgrade
:GoUpdateBinaries

# coc-go, coc-json, coc-lua, coc-git, coc-snippets
:CocInstall coc-go
:CocInstall coc-pyright
```
