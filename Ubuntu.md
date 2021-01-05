Sen's Server 搭建(ubuntu 18.04)
===

**一、登录服务器**

```
ssh <username>@<IP>
```

1.1 ssh key 登录
```bash
# 在腾讯云下载 ssh 公钥文件
chmod 400 <公钥文件路径>
# 绑定公钥(如果失败，先检查一下 IP 是否已被添加过)
ssh -i <公钥文件路径> <username>@<IP>
```

1.2 密码登录
```bash
ssh ubuntu@<IP>
```


**二、新建用户**

1. 添加用户并设置密码
```bash
# 方式 1
sudo adduser sen # 推荐
# 方式 2
sudo useradd sen
sudo passwd sen
```

2. 查找 sudosers 文件路径并赋予可写权限
```bash
whereis sudoers
chmod -v u+w /etc/sudoers
```

3. 修改 sudoers 文件
```bash
sudo vim /etc/sudoers
## Allow root to run any commands anywhere
root ALL=(ALL) ALL
sen ALL=(ALL) ALL # 在此新加
:wq! #保存
```

4. 收回权限
```bash
chmod -v u-w /etc/sudoers
```

**三、设置心跳**

```bash
sudo vim /etc/ssh/ssh_config
# 随便找个地方添加
ServerAliveInterval 30 # 30 秒发一次心跳
ServerAliveCountMax 6 # 超过 6 此则断开
sudo service sshd restart # 重启服务
```

**四、apt 源换成阿里源**

```bash
sudo vim /etc/apt/sources.list
# 注释原来的 然后添加以下内容
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
# 更新
sudo apt-get update
sudo apt-get upgrade
```

