# 网络&与安全汇总

- [http和https]
    http是超文本传输协议，是互联网上应用最为广泛的一种网络协议，是一个客户端和服务器端请求和应答的标准，用于从WWW服务器传输超文本到本地浏览器的传输协议，它可以使浏览器更加高效，使网络传输减少。 [而对于https，他是以安全为目标的http通道，是http的安全版，在http中加入了ssl层，https的安全基础是ssl]。
    http的连接很简单的，是无状态的，http传输的数据都是没有加密的，也就是明文的，网景公司设置了ssl协议来对http协议传输的数据进行加密处理，所以说https协议是由http和ssl协议构建的可进行加密传输和身份认证的网络协议，比http协议的安全性更高。
    https协议是需要证书的，费用较高，http是超文本传输协议，信息是明文传输，https是具有安全性的ssl加密传输协议，使用不同的链接方式，端口不同，一般来说，[http协议的端口是80，而https的端口为443]。
    使用https协议可以认证用户和服务器，确保数据发送到正确的客户端和服务器端。对于https协议的工作原理，客户端在使用https方式与web服务器通信时，首先客户使用https url访问服务器，则要求web服务器建立ssl链接，web服务器接收到客户端的请求之后，会将网站的证书，返回或者说传输给客户端，然后客户端和web服务器端开始协商ssl链接的安全等级，也就是加密等级，通过双方协商一致的安全等级，建立会话密钥，然后通过网站的公钥来加密会话密钥，并传送给网站.web服务器通过自己的私钥解密出会话密钥，通过会话密钥加密与客户端之间的通信
    建议使用https，比起同等http网站，使用https加密的网站在搜索结果中的排名会更高哦.
    https协议握手阶段比较费时，会使页面加载时间延长50%,增加10%~20%的耗电，https缓存也不如http高效，会增加数据开销，ssl证书也是需要钱的，功能越强大证书费用越高，ssl证书时需要绑定ip的，不能再同一个ip上绑定多个域名。


- [TCP和UDP的区别]
    TCP是面向连接的，UPD是无连接的，即发送数据前不需要先建立连接；TCP提供可靠的服务，通过TCP连接传送的数据，无差错，不丢失，不重复，且按序到达，UDP尽最大努力交付，即不保证可靠交付。
    并且因为 tcp 可靠，面向连接，不会丢失数据因此适合大数据量的交换；TCP面向字节流，实际上是TCP把数据看成一连串无结构的字节流，UDP是面向报文的，UDP没有拥塞控制，因此，网络出现拥塞不会使原主机的发送速率降低，因此会出现丢包，对实时的应用很有用，比如IP电话和视频会议等
    每一条TCP连接，只能是1对1的，UDP支持1对1，1对多，多对1，多对多的交互通讯；TCP的首部开销为20字节，而UDP的只有8字节；TCP面向连接的可靠性传输，而UDP是不可靠的。
                     UDP                                            tcp
    是否连接         无连接                                         面向连接
    是否可靠       不可靠传输，不使用流量控制和拥塞控制       可靠传输，使用流量控制和拥塞控制
    连接对象个数   支持1对1，1对多，多对1，多对多                       1对1
    传输方式        面向报文                                        面向字节流
    首部开销      首部开销较少，8字节                          首部最少20字节，最大60字节
    适用场景     实时应用（IP电话、视频会议、直播等）            适用于要求可靠传输的应用，如文件传输

- [http状态码]
常见的状态码
    200 -请求成功
    301 - 资源（网页等）被永久转移到其他URL
    404 -请求的资源（网页等）不存在
    500 -服务器内部错误

1**	信息，服务器收到请求，需要请求者继续执行操作
    100	Continue	继续。客户端应继续其请求
    101	Switching Protocols	切换协议。服务器根据客户端的请求切换协议。只能切换到更高级的协议，例如，切换到HTTP的新版本协议
2**	成功，操作被成功接收并处理  
    200	OK	请求成功。一般用于GET与POST请求
    201	Created	已创建。成功请求并创建了新的资源
    202	Accepted	已接受。已经接受请求，但未处理完成
    203	Non-Authoritative Information	非授权信息。请求成功。但返回的meta信息不在原始的服务器，而是一个副本
    204	No Content	无内容。服务器成功处理，但未返回内容。在未更新网页的情况下，可确保浏览器继续显示当前文档
    205	Reset Content	重置内容。服务器处理成功，用户终端（例如：浏览器）应重置文档视图。可通过此返回码清除浏览器的表单域
    206	Partial Content	部分内容。服务器成功处理了部分GET请求
