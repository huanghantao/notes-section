# notes-section

记录常用的东西（和我的环境强相关，没有`star`的价值，仅仅是个人复制粘贴用的）

## HTTP2

文档：<https://http2.github.io>

### wireshark抓取谷歌浏览器的SSL包

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

### 测试http2协议升级

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

### Header头部压缩

通过HPACK算法进行压缩。

可以通过`h2load`工具来检验`header`压缩比：

```bash
h2load https://blog.cloudflare.com -n 2
```

## Swoole

### 编译libswoole

> 为了正常编译，务必先用`phpize`和`make clean`先清理一下环境。

`MacOS`下：

```bash
cmake . -Dopenssl_dir=/usr/local/opt/openssl/ -Dbrotli_dir=/usr/local/
```

`Linux`下：

```bash
cmake . -Dopenssl_dir=/usr/local/openssl/
```

## DNS

```bash
nslookup test.com 127.0.0.1 -port=9502
```

```bash
nslookup -type=CNAME test.com 127.0.0.1 -port=9502
```

## WebSocket

工具：<https://github.com/websockets/wscat>

```bash
npm install -g wscat
```

## Homebrew

### 切换PHP版本

```bash
/Users/hantaohuang/.phpbrew/distfiles/php-7.2.31.tar.bz2
```

```bash
phpbrew install 7.2.31 \
    +default +mysql \
    +iconv="$(brew --prefix libiconv)" \
    +bz2="$(brew --prefix bzip2)" \
    +zlib="$(brew --prefix zlib)"

phpbrew install 7.2.31 +default +mysql +bz2=/usr/local/opt/bzip2 +zlib=/usr/local/opt/zlib
```

## 正则表达式验证网站

<https://regex101.com>

## PHP多版本测试网站

<https://3v4l.org>

## curl

### 读json文件

```bash
curl localhost:3000/api/json -X POST -d @data.json --header "Content-Type: application/json"
```

## 破解软件

<http://www.sdifen.com>

## nvim调试php language server

一、启动服务器

```bash
phpactor language-server --address=127.0.0.1:9901 -vvv
```

二、配置`init.vim`如下：

```vim
call plug#begin('~/.vim/plugged')

Plug 'neoclide/coc.nvim', {'branch': 'release'}

call plug#end()
```

三、然后安装插件：

在`nvim`里面输入：

```vim
:PlugInstall
```

四、配置CocConfig

在`nvim`里面输入：

```vim
:CocConfig
```

然后配置如下：

```json
{
"languageserver": {
    "socketserver": {
    "filetypes": ["php"],

    "host": "127.0.0.1",
    "port": 9901
    }
}
}
```

然后，打开一个`PHP`文件，即可调试服务器了。

## 搭建HTTP代理服务器

```bash
git@github.com:getlantern/http-proxy.git
go run http_proxy.go
```

## 使用host和port操作Docker

因为`Docker`默认是通过`unix socket`来通信的。但是它也支持通过`ip + port`的方式来访问`Docker`。这需要配置`Docker`，有点麻烦，所以我们可以加一个代理：

```bash
socat TCP-LISTEN:2375,reuseaddr,fork UNIX-CONNECT:/var/run/docker.sock
```

这样，我们就可以通过访问`2375`端口来访问`Docker`了。
