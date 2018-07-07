---
layout: post
title: "Shadowsocks, 批量导入配置 - Android, Mac"
description: ""
category: misc
tags: []
modify: 2017-06-25 11:56:14
---

update: 2017-06-25


最近手腕有些不适，医生说是肌肉疲劳，建议多休息，于是周末宅住处。不太敢用电脑，可闲不住呐，就折腾了些脚本，均放在[这里](https://github.com/facaiy/facai-tools/tree/master/script/vpnso)，以供下载。

### A. Shadowsocks-android

#### 0.0 起因

需求是这样产生的。我使用的科学上网提供商只有两种配置，一种是json文件，它有全部IP的配置信息，但shadowsocks不支持批量导入；另一种是毎IP有对应QR码，分别扫码导入。但是，我想要更简单点，一键导入全部配置。作为一只程序猿，别人不支持，咱就自己捋袖子开干。

#### 0.1 过程

Shadowsocks-android唯一能批量导入的功能是「从剪贴板导入」(Import from Clipboard)。查看它导入导出，使用的是私有ss协议，形如`ss://abcdx#jp`，看上去是对数据做了个简单的编码。因为代码是开源的，就去官方扒。   
版本如下：`commit 095f4831db806d55f164e52a6164cd92a5919339`

配合关键字使用`git grep`命令，很快就定位到`Profile.scala`文件：

```scala
def toUri: Uri = {
  val builder = new Uri.Builder()
    .scheme("ss")
    .encodedAuthority("%s@%s:%d".formatLocal(Locale.ENGLISH,
      Base64.encodeToString("%s:%s".formatLocal(Locale.ENGLISH, method, password).getBytes,
        Base64.NO_PADDING | Base64.NO_WRAP | Base64.URL_SAFE),
      if (host.contains(':')) '[' + host + ']' else host, remotePort))
  val configuration = new PluginConfiguration(plugin)
  if (configuration.selected.nonEmpty)
    builder.appendQueryParameter(Key.plugin, configuration.selectedOptions.toString(false))
  if (!nameIsEmpty) builder.fragment(name)
  builder.build()
}
```

思路比较简单，对`method`和密码编码后，组成URL。问题是这里格式应该是`ss://base64@host:port?...#jp`，和前面看到的不符合。可能是存在`encodeAuthority`这个方法，尝试追了下，发现是Android官方库，而且有点绕。

转换思路，从解码端反查下如何，于是定位到`Parser.scala`文件。

```scala
private val pattern = "(?i)ss://[-a-zA-Z0-9+&@#/%?=~_|!:,.;\\[\\]]*[-a-zA-Z0-9+&@#/%=~_|\\[\\]]".r
private val legacyPattern = "^(.+?):(.*)@(.+?):(\\d+?)$".r

def findAll(data: CharSequence): Iterator[Profile] =
  pattern.findAllMatchIn(if (data == null) "" else data).map(m => {
    val uri = Uri.parse(m.matched)
    uri.getUserInfo match {
      case null => val str = new String(Base64.decode(uri.getHost, Base64.NO_PADDING), "UTF-8"); str match {
        case legacyPattern(method, password, host, port) =>  // legacy uri
      }
```

原来这个协议还有个老版本格式，对`method:password@server:port`进行编码就行。

思路就简单了，读json文件，提取出相关数据进行编码，得到全部ss链接，全部复制，导入完成。

#### 0.2 结果

Python脚本非常简单，如下：

```python
#!/usr/bin/env python3

import sys
json_file = sys.argv[1]

import json
with open(json_file) as json_data:
    data = json.load(json_data)

configs = ["{}:{}@{}:{}".format(
            c["method"], c["password"], c["server"], c["server_port"])
			for c in data["configs"]]

import base64
ss = [base64.urlsafe_b64encode(bytes(x, "utf8")) for x in configs]

for s in ss:
    print(str(s, "utf8"))
# ss://YW........
# ss://YW........
# ss://YW........
# ss://YW........
```

另外，要定位速度最快的IP，就写了对每个IP测速并排序的Bash脚本，单线程，比较慢，但够用了。

```bash
#!/bin/bash

COUNTS=5

while getopts "s:c::" opt; do
  case $opt in
    s) SERVERS=$OPTARG;;
    c) COUNTS=$OPTARG;;
    \?) echo "usage: pingall -h [server file] -c [ping counts]";;
  esac
done

echo "servers file: $SERVERS, ping counts:  $COUNTS"

echo "server\t\t\tmin/avg/max/stddev"

TMPFILES_PREFIX=/tmp/.$SERVERS.info

TMPDATA=$TMPFILES_PREFIX.origin
cat $SERVERS | xargs -n 1 ping -c $COUNTS > $TMPDATA

TMPFILTER=$TMPFILES_PREFIX.filter
cat $TMPDATA | grep -E '^round-trip|^---' > $TMPFILTER

cat $TMPFILTER | \
awk '{ if ($1 ~ /---/) { SERVER=$2 } else { print SERVER "\t" $4, $5} }' | \
sort -t/ -k 2n -b

rm $TMPDATA $TMPFILTER
#servers file: server.txt, ping counts:  5
#server                  min/avg/max/stddev
#xx.xxx.com     44.706/45.730/48.300/1.325 ms
#yy.yyy.com     51.092/52.552/53.528/0.826 ms
```

人生苦短，Python当歌，收工！

<br/>

### B. ShadowsocksX, Mac

更新：2018-07-07: 使用ShadowsocksX-NG，已经支持全局配置导入导出。

<br/>


参考：[OS X下如何导入gui-config.json](https://github.com/shadowsocks/shadowsocks-iOS/issues/150)

Mac下的ShadowsocksX没有任何批量导入的功能，所以偏方是直接改它的配置文件。因为配置文件数据段是编码的json数据，恰好提供商给我的就是json格式，所以只需要将文件编码后，置换数据段就行。工作量非常小，具体如下：

+ 将ShadowsocksX配置文件plist转成xml:

  ```bash
  plutil -convert xml1 ~/Library/Preferences/clowwindy.ShadowsocksX.plist -o xxx.xml
  ```

+ 将IP的json配置文件编码：
  1. 改json文件：

     ```json
     {"current":9,"profiles":[{"password":"xxxxxxxx","method":"aes-256-cfb","server_port":8888,"remarks":"US-Bandwagon","server":"xxxxxxxxxx"}]}
     ```

     注意：字段要合符上述样式。`current`是正使用IP序号，要和原始配置匹配。

  2. 编码：

     ```python
     #!/usr/bin/env python3

     import sys
     json_file = sys.argv[1]

     import json
     with open(json_file) as json_data:
         json_str = json.dumps(json.load(json_data))

     import base64
     byte_res = base64.urlsafe_b64encode(bytes(json_str, "utf8"))
     print(str(byte_res, "utf8"))
     ```

+ 置换数据段：用如上输出二进制数据置换xml中data字段。

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
  <plist version="1.0">
  <dict>
      <key>config</key>
      <data>
      bbabccfdeeffafafa....
      </data>
  </dict>
  </plist>
  ```

+ 将xml转回plist:

  ```bash
  plutil -convert binary1 xxx.xml -o yyy.plist
  ```

+ 重新导入配置：

  ```bash
  defaults import clowwindy.ShadowsocksX yyy.plist
  ```

+ 重启客户端，完成。
