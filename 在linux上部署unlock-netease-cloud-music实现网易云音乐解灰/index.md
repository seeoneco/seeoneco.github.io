# 在Linux上部署Unlock-netease-cloud-music实现网易云音乐解灰


### 关于

最近发现很多好听的歌曲网易云没有版权。

以前在超哥的服务器上部署过，后来不知道为什么失效了，正好现在自己的服务器闲置。

由于开发者在 Github 上Linux的教程使用的是宝塔面板，但是我不太想安装面板，正好参考其他大佬的教程使用命令行，顺带温习一下命令。

感谢插件开发大佬，感谢几位博主的文章！

### 参考引用

> 1、[Unlock-netease-cloud-music](https://github.com/meng-chuan/Unlock-netease-cloud-music)
>
> 2、[【操作记录】在服务器上搭建UnblockNeteaseMusic解锁网易云音乐客户端变灰歌曲](https://simplestark.com/archives/cloudmusic)
>
> 3、[CentOS 7 安装配置git](https://www.jianshu.com/p/e6ecd86397fb)

### 基础环境安装

#### Nodejs

**新建文件夹**
在/usr/local目录下创建nodejs文件夹

```bash
[root@seeoneno ~]# cd /usr/local
[root@seeoneno local]# mkdir nodejs
```

 **下载nodejs**

进入官网：http://nodejs.cn/download/，选择`Linux 二进制文件（x64）`，右键复制下载地址

```bash
[root@seeoneno local]# cd nodejs
[root@seeoneno nodejs]# wget https://npmmirror.com/mirrors/node/v14.18.3/node-v14.18.3-linux-x64.tar.xz
```

**查看压缩包**

```bash
[root@seeoneno nodejs]# ls
node-v14.18.3-linux-x64.tar.xz
```

**使用tar命令解压**

```bash
[root@seeoneno nodejs]# tar -xf node-v14.18.3-linux-x64.tar.xz 
[root@seeoneno nodejs]# ls
node-v14.18.3-linux-x64  node-v14.18.3-linux-x64.tar.xz
```

**将node-v14.18.3-linux-x64目录下文件移动到当前目录，并删除其余文件**

```bash
[root@seeoneno nodejs]# mv node-v14.18.3-linux-x64/* /usr/local/nodejs/
[root@seeoneno nodejs]# rm -rf node-v14.18.3-linux-x64.tar.xz
[root@seeoneno nodejs]# rm -rf node-v14.18.3-linux-x64/
[root@seeoneno nodejs]# ls
bin  CHANGELOG.md  include  lib  LICENSE  README.md  share
```

**建立软连接**

```bash
[root@seeoneno nodejs]# ln -s /usr/local/nodejs/bin/node /usr/local/bin
[root@seeoneno nodejs]# ln -s /usr/local/nodejs/bin/npm /usr/local/bin
```

**验证是否安装成功**

```bash
[root@seeoneno nodejs]# node -v
v14.18.3
[root@seeoneno nodejs]# npm -v
6.14.15
```

#### Git

**安装**

```bash
yum install git
```

**验证**

```bash
[root@seeoneno ~]# git --version
git version 1.8.3.1
```

### 安装 *Unlock*-*netease*-*cloud*-*music* 插件

Github: https://github.com/meng-chuan/Unlock-netease-cloud-music

**新建文件夹**

```bash
[root@seeoneno ~]# cd /usr
[root@seeoneno usr]# mkdir seeoneno
[root@seeoneno usr]# cd seeoneno/
[root@seeoneno seeoneno]# mkdir cloud-music
```

**拉取github仓库地址（使用加速镜像）**

```bash
[root@seeoneno seeoneno]# cd cloud-music/
[root@seeoneno cloud-music]# git clone https://hub.fastgit.xyz/meng-chuan/Unlock-netease-cloud-music.git
[root@seeoneno cloud-music]# ls
Unlock-netease-cloud-music
[root@seeoneno cloud-music]# cd Unlock-netease-cloud-music/
[root@seeoneno Unlock-netease-cloud-music]# ls
app.js     ca.crt              Dockerfile          LICENSE       README.md   server.key  网易☁?.bat
bridge.js  docker-compose.yml  endpoint.worker.js  package.json  server.crt  src         网易☁?.vbs
[root@seeoneno Unlock-netease-cloud-music]# 
```

**使用窗口运行程序（窗口关闭即停止运行）**

在文件夹内：

```
[root@seeoneno Unlock-netease-cloud-music]# node app.js -p 52000
HTTP Server running @ http://0.0.0.0:52000
```

使用前提是服务器的安全组内开放了52000端口

**在网易云音乐设置代理服务器**

```bash
http://47.101.169.184:52000/
```

测试成功！

### 后台运行

上面的方式，xshell断开连接就会停止进程。

**安装pm2**

```bash
npm install pm2 -g
```

**建立软连接**

```bash
ln -s /usr/local/nodejs/bin/pm2 /usr/local/bin/
```

**验证**

```bash
pm2 -v
```

**进入插件/src目录，修改app.js文件**

修改端口为52000，保存

**启动**

```bash
pm2 start app.js
```

**验证是否启动成功**

```bash
pm2 list
```

### 结果测试

![image-20220203183629725](https://gitee.com/lonercci/picbed/raw/master/img/202202031836937.png)

![image-20220203183720692](https://gitee.com/lonercci/picbed/raw/master/img/202202031837877.png)

![image-20220203183746268](https://gitee.com/lonercci/picbed/raw/master/img/202202031837813.png)

成功，完美！

