# 全民K歌下载器




### 前言
希望最终的美好都能为你如期而置。

### 获取接口
1、目标：下载指定用户主页的全部歌曲。
2、对比`url`，删除不必要的参数后发现，用户`uid`为个人凭证。
3、分析网页源代码得到用户所有的json数据接口为`https://node.kg.qq.com/cgi/fcgi-bin/kg_ugc_get_homepage?type=get_uinfo&start={}&num=10&share_uid={}`
第一个参数是页码，每页的`json`数据有10条，第二个参数为用户凭证信息。

### 代码实现
```python
import requests
from lxml import etree
import json
import time
import re
import os


headers = {
        "User-Agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.116 Safari/537.36",
}
path = os.path.dirname(__file__)
PATH = os.path.join(path,'music').replace('\\','/')
if not os.path.exists(PATH):
    PATH = os.mkdir(PATH)
print("下载路径："+PATH)


def get_data():
     c=0
     uid = input("请输入k歌主页uid：")
     header_url = "https://kg.qq.com/node/personal?uid={}".format(uid)
     r = requests.get(header_url,headers=headers).content.decode()
     user_name = "".join(etree.HTML(r).xpath("//*[@class = 'my_show__name']/text()"))
     music_num = "".join(etree.HTML(r).xpath("//*[@class = 'my_nav__list']/li[1]/a/text()"))[3:]
     num = int(music_num) // 10
     if num != int(music_num) / 10:
         num = num + 2
     print("昵称：{} ——> 一共有{}首音乐,数据共{}页\n".format(user_name,music_num,num-1))

     for i in range(1,num-1):
         try:
             url = "https://node.kg.qq.com/cgi/fcgi-bin/kg_ugc_get_homepage?type=get_uinfo&start={}&num=10&share_uid={}".format(i,uid)
             # print(url)
             print("\n这是第{}页数据：{}".format(i,url))
             res = requests.get(url,headers=headers).text
             time.sleep(1)
             r = json.loads(res[18:-1])
             music_name = r["data"]["ugclist"]
             for a in music_name:
                 title = a["title"]
                 if "|" or "?" or ":" in title:
                     title = title.replace("|", " ")
                     title = title.replace("?", "")
                     title = title.replace(":", "")
                 shareid = a["shareid"]
                 try:
                     print("  "+title+"--> "+"https://kg.qq.com/node/personal?uid="+shareid)
                     print("    正在下载……")
                     music_url = "http://node.kg.qq.com/play?s={}".format(shareid)
                     data = requests.get(music_url,headers).text
                     data_url = re.findall(r'playurl":"(.*?)"', data)[0]
                     res = requests.get(data_url, headers=headers,timeout=(1,30))
                     music = res.content
                     path = os.path.join(PATH, f'{title}.mp3').replace('\\', '/')
                     try:
                         with open(path, 'ab')as f:
                             f.write(music)
                             f.flush()
                         c = c+1
                         print("      【第{}首-——>{}--->下载成功】".format(c,title))
                     except Exception as e:
                         # news = "!!!sorry，{}下载失败!!!,稍后重新下载".format(title)
                         print(e)
                 except:
                     pass
         except:
             pass
get_data()

```
### 运行截图
![](https://s1.ax1x.com/2020/10/14/0I6GFS.png)

### 问题总结
1、由于把歌名作为MP3的文件名，所以歌名中如果出现`"|","?",":"`等特殊符号，会报错，当时还纠结了好长时间，为什么会报错。。。
2、不足之处有一下几点：
   1）如果用户把相同名称的歌曲重复发布，则会造成下载覆盖，只保留一首歌曲。
   2）经过测试，发现下载成功率还是很高的。但是总有些例外没能下载下来，也没做下载失败的歌曲保存再等全体结束后重新下载。
   3）这个程序只能一键下载某个用户的全部音乐，但是如果此用户后续更新了，不能针对更新的内容单独下载，体验性不高。
3、可能会更新，也可能不会，，，

(本文由老博客迁移至此....)

============================   后续     ===========================

### 考古更新

>   2021年7月23日因为好基友的原因，特意考古，发现原来的代码有些问题，用不了了。晚上花了一个多小时重构了一下，弄成一个一个的方法，在主函数中调用。写的过程中发现有一些重复性的代码，写进一个方法里显然不是最合适的，更好的实现方式应该是写一个类。。但是太菜了，不会。。。后面有机会，再说吧

```python
import requests
import json
import math
import re
import os


# 获取请求头
def get_headers():
    headers = {
        "user-agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) "
                      "Chrome/91.0.4472.101 Safari/537.36",
        "refer": "https://node.kg.qq.com/"
    }
    return headers


# 获取API接口链接
def public_url(num, id):
    url = f"https://node.kg.qq.com/cgi/fcgi-bin/kg_ugc_get_homepage?type=get_uinfo&start={num}&num=10&share_uid={id}"
    headers = {
        "user-agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) "
                      "Chrome/91.0.4472.101 Safari/537.36",
        "refer": "https://node.kg.qq.com/"
    }
    return url, headers


# 运行时的头部装逼图
def zb_banner():
    print("|" + "=" * 41 + " " + "=" * 41 + "|")
    print("|" + "=" * 17 + " " * 15 + " 全 民 K 歌 下 载 器 " + " " * 15 + "=" * 17 + "|")
    print("|" + "=" * 17 + " " * 25 + "By --- Lone" + " " * 13 + "=" * 17 + "|")
    print("|" + "=" * 41 + " " + "=" * 41 + "|")


# 获得歌曲总数，进而得到要循环接口链接几次
def get_basic_data(uid):
    _, __ = public_url(1, uid)
    res = requests.get(_, headers=__).text
    # print(res[18:-2])
    user_basic_data = json.loads(res[18:-1])
    user_name = user_basic_data["data"]["nickname"]  # 用户昵称
    user_fans_num = user_basic_data["data"]["follower"]  # 用户的粉丝数目
    music_num = user_basic_data["data"]["ugc_total_count"]  # 歌曲总数目
    # print(user_basic_data["data"]["ugc_total_count"])
    count = math.ceil(int(music_num) // 10)  # 取出url要遍历的次数
    print(f"您查询的用户 昵称 => {user_name}" + f"  粉丝数 => {user_fans_num}位 ！")
    print(f"根据查询，目前此用户公开发布歌曲一共有 {music_num} 首 ！")
    return count


# 通过接口，拿到了歌曲的名字和歌曲展示页面的网页（不是下载链接）
def get_music_data(num, uid):
    music_list = []  # 歌曲列表
    for i in range(1, num + 2):
        _, __ = public_url(i, uid)
        res = json.loads(requests.get(_, headers=__).text[18:-1])
        # print(res)
        for a in res["data"]["ugclist"]:
            shareId = a["shareid"]
            music_name = a["title"]
            # print(music_name)
            if "|" or "?" or ":" in music_name:
                music_name = music_name.replace("|", " ")
                music_name = music_name.replace("?", "")
                music_name = music_name.replace(":", "")
            # print(shareId, music_name)
            music_view_url = "http://node.kg.qq.com/play?s=" + shareId
            music = [music_name, music_view_url]
            # print(music)
            music_list.append(music)
    # [print(i) for i in music_list]
    return music_list


# 使用正则，拿到歌曲展示页面的下载地址和名字
def get_down_url(info):
    music_down_list = []
    for i in info:
        music_name = i[0]
        view_url = i[1]
        headers = get_headers()
        res = requests.get(view_url, headers=headers).text
        down_url = re.findall(r'playurl":"(.*?)"', res)[0]
        music_down = [music_name, down_url]
        music_down_list.append(music_down)
    # [print(i) for i in music_down_list]
    return music_down_list


# 通过访问下载链接，进行下载
def down_music(PATH, info):
    count = 0
    for i in info:
        name = i[0]
        down_url = i[1]
        headers = get_headers()
        res = requests.get(down_url, headers=headers, timeout=(1, 30)).content
        path = os.path.join(PATH, f'{name}.mp3').replace('\\', '/')
        try:
            with open(path, 'ab')as f:
                f.write(res)
                f.flush()
            count += 1
            print("      【第{}首-——>{}--->下载成功】".format(count, name))
        except Exception as e:
            print(e)


# 函数入口，主函数
if __name__ == '__main__':
    zb_banner()
    uid = input("请输入想要查询用户的UID:")
    path = os.path.dirname(__file__)
    PATH = os.path.join(path, 'music').replace('\\', '/')
    if not os.path.exists(PATH):
        os.mkdir(PATH)
    count = get_basic_data(uid)
    music_list = get_music_data(count, uid)
    print("已为您选择下载路径：" + PATH)
    music_down_list = get_down_url(music_list)
    print("请耐心等待")
    down_music(PATH, music_down_list)

```

![](https://gitee.com/lonercci/picbed/raw/master/img/20210724172438.png)

###  一些碎碎念

就在笔者写这些话的时候，

恍惚中，仿佛又回到了去年校园里那个燥热的夏天，

蝉鸣依旧，热烈而又执着，

那段时光应该是我大学里最快乐的时光，

这个软件最初的目的就是为了纪念我曾深爱着的那个女孩，

往事回忆，到平淡，那些重复或许仅仅是为了错失的过往……

