# Web 前端黑魔法

![](logo.png)

收集整理过去发表的 #前端黑魔法# 话题。

在此怀念和 [@zswang](https://github.com/zswang) 交流探讨前端黑魔法的时光。


# 2019

## HTTP 缓存更新

使用 fetch API 可更新 HTTP 缓存数据：

```js
fetch(url, {
  cache: 'no-cache',
})
```

设置 `cache` 选项为 `no-cache` 时，浏览器无视本地 HTTP 缓存，强制发起请求，并将最新内容覆盖已有缓存。

详细查看：https://www.cnblogs.com/index-html/p/http-cache-hotfix.html


## 检测浏览器代理

由于大部分 socks5 代理只转发数据，不转发 TCP 头，因此后端返回数据时附带一个特殊的 TCP 头，然后前端验证该头是否生效，即可推断否是存在代理。

例如后端下发 tcpwin=0 的控制信息，正常情况下前端收到后不会再往外发包，数据累计在协议栈缓冲区里。该特征可通过 WebSocket.bufferAmount 属性检测；而有代理的情况下，数据被累积在了代理服务的缓冲区，前端仍能往代理服务发包，缓冲区不会有累积。

演示：https://www.etherdream.com/proxy-detect/tcpwin.html

后端实现也很简单，NodeJS 实现 ws 服务，通过 setsockopt 给 socket 设置 mark，结合 tc 改包。

<details>
<summary>SHOW ME THE CODE</summary>

```js
// test.js
const {setsockopt} = require('sockopt')
const {WebSocketServer} = require('ws')
const wss = new WebSocketServer({port: 8080})

wss.on('connection', (ws, req) => {
  console.log(req.socket.remoteAddress + ':' + req.socket.remotePort)
  setsockopt(req.socket, 1 /* SOL_SOCKET */, 36 /* SO_MARK */, 1 /* mark */)
  ws.send('')
  setTimeout(() => ws.terminate(), 2000)
})
```

```bash
# reset tc rules
tc qdisc del dev eth0 root
tc qdisc replace dev eth0 root handle 1: htb

# if mark = 1 then tcp.win = 0
tc filter add dev eth0 parent 1: u32 \
  match mark 1 0xffffffff \
  action pedit munge offset 34 u16 set 0 pipe \
  csum ip and tcp

npm install ws sockopt
node test.js
```

HTTPS 同样支持。但不能运行在 CloudFlare 之类的服务后面，否则 TCP 特殊头就丢失了。如果网络会自动重算 checksum，那么 `pipe csum ip and tcp` 可省略。
</details>


## 发送 UDP 无状态包

WebRTC 可发送 UDP 数据，但必须先经过握手等环节。有没有办法，第一个包就能携带自定义数据？

WebRTC 传统用法是 P2P 通信，双方先通过 STUN 服务查询自己的公网地址并完成打洞，然后交换 SDP 等信息，相互连接。

但如果是 C/S 通信，服务端地址已知，那么客户端可跳过打洞步骤，直接虚构一份 SDP，把对方地址填进去就可以发包了。SDP 中的 `ice-ufrag` 会明文出现在 UDP 包中。该参数接受字母、数字和 #+-/=_ 共 68 种字符，最大长度 256，因此可携带一定量的信息到服务端。

----

一个比较有意义的应用场景是端口敲门：目标服务器（任意服务都可以）默认禁止所有 IP，通过传送门页面给它发 UDP 敲门包。服务器收到预期的 UDP 后，将该包的源 IP 加入白名单。 

演示：https://www.etherdream.com/port-knocking/

点击按钮之前无法访问目标服务，点击按钮之后就可以访问了。使用这个方案，即可抵挡后台管理服务的暴力破解，以及其他的扫描流量。也可以用于 DDOS/CC 防御（比如用 GitHub Pages 这种打不垮的页面做传送门）

![image](https://user-images.githubusercontent.com/1072787/130002091-ff85caad-7866-423c-b41e-a84861553e52.png)

![image](https://user-images.githubusercontent.com/1072787/130002182-f14dfb40-400d-4e1f-ac4d-13d10a1c1432.png)

更多参考：https://www.cnblogs.com/index-html/p/js-port-knocking.html


## 获取往返数据包的 TTL

后端获取用户的 TTL 很容易，但在浏览器上获取后端的 TTL 很麻烦。

不过有个黑科技，可以让后端返回的包反弹回服务器 —— 只要前端在收到数据前释放 socket，之后操作系统找不到目标端口，就会发送「ICMP 端口不可到达」给服务器，其中封装了返回包的头部信息。

演示：https://www.etherdream.com/test/stunex.html

详细参考：https://www.cnblogs.com/index-html/p/js-get-round-trip-ttl.html


# 更早的

下面大多从 weibo 搬运过来，账号 @EtherDream 去年被禁言，以后在此更新。表情符已替换成 ~（大部分可脑补成狗头）

[2018.md](https://github.com/EtherDream/web-frontend-magic/blob/master/2018.md)

[2017.md](https://github.com/EtherDream/web-frontend-magic/blob/master/2017.md)

[2016.md](https://github.com/EtherDream/web-frontend-magic/blob/master/2016.md)

[2015.md](https://github.com/EtherDream/web-frontend-magic/blob/master/2015.md)

[2014.md](https://github.com/EtherDream/web-frontend-magic/blob/master/2014.md)