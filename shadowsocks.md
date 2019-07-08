@[TOC](目录)
## shadowsocks
### server
有待补充；搭建不是很麻烦，但是买Amazon的服务器有点麻烦，所以我都用别人的；
### windows client
[windows client](https://github.com/shadowsocks/shadowsocks-windows/releases)下载页面，下载Shadowsocks-4.1.6.zip
直接运行即可，可能需要升级 .NET Framework，是自动进行的，升级即可；然后配置；

### linux client
- 安装pip
首先要确保安装了pip，大部分发行版都默认没有安装pip
	```bash
  # 先确认自己安装了python
  # python2.7就行
  python
	
  # 如果没有安装
  sudo apt install python
	
  # 安装pip
  curl "https://bootstrap.pypa.io/get-pip.py" -o "get-pip.py"
  python get-pip.py
  ```
- 安装shadowsocks  
    ```bash
    pip install shadowsocks
    ```
- 配置shadowsocks
    ```bash
  vim /etc/shadowsocks.json
  # 配置文件名字可以自己决定，ss.json
  # 增加以下内容：
  # "method"要匹配自己服务器的设置
  # 直接vim 创建文件会写入失败
  # /etc目录下文件其他用户默认只有读权限，先建文件，再给权限为好
  {
      "server": "XX.XX.XX.XX",
      "server_port": 12345,
      "local_address": "127.0.0.1",
      "local_port": 1080,
      "password":"ur pwd",
      "timeout":300,
      "method": "aes-128-ctr",
      "fast_open": false
  }
  ```
- 启动shadowsocks客户端
    ```bash
    # 后台运行，关闭终端会结束后台进程
    # sslocal -c 你配置文件的路径
    sslocal -c /etc/shadowsocks.json &
    
    # 如果想在关闭终端后继续在后台运行shadowsocks
    # 需要
    nohup sslocal -c /etc/shadowsocks.json &
    ```
    
- 测试shadowsocks是否正常运行
```bash
  # curl --socks5 127.0.0.1:1080 http://httpbin.org/ip
  {
      "origin": "X.X.X.X"   #此处显示shadowsock服务器外网地址即为正常
  }
```
- 安装Privoxy  
Shadowsocks 是一个 socket5 服务，我们需要使用 Privoxy 把流量转到 http/https 上。
```bash
     sudo yum -y install privoxy
```
- 配置Privoxy
    - 配置文件路径：/etc/privoxy/config
    - 确保以下两行未被注释，且forward-socks5t的端口号与shadowsock的local端口一致
        ```bash
        listen-address 127.0.0.1:8118   
        forward-socks5t / 127.0.0.1:1080 .
        ```
        
    - 临时添加环境变量  
    bash里运行
        ```bash
        export http_proxy=http://127.0.0.1:8118
        export https_proxy=http://127.0.0.1:8118
        ```
    - 持久环境变量
    
- 启动
自己写个shell用来打开关闭代理就行
```bash
# 创建一个文件 on.sh
export http_proxy=http://127.0.0.1:8118
export https_proxy=http://127.0.0.1:8118
# 创建一个文件 off.sh
export http_proxy=""
export https_proxy=""

#turn_on_porxy.sh
# 启动privoxy
systemctl start privoxy
# 后台运行shadowsocks client
# 这里会有个问题，在终端工作的时候shadowsocks会输出很多日志
# 可以永久重定向丢弃不输出这些日志
sslocal -c /etc/shadowsocks.json &
source on.sh

#turn_off_porxy.sh
#环境变量取消掉就行，还需要关闭privoxy，ss，下次补上
source off.sh

```
