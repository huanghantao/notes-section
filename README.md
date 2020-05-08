# notes-section

记录常用的东西

## wireshark抓取谷歌浏览器的SSL包

1、查找谷歌浏览器路径：

```bash
sudo find / -iname "Google Chrome"
```

假设是`/Applications/Google Chrome.app/Contents/MacOS/Google Chrome`

2、设置谷歌浏览器sslkey logfile

```bash
sudo /Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --ssl-key-log-file=/Users/`whoami`/sslkeylog.log
```

3、启动wireshark，并配置sslkey文件路径

4、访问网站<https://http2.akamai.com/demo>

## 测试http2协议升级

`h2`：基于`TLS`协议进行升级。

`h2c`：基于`TCP`协议进行升级。（类似于`WebSocket`协议升级）

测试站点: <http://nghttp2.org>

1、通过tcpdump进行抓包

```bash
tcpdump port 80 and host nghttp2.org -w h2c.pcap
```

2、通过curl发起http2请求（基于TCP协议升级）

```bash
curl http://nghttp2.org --http2 -v
```