3**	重定向，需要进一步的操作以完成请求
    300	Multiple Choices	多种选择。请求的资源可包括多个位置，相应可返回一个资源特征与地址的列表用于用户终端（例如：浏览器）选择
    301	Moved Permanently	永久移动。请求的资源已被永久的移动到新URI，返回信息会包括新的URI，浏览器会自动定向到新URI。今后任何新的请求都应使用新的URI代替
    302	Found	临时移动。与301类似。但资源只是临时被移动。客户端应继续使用原有URI
    303	See Other	查看其它地址。与301类似。使用GET和POST请求查看
    304	Not Modified	未修改。所请求的资源未修改，服务器返回此状态码时，不会返回任何资源。客户端通常会缓存访问过的资源，通过提供一个头信息指出客户端希望只返回在指定日期之后修改的资源
    305	Use Proxy	使用代理。所请求的资源必须通过代理访问
4**	客户端错误，请求包含语法错误或无法完成请求
    400	Bad Request	客户端请求的语法错误，服务器无法理解
    401	Unauthorized	请求要求用户的身份认证
    402	Payment Required	保留，将来使用
    403	Forbidden	服务器理解请求客户端的请求，但是拒绝执行此请求
    404	Not Found	服务器无法根据客户端的请求找到资源（网页）。通过此代码，网站设计人员可设置"您所请求的资源无法找到"的个性页面
    405	Method Not Allowed	客户端请求中的方法被禁止
5**	服务器错误，服务器在处理请求的过程中发生了错误   
    500	Internal Server Error	服务器内部错误，无法完成请求
    501	Not Implemented	服务器不支持请求的功能，无法完成请求
    502	Bad Gateway	作为网关或者代理工作的服务器尝试执行请求时，从远程服务器接收到了一个无效的响应
    503	Service Unavailable	由于超载或系统维护，服务器暂时的无法处理客户端的请求。延时的长度可包含在服务器的Retry-After头信息中
    504	Gateway Time-out	充当网关或代理的服务器，未及时从远端服务器获取请求
    505	HTTP Version not supported	服务器不支持请求的HTTP协议的版本，无法完成处理

- [http常见的请求头]
    Accept  可接受的响应内容类型（Content-Types）  Accept: text/plain
    Accept-Charset    可接受的字符集                Accept-Charset: utf-8
    Accept-Encoding 可接受的响应内容的编码方式。    Accept-Encoding: gzip, deflate
    Accept-Language 可接受的响应内容语言列表。      Accept-Language: en-US
    Accept-Datetime  可接受的按照时间来表示的响应内容版本
    Authorization  用于表示 HTTP 协议中需要认证资源的认证信息       Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==
    Cache-Control  用来指定当前的请求/回复中的，是否使用缓存机制。      Cache-Control: no-cache
    Connection  客户端（浏览器）想要优先使用的连接类型                  Connection: keep-alive    Connection: Upgrade
    Cookie  由之前服务器通过Set-Cookie设置的一个HTTP协议Cookie          Cookie: $Version=1; Skin=new;
    Content-Length  以 8 进制表示的请求体的长度                         Content-Length: 348
    Content-MD5 请求体的内容的二进制 MD5 散列值（数字签名），以 Base64 编码的结果   
    Content-Type  请求体的 MIME 类型 （用于 POST 和 PUT 请求中）                Content-Type: application/x-www-form-urlencoded
    Date 发送该消息的日期和时间                                             Date: Tue, 15 Nov 1994 08:12:31 GMT
    Expect  表示客户端要求服务器做出特定的行为                      Expect: 100-continue
    From  发起此请求的用户的邮件地址
    Host    服务器的域名(用于虚拟主机 )，以及服务器所监听的传输控制协议端口号   Host: en.wikipedia.org:80   Host: en.wikipedia.org

   
    If-Modified-Since  允许在对应的资源未被修改的情况下返回 304 未修改      If-Modified-Since: Sat, 29 Oct 1994 19:43:31 GMT
    If-None-Match 允许在对应的内容未被修改的情况下返回 304 未修改（ 304 Not Modified ），参考 超文本传输协议 的实体标记 If-None-Match: "737060cd8c284d8af7ad3082f209582d"
    If-Range 如果该实体未被修改过，则向返回所缺少的那一个或多个部分。否 则，返回整个新的实体
    If-Unmodified-Since 仅当该实体自某个特定时间以来未被修改的情况下，才发送回应。
    Max-Forwards 限制该消息可被代理及网关转发的次数。
    Range  表示请求某个实体的一部分，字节偏移以 0 开始。
    User-Agent 浏览器的身份标识字符串   User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:12.0) Gecko/20100101 Firefox/21.0
    Upgrade 要求服务器升级到一个高版本协议。
    Origin 发起一个针对 跨来源资源共享 的请求   Origin: http://www.example-social-network.com

