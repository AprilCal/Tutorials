@[TOC](目录)
# Installation
```bash
# 下载想要的Go版本
# go后面跟的版本号自己根据需要写
# 这个网址就可以下，去官网找也行；
$wget https://studygolang.com/dl/golang/go1.10.1.linux-amd64.tar.gz    
# 解压，解压出来的目录名字就是go
sudo tar -zvxf go1.6.2.linux-amd64.tar.gz
    
# 把解压后的目录移动到这个目录下面
# 这个路径就是GOROOT
sudo mv go /usr/local/go
    
# 设置环境变量
# 有时候sudo vim编辑也会被拒绝
# 可以进入root再编辑，也可以在编辑用户自己的.bashrc
# 我一般直接给普通用户编辑profile的权限
sudo vim /etc/profile
    
# 也可以设置在 ~/.bashrc中，具体位置取决于你自己用的发行版本
export GOROOT=/usr/local/go  #设置为go安装的路径
export GOPATH=$HOME/gocode   #默认安装包的路径
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin
source /etc/profile
    
# 确认是否安装成功
# 如果显示版本号，就成功了，否则就去看看别的教程吧 :P
go version
```
# Note

如果你刚接触Linux，可能会对环境变量的设置有些困惑，这里解释一下；如果还有疑问，应该去查详细的文档；
- **GOROOT**  
就是Go的安装路径，除此之外没有什么深层含义；  
解压后的文件夹移动到了usr/local/下，因此就把GOROOT的值设置成/usr/local/go  
注意设置环境变量的时候，在PATH后面追加了$GOROOT/bin，这样做是为了能够执行go命令以及go工具；  
$GOROOT/bin中就包含了go这个命令的可执行文件；  
Linux中是在PATH中的目录下搜索可执行文件的；

- **GOPATH**  
GOPATH下有三个目录，src、pkg、bin
    - src 放你自己的源码
    - pkg 不知道放啥，也存放一些源码，go.mod里配置的依赖的源码会被保存在这里；
    export GO111MODULE=on，就可以用go.mod管理依赖了，但是最好别，尤其是项目被分成多个工程的时候；
    - bin 放可执行文件

- **go install**  
执行go install之后，可执行文件会直接被写到 $ GOPATH/bin中，可以把$GOPATH/bin追加到PATH变量中，这样install后的工程就可以直接以命令的形式运行；
- **go build**  
执行go build之后，可执行文件只会被写到工程目录下；执行时要在工程目录下./ur_program
