# Web 前端黑魔法

![](logo.png)

收集整理过去发表的 #前端黑魔法# 话题。

在此怀念和 [@zswang](https://github.com/zswang) 交流探讨前端黑魔法的时光。



# 2019

整理中...

下面大多从 weibo 搬运过来，账号 @EtherDream 去年被禁言，以后在此更新。表情符已替换成 `~`（大部分可脑补成狗头）



# 2018-12-19

有人说，能用 const 的时候尽量用 const。所以，循环因子也可以。。。

```js
for (const i of range(0, 5)) {
  console.log(i)  // 0, 1, 2, 3, 4
}

function* range(beg, end, step = 1) {
  for (let i = beg; i < end; i += step)
    yield i
}
```

> 有没有 Python 的感觉~



# 2018-11-19

思考题：如何获取闭包内 key 变量的值：

```js
// 挑战目标：获取 key 的值
(function() {
  // 一个内部变量，外部无法获取
  var key = Math.random()

  console.log('[test] key:', key)

  // 一个内部函数
  function internal(x) {
    return x
  }

  // 对外暴露的函数
  apiX = function(x) {
    try {
      return internal(x)
    } catch (err) {
      return key
    }
  }
})()

// 你的代码写在此处：
// ...
```

答案：https://github.com/EtherDream/web-frontend-magic/issues/1



# 2018-11-19

思考题：检测隐形人是否存在

```js
// 挑战目标：检测 Math 是否被代理
if (Math.random() < 0.5) {
  console.log('hooked')

  self.Math = new Proxy(Math, {
    get(obj, prop) {
      return obj[prop]
    }
  })
}

// 你的代码写在此处：
// 如果控制台有显示 hooked，那么你输出 true，反之 false
// ...
```

Demo: https://jsfiddle.net/k0nupd58/

> 答案有多个，但有一种特别巧妙，可以参考「如何获取闭包内 key 变量的值」的思路。



# 2018-10-26

点一下，CPU 开始咆哮

Demo: https://www.etherdream.com/FunnyScript/cpu-hog/

> 该页面创建 1000 个 Service Worker 消耗大量 CPU。即使关闭页面，它们仍在后台运行。想尝试的话务必在隐身模式中打开，感觉卡了就可以关闭了。



# 2018-10-18

一种统计 key 的个数，无需使用判断的写法：

```js
const map = {}
const str = 'hello world'

str.split('').forEach(key => {
  map[key] = -~map[key]
})

console.log(map)
// {" ": 1, d: 1, e: 1, h: 1, l: 3, o: 2, r: 1, w: 1}
```

Demo: https://jsbin.com/lumexovida/edit?js,console,output

> ~i = -(i + 1)，~undefined = -1



# 2018-10-17

自从 Chrome 69 支持 OffscreenCanvas API 后，终于能在 Service Worker 里调用 GPU 资源了~ 所以用户关闭网页后，XSS 还可以继续使用 100% CPU 和 GPU 资源几分钟时间~

拿之前的 SHA256 PoW 改了下貌似可行~

Demo: https://www.etherdream.com/FunnyScript/glminer/sw-miner/

![image](https://user-images.githubusercontent.com/1072787/68910425-aeeffa00-078c-11ea-96ed-3e50a39f97ed.png)

![image](https://user-images.githubusercontent.com/1072787/68910436-b44d4480-078c-11ea-8bf9-fd8637bd91f8.png)

![image](https://user-images.githubusercontent.com/1072787/68910445-b8796200-078c-11ea-90c8-5448dce37f35.png)


当然，用 GPU 挖矿意义不大，网页还是适合挖 CPU 算法的币。

不过 GPU 这种通用的硬件，显然更适合破解密码，比如 WiFi 密码。Shader 好好优化下，破解速度可以达到 hashcat 一半左右。论坛 XSS 每个用户贡献几分钟，还是不错的~ WiFi 破解后，还可以劫持流量植入 JS，于是又可以增加一些算力~

> 关于 Service Worker 各种玩法，可参考 https://github.com/etherdream/sw-sec



# 2018-9-28

思考题：如何实现一个网页版的 CPU 跑分程序，并且带有用户排行榜功能。（重点是确保用户不易作弊，比如自定义提交成绩；以及不能使用多核 CPU 甚至 GPU 来加速，只拼单核性能）

> 哪些密码学算法是无法利用多线程加速的，并且能在网页里高效率运行？



# 2018-9-13

《大航海时代 2》风格的 Google Map

Demo: https://jsfiddle.net/84j1cvt9/29/

![image](https://user-images.githubusercontent.com/1072787/68910989-9680df00-078e-11ea-9d69-d4c2c80796d9.png)

![image](https://user-images.githubusercontent.com/1072787/68910993-9a146600-078e-11ea-8023-e550f80d3b27.png)





# 2018-9-12

Win1.0 的 calc.exe（非原创）

![image](https://user-images.githubusercontent.com/1072787/68911012-a1d40a80-078e-11ea-9128-2ff1a5363326.png)

Demo: https://classicreload.com/Windows-1-01.html

> Win1.0 只比 Win10 差一点而已~ 



# 2018-9-11

直到现在，仍有相当多的用户甚至开发者都不知道【允许三方 Cookie】存在多大的危险~

一句话概况【允许三方 Cookie】的危险：在不安全的网络环境下，访问任何一个 HTTP 页面，各大网站 Cookie 可能瞬间被中间人拿到。

![image](https://user-images.githubusercontent.com/1072787/68911051-c03a0600-078e-11ea-96e3-01c89173af92.png)


> 原理说过很多次了，不再解释



# 2018-9-7

出个思考题：尝试读取 obj 中那个带随机数的隐藏属性

```js
var obj = {}

Object.defineProperty(obj, '$' + Math.random(), {
  get: () => alert('You win!'),
  enumerable: false,
})

// write code here:
```

Demo: https://jsfiddle.net/2c358wxp/

> 有多个答案



# 2018-8-17

有什么场合，必须使用 ES6 的 Reflect?

答案：https://github.com/EtherDream/web-frontend-magic/issues/2


# 2018-8-1

思考题：用最少的字符实现下图效果。

（鼠标悬浮在超链接上，状态栏显示的是网站 A，但点击进入的却是网站 B）

![image](https://user-images.githubusercontent.com/1072787/68911068-c8924100-078e-11ea-9be1-4aa221ea20fb.png)

![image](https://user-images.githubusercontent.com/1072787/68911082-cfb94f00-078e-11ea-85b2-5d41b2ed2f92.png)

当然这个是上古时代的黑魔法了，稍懂前端的都知道怎么实现。所以这里只问最短的实现~

目前想到的是 55 字符（包括目标域名）：https://www.etherdream.com/FunnyScript/statusbar_spoof_2.html

----

讲解：超链接和 `location` 很像，都有 `href`、`host` 等 URL 属性。不少人以为只有 `href` 属性可赋值，事实上其他属性也可以。

比如想把当前页面从 http 跳转到 https，只需修改 `protocol` 即可：

```js
location.protocol = 'https:'
```

另外 HTML 内联事件默认套有 `with(this)`，所以可省略 `this.` 这几个字符。



# 2018-7-27

一种检测 HTTPS 中间人降级攻击的猥琐思路~ ​​​​

```html
<script>
 if ('https://a.com'.startsWith('http:')) {
   // send_log('SSLStrip detected')
 }
</script>
```

在公司的脚本上尝试后发现竟然有不少日志，看来这类攻击目前还是存在的。。。

> HTTPS 中间人降级攻击，本质上就是把流量中的 `https://` 替换成 `http://`。所以又称 -1s 攻击~



# ​​​​2018-7-26

由于大部分用户都不会开启浏览器 `Do Not Track`，因此少数开了的用户反而成了另类，被用于浏览器指纹特征了。。。



# 2018-7-20

思考题：在 JS 中用 0 0 0 0 四个常量计算 24。只能用符号，表达式越短越好~ ​​​​

> 目前已知最短的答案 14 字符。https://jsfiddle.net/qLo2jg7x/



# 2018-6-6

BitInt 出现后，不用死循环也能实现 CPU 100% 的效果了。一个 2 层幂塔就能卡死浏览器~

```js
9n ** 9n ** 9n > 0
```

----

为什么这里要加 > 0 的判断，因为

```
9 ^ 9 ^ 9 ≈ 4.28 x 10^369693099
```

这个数字，光是位数就有 `369,693,099` 之多，相当于控制台要输出一个 300 多兆的字符串！所以为了不在输出时卡死，于是加了个判断。

（事实上基本等不到字符串输出那步）



# 2018-5-22

nodejs 恶作剧：给系统创建一个叫 `node_modules` 的用户，然后 `npm install` 就无法使用了~~~

![image](https://user-images.githubusercontent.com/1072787/68911116-ea8bc380-078e-11ea-8860-666775ecd005.png)

![image](https://user-images.githubusercontent.com/1072787/68911122-ee1f4a80-078e-11ea-84c7-c5fc9374764d.png)


> 如果真遇到项目文件夹之外有 `node_modules` 的话，只要在 `npm install` 时加上参数 `--prefix path` 就可以强制指定 `node_modules` 的路径。



# 2018-5-21

之前遇到个网站 WAF 的 JS 脚本，代码经过多次动态混淆。

为了防止被人调试，前端不断执行 debugger 指令，然后检测执行用时 ——  如果控制台开着，debugger 指令会触发断点，用时显然高出平常。于是 JS 会给后端发送日志，然后 IP 就被网站 ban 了。

```js
setInterval(function() {
  var t1 = Date.now();
  debugger;
  var t2 = Date.now();
  if (t2 - t1 > 100) {
    console.log('debug detected');
    // send_log('ban this ip');
  }
}, 500);
```

简单演示：https://codepen.io/anon/pen/rvPYxR?editors=0010

当然，在经过几次 IP 更换后，很快就摸索出了一个超级简单的规避方案。猜猜是如何实现~

----

其实非常简单，禁用调试器断点功能就可以，比如 Chrome DevTool 的箭头图标。

<img src="https://user-images.githubusercontent.com/1072787/68911162-0a22ec00-078f-11ea-9f3a-46767824050e.png" height="150">


不过想要一次成功，还是需要些小技巧的，毕竟刚打开 DevTool 的时候还是会触发 debugger 断下来的：

![image](https://user-images.githubusercontent.com/1072787/68911167-0becaf80-078f-11ea-9346-b2ac0fc1d405.png)

所以正确的打开方式，应该先随便开一个网页，打开 DevTool 禁用断点：

![image](https://user-images.githubusercontent.com/1072787/68911171-0e4f0980-078f-11ea-83b3-2d4c137b1d7f.png)

然后再打开想要访问的网页，这时 debugger 就不会触发了：

![image](https://user-images.githubusercontent.com/1072787/68911176-10b16380-078f-11ea-9020-d3480172c6cd.png)



# 2018-5-21

大写的 αß 服不服~

```js
'αß'.toUpperCase()    // "ΑSS"
```

> 类似的还有 `'ﬃ'.toUpperCase() === 'FFI'`。然而 `'嬲'.toUpperCase() !== '男女男'` ~



# 2018-5-21

随着 JS 引擎优化能力的提升，越来越多的性能黑魔法将会消失~

<img src="https://user-images.githubusercontent.com/1072787/68911351-af3dc480-078f-11ea-8aea-61cc09007b5c.png" height="250">

<img src="https://user-images.githubusercontent.com/1072787/68911352-b1078800-078f-11ea-977d-1aeae6473a55.png" height="100">



# 2018-5-15

思考题：定义 x 使得满足条件判断，字数越少越好。目前我的答案是 28 个字符，看看有没有更短的~

```js
const x = _______
const win = ('a' in x) && !('a' in x)
console.log(win)    // true
```

答案：https://jsfiddle.net/r6gk1ob8/


# 2018-5-14

思考题：有些 JS 代码混淆后，会变成 `eval(一大坨代码)` 的形式。当然解密也非常容易，把 `eval` 换成 `console.log` 就能原形毕露。

下面请思考，如何在「一大坨代码」中加入陷阱，跟踪有哪些人把 `eval` 换成了其他的函数？

答案：https://codepen.io/anon/pen/odMbBX?editors=0010 

（`eval` 换成 `console.log` 即可触发陷阱）



# 2018-5-14

如何使用 JS 缓解网站 DDOS 的攻击？

https://yq.aliyun.com/articles/236585

大致原理：网页多用强缓存（目前是 Service Worker），发生故障时可毫秒级切换流量到 N 个后备节点。网站被打垮也只影响新用户，老用户照样可以流畅访问~

> 当然这其中细节问题很多，以后会在 https://github.com/EtherDream/js-anti-ddos 中讨论。



# 2018-5-8

相册里翻到个上古时代用 `<SCRIPT LANGUAGE=VBSCRIPT>` 写的魔方小游戏。。。猜猜是用什么黑科技在 IE6 上实现伪 3D 的~

<img src="https://user-images.githubusercontent.com/1072787/68911371-bbc21d00-078f-11ea-89e8-8937b8f2411b.png" height="300">

演示：https://www.etherdream.com/creative/BaiduCube.html

> DXImageTransform.Microsoft.Matrix。当年硬着头皮用初中数学知识琢磨 Martix 的计算规律，陆续花了几个月才写出来~



# 2018-5-6

思考题：JS 如何自我检测代码是否被人修改？

举个栗子，有个脚本混淆后去除了换行、缩进等格式，并且变量名故意弄得特别长，干扰分析。这时破解者通常会格式化代码，然后变量名短化，保存后再运行分析。所以，脚本如何自我检测是否被篡改？

----

分享之前写的一个 PPT[《前端加密与混淆》](https://github.com/EtherDream/myppt/blob/master/%E5%89%8D%E7%AB%AF%E5%8A%A0%E5%AF%86%E4%B8%8E%E6%B7%B7%E6%B7%86.pdf)，其中提到 JS 如何检测自身模块是否被篡改。以及检测到异常情况后，该如何优雅的让程序逐渐错误，而不是立即崩溃~


# 2018-5-4

分享曾经折腾的一种 JS 混淆思路：

```js
(function(id, name, age, lang) {
  var PTR = arguments

  console.log(id, name, age, lang)  // 1001 "alice" 20 "en"

  PTR[0] = 1002
  PTR[1] = 'bob'
  PTR[2] = 30
  PTR[3] = 'us'

  console.log(id, name, age, lang)  // 1002 "bob" 30 "us"

})(1001, 'alice', 20, 'en')
```

把函数中所有的局部变量都定义在形参里，然后创建一个叫 `PTR` 的变量（模拟指针）指向 `arguments`。

之后通过 `PTR[数字]` 即可读写局部变量，而不用对具体变量指名点姓的访问，增加分析难度~

当然，后来 `strict mode` 普及之后就再没考虑这方案了。 



# 2018-5-3

现代浏览器 referrer-policy 的出现，使得大量网站可当图床使用~

> 所以 referrer 为空也是一种盗链



# 2018-4-25

思考题：现有一款浏览器安全插件，会对 `performance.now` 接口进行 Hook，以降低 JS 获取到的时间精度，减少边信道攻击的风险。

该插件会将如下代码注入到页面最先运行（包括框架页、空白页等）：

```js
(function() {
  const obj = performance
  const rawfn = Performance.prototype.now

  Performance.prototype.now = function() {
    let val = rawfn.apply(obj, arguments)
    return ((val * 10) | 0) / 10    // 精度降低到 0.1ms
  }
})()
```

下面思考，有哪些方案可绕过该插件的防护，从而获取到原生的高精度时间。

并且对于这种绕过方案，怎样改进才能防范？

> 答案参考「有什么场合，必须使用 ES6 的 Reflect」的思路。另外，`new Event(0).timeStamp` 也可以获取高精度时间。



# 2018-4-23

记得很久前就有人提过，使用 localStorage 缓存 html、js 存在较大的安全隐患，万一出现 XSS 可能会导致恶意代码长期感染，从而形成 Rootkit。

尽管现在用 localStorage 的已经不多，但类似的缓存方案仍存在。目前最主流的方案是 Cache Storage + Service Worker，不少 PWA 都采用这种缓存方式。然而不幸的是，Cache Storage 同样存在上述安全隐患 —— 由于「Service Worker」和「页面」是共享同个 Cache Storage 的，因此一旦页面修改了 Storage 的资源，同样会影响 Service Worker 获取的内容。

我们随便找一个使用 Cache Storage + Service Worker 缓存的网站，几乎都存在这个问题。在页面里修改 Cache Storage 中的某个 JS 内容，之后重启浏览器，恶意代码仍然生效。

![image](https://user-images.githubusercontent.com/1072787/68911262-4f471e00-078f-11ea-8416-ffffa059f4f4.png)

![image](https://user-images.githubusercontent.com/1072787/68911269-52420e80-078f-11ea-895b-c7d6cd6c56c3.png)

![image](https://user-images.githubusercontent.com/1072787/68911270-54a46880-078f-11ea-976c-109f3a44741f.png)

![image](https://user-images.githubusercontent.com/1072787/68911273-5837ef80-078f-11ea-9a99-ee0fb260a8fc.png)



# 2018-4-19

尝试用指令 + 寄存器的思路，在 WebAssembly 中操作 JS 和 DOM~

![image](https://user-images.githubusercontent.com/1072787/68986481-bda4e280-085a-11ea-8c64-dfc1d2111d45.png)

Demo: https://webassembly.studio/?f=mxnrb7oofth



# 2018-4-18

思考题：上传文件的过程中，如果用户刷新或关闭页面，那么就前功尽弃了。之后用户还得重新打开文件选择框、重新上传。

下面请思考，如何实现：

1.用户只需打开一次文件选择框，之后即使刷新页面，也无需再选择文件，继续之前的上传？

2.用户即使关闭页面（但未退出浏览器），文件仍能持续传输一段时间？



# 2018-4-17

思考题：如何产生这种效果~

（不能重写 document.cookie，而是真有这么多重复的键值）

![image](https://user-images.githubusercontent.com/1072787/68911283-6259ee00-078f-11ea-8a89-217fa6000f96.png)

[Demo](https://www.etherdream.com/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/x/multi_cookie.html)


# 2018-4-8

整理文件时发现过去写的一个 JS 思考题：用最简单的办法让 `console.log(1)` 输出 `0`。被自己的答案惊呆。。。

<img src="https://user-images.githubusercontent.com/1072787/68911291-6dad1980-078f-11ea-882b-0e2f1a86f47c.png" height="100">

> 写的不严谨，应该说是返回 0，而不是输出 0。



# 2018-3-22

分享一个网站去中心化的探索~

原理：把网站资源上传到各大图床、相册上，前端通过 Service Worker 代理，然后从图片中还原出原始数据。于是自己的网站只需放置一个极小的 HTML 和 JS 文件，即可享受无限带宽。

演示：https://fanhtml5.github.io/

因为 Service Worker 具有持续性，所以只要访问过一次，之后就算网站挂了，用户仍能正常访问 —— 除非所有的备用节点都挂了。（装上 Service Worker 就去中心了）

此外，这个方案还可用于 DDOS 防御 —— 网站即使被打垮了，但之前访问过的用户，仍能访问。

相比传统 DNS 负载均衡至少有好几秒的延时，Service Worker 通过 JS 实现节点切换，延时可以精确到 ms 级别；并且 DNS 协议是公开的，很容易遍历对应的 IP，而 JS 则可加上代码混淆，增加分析的难度。。。

关于前端负载均衡，之前写的一些思路：https://yq.aliyun.com/articles/236582

----

打开浏览器隐身模式，访问 https://fanhtml5.github.io/big-pic.png 出现的是个图片，但用 curl 访问并不是图片。这就是这个方案有趣的地方~



# 2018-3-17

打开浏览器 console 会触发浏览器加载 js 尾部的 source map url，于是后端就可以收到日志。

并且动态创建的脚本也支持 source map url，所以 url query 里还可以加上用户相关的参数，跟踪哪些人开了控制台~


 ​​​​
# 2018-3-9

控制台 FBI WARNING 恶作剧~

![image](https://user-images.githubusercontent.com/1072787/68911305-79004500-078f-11ea-996a-293c07b31973.png)

Demo: https://jsfiddle.net/tb54ywa3/ （暂只支持 Chrome）



# 2018-3-9

分享跳转 opener 的另类玩法~

大家知道 Service Worker 可以持续运行一段时间，可以用来挖矿、破解密码等等。。。不过 XSS 没法安装 SW，因为 SW 的脚本必须和页面同源；iframe 也没法安装，因为浏览器规定只有主页面才可以安装。

所以，唯一可行的就是把 `opener` （假如存在的话）跳转到自己网站上，装完 SW 再自动返回~

> 关于 Service Worker 各种玩法，可参考 https://github.com/etherdream/sw-sec



# 2018-1-25

红白机游戏《超级玛丽》汇编指令重编译成 JavaScript：

![image](https://user-images.githubusercontent.com/1072787/68911322-8e756f00-078f-11ea-8c4d-e6b85eb78d53.png)

Demo：https://www.etherdream.com/FunnyScript/smb-js/game.html

原理：https://www.cnblogs.com/index-html/p/6492418.html


# 2017

[2017.md](https://github.com/EtherDream/web-frontend-magic/blob/master/2017.md)
