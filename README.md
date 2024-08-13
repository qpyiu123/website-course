# website-course
一个建站教程的markdown  
如果有什么问题请提交lssues,或者直接提交修改好的md文件发Pull  


# 建站前的基础知识
什么是 web 服务器？
------------

在这篇文章中我们会重温什么是 web 服务器，它们如何工作，以及为什么它们很重要。

[概述](https://developer.mozilla.org/zh-CN/docs/Learn/Common_questions/Web_mechanics/What_is_a_web_server#概述)
-----------------------------------------------------------------------------------------------------------

_web 服务器_一词可以代指硬件或软件，或者是它们协同工作的整体。

1.  硬件部分，web 服务器是一台存储了 web 服务器软件以及网站的组成文件（比如，HTML 文档、图片、CSS 样式表和 JavaScript 文件）的计算机。它接入到互联网并且支持与其他连接到互联网的设备进行物理数据的交互。
2.  软件部分，web 服务器包括控制网络用户如何访问托管文件的几个部分，至少是一台 _HTTP 服务器_。一台 HTTP 服务器是一种能够理解 [URL](https://developer.mozilla.org/zh-CN/docs/Glossary/URL)（网络地址）和 [HTTP](https://developer.mozilla.org/zh-CN/docs/Glossary/HTTP)（浏览器用来查看网页的协议）的软件。一个 HTTP 服务器可以通过它所存储的网站域名进行访问，并将这些托管网站的内容传递给最终用户的设备。

基本上，当浏览器需要一个托管在网络服务器上的文件的时候，浏览器通过 HTTP 请求这个文件。当这个请求到达正确的 web 服务器（硬件）时，_HTTP 服务器_（软件）收到这个请求，找到这个被请求的文档（如果这个文档不存在，那么将返回一个 [404](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/404) 响应），并把这个文档通过 HTTP 发送给浏览器。

![通过 HTTP 的客户/服务器连接的基本表示方法](建站前的基础知识_web-server.svg)

要发布一个网站，你需要一个静态或动态的服务器。

**静态 web 服务器**（static web server）由一个计算机（硬件）和一个 HTTP 服务器（软件）组成。我们称它为“静态”是因为这个服务器把它托管文件的“保持原样”地传送到你的浏览器。

**动态 web 服务器**（dynamic web server）由一个静态的网络服务器加上额外的软件组成，最普遍的是一个_应用服务器_和一个_数据库_。我们称它为“动态”是因为这个应用服务器会在通过 HTTP 服务器把托管文件传送到你的浏览器之前会对这些托管文件进行更新。

举个例子，要生成你在浏览器中看到的最终网页，应用服务器或许会用一个数据库中的内容填充一个 HTML 模板。像 MDN 或维基百科这样的网站有成千上万的网页。通常情况下，这类网站只由几个 HTML 模板和一个巨大的数据库组成，而不是成千上万的静态 HTML 文档。这种设置使得维护和提供内容更加容易。

关于协议的部分我就不展开说太多了，毕竟大家不是都是计算机系的，不过感兴趣的话我推荐可以去看一下《计算机网络》-第7版-谢希仁  学校图书馆也可以借。单纯的建站并不需要太多底层网络知识，大概了解即可。

HTTP/HTTPS协议
------------

       网络中有着各种各样的协议，通过这些协议我规范我们才能够在全球范围内“无障碍”的访问网站。正如前文所述我们在这里只关心和建站关系最大的两个应用层协议即：HTTP协议和HTTPS协议（其实FTP也很常用但是对我们单纯建简单服务器来说不需要太关心）

**HTTP有以下几个特征**：

*   [HTTP 是简约的](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Overview#http_是简约的)
    *   大体上看，HTTP 被设计得简单且易读，尽管在 HTTP/2 中，HTTP 消息被封装进帧（frame）这点引入了额外的复杂度。HTTP 报文能够被人读懂并理解，向开发者提供了更简单的测试方式，也对初学者降低了门槛。
*   [HTTP 是可扩展的](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Overview#http_是可扩展的)
    *   在 HTTP/1.0 中引入的 [HTTP 标头](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers)让该协议易于扩展和实验。只要服务器客户端之间对新标头的语义经过简单协商，新功能就可以被加入进来。
*   [HTTP 无状态，但并非无会话](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Overview#http_无状态，但并非无会话)
    *   HTTP 是无状态的：在同一个连接中，两个执行成功的请求之间是没有关系的。这就带来了一个问题，用户没有办法在同一个网站中进行连贯的交互，比如在电商网站中使用购物车功能。尽管 HTTP 根本上来说是无状态的，但借助 HTTP Cookie 就可使用有状态的会话。利用标头的扩展性，HTTP Cookie 被加进了协议工作流程，每个请求之间就能够创建会话，让每个请求都能共享相同的上下文信息或相同的状态。
*   [HTTP 和连接](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Overview#http_和连接)
    *   连接是由传输层来控制的，因此从根本上说不属于 HTTP 的范畴。HTTP 并不需要底层的传输协议是面向连接的，仅仅需要它是_可靠的_，或不会丢失消息（至少，某个情况下告知错误）。在互联网两个最常用的传输协议中，TCP 是可靠的而 UDP 不是。HTTP 因此而依靠于 TCP 的标准，即面向连接的。
    *   在客户端与服务器能够传递请求、响应之前，这两者间必须建立 TCP 连接，这个过程需要多次往返交互。HTTP/1.0 默认为每一对 HTTP 请求/响应都打开一个单独的 TCP 连接。当需要接连发起多个请求时，工作效率相比于它们之间共享同一个 TCP 连接要低。
    *   为了减轻这个缺陷，HTTP/1.1 引入了_流水线_（已被证明难以实现）和_持久化连接_：可以通过 [`Connection`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Connection) 标头来部分控制底层的 TCP 连接。HTTP/2 则更进一步，通过在一个连接中复合多个消息，让这个连接始终活跃并更加高效。
    *   为了设计一种更匹配 HTTP 的传输协议，各种实验正在进行中。例如，Google 正在测试一种基于 UDP 构建，更可靠、高效的传输协议——[QUIC](https://zh.wikipedia.org/wiki/QUIC)。

看到这是不是还是一头雾水😨所以我们来简化一下

简而言之HTTP的模式很像在餐厅点菜，用户（食客）不点餐服务员就不会给你上菜，同样让你点餐的服务员也不会记住你以前点过什么菜（当然在现代web中有办法让记住用户的上下文，不过这不是本文要讨论的内容）。

理解了HTTP的基本特征我们就能明白我们的服务器是拿来干嘛的，在点餐的例子里面我们的服务器就是这个餐厅——给用户提供餐食（web页面）我们写的前端页面就是菜品，服务器的作用就是吧这些东西交到用户手上。

 而HTTPS协议就是HTTP的加密版本，用来解决HTTP的明文传输问题，目前大多数网站使用的都是HTTPS协议了，不过HTTPS需要域名和SSL证书，域名基本上需要去域名供应商购买我们单纯学习服务器搭建不需要使用域名，到时候如果有人有需要域名的可以找部长借一下，我的域名已经配置好了SSL证书了到时候有需要的话可以给大家用一下。

ip地址和端口
-------

说完了基础的协议我们就来说说关于地址和端口的一些事

在前文我们有了一套“协议”可以用来在两台计算机直接进行信息的交流。不过能够交流还不够，现在的问题是在互联网上有很多的计算机，我们需要能够用一种方式来准确的找到我们想“交流”的对象，就像依据导航来找到“餐厅”一样。这样一来我们就有了ip地址

> **IP地址（Internet Protocol Address）是指互联网协议地址，又译为网际协议地址**。IP地址类似于电话号码：第一部分是区号，指定了一个非常大的区域；第二部分是前缀，将范围缩小到本地呼叫区域；最后一部分是用户号码，将范围缩小到具体的连接。
> 
> 也可以把IP地址比作一个门牌号，每家每户都会有一个门牌号，而且是唯一的，只有地址唯一，邮递员才能准确地把我们的包裹送到，**IP地址也是全球唯一的(注意，由于国内特殊网络环境家庭宽带通常没有公网ip)**，我们这里说的IP地址是公网IP地址。

ip地址分为两个版本ipv4和ipv6

这两个版本最主要的差异就是总地址数目不同，不过也有一些其他的差异比如“无NAT”（严格意义上来说还是有ipv6 NAT 技术的）

> **IPv4 特点：**
> 
> **32 位地址空间：**IPv4 使用 32 位地址表示 IP 地址，它由 4 个十进制数（0-255）组成，每个数之间使用句点分隔。例如，192.168.0.1 是一个 IPv4 地址。
> 
> **有限的地址空间：**由于 IPv4 的地址长度有限，它的地址空间有限。IPv4 总共提供约 43 亿个地址，然而随着互联网的快速发展，这个地址空间很快耗尽。
> 
> **NAT（网络地址转换）：**为了解决 IPv4 地址不足的问题，引入了网络地址转换（NAT）技术，允许多个设备共享同一个公共 IPv4 地址。NAT 在一定程度上缓解了 IPv4 地址短缺问题。
> 
> **广泛应用：**由于其成熟性和广泛应用，IPv4 目前仍然是互联网上更常见的 IP 地址协议。
> 
> **IPv6 特点:**
> 
> **128 位地址空间：**IPv6 使用 128 位地址表示 IP 地址，它由 8 组四位十六进制数（0-9、a-f）组成，每个组之间使用冒号分隔。例如：2001:0db8:85a3:0000:0000:8a2e:0370:7334 是一个 IPv6 地址。
> 
> **持续的地址供应：**IPv6 的地址空间非常庞大，提供了约 340 亿亿亿亿个 IP 地址。这种庞大的地址空间可以满足未来互联网发展的需求。
> 
> **简化的地址分配：**IPv6 采用了简化的地址分配方式，使得分配更加高效和灵活。它支持自动配置和动态主机配置协议，简化了网络管理员对地址分配的管理。
> 
> **改进的安全性：**IPv6 在设计上考虑了安全性，包括内置的加密和身份验证机制。这有助于提高网络的安全性和隐私保护。

各位想要在自家电脑上建站的同学们在这里要注意一下了，在中国由于互联网起步较晚所以分得的ipv4地址数量较少（只有3亿个），显然远远大于国内的联网设备数量因此国内广泛使用了NAT技术来解决问题（也就是所谓大内网）。也就是相当于大家在公网上公用一个ip（这里点名批评移动，你个\*\*东西搞\*\*的NAT3真的不要脸啊，趁早似罢，虽然说国内其他运营商也差不多基本上都依托答辩）因此无法从外部直接访问到内部的设备（但是由内向外是可以的），所以使用ipv4在家庭宽带下直接搭建服务器基本上不现实。（其实也不是完全不能解决，具体的解决方法在建站篇会详细说）

另外提一嘴，国内是禁止个人无备案建站的，因此封掉了不少和服务器有关的端口，如果你买了一个在国内的云服务器并且在上面挂了面向公网的web页面但是没备案的话属于违法行为

端口

接下来我们说说端口的事，有了地址那么我们就需要通过浏览器去访问服务器提供的服务，但是有的时候往往一台服务器提供了多个服务，那么我们怎么才能访问到我们需要的服务呢？

接下来我们用一个典型案例来说明如何使用“端口”来限定和特定的应用程序交互

> 蓬莱山辉夜整天宅在家里一个人打游戏十分无聊，想要找大家一起玩，于是在“雨雲”服务器提供商购买了使用“酷睿™14900k氧化缩肛王”单核主频高达6ghz的4c8t服务器用于开设拥有400+mod的”GTNH整合包“的mc服务器。同时由于看过了不知道之前在哪里捡来的一本《零基础web开发》之后做了一个个人博客，趁此机会想要同时挂载到vps上。在进行了一番操作之后她将web页面和mc服务器同时运行在同一个vps上，测试时发现只要在浏览器输入vps地址"xxx.xxx.xxx.xxx"就可以连接到web页面，而在mc“添加服务器”输入框中则需要输入形如"xxx.xxx.xxx.xxx:25565"的地址后方能加入服务器

显然应用程序没有那么智能能够自行找到服务器上所需要的服务，因此我们需要端口来让确定应用程序的“位置”（这里可能会有人觉得说可以用进程pid来解决这个问题，但是进程pid在每次应用启动后是会变化的因此并不方便），但是我们发现我们在浏览器浏览网站的时候链接后面可没有跟着端口号啊

> 协议的缺省端口
> 
> 当你没有显式的在 url 中输入端口时, 浏览器实际上会根据所用的协议来为你指定一个缺省端口:
> 
> *   如果是 http 协议, 就使用 80 端口
> *   如果是 https 协议, 就使用 443 端口

所以说其实并不是我们不需要输入端口只是浏览器帮我们自己填了而已，程序员都是懒鬼.jpg

当然HTTP/HTTPS协议本身并不是说一定要走80/443端口，这也和下一部分一个在家庭宽带建一个外网可访问的站点可以使用的一种方法有关。当然以前有在自家电脑上开过mc服务器的或者连过机的同学已经想到其中的一个办法了，不过我们为了教程的完整性在下一节依旧会尽量吧全部的方法交给大家

看完这一节的内容大家想必已经对服务器和协议有一定的了解了，在下一节我们就开始搭建自己的服务器。
