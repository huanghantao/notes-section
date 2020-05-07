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
