# BGmi

BGmi是一个用来追番的命令行程序.

[![](https://img.shields.io/pypi/v/bgmi.svg)](https://pypi.python.org/pypi/bgmi)
[![](https://pypistats.com/badge/bgmi.svg)](https://pypi.python.org/pypi/bgmi)
[![](https://travis-ci.org/BGmi/BGmi.svg?branch=master)](https://travis-ci.org/BGmi/BGmi)
[![](https://codecov.io/gh/BGmi/BGmi/branch/master/graph/badge.svg)](https://codecov.io/gh/BGmi/BGmi/branch/master/graph/badge.svg)
[![](https://img.shields.io/badge/License-MIT-blue.svg)](https://github.com/BGmi/BGmi/blob/master/LICENSE)

## TODO

[同时抓取多个数据源](https://github.com/BGmi/BGmi/projects/1)


## 更新日志

- 你现在看到的BGmi版本是最后一个支持python2的版本, 快拥抱python3吧.
- Transmission rpc 认证设置
- 新的下载方式 [deluge-rpc](https://www.deluge-torrent.org/)
- 使用最大和最小集数筛选搜索结果

## 特性

- 多个数据源可选: [bangumi_moe](https://bangumi.moe), [mikan_project](https://mikanani.me) 或者[dmhy](https://share.dmhy.org/)
- 使用 aria2, transmission 或者deluge来下载你的番剧.
- 提供一个管理和观看订阅番剧的前端.
- 弹幕支持
- 提供uTorrent支持的RSS Feed和移动设备支持的ICS格式日历.
- Bangumi Script: 添加自己的番剧解析器
- 番剧放松列表和剧集信息
- 下载番剧时的过滤器(支持关键词,字幕组和正则)
- 多平台支持: Windows, *nux 以及 Router system

![](./images/bgmi_cli.png?raw=true)
![](./images/bgmi_http.png?raw=true)
![](./images/bgmi_player.png?raw=true)
![](./images/bgmi_admin.png?raw=true)

## 安装

使用pip安装稳定版本:

```bash
pip install bgmi
```

或者从Github安装最新版本（强烈不推荐）

```bash
pip install https://github.com/Bgmi/BGmi/tarball/dev
```



安装`BGmi`所需的依赖以及下载`BGmi`的前端文件

```bash
bgmi install
```

## 升级

```bash
pip install bgmi -U
bgmi upgrade
```

在升级后请确保运行`bgmi upgrade`

## 使用Docker

见 [BGmi/bgmi-docker-all-in-one](https://github.com/BGmi/bgmi-docker-all-in-one)

## 使用

查看可用的命令

```bash
bgmi -h
```

启用命令自动补全

```bash
eval "$(bgmi complete)"
```

设置`BGMI_PATH`:

```bash
BGMI_PATH=/bgmi bgmi -h
```

或者在你的`.bashrc`里添加一个alias

```bash
alias bgmi='BGMI_PATH=/tmp bgmi'
```

支持的数据源:

+ [bangumi_moe(default)](https://bangumi.moe)
+ [mikan_project](https://mikanani.me)
+ [dmhy](https://share.dmhy.org/)

换一个数据源:

**更换数据源会清空番剧数据库, 但是bgmi script不受影响.** 之前下载的视频文件不会删除, 但是不会在前端显示

```bash
bgmi source mikan_project
```

查看目前正在更新的新番:

```bash
bgmi cal
```


订阅番剧:

```bash
bgmi add "进击的巨人 第三季" "刃牙" "哆啦A梦"
bgmi add "高分少女" --episode 0
```


退订:

```bash
bgmi delete --name "Re:CREATORS"
```

更新番剧列表并且下载番剧:

```bash
bgmi update --download
bgmi update "从零开始的魔法书" --download 2 3
bgmi update "时钟机关之星" --download
```

设置筛选条件:

```bash
bgmi list # 列出目前订阅的番剧
bgmi fetch "Re:CREATORS"
bgmi filter "Re:CREATORS" --subtitle "DHR動研字幕組,豌豆字幕组" --include 720P --exclude BIG5
bgmi fetch "Re:CREATORS"
# remove subtitle, include and exclude keyword filter and add regex filter
bgmi filter "Re:CREATORS" --subtitle "" --include "" --exclude "" --regex
bgmi filter "Re:CREATORS" --regex "(DHR動研字幕組|豌豆字幕组).*(720P)"
bgmi fetch "Re:CREATORS"
```

最后使用`bgmi fetch`来看看筛选的结果.


搜索番剧并下载:

```bash
bgmi search '为美好的世界献上祝福！' --regex-filter '.*动漫国字幕组.*为美好的世界献上祝福！].*720P.*'

```

使用`--min-episode`和`--max-episode`来根据集数筛选下载结果

```bash
bgmi search 海贼王 --min-episode 800 --max-episode 820
# 下载
bgmi search 海贼王 --min-episode 800 --max-episode 820 --download
```

`bgmi search`命令默认不会显示重复的集数, 如果要显示重复的集数来方便过滤, 在命令后加上`--dupe`来显示全部的搜索结果

手动修改最近下载的剧集

```bash
bgmi list
bgmi mark "Re:CREATORS" 1
```

查看`BGmi`的设置并且修改对应设置:

```bash
bgmi config # 查看当前各项设置默认值.
bgmi config KEY # 查看某项设置的默认值
bgmi config KEY value # 修改某项设置
# example:
bgmi config ARIA2_RPC_TOKEN 'token:token233'
```

各项设置的含义.

BGmi:

+ `BGMI_SAVE_PATH`: 下载番剧保存地址
+ `DOWNLOAD_DELEGATE`: 番剧下载工具 (aria2-rpc, transmission-rpc, deluge-rpc)
+ `MAX_PAGE`: 抓取数据时每个番剧最大抓取页数
+ `BGMI_TMP_PATH`: 临时文件夹
+ `DANMAKU_API_URL`: danmaku api 服务器地址
+ `LANG`: 语言
+ `ADMIN_TOKEN`: 管理
+ `ENABLE_GLOBAL_FILTER`: 是否启用全局排除关键词, 这些关键词将在所有番剧中启用.
+ `GLOBAL_FILTER`: 默认全局排除的关键词. 现在包括了浏览器不支持的x265压制方式和生肉
+ `TORNADO_SERVE_STATIC_FILES`: 是否使用bgmi自带的http服务器代理静态文件. 启用后bgmi_http会直接使用`tornado.web.StaticFileHandler`代理静态文件.
+ `BANGUMI_MOE_URL`: bangumi.moe 的官方网站或者镜像站链接
+ `SHARE_DMHY_URL`: 动漫花园的官方网站或者镜像站链接

Aria2-rpc:

+ :code:`ARIA2_RPC_URL`: aria2c RPC URL (**不是jsonrpc URL, 如果你的aria2c运行在localhost:6800, 对应的链接为 `http://localhost:6800/rpc`**)
+ :code:`ARIA2_RPC_TOKEN`: aria2c RPC token(如果没有设置token, 留空或者设置为 `token:`)


Transmission-rpc:

+ `TRANSMISSION_RPC_URL`: transmission rpc host
+ `TRANSMISSION_RPC_PORT`: transmission rpc port
+ `TRANSMISSION_RPC_USERNAME`: transmission rpc username
+ `TRANSMISSION_RPC_PASSWORD`: transmission rpc password

Deluge-rpc:

+ `DELUGE_RPC_URL`: deluge rpc url
+ `DELUGE_RPC_PASSWORD`: deluge rpc password

## 使用`bgmi_http`

`.先下载所有更新中番剧的封面

```bash
bgmi cal --download-cover
```

2.根据你是否使用nginx, 设置`TORNADO_SERVE_STATIC_FILES`(使用nginx的情况下使用默认设置`0`, 不使用的情况下设置为`1`)

3.下载前端的静态文件(你可能在安装的时候已经下载过了):

```bash
bgmi install
```

4.在`8888`端口启动BGmi HTTP服务器:

```bash
bgmi_http --port=8888 --address=0.0.0.0
```

### 在Windows上使用`bgmi_http`

参照上面启动服务器, 然后访问[http://localhost:8888/](http://localhost:8888/).

### 在*nux上使用bgmi_http

可以让`BGmi`帮助你生成对应的nginx配置文件

```bash
bgmi gen nginx.conf --server-name bgmi.whatever.com
```

你也可以手动写一份nginx配置, 来满足你的更多需求(比如启用https), 这是一份例子

```bash
server {
    listen 80;
    server_name bgmi;

    autoindex on;
    charset utf-8;

    location /bangumi {
        # ~/.bgmi/bangumi
        # alias到你的`SAVE_PATH` 注意以/结尾
        alias /path/to/bangumi/;
    }

    location /api {
        proxy_pass http://127.0.0.1:8888;
    }

    location /resource {
        proxy_pass http://127.0.0.1:8888;
    }

    location / {
        # alias到你的`BGMI_PATH/front_static/`注意以/结尾
        alias /path/to/front_static/;
    }
}
```

或者添加一个aria2c前端之类的, 具体办法百度上有很多,不在此赘述.

macOS launchctl service controller

参照 [issue #77](https://github.com/BGmi/BGmi/pull/77)自行设置

[me.ricterz.bgmi.plist](https://github.com/BGmi/BGmi/blob/master/bgmi/others/me.ricterz.bgmi.plist)

## 弹幕支持

BGmi 使用[`DPlayer`](https://github.com/DIYgod/DPlayer)做为前端播放器

如果你想要添加弹幕支持, 在这里[DPlayer#related-projects](https://github.com/DIYgod/DPlayer#related-projects)选择一个后端自行搭建, 或者使用`DPlayer`提供的现成接口`https://api.prprpr.me/dplayer/`

然后使用 `bgmi config` 来设置你的后端api地址

```bash
bgmi config DANMAKU_API_URL https://api.prprpr.me/dplayer/
```
设置你的`bgmi_http`, 享受弹幕支援吧.


## 调试

设置环境变量`BGMI_LOG`为`debug`, `info`, `warning`, 或者`error`

log文件位于`{TMP_PATH}/bgmi.log`


## 卸载

由于pip的限制, 你需要手动清理`BGmi`产生的位于`~/.bgmi`的文件

同样, `BMmi`添加到你系统的定时任务也不会被自动删除, 请手动删除.

*nix:

    请手动清理crontab

windows:

```bash
schtasks /Delete /TN 'bgmi updater'
```

## 如果你对python有一点了解, 并且觉得还不够的话, 下面是为你准备的.

## Bangumi Script

你可以写一个`BGmi Script`来解析你自己的想看的番剧或者美剧. BGmi会加载你的script, 视作一个番剧来对待. 而你所需要做的只是继承`ScriptBase`类, 然后实现特定的方法, 再把你的script文件放到`BGMI_PATH/script`文件夹内.

Example: <./script_example.py>

`get_download_url()`返回一个`dict`, 以对应集数为键, 对应的下载链接为值

```python
{
    1: 'http://example.com/Bangumi/1/1.mp4',
    2: 'http://example.com/Bangumi/1/2.torrent',
    3: 'http://example.com/Bangumi/1/3.mp4'
}
```

## BGmi 数据源
通过扩展`bgmi.website.base.BaseWebsite`类并且实现对应的三个方法,你也可以简单的添加一个数据源

每个方法具体的意义和返回值格式请参照每个方法对应的注释

```python
class DataSource(bgmi.website.base.BaseWebsite)
    cover_url=''

    def search_by_keyword(self, keyword, count):
        """
        return a list of dict with at least 4 key: download, name, title, episode
        example:
        ```
            [
                {
                    'name':"路人女主的养成方法",
                    'download': 'magnet:?xt=urn:btih:what ever',
                    'title': "[澄空学园] 路人女主的养成方法 第12话 MP4 720p  完",
                    'episode': 12
                },
            ]

        :param keyword: search key word
        :type keyword: str
        :param count: how many page to fetch from website
        :type count: int

        :return: list of episode search result
        :rtype: list[dict]
        """
        raise NotImplementedError

    def fetch_bangumi_calendar_and_subtitle_group(self):
        """
        return a list of all bangumi and a list of all subtitle group

        list of bangumi dict:
        update time should be one of ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat']
        example:
        ```
            [
                {
                    "status": 0,
                    "subtitle_group": [
                        "123",
                        "456"
                    ],
                    "name": "名侦探柯南",
                    "keyword": "1234", #bangumi id
                    "update_time": "Sat",
                    "cover": "data/images/cover1.jpg"
                },
            ]
        ```
        when downloading cover images, BGmi will try to get `self.cover_url + bangumi['cover']`


        list of subtitle group dict:
        example:
        ```
            [
                {
                    'id': '233',
                    'name': 'bgmi字幕组'
                }
            ]
        ```


        :return: list of bangumi, list of subtitile group
        :rtype: (list[dict], list[dict])
        """
        raise NotImplementedError

    def fetch_episode_of_bangumi(self, bangumi_id, subtitle_list=None, max_page=MAX_PAGE):
        """
        get all episode by bangumi id
        example
        ```
            [
                {
                    "download": "magnet:?xt=urn:btih:e43b3b6b53dd9fd6af1199e112d3c7ff15cab82c",
                    "subtitle_group": "58a9c1c9f5dc363606ab42ec",
                    "title": "【喵萌奶茶屋】★七月新番★[来自深渊/Made in Abyss][07][GB][720P]",
                    "episode": 0,
                    "time": 1503301292
                },
            ]
        ```

        :param bangumi_id: bangumi_id
        :param subtitle_list: list of subtitle group
        :type subtitle_list: list
        :param max_page: how many page you want to crawl if there is no subtitle list
        :type max_page: int
        :return: list of bangumi
        :rtype: list[dict]
        """
        raise NotImplementedError
```

## License

[MIT License](./LICENSE)