**五、通过 [nvm](https://github.com/nvm-sh/nvm)** 安装 nodejs

1. 安装
```bash
# curl
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash
# wget
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash
```

2. 在当前使用的 bash 配置文件(~/.bashrc / ~/.zshrc)内加入
```bash
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
```

3. 命令相关
```bash
# 查看本地已安装的 node 版本
nvm ls
# 查看远程可安装的 node 版本
nvm ls-remote
# 安装指定版本的 node
nvm install v12.18.2
# 删除指定版本的 node
nvm uninstall v12.18.2
# 使用指定版本的 node
nvm use v12.18.2
# 更换 node 源为淘宝源
vim ~/.zshrc
export NVM_NODEJS_ORG_MIRROR=http://npm.taobao.org/mirrors/node # 在后面添加 NVM_NODEJS_ORG_MIRROR 环境变量
source ~/.zshrc
echo $NVM_NODEJS_ORG_MIRROR # 检查是否生效
```

**六、安装 docker相关**

1. 安装 docker
```bash
sudo apt-get update
# 安装依赖
sudo apt-get -y install curl apt-transport-https ca-certificates software-properties-common
# 添加 key
curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
# 设置为稳定版
sudo add-apt-repository "deb [arch=amd64] https://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
# 安装社区版
sudo apt-get -y install docker-ce
# 测试是否安装成功
sudo docker run --rm hello-world
# -----换源-----
sudo vim /etc/docker/daemon.json # 没有就创建
# 添加
{
  "registry-mirrors": [
      "https://81i5bbb5.mirror.aliyuncs.com"
  ]
}
# 重启服务
sudo systemctl restart docker
# -----免 sudo 执行 docker-----
# 先查看是否已创建 docker 用户组
cat /etc/group | grep docker
# 如果没有
sudo groupadd docker
# 添加用户到 docker 组
sudo usermod -aG docker <用户名>
# 查看是否添加
cat /etc/group
# 重启 docker
sudo systemctl restart docker
# 给 docker.sock 添加权限
sudo chmod a+rw /var/run/docker.sock
```

2. 安装 docker-compose

2.1 安装步骤
```bash
# 获取稳定版
sudo curl -L "https://github.com/docker/compose/releases/download/1.26.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
# 如果太慢就换加速源
sudo curl -L "https://get.daocloud.io/docker/compose/releases/download/1.26.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
# 添加可执行权限
sudo chmod +x /usr/local/bin/docker-compose
```

2.2 docker-compose 相关命令
```bash
```


3. 相关命令

```bash
# 查看正在运行的镜像
docker ps
# 查看所有镜像(包括未运行的)
docker ps -a
# 删除镜像
docker rm CONTAINER
# 创建容器
# 1. 创建后运行一个命令
docker run -it IMAGE ...
# 1.1 创建后运行 且 关闭后删除
docker run -it --rm IMAGE /bin/bash
# 2. 创建后不启动
docker create IMAGE ...
# 启动/停止/重启
docker start CONTAINER
docker stop CONTAINER
docker restart CONTAINER
# 在运行的容器中执行命令
docker exec -it CONTAINER ...
# 查看端口映射情况(需要在运行情况下)
docker container port CONTAINER
# 端口映射说明(本地的 8000 端口映射到容器的 80 端口)
-p 8000:80
# 端口映射说明(容器的 27017 端口映射到本地的 27018 端口)
27017/tcp -> 0.0.0.0:27018
# 查看已下载的镜像
docker images
docker image ls
# 删除已下载的镜像
docker image rm IMAGE
docker rmi IMAGE
# 查看容器挂载的目录映射地址
docker inspect CONTAINER | grep Mounts -A 20
```

**七、安装 nginx**

```bash
sudo apt-get update
sudo apt-get install -y nginx
# 启动 nginx
sudo systemctl start nginx
# 关闭 nginx
sudo systemctl stop nginx
# 重启 nginx
sudo systemctl restart nginx
# 重载 nginx(修改配置后)
sudo systemctl reload nginx
# 修改 nginx 配置
sudo vim /etc/nginx/nginx.conf
```

**八、安装 zsh、oh-my-zsh**

1. 安装 zsh
```bash
cat /etc/shells #查看系统有几种shell
sudo apt-get update
sudo apt-get install zsh
# 替换 bash 为 zsh
chsh -s /bin/zsh
# 重启
sudo reboot
# 启动后会看到 zsh 配置选项，先输入 q，然后显示当前 shell
echo $SHELL
```

2. 安装 oh-my-zsh
```bash
# 克隆到本地 .oh-my-zsh
git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh
# 复制 oh-my-zsh 模板配置文件 为 .zshrc
cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
# 添加插件
vim ~/.zshrc
# 找到 plugins，添加
# kubectl 自动补全
# zsh-syntax-highlighting 自动提示
# zsh-autosuggestions 命令高亮
plugins=(
  git
  kubectl
  zsh-syntax-highlighting
  zsh-autosuggestions
)
```

2.1 安装 zsh-syntax-highlighting
```bash
git clone --depth=1 git://github.com/zsh-users/zsh-autosuggestions ~/.oh-my-zsh/plugins/zsh-autosuggestions
```

2.2 安装 zsh-syntax-highlighting
```bash
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ~/.oh-my-zsh/plugins/zsh-syntax-highlighting
```

3. 设置主题 ([内置主题](https://github.com/ohmyzsh/ohmyzsh/wiki/Themes#miloshadzic))
```bash
vi ~/.zshrc
# 找到 ZSH_THEME
ZSH_THEME="candy"
```

4. 重新载入配置
```bash
source ~/.zshrc
```

**九、单独安装 node.js**

1. [官网下载 Linux Binaries (x64)](https://nodejs.org/en/download/)

2. 通过 scp 命令将文件整到服务器上
```bash
scp node-v12.18.2-linux-x64.tar.xz sen@106.55.33.34:/home/sen
```

2.1 (2020-07-03补充) 也可以通过 wget 命令安装
```bash
wget https://nodejs.org/dist/v12.18.2/node-v12.18.2-linux-x64.tar.xz
```

3. 解压并重命名
```bash
tar -xvf node-v12.18.2-linux-x64.tar.xz # 解压
mv node-v12.18.2-linux-x64 nodejs # 重命名
```

4. 添加命令到环境内

4.1 (方式一) 将解压的包移动到 $HOME/.local 内，并添加软连接
```bash
cd ~/.local
sudo mv /home/sen/nodejs .
sudo ln -s /usr/local/nodejs/bin/npm /usr/local/bin # 添加 npm 命令到环境内
 sudo ln -s /usr/local/nodejs/bin/node /usr/local/bin # 添加 node 命令到环境内
```

4.2 (方式二) 在 ~/.bashrc 添加环境
```bash
export PATH="$PATH:$HOME/.local/nodejs/bin"
```

0. 命令相关
```bash
# 查看 npm 源地址
npm config get registry
# npm 更换淘宝源
npm config set registry=https://registry.npm.taobao.org
# node-sass 更换淘宝源
npm config set sass_binary_site=https://npm.taobao.org/mirrors/node-sass/
# phantomjs 更换淘宝源
npm config set phantomjs_cdnurl=https://npm.taobao.org/mirrors/phantomjs/
# electron 更换淘宝源
npm config set electron_mirror=https://npm.taobao.org/mirrors/electron/
# puppeteer 更换淘宝源
npm config set puppeteer_download_host=https://npm.taobao.org/mirrors
```

**十、安装 [yarn](https://classic.yarnpkg.com/zh-Hans/docs/install#debian-stable)**

```bash
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt-get update
# 在安装了 node 的情况下，正常安装，未安装 node 时，则会安装 node
sudo apt-get install yarn
# 如果使用了 nvm，可以通过以下操作避免 node 的安装
sudo apt-get install --no-install-recommends yarn
# 换淘宝源
yarn config set registry https://registry.npm.taobao.org
# 查看源
yarn config get registry
```

**十一、设置 ssr 客户端**

1. 安装 ShadowsocksR
```bash
git clone -b manyuser https://github.com/shadowsocksr-backup/shadowsocksr.git
cd ~/shadowsocksr
bash initcfg.sh
cd ~/shadowsocksr/shadowsocks
sudo python server.py -p 443 -k password -m aes-128-cfb -O auth_aes128_md5 -o tls1.2_ticket_auth_compatible # 启动测试
sudo ~/shadowsocksr/shadowsocks/stop.sh # 关闭服务
```

2. 配置 SSR Helper CLI
```bash
# 安装
npm i -g ssh-helper
ssr config ~/shadowsocksr
ssr add <url> # ssr 服务端生成的 ssr url
ssr connect # 选择默认服务器
ssr stop
ssr start
```

**十四、 mongodb**

相关命令
```bash
# 还原数据库
mongorestore --authenticationDatabase admin -u <用户名> -p <密码> -d <表名> <路径>
```