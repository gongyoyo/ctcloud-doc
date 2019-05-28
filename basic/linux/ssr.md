# ShadowsocksR

> 本文当中的环境为 `CentOS 7 64 Bit 操作系统`

### 前言

{% hint style="info" %}
在使用本站服务前，我们建议您保存好本站的联系方式，以防止与我们失联。
{% endhint %}

1. 地址发布页，建议收藏！地址：[http://ctfb.xyz](http://ctfb.xyz)
2. TG频道：[点击关注](https://t.me/cctcloud) （TG是一个国外通讯软件，需要翻墙，具体的教程[在这里](../../advanced/telegram.md)！\)
3. TG群：TG群仅允许VIP会员加入，购买会员后，在用户中心的用户须知可见！

## 准备环境

通过包管理安装几个包

> 不同的系统 例如 `Ubuntu` 和 `Arch Linux` 请自行用系统内置的包管理安装

```text
yum install git wget epel-release -y
```

### 安装 Libsodium

> Libsodium 是一个 现代化的 易于使用的加密依赖库

ShadowsocksR 的 Python 版本客户端有对 `libsodium` 进行支持来进行一些加密, 所以我们需要安装它.

> 例如 `Arch Linux` 这些系统在内置的包管理器中有提供 `libsodium` 所以我们只需要 `pacman -S libsodium` 来安装就好了。如果你的系统内置的包管理器中没有提供，请按照下述步骤下载安装。

```text
yum -y groupinstall "Development Tools"
wget https://github.com/jedisct1/libsodium/releases/download/1.0.13/libsodium-1.0.13.tar.gz
tar xf libsodium-1.0.13.tar.gz && cd libsodium-1.0.13
./configure && make -j2 && make install
echo /usr/local/lib > /etc/ld.so.conf.d/usr_local_lib.conf
ldconfig
cd ..
rm -rf libsodium-1.0.13*
```

### 安装 Nodejs 和 PM2

在本文中我们使用了 `PM2` 来进行进程的守护

> 使用 `systemctl` 等方式的进程守护在本文中不进行介绍, 您可以通过搜索引擎查阅相关的资料

```text
wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
nvm install 9
npm install -g pm2
pm2 startup
```

### 安装 ShadowsocksR

现在让我们开始安装流量中继的核心组件 `ShadowsocksR`

```text
git clone https://github.com/shadowsocksr-backup/shadowsocksr -b manyuser shadowsocks
```

### 订阅的支持

我们提供了一个 `Python 脚本来进行订阅`，使用该脚本需要安装 `requests` 依赖

```text
yum install python-pip -y
pip install requests
```

然后将以下文件写入到 `sub.py`

```text
#!/usr/bin/python2
import urlparse
import requests
import base64
import json

UPDATE_LINK = "{{REPLACE ME}}"

def fill_missing_padding(originString):
    str_len = len(originString)
    padding = 4 - str_len % 4
    return originString + "=" * padding

def decode_link(link):
    data = link[6:]
    data = base64.urlsafe_b64decode(fill_missing_padding(data))
    ret = {}

    ret["server"] = data[:data.index(":")]
    data = data[data.index(":") + 1:]
    ret["server_port"] = int(data[:data.index(":")])
    data = data[data.index(":") + 1:]
    ret["protocol"] = data[:data.index(":")]
    data = data[data.index(":") + 1:]
    ret['method'] = data[:data.index(":")]
    data = data[data.index(":") + 1:]
    ret['obfs'] = data[:data.index(":")]
    data = data[data.index(":") + 1:]
    ret['password'] = base64.decodestring(fill_missing_padding(data[:data.index("/")]))
    data = data[data.index("/") + 1:]

    ret['local_address'] = '0.0.0.0'
    ret['local_port'] = '1080'
    ret['timeout'] = 300
    ret['udp_timeout'] = 60
    ret['protocol_param'] = ''
    ret['obfs_param'] = ''
    ret['relay_rules'] = []
    ret['detect_hex_list'] = []
    ret['detect_text_list'] = []
    ret['is_multi_user'] = False

    qs = urlparse.parse_qs(data)

    # print qs
    ret['name'] = base64.urlsafe_b64decode(fill_missing_padding(qs["remarks"][0]))
    return ret

def get_nodes_link():
    req = requests.get(UPDATE_LINK)
    uncoded = base64.b64decode(fill_missing_padding(req.text))
    return filter(lambda x: x.startswith("ssr:"), uncoded.split("\n"))

for url in get_nodes_link():
    doc = decode_link(url)
    node_name = doc["name"][:doc["name"].index(" ")]
    print "Writing config file: %s.json" % node_name
    del doc['name']
    with open(node_name + ".json", 'w') as file:
        file.write(json.dumps(doc, indent=4))
```

> 将其中的 `{{REPLACE ME}}` 替换为你的订阅地址

然后我们就可以通过以下命令来进行订阅更新操作了

```text
rm -f *.json
python update.py
```

订阅会在当前目录下生成类似 `Alpha.json` 和 `Cola.json` 的文件

## 运行

然后我们需要建立一个 `start.json` 的文件 内容为

```text
{
  "name": "SHADOWSOCKS",
  "cwd": "/root/shadowsocks/shadowsocks",
  "interpreter": "python2",
  "script": "local.py",
  "args": ["-c", "/root/Alpha.json"]
}
```

将其中 `args` 中第二个参数换为你想要使用的节点

然后执行以下命令来启动本地服务器

```text
pm2 start start.json
pm2 save
```

这时候 `PM2` 就已经启动了服务器了 运行 `pm2 l` 你会看到以下输出

```text
┌─────────────┬────┬──────┬───────┬────────┬─────────┬────────┬─────┬───────────┬──────┬──────────┐
│ App name    │ id │ mode │ pid   │ status │ restart │ uptime │ cpu │ mem       │ user │ watching │
├─────────────┼────┼──────┼───────┼────────┼─────────┼────────┼─────┼───────────┼──────┼──────────┤
│ SHADOWSOCKS │ 0  │ fork │ 8885  │ online │ 3       │ 23h    │ 0%  │ 10.7 MB   │ root │ disabled │
└─────────────┴────┴──────┴───────┴────────┴─────────┴────────┴─────┴───────────┴──────┴──────────┘
 Use `pm2 show <id|name>` to get more details about an app
```

这便代表 `Shadowsocks` 服务器已经监听了 `本地 1080` 端口了。

