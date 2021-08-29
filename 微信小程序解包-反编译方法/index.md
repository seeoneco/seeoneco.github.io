# 微信小程序解包+反编译方法




# 微信小程序解包+反编译方法


## 微信小程序解包+反编译方法

### 准备工作

1、微信电脑版（建议最新版）
2、Node.js环境
3、小程序程序包解密工具
4、反编译脚本

小程序程序包解密工具+反编译脚本： 

>   工具来源于吾爱破解论坛

1、安装Node.js环境
Node.js下载：https://nodejs.org/zh-cn/

2、登录微信电脑版，运行你想要反编译的小程序，每个页面都点一下，确保所有页面的加载。完成后，找到你的微信文件储存目录。

找到Applet这个目录，里面找到你刚才打开的小程序的appid就是对应的加密程序包了

3、解密。打开解密工具，选择刚才找到的加密包目录，导入解密工具进行解密，解密后就得到wxapkg程序包了

4、将`wxapkg`程序包复制到反编译脚本目录`wxappUnpacker-master`里面，用`cmd`工具`cd`到`wxappUnpacker-master`目录，依次安装以下依赖

```shell
npm install esprima 
npm install css-tree 
npm install cssbeautify 
npm install vm2 
npm install uglify-es 
npm install js-beautify
```

所有依赖安装完，即可开始反编译。

5、反编译。运行反编译命令，即 `node wuWxapkg.js` 解密程序包

```shell
node wuWxapkg.js wxe1577ebe280af504.wxapkg
```

6、反编译成功就会在同一目录下生成当前反编译的小程序appid的目录

反编译后的wxml、wxss、json基本都是跟之前你写的一模一样，但是js会有少量地方被其他字符串替代了，例如true用!1，false用!0等替代了，基本可以自己手动改改就可以，不改也不影响。