- [强缓存，协商缓存]
    缓存分为两种：强缓存和协商缓存，根据响应的 header 内容来决定
    强缓存
        获取资源形式 从缓存取
        状态码 200（from cache）
        发送请求到服务器 否，直接从缓存取
    协商缓存
        获取资源形式 从缓存取
        状态码 304（not modified）
        发送请求到服务器 是，通过服务器来告知缓存是否可用
    强缓存相关字段有 expires，cache-control。如果 cache-control(http1.1) 与 expires (http1.0)同时存在的话， cache-control 的优先级高于 expires。
    Control:max-age=3600  no-cache 不使用本地缓存。需要使用协商缓存。
    
    协商缓存相关字段有 Last-Modified(第一次请求返回的)/If-Modified-Since(下次请求带上的)，Etag(第一次请求返回的)/If-None-Match（下次请求带上的）
    Last-Modified/If-Modified-Since 二者的值都是GMT格式的时间字符串， Last-Modified 标记最后文件修改时间， 下一次请求时，请求头中会带上 If-Modified-Since 值就是 Last-Modified 告诉服务器我本地缓存的文件最后修改的时间，在服务器上根据文件的最后修改时间判断资源是否有变化， 如果文件没有变更则返回 304 Not Modified ，请求不会返回资源内容，浏览器直接使用本地缓存。当服务器返回 304 Not Modified 的响应时，response header 中不会再添加的 Last-Modified 去试图更新本地缓存的 Last-Modified， 因为既然资源没有变化，那么 Last-Modified 也就不会改变；如果资源有变化，就正常返回返回资源内容，新的 Last-Modified 会在 response header 返回，并在下次请求之前更新本地缓存的 Last-Modified，下次请求时，If-Modified-Since会启用更新后的 Last-Modified。
    Etag/If-None-Match， 值都是由服务器为每一个资源生成的唯一标识串，只要资源有变化就这个值就会改变。服务器根据文件本身算出一个哈希值并通过 ETag字段返回给浏览器，接收到 If-None-Match 字段以后，服务器通过比较两者是否一致来判定文件内容是否被改变。与 Last-Modified 不一样的是，当服务器返回 304 Not Modified 的响应时，由于在服务器上ETag 重新计算过，response header中还会把这个 ETag 返回，即使这个 ETag 跟之前的没有变化。

    为什么要有 Etag？
    HTTP1.1 中 Etag 的出现主要是为了解决几个 Last-Modified 比较难解决的问题：
    一些文件也许会周期性的更改，但是内容并不改变(仅仅改变的修改时间)，这个时候我们并不希望客户端认为这个文件被修改了，而重新GET；
    某些文件修改非常频繁，比如在秒以下的时间内进行修改，(比方说1s内修改了N次)，If-Modified-Since 能检查到的粒度是秒级的，使用 Etag 就能够保证这种需求下客户端在1秒内能刷新 N 次 cache。
    某些服务器不能精确的得到文件的最后修改时间。

    优先级 Cache-Control  > expires > Etag > Last-Modified

    如何设置强缓存和协商缓存？
    后端服务器，写入代码逻辑中：
        res.setHeader('max-age': '3600 public')
        res.setHeader(etag: '5c20abbd-e2e8')
        res.setHeader('last-modified': Mon, 24 Dec 2018 09:49:49 GMT)
    Nginx 配置 add_header Cache-Control "max-age=3600"

    三级缓存原理（大白话）
    最后总结一下浏览器的三级缓存原理：
        先去内存看，如果有，直接加载
        如果内存没有，择取硬盘获取，如果有直接加载
        如果硬盘也没有，那么就进行网络请求
        加载到的资源缓存到硬盘和内存

-[说说 HTTP1.0/1.1/2.0 的区别?]()
    HTTP1.0(1996 )：
        HTTP 1.0 仅仅提供了最基本的认证，这时候用户名和密码还未经加密，因此很容易收到窥探。
        HTTP 1.0 被设计用来使用短链接，即每次发送数据都会经过 TCP 的三次握手和四次挥手，效率比较低。
        HTTP 1.0 只使用 header 中的 If-Modified-Since 和 Expires 作为缓存失效的标准。
        HTTP 1.0 不支持断点续传，也就是说，每次都会传送全部的页面和数据。
        HTTP 1.0 认为每台计算机只能绑定一个 IP，所以请求消息中的 URL 并没有传递主机名（hostname）。
    HTTP1.1( 1999)：
        HTTP 1.1 使用了摘要算法来进行身份验证
        HTTP 1.1 默认使用长连接，长连接就是只需一次建立就可以传输多次数据，传输完成后，只需要一次切断连接即可。长连接的连接时长可以通过请求头中的 keep-alive 来设置
        HTTP 1.1 中新增加了 E-tag，If-Unmodified-Since, If-Match, If-None-Match 等缓存控制标头来控制缓存失效。
        HTTP 1.1 支持断点续传，通过使用请求头中的 Range 来实现。
        HTTP 1.1 使用了虚拟网络，在一台物理服务器上可以存在多个虚拟主机（Multi-homed Web Servers），并且它们共享一个IP地址。
        虽然允许复用TCP连接，但是同一个TCP连接里面，所有的数据通信是按次序进行的，服务器只有处理完一个请求，才会接着处理下一个请求。如果前面的处理特别慢，后面就会有许多请求排队等着
        新增了一些请求方法
        新增了一些请求头和响应头
    HTTP2.0(2015)：
        二进制格式，HTTP 2.0 使用了更加靠近 TCP/IP 的二进制格式，而抛弃了 ASCII 码，提升了解析效率
        强化安全，由于安全已经成为重中之重，所以 HTTP2.0 一般都跑在 HTTPS 上。
        多路复用，即每一个请求都是是用作连接共享。一个请求对应一个id，这样一个连接上可以有多个请求。完全多路复用，而非有序并阻塞的、只需一个连接即可实现并行
        使用报头压缩，降低开销
        服务器推送
        头部压缩，由于 HTTP 1.1 经常会出现 User-Agent、Cookie、Accept、Server、Range 等字段可能会占用几百甚至几千字节，而 Body 却经常只有几十字节，所以导致头部偏重。HTTP 2.0 使用 HPACK 算法进行压缩。

- [说一下 GET 和 POST 的区别？]()
    GET在浏览器回退时是无害的，而POST会再次提交请求。
    GET产生的URL地址可以被Bookmark，而POST不可以。
    GET请求会被浏览器主动cache，而POST不会，除非手动设置。
    GET请求只能进行url编码，而POST支持多种编码方式。
    GET请求参数会被完整保留在浏览器历史记录里，而POST中的参数不会被保留。
    GET请求在URL中传送的参数是有长度限制的，而POST没有。
    对参数的数据类型，GET只接受ASCII字符，而POST没有限制。
    GET比POST更不安全，因为参数直接暴露在URL上，所以不能用来传递敏感信息。
    GET参数通过URL传递，POST放在Request body中
    
    对于GET方式的请求，浏览器会把http header和data一并发送出去，服务器响应200（返回数据）对于POST，浏览器先发送header，服务器响应100 continue，浏览器再发送data，服务器响应200 ok.注意：并不是所有浏览器都会在POST中发送两次包，Firefox就只发送一次

- [说说地址栏输入 URL 敲下回车后发生了什么?]()
    1、URL解析 判断你输入的是一个合法的URL 还是一个待搜索的关键词，并且根据你输入的内容进行对应操作
    2、DNS 查询（迭代查询）（本地域名服务器，根域名服务器，顶级域名服务器，权限域名服务器）获取到了域名对应的目标服务器IP地址
    3、TCP 连接 （3次握手）
        第一次握手：建立连接时，客户端发送syn包（syn=j）到服务器，并进入SYN_SENT状态，等待服务器确认；SYN：同步序列编号（Synchronize Sequence Numbers）。
        第二次握手：服务器收到syn包，必须确认客户的SYN（ack=j+1），同时自己也发送一个SYN包（syn=k），即SYN+ACK包，此时服务器进入SYN_RECV状态；
        第三次握手：客户端收到服务器的SYN+ACK包，向服务器发送确认包ACK(ack=k+1），此包发送完毕，客户端和服务器进入ESTABLISHED（TCP连接成功）状态，完成三次握手。
    4、HTTP 请求 
        当建立tcp连接之后，就可以在这基础上进行通信，浏览器发送 http 请求到目标服务器.请求的内容包括：请求行、请求头和请求主体
    5、响应请求 
        当服务器接收到浏览器的请求之后，就会进行逻辑操作，处理完成之后返回一个HTTP响应消息，包括：状态行、状态行和响应正文。在服务器响应之后，由于现在http默认开始长连接keep-alive，当页面关闭之后，tcp链接则会经过四次挥手完成断开
        TCP4次挥手：
            1、客户端进程发出连接释放报文，并且停止发送数据。释放数据报文首部，FIN=1，其序列号为seq=u（等于前面已经传送过来的数据的最后一个字节的序号加1），此时，客户端进入FIN-WAIT-1（终止等待1）状态。 TCP规定，FIN报文段即使不携带数据，也要消耗一个序号。
            2、服务器收到连接释放报文，发出确认报文，ACK=1，ack=u+1，并且带上自己的序列号seq=v，此时，服务端就进入了CLOSE-WAIT（关闭等待）状态。TCP服务器通知高层的应用进程，客户端向服务器的方向就释放了，这时候处于半关闭状态，即客户端已经没有数据要发送了，但是服务器若发送数据，客户端依然要接受。这个状态还要持续一段时间，也就是整个CLOSE-WAIT状态持续的时间。
            3、客户端收到服务器的确认请求后，此时，客户端就进入FIN-WAIT-2（终止等待2）状态，等待服务器发送连接释放报文（在这之前还需要接受服务器发送的最后的数据）。
            4、服务器将最后的数据发送完毕后，就向客户端发送连接释放报文，FIN=1，ack=u+1，由于在半关闭状态，服务器很可能又发送了一些数据，假定此时的序列号为seq=w，此时，服务器就进入了LAST-ACK（最后确认）状态，等待客户端的确认。
            5、客户端收到服务器的连接释放报文后，必须发出确认，ACK=1，ack=w+1，而自己的序列号是seq=u+1，此时，客户端就进入了TIME-WAIT（时间等待）状态。注意此时TCP连接还没有释放，必须经过2∗ *∗MSL（最长报文段寿命）的时间后，当客户端撤销相应的TCB后，才进入CLOSED状态
            6、服务器只要收到了客户端发出的确认，立即进入CLOSED状态。同样，撤销TCP后，就结束了这次的TCP连接。可以看到，服务器结束TCP连接的时间要比客户端早一些。

    6、页面渲染
        当浏览器接收到服务器响应的资源后，首先会对资源进行解析。查看响应头的信息，根据不同的指示做对应处理，比如重定向，存储cookie，解压gzip，缓存资源等。◦查看响应头的 Content-Type的值，根据不同的资源类型采用不同的解析方式
        关于页面的渲染过程如下：
            解析HTML，构建 DOM 树
            解析 CSS ，生成 CSS 规则树
            合并 DOM 树和 CSS 规则，生成 render 树
            布局 render 树（ Layout / reflow ），负责各元素尺寸、位置的计算
            绘制 render 树（ paint ），绘制页面像素信息
            浏览器会将各层的信息发送给 GPU，GPU 会将各层合成（ composite ），显示在屏幕上


- [介绍下 Https，和 http 的区别是什么？https 为什么比 http 安全？如何进行配置？](#介绍下-https和-http-的区别是什么https-为什么比-http-安全如何进行配置)
- [介绍chrome 浏览器的几个版本](#介绍chrome-浏览器的几个版本)
- [说一下 Http 缓存策略，有什么区别，分别解决了什么问题](#说一下-http-缓存策略有什么区别分别解决了什么问题)
- [前端安全、中间人攻击](#前端安全中间人攻击)
- [V8 机制讲解](#v8-机制讲解)
- [CDN 是什么？描述下 CDN 原理？为什么要用 CDN?](#cdn-是什么描述下-cdn-原理为什么要用-cdn)
- [PWA 是什么？对 PWA 有什么了解](#pwa-是什么对-pwa-有什么了解)
- [说一下浏览器解析 Html 文件的过程](#说一下浏览器解析-html-文件的过程)
- [从输入 URL 到页面加载全过程](#从输入-url-到页面加载全过程)
- [DNS 解析的具体过程](#dns-解析的具体过程)
- [常见的 http 请求头都有哪些，以及它们的作用](#常见的-http-请求头都有哪些以及它们的作用)
- [encoding 头都有哪些编码方式](#encoding-头都有哪些编码方式)
- [utf-8 和 asc 码有什么区别](#utf-8-和-asc-码有什么区别)
- [tcp 和 udp 有什么区别？tcp 怎样确保数据正确性？tcp 头包含什么？tcp 属于那一层？](#tcp-和-udp-有什么区别tcp-怎样确保数据正确性tcp-头包含什么tcp-属于那一层)
- [传输层和网络层分别负责什么，端口在什么层标记](#传输层和网络层分别负责什么端口在什么层标记)
- [说一下加密握手的过程](#说一下加密握手的过程)
- [对称加密和非对称加密的区别和用处](#对称加密和非对称加密的区别和用处)
- [浏览器都有哪些进程，渲染进程中都有什么线程](#浏览器都有哪些进程渲染进程中都有什么线程)
- [说说浏览器渲染流程，分层之后在什么时候合成，什么是重排、重绘，怎样避免](#说说浏览器渲染流程分层之后在什么时候合成什么是重排重绘怎样避免)
- [什么是同源策略？什么是跨域？都有哪些方式会造成跨域？解决跨域都有什么手段？](#什么是同源策略什么是跨域都有哪些方式会造成跨域解决跨域都有什么手段)
- [什么是 CORS，CORS 需要前端配置还是后端配置？](#什么是-corscors-需要前端配置还是后端配置)
- [Http1 和 Http2 有什么区别，和 http1.1 相比，http2 都有什么特性](#http1-和-http2-有什么区别和-http11-相比http2-都有什么特性)
- [说一下 etag 和 lastmodify 的区别](#说一下-etag-和-lastmodify-的区别)
- [强缓存都有哪些方法来控制](#强缓存都有哪些方法来控制)
- [协商缓存都有哪些参数](#协商缓存都有哪些参数)
- [请求是异步的为什么会造成阻塞](#请求是异步的为什么会造成阻塞)
- [CDN 有哪些优化静态资源加载速度的机制](#cdn-有哪些优化静态资源加载速度的机制)
- [说一下 Nginx 的缓存策略，强缓存与弱缓存的区别，二者的使用场景](#说一下-nginx-的缓存策略强缓存与弱缓存的区别二者的使用场景)
- [请描述 CSRF、XSS 的基本概念、攻击原理和防御措施？](#请描述-csrfxss-的基本概念攻击原理和防御措施)
- [请描述提升页面性能的方式有哪些，如何进行首页加载优化](#请描述提升页面性能的方式有哪些如何进行首页加载优化)
- [Http 报文的请求会有几个部分？请写出 HTTP 报文的组成部分](#http-报文的请求会有几个部分请写出-http-报文的组成部分)
- [301，302，304 的区别](#301302304-的区别)
- [说一下 https 获取加密秘钥的过程](#说一下-https-获取加密秘钥的过程)
- [localstorage、sessionStorage、indexDB 和 cookie 的区别](#localstoragesessionstorageindexdb-和-cookie-的区别)
- [点击一个按钮，浏览器会做些什么](#点击一个按钮浏览器会做些什么)
- [TCP 三次握手](#tcp-三次握手)
- [LocalStorage 加密原理](#localstorage-加密原理)
- [说一下常见的状态码](#说一下常见的状态码)
- [304 页面的原理](#304-页面的原理)
- [客户端缓存有几种方式？浏览器出现 from disk、from memory 的策略是啥](#客户端缓存有几种方式浏览器出现-from-diskfrom-memory-的策略是啥)
- [什么是 http？什么是 http2？说下 http 与 http2 的工作流程](#什么是-http什么是-http2说下-http-与-http2-的工作流程)
- [客户端如何发送 http 请求](#客户端如何发送-http-请求)
- [http1.1 时如何复用 tcp 连接](#http11-时如何复用-tcp-连接)
- [介绍浏览器事件流向](#介绍浏览器事件流向)
- [cookie 放哪里，cookie 能做的事情和存在的价值](#cookie-放哪里cookie-能做的事情和存在的价值)
- [cookie 和 token 都存放在 header 里面，为什么只劫持前者](#cookie-和-token-都存放在-header-里面为什么只劫持前者)
- [cookie 的引用为了解决什么问题](#cookie-的引用为了解决什么问题)
- [403、301、302 是什么](#403301302-是什么)
- [HTTPS 怎么建立安全通道，Https 的加密过程](#https-怎么建立安全通道https-的加密过程)
- [介绍下数字签名的原理](#介绍下数字签名的原理)
- [介绍一下网络的五层模型](#介绍一下网络的五层模型)
- [介绍 SSL 与 TLS](#介绍-ssl-与-tls)
- [服务端怎么做统一的状态处理](#服务端怎么做统一的状态处理)
- [以下说法中对协议描述不正确的是？(单选题)](#以下说法中对协议描述不正确的是单选题)
- [以下哪些是 HTTP 请求中浏览器缓存机制会用到的协议头？(单选题)](#以下哪些是-http-请求中浏览器缓存机制会用到的协议头单选题)
- [请写下你对协商缓存和强缓存的理解？](#请写下你对协商缓存和强缓存的理解)
- [给出页面的加载顺序](#给出页面的加载顺序)
- [详细描述一个 http 请求从发起请求到收到响应的全部过程(越细越好)](#详细描述一个-http-请求从发起请求到收到响应的全部过程越细越好)
- [简述静态链接和动态链接的区别，并举例说明](#简述静态链接和动态链接的区别并举例说明)
- [缓存有哪些？前端缓存有哪些？怎么用？如何和后台配合](#缓存有哪些前端缓存有哪些怎么用如何和后台配合)
- [Dom tree 和 cssdom 是如何合并成 render tree 的](#dom-tree-和-cssdom-是如何合并成-render-tree-的)
- [cdn 分布式部署，如果处理用户请求最近的资源](#cdn-分布式部署如果处理用户请求最近的资源)
- [说一下缓存有哪几种，具体都是怎么实现和比较的，缓存优先级，相互之间的对比](#说一下缓存有哪几种具体都是怎么实现和比较的缓存优先级相互之间的对比)
- [说下你对浏览器缓存理解](#说下你对浏览器缓存理解)
- [Http 连接是如何复用的](#http-连接是如何复用的)
- [301、302 的 https 被挟持怎么办？](#301302-的-https-被挟持怎么办)
- [介绍 Http2 特性，Http2 怎么确保文件同时传输不会报错](#介绍-http2-特性http2-怎么确保文件同时传输不会报错)
- [HTTP2.0的多路复⽤和HTTP1.X中的⻓连接复⽤有什么区别？](#http20的多路复和http1x中的连接复有什么区别)
- [HTTP2.0多路复⽤有多好？](#http20多路复有多好)
- [http请求由什么组成？](#http请求由什么组成)
- [列举并解释一下 http 的所有请求方法，](#列举并解释一下-http-的所有请求方法)
- [列举出在浏览器中，页面加载过程中发出了哪些事件？并画出这些事件的执行顺序？](#列举出在浏览器中页面加载过程中发出了哪些事件并画出这些事件的执行顺序)
- [请列出 HTTP/1.1 协议 Response 状态码：20x、30x、40x、50x 等各区间的含义，并说明 Action 在 Restful API 中分别使用哪些 Http 副词(action)表现出 CRUD?](#请列出-http11-协议-response-状态码20x30x40x50x-等各区间的含义并说明-action-在-restful-api-中分别使用哪些-http-副词action表现出-crud)
- [catch-control 有哪些设定值](#catch-control-有哪些设定值)

### 介绍chrome 浏览器的几个版本

公司：滴滴

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/11)

<br/>

### 说一下 Http 缓存策略，有什么区别，分别解决了什么问题

公司：滴滴、头条、网易、易车、脉脉、掌门一对一、虎扑、挖财、爱范儿

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/14)

<br/>

### 前端安全、中间人攻击

公司：滴滴、边锋

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/16)

<br/>

### V8 机制讲解

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/232)

<br/>

### CDN 是什么？描述下 CDN 原理？为什么要用 CDN?

公司：头条、滴滴、网易

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/218)

<br/>

### PWA 是什么？对 PWA 有什么了解

公司：头条、喜马拉雅

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/217)

<br/>

### 说一下浏览器解析 Html 文件的过程

公司：伴鱼

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/193)

<br/>

### 从输入 URL 到页面加载全过程

公司：头条、边锋、菜鸟网络、爱范儿、心娱

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/186)

### DNS 解析的具体过程

公司：边锋、寺库

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/184)

<br/>

### 常见的 http 请求头都有哪些，以及它们的作用

公司：阿里、边锋、喜马拉雅、玄武科技

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/183)

<br/>

### encoding 头都有哪些编码方式

公司：边锋

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/182)

<br/>

### utf-8 和 asc 码有什么区别

公司：边锋

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/181)

<br/>

### tcp 和 udp 有什么区别？tcp 怎样确保数据正确性？tcp 头包含什么？tcp 属于那一层？

公司：头条、边锋

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/180)

<br/>

### 传输层和网络层分别负责什么，端口在什么层标记

公司：边锋

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/179)

<br/>

### 介绍下 Https，和 http 的区别是什么？https 为什么比 http 安全？如何进行配置？

公司：边锋、老虎、脉脉、掌门一对一、喜马拉雅、寺库、腾讯应用宝、快手

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/178)

<br/>

### 说一下加密握手的过程

公司：边锋、老虎

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/177)

<br/>

### 对称加密和非对称加密的区别和用处

公司：边锋

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/176)

<br/>

### 浏览器都有哪些进程，渲染进程中都有什么线程

公司：老虎

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/167)

<br/>

### 说说浏览器渲染流程，分层之后在什么时候合成，什么是重排、重绘，怎样避免

公司：老虎

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/166)

<br/>

### 什么是同源策略？什么是跨域？都有哪些方式会造成跨域？解决跨域都有什么手段？

公司：阿里、滴滴、老虎、完美世界、沪江、喜马拉雅、兑吧、寺库、玄武科技

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/165)

<br/>

### 什么是 CORS，CORS 需要前端配置还是后端配置？

公司：酷狗

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/164)

<br/>

### Http1 和 Http2 有什么区别，和 http1.1 相比，http2 都有什么特性

公司：完美世界、网易、脉脉、高思教育

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/361)

<br/>

### 说一下 etag 和 lastmodify 的区别

公司：网易

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/353)

<br/>

### 强缓存都有哪些方法来控制

公司：网易、易车

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/352)

<br/>

### 协商缓存都有哪些参数

公司：网易、易车

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/351)

<br/>

### 请求是异步的为什么会造成阻塞

公司：易车

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/341)

<br/>

### CDN 有哪些优化静态资源加载速度的机制

公司：头条

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/326)

<br/>

### 说一下 Nginx 的缓存策略，强缓存与弱缓存的区别，二者的使用场景

公司：高德

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/304)

<br/>

### 请描述 CSRF、XSS 的基本概念、攻击原理和防御措施？

公司：自如、挖财、腾讯应用宝、沪江、喜马拉雅

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/274)

<br/>

### 请描述提升页面性能的方式有哪些，如何进行首页加载优化

公司：自如

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/273)

<br/>

### Http 报文的请求会有几个部分？请写出 HTTP 报文的组成部分

公司：自如、滴滴

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/)

<br/>

### 301，302，304 的区别

公司：自如

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/270)

<br/>

### 说一下 https 获取加密秘钥的过程

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/414)

<br/>

### localstorage、sessionStorage、indexDB 和 cookie 的区别

公司：掌门一对一、滴滴、兑吧、寺库

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/409)

<br/>

### 点击一个按钮，浏览器会做些什么

公司：虎扑

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/402)

<br/>

### TCP 三次握手

公司：菜鸟网络、头条

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/394)

<br/>

### LocalStorage 加密原理

公司：腾讯应用宝

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/385)

<br/>

### 说一下常见的状态码

公司：腾讯应用宝、喜马拉雅

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/383)

<br/>

### 304 页面的原理

公司：腾讯应用宝

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/381)

<br/>

### 客户端缓存有几种方式？浏览器出现 from disk、from memory 的策略是啥

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/573)

<br/>

### 什么是 http？什么是 http2？说下 http 与 http2 的工作流程

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/572)

<br/>

### 客户端如何发送 http 请求

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/571)

<br/>

### http1.1 时如何复用 tcp 连接

公司：网易

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/)

<br/>

### 介绍浏览器事件流向

公司：网易

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/527)

<br/>

### cookie 放哪里，cookie 能做的事情和存在的价值

公司：滴滴

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/514)

<br/>

### cookie 和 token 都存放在 header 里面，为什么只劫持前者

公司：滴滴

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/513)

<br/>

### cookie 的引用为了解决什么问题

公司：寺库

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/478)

<br/>

### 403、301、302 是什么

公司：喜马拉雅

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/438)

<br/>

### HTTPS 怎么建立安全通道，Https 的加密过程

公司：喜马拉雅、寺库

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/437)

<br/>

### 介绍下数字签名的原理

公司：喜马拉雅

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/435)

<br/>

### 介绍一下网络的五层模型

公司：寺库

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/781)

<br/>

### 介绍 SSL 与 TLS

公司：寺库

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/780)

<br/>

### 服务端怎么做统一的状态处理

公司：宝宝树

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/774)

<br/>

### 以下说法中对协议描述不正确的是？(单选题)

```js
A.HTTP 协议是在 TCP/IP 协议之上的应用层协议
B.HTTP 1.1 缺省支持 keep-alive
C.WebSocket 是由 Socket 发展而来的新规范
D.WebSocket 可以建立持久连接
```

公司：会小二

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/692)

<br/>

### 以下哪些是 HTTP 请求中浏览器缓存机制会用到的协议头？(单选题)

```js
A.Last-Modified
B.Etag
C.Referer
D.Authorization
```

公司：会小二

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/691)

<br/>

### 请写下你对协商缓存和强缓存的理解？

公司：会小二、58

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/680)

<br/>

### 给出页面的加载顺序

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/677)

<br/>

### 详细描述一个 http 请求从发起请求到收到响应的全部过程(越细越好)

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/669)

<br/>

### 简述静态链接和动态链接的区别，并举例说明

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/668)

<br/>

### 缓存有哪些？前端缓存有哪些？怎么用？如何和后台配合

公司：高思教育

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/656)

<br/>

### Dom tree 和 cssdom 是如何合并成 render tree 的

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/652)

<br/>

### cdn 分布式部署，如果处理用户请求最近的资源

公司：快手

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/632)

<br/>

### 说一下缓存有哪几种，具体都是怎么实现和比较的，缓存优先级，相互之间的对比

公司：快手

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/631)

<br/>

### 说下你对浏览器缓存理解

公司：头条

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/629)

<br/>

### Http 连接是如何复用的

公司：酷狗

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/623)

<br/>

### 301、302 的 https 被挟持怎么办？

公司：网易

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/857)

<br/>

### 介绍 Http2 特性，Http2 怎么确保文件同时传输不会报错

公司：网易

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/856)

<br/>

### HTTP2.0的多路复⽤和HTTP1.X中的⻓连接复⽤有什么区别？

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/788)

<br/>

### HTTP2.0多路复⽤有多好？

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/787)

<br/>

### http请求由什么组成？

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/868)

<br/>

### 列举并解释一下 http 的所有请求方法，

公司：爱范儿、乘法云

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/851)

<br/>

### 列举出在浏览器中，页面加载过程中发出了哪些事件？并画出这些事件的执行顺序？

公司：玄武科技

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/842)

<br/>

### 请列出 HTTP/1.1 协议 Response 状态码：20x、30x、40x、50x 等各区间的含义，并说明 Action 在 Restful API 中分别使用哪些 Http 副词(action)表现出 CRUD?

公司：玄武科技

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/835)

<br/>

### catch-control 有哪些设定值

公司：58

分类：网络&安全

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/816)

<br/>


