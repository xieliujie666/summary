

# 1.浏览器渲染原理 

(1)渲染主流程

　　渲染引擎首先通过网络获得所请求文档的内容，通常以8K分块的方式完成。下面是渲染引擎在取得内容之后的基本流程：

　　解析html以构建dom树 -> 构建render树 -> 布局render树 -> 绘制render树

![img](C:\Users\superssssss\Desktop\Interview\mj-images\2011110316263715.png)

注：DOM Tree：浏览器将HTML解析成树形的数据结构。

　　CSS Rule Tree：浏览器将CSS解析成树形的数据结构。

　　Render Tree: DOM和CSSOM合并后生成Render Tree。

　　layout: 有了Render Tree，浏览器已经能知道网页中有哪些节点、各个节点的CSS定义以及他们的从属关系，从而去计算出每个节点在屏幕中的位置（几何信息 ）。

　　painting: 按照算出来的规则，通过显卡，把内容画到屏幕上。

(2)尽管Webkit与Gecko使用略微不同的术语，过程还是基本相同，如下：

　　1. 浏览器会将HTML解析成一个DOM树，DOM 树的构建过程是一个深度遍历过程：当前节点的所有子节点都构建好后才会去构建当前节点的下一个兄弟节点。

　　2. 将CSS解析成 CSS Rule Tree 。

　　3. 根据DOM树和CSSOM来构造 Rendering Tree。注意：Rendering Tree 渲染树并不等同于 DOM 树，因为一些像Header或display:none的东西就没必要放在渲染树中了。

　　4. 有了Render Tree，浏览器已经能知道网页中有哪些节点、各个节点的CSS定义以及他们的从属关系。下一步操作称之为layout，顾名思义就是计算出每个节点在屏幕中的位置。

　　5. 再下一步就是绘制，即遍历render树，并使用UI后端层绘制每个节点。

　　注意：*上述这个过程是逐步完成的，为了更好的用户体验，渲染引擎将会尽可能早的将内容呈现到屏幕上，并不会等到所有的html都解析完成之后再去构建和布局render树。它是解析完一部分内容就显示一部分内容，同时，可能还在通过网络下载其余内容*。



# 2.http和https

(1)概念

http: 超文本传输协议，是互联网上应用最为广泛的一种网络协议，是一个客户端和服务器端请求和应答的标准（TCP）

https: 是以安全为目标的HTTP通道，即HTTP下加入SSL层 。作用是建立一个信息安全通道，来确保数组的传输，确保网站的真实性 。

​	   优点：1.使用HTTPS协议可认证用户和服务器，确保数据发送到正确的客户机和服务器；2.HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，要比http协议安全，可防止数据在传输过程中不被窃取、改变，确保数据的完整性。大幅增加了中间人攻击的成本

​	    缺点：1.https握手阶段比较费时，会使页面加载时间延长50%，增加10%~20%的耗电。2.https缓存不如http高效，会增加数据开销。3.SSL证书也需要钱，功能越强大的证书费用越高。3.SSL证书需要绑定IP，不能再同一个ip上绑定多个域名，ipv4资源支持不了这种消耗。？？  

(2)区别

http传输的数据都是未加密的，网景公司设置了SSL协议来对http协议传输的数据进行加密处理，简单来说1.https协议是由http和ssl协议构建的可进行加密传输和身份认证的网络协议，比http协议（连接很简单，无状态）的安全性更高。 2.需要ca证书，费用较高 。3.使用不同的链接方式，端口也不同，一般而言，http协议的端口为80，https的端口为443 

# 3.TCP和UDP

(1)概念

TCP：（TransmissionControlProtocol传输控制协议）是一种面向连接的、可靠的、基于字节流的传输层通信协议。在简化的计算机网络OSI模型中 TCP完成第四层传输层所指定的功能，（UDP）是同一层内另一个重要的传输协议。 

UDP：用户数据包协议（UDP，User Datagram Protocol）。UDP 为应用程序提供了一种无需建立连接就可以发送封装的 IP 数据包的方法 。

(2）区别

- （1）TCP是面向连接的，udp是无连接的即发送数据前不需要先建立链接。

- （2）TCP提供可靠的服务。也就是说，通过TCP连接传送的数据，无差错，不丢失，不重复，且按序到达;UDP尽最大努力交付，即不保证可靠交付。 并且因为tcp可靠，面向连接，不会丢失数据因此适合大数据量的交换。

- （3）TCP是面向字节流，UDP面向报文，并且网络出现拥塞不会使得发送速率降低（因此会出现丢包，对实时的应用比如IP电话和视频会议等）。

- （4）TCP只能是1对1的，UDP支持1对1,1对多。

- （5）TCP的首部较大为20字节，而UDP只有8字节。

- （6）TCP是面向连接的可靠性传输，而UDP是不可靠的。


# 4.TCP和HTTP

TCP是传输层，而http是应用层， http是要基于TCP连接基础上的，简单的说，TCP就是单纯建立连接，不涉及任何我们需要请求的实际数据，简单的传输。http是用来收发数据，即实际应用上来的。

​	第一：从传输层，先说下TCP连接，我们要和服务端连接TCP连接，需要通过三次连接，包括：请求，确认，建立连接。即传说中的“三次握手协议”。

​	第二：从实际上的数据应用来说http，在前面客户端和应用服务器建立TCP连接之后，就需要用http协议来传送数据了，HTTP协议简单来说，还是请求，确认，连接。总体就是C发送一个HTTP请求给S，S收到了这个http请求，然后返回给Chttp响应，然后C的中间件或者说浏览器把这些数据渲染成为了网页，展示在用户面前。

​      TCP是底层通讯协议，定义的是数据传输和连接方式的规范
      HTTP是应用层协议，定义的是传输数据的内容的规范
      HTTP协议中的数据是利用TCP协议传输的，所以支持HTTP也就一定支持TCP      

？？HTTP支持的是www服务 而TCP/IP是协议  它是Internet国际互联网络的基础。TCP/IP是网络中使用的基本的通信协议。TCP/IP实际上是一组协议，它包括上百个各种功能的协议，如：远程登录、文件传输和电子邮件等，而TCP协议和IP协议是保证数据完整传输的两个基本的重要协议。通常说TCP/IP是Internet协议族，而不单单是TCP和IP。

## *TCP三次握手四次挥手

三次握手：

![1617001655183](C:\Users\superssssss\Desktop\Interview\mj-images\1617001655183.png)

1.客户端随机初始化序列号（client_isn)，置于TCP首部，把SYN标志位 置为1，接着把第一个SYN报文发送给服务端，表示向服务端发起连接。客户端处于SYN-SENT状态。

2.服务端收到客户端的SYN报文后，也随机初始化序号（server_isn)置于TCP首部，并把CP首部的【确认应答号】字段填入client_isn+1，接着把SYN和ACK标志位置为1，把第二个SYN+ACK报文发送给客户端。服务器处于SYN-RCVD状态。

3.客户端收到SYN+ACK报文后，还要向客户端回应最后一个应答报文。首先报文首部ACK标志位置为1，然后【确认应答号】填入server_isn+1，最后发送，可携带数据。客户端处于established状态。

(服务器端收到客户端的应答报文后，也进入establishe状态)，此时链接已建立完成。

四次挥手：

![1617001390673](C:\Users\superssssss\Desktop\Interview\mj-images\1617001390673.png)

1.客户端打算关闭连接，此时会发送一个 TCP 首部 FIN 标志位被置为 1 的报文，也即 FIN报文，之后客户端进入 FIN_WAIT_1 状态。
2.服务端收到该报文后，就向客户端发送 ACK 应答报文，接着服务端进入CLOSED_WAIT 状态。客户端收到服务端的 ACK 应答报文后，之后进入 FIN_WAIT_2 状态。
3.等待服务端处理完数据后，也向客户端发送 FIN 报文，之后服务端进入 LAST_ACK 状态。
4.客户端收到服务端的 FIN 报文后，回一个 ACK 应答报文，之后进入 TIME_WAIT 状态
服务器收到了 ACK 应答报文后，进入 CLOSED 状态，至此服务端完成连接的关闭。客户端在经过 2MSL 一段时间后，自动进入 CLOSED 状态，至此客户端也完成连接的关闭。





# 5.HTTP2.0 的特性？

1、内容安全，应为http2.0是基于https的，天然具有安全特性，通过http2.0的特性可以避免单纯使用https的性能下降

2、二进制格式，http1.X的解析是基于文本的，http2.0将所有的传输信息分割为更小的消息和帧，并对他们采用二进制格式编码，基于二进制可以让协议有更多的扩展性，比如引入了帧来传输数据和指令

3、多路复用，这个功能相当于是长连接的增强，每个request请求可以随机的混杂在一起，接收方可以根据request的id将request再归属到各自不同的服务端请求里面，另外多路复用中也支持了流的优先级，允许客户端告诉服务器那些内容是更优先级的资源，可以优先传输，

# 5.HTTP状态码？

200    OK    请求成功。一般用于GET与POST请求 

304：如果客户端发送了一个带条件的GET 请求且该请求已被允许，而文档的内容（自上次访问以来或者根据请求的条件）并没有改变，则服务器应当返回这个304状态码。 

400    Bad Request    客户端请求的语法错误，服务器无法理解

401    Unauthorized    请求要求用户的身份认证

404    Not Found    服务器无法根据客户端的请求找到资源（网页）。通过此代码，网站设计人员可设置"您所请求的资源无法找到"的个性页面 

## ● 301和302的区别

301 Moved Permanently 被请求的资源已永久移动到新位置，并且将来任何对此资源的引用都应该使用本响应返回的若干个URI之一。如果可能，拥有链接编辑功能的客户端应当自动把请求的地址修改为从服务器反馈回来的地址。除非额外指定，否则这个响应也是可缓存的。

302 Found 请求的资源现在临时从不同的URI响应请求。由于这样的重定向是临时的，客户端应当继续向原有地址发送以后的请求。只有在Cache-Control或Expires中进行了指定的情况下，这个响应才是可缓存的。

字面上的区别就是301是永久重定向，而302是临时重定向。

301比较常用的场景是使用域名跳转。302用来做临时跳转 比如未登陆的用户访问用户中心重定向到登录页面。

# 6.BOM属性对象方法

 Bom是浏览器对象 

(1)location对象

location.href-- 返回或设置当前文档的URL
location.search -- 返回URL中的查询字符串部分

location.reload() -- 重载当前页面

(2)history对象

history.go() -- 前进或后退指定的页面数 history.go(num);
history.back() -- 后退一页
history.forward() -- 前进一页

(3)Navigator对象

navigator.userAgent -- 返回用户代理头的字符串表示(就是包括浏览器版本信息等的字符串)
navigator.cookieEnabled -- 返回浏览器是否支持(启用)cookie



# 7.Cookie、sessionStorage、localStorage @

(1)共同点：都是保存在浏览器端，并且是同源的 

(2)Cookie：cookie数据始终在同源的http请求中携带（即使不需要），即cookie在浏览器和服务器间来回传递。而sessionStorage和localStorage不会自动把数据发给服务器，仅在本地保存。cookie数据还有路径（path）的概念，可以限制cookie只属于某个路径下,存储的大小很小只有4K左右。 （key：可以在浏览器和服务器端来回传递，存储容量小，只有大约4K左右）

sessionStorage：将数据保存在session对象中。session对象可以用来保存在这段时间内所要求保存的任何数据。（key：本身就是一个回话过程，关闭页面或浏览器 后消失，session为一个回话，当页面不同即使是同一页面打开两次，也被视为同一次回话？）

localStorage：将数据保存在客户端本地的硬件设备(通常指硬盘，也可以是其他硬件设备)中，即使浏览器被关闭了，该数据仍然存在，下次打开浏览器访问网站时仍然可以继续使用。localStorage 在所有同源窗口中都是共享的；cookie也是在所有同源窗口中都是共享的。（key：同源窗口都会共享，并且不会失效，不管窗口或者浏览器关闭与否都会始终生效）

![1615814861918](C:\Users\superssssss\Desktop\Interview\mj-images\1615814861918.png)

# 8.Cookie和session

1.    cookie数据存放在客户的浏览器上，session数据放在服务器上。

2.    cookie不是很安全，别人可以分析存放在本地的COOKIE并进行COOKIE欺骗考虑到安全应当使用session。

3.    session会在一定时间内保存在服务器上。当访问增多，会比较占用你服务器的性能考虑到减轻服务器性能方面，应当使用COOKIE。

4.    单个cookie保存的数据不能超过4K，很多浏览器都限制一个站点最多保存20个cookie。

# 9.Cookie如何防范XSS攻击？



## 描述一下XSS和CRSF攻击？防御方法？

XSS, 即为（Cross Site Scripting）, 中文名为跨站脚本, 是发生在目标用户的浏览器层面上的，当渲染DOM树的过程成发生了不在预期内执行的JS代码时，就发生了XSS攻击。大多数XSS攻击的主要方式是嵌入一段远程或者第三方域上的JS代码。实际上是在目标网站的作用域下执行了这段js代码。

CSRF（Cross Site Request Forgery，跨站请求伪造），字面理解意思就是在别的站点伪造了一个请求。专业术语来说就是在受害者访问一个网站时，其 Cookie 还没有过期的情况下，攻击者伪造一个链接地址发送受害者并欺骗让其点击，从而形成 CSRF 攻击。

XSS防御的总体思路是：对输入(和URL参数)进行过滤，对输出进行编码。也就是对提交的所有内容进行过滤，对url中的参数进行过滤，过滤掉会导致脚本执行的相关内容；然后对动态输出到页面的内容进行html编码，使脚本无法在浏览器中执行。虽然对输入过滤可以被绕过，但是也还是会拦截很大一部分的XSS攻击。

防御CSRF 攻击主要有三种策略：验证 HTTP Referer 字段；在请求地址中添加 token 并验证；在 HTTP 头中自定义属性并验证。



#  10.在地址栏里输入一个URL,到这个页面呈现出来，中间会发生什么？

DNS解析、TCP连接、发送HTTP请求、服务器处理请求并返回HTTP报文、浏览器解析渲染页面、连接结束

首先，我们假设输入的url的请求为最简单的Http请求，以GET请求为例，大致分以下几个步骤：

1. 用户在浏览器的地址栏输入访问的URL地址。浏览器会先根据这个URL查看浏览器缓存-系统缓存-路由器缓存，若缓存中有，直接跳到第6步操作，若没有，则按照下面的步骤进行操作。
2. 浏览器根据输入的URL地址解析出主机名。
3. 浏览器将主机名转换成服务器ip地址。浏览器先查找本地DNS缓存列表，看缓存里面是否存在这个ip,如果有则进入第4步，如果缓存中不存在这个ip地址，就再向浏览器默认的DNS服务器发送查询请求，同时缓存当前这个ip到DNS缓存列表中。
4. 拿到ip地址后，浏览器再从URL中解析出端口号。
5. 拿到ip和端口后，浏览器会建立一条与目标Web服务器的TCP连接，也就是传说中的三次握手。传送门：[完整的tcp链接](http://www.cnblogs.com/xsilence/p/6034361.html)。
6. 浏览器向服务器发送一条HTTP请求报文。
7. 服务器向浏览器返回一条HTTP响应报文。
8. 关闭连接 浏览器解析文档。
9. 如果文档中有资源则重复6、7、8动作，直至资源全部加载完毕。

输入url后，首先需要找到这个url域名的服务器ip,为了寻找这个ip，浏览器首先会寻找缓存，查看缓存中是否有记录，缓存的查找记录为：浏览器缓存-》系统缓存-》路由器缓存，缓存中没有则查找系统的hosts文件中是否有记录，如果没有则查询DNS服务器，得到服务器的ip地址后，浏览器根据这个ip以及相应的端口号，构造一个http请求，这个请求报文会包括这次请求的信息，主要是请求方法，请求说明和请求附带的数据，并将这个http请求封装在一个tcp包中，这个tcp包会依次经过传输层，网络层，数据链路层，物理层到达服务器，服务器解析这个请求来作出响应，返回相应的html给浏览器，《因为html是一个树形结构，浏览器根据这个html来构建DOM树，在dom树的构建过程中如果遇到JS脚本和外部JS连接，则会停止构建DOM树来执行和下载相应的代码，这会造成阻塞，这就是为什么推荐JS代码应该放在html代码的后面，之后根据外部央视，内部央视，内联样式构建一个CSS对象模型树CSSOM树，构建完成后和DOM树合并为渲染树，这里主要做的是排除非视觉节点，比如script，meta标签和排除display为none的节点，之后进行布局，布局主要是确定各个元素的位置和尺寸，之后是渲染页面》，因为html文件中会含有图片，视频，音频等资源，在解析DOM的过程中，遇到这些都会进行并行下载，浏览器对每个域的并行下载数量有一定的限制，一般是4-6个，当然在这些所有的请求中我们还需要关注的就是缓存，缓存一般通过Cache-Control、Last-Modify、Expires等首部字段控制。 Cache-Control和Expires的区别在于Cache-Control使用相对时间，Expires使用的是基于服务器 端的绝对时间，因为存在时差问题，一般采用Cache-Control，在请求这些有设置了缓存的数据时，会先 查看是否过期，如果没有过期则直接使用本地缓存，过期则请求并在服务器校验文件是否修改，如果上一次 响应设置了ETag值会在这次请求的时候作为If-None-Match的值交给服务器校验，如果一致，继续校验 Last-Modified，没有设置ETag则直接验证Last-Modified，再决定是否返回304

# ？？？解决阻塞？脚本放的位置 defer和async区别？

 

# 11.重排和重绘

以及优化重排重绘的方案？ 

（1）由于浏览器渲染界面是基于流式布局模型的，也就是某一个DOM节点信息更改了，就需要对DOM结构进行重新计算，重新布局界面，再次引发回流 。

DOM节点的变化影响到了页面元素的几何属性比如宽高，浏览器重新计算元素的位置大小等几何属性，浏览器需要重新构造渲染书，这个过程称之为重排，浏览器将受到影响的部分重新绘制在屏幕上 的过程称为重绘，

所谓重绘，就是当页面中元素样式的改变并不影响它在文档流中的位置时，例如更改了字体颜色,浏览器会将新样式赋予给元素并重新绘制的过程 

（2）引起重排重绘的原因有：任何页面布局和几何属性的改变都会触发重排，触发重绘的条件：改变元素外观属性。如：color，background-color等。

1. 页面首次渲染。
2. 浏览器窗口大小发生改变。
3. 元素尺寸或位置发生改变。
4. 元素内容变化（文字数量或图片大小等等）。
5. 元素字体大小变化。
6. 添加或者删除可见的DOM元素。
7. 激活CSS伪类（例如：:hover）。
8. 设置style属性
9. 查询某些属性或调用某些方法。

一次的dom更改或者css几何属性更改，都会引起一次浏览器的重排/重绘过程，而如果是css的非几何属性更改，则只会引起重绘过程。所以说重排一定会引起重绘，而重绘不一定会引起重排。 

（3）减少重绘重排的方法：
不在布局信息改变时做 DOM 查询，
使用 csstext,className 一次性改变属性
使用 fragment
对于多次重排的元素，比如说动画。使用绝对定位脱离文档流，使其不影响其他元素

减少重排：

- 避免设置大量的style属性，因为通过设置style属性改变结点样式的话，每一次设置都会触发一次reflow，所以最好是使用class属性

- 实现元素的动画，它的position属性，最好是设为absoulte或fixed，这样不会影响其他元素的布局

- 动画实现的速度的选择。比如实现一个动画，以1个像素为单位移动这样最平滑，但是reflow就会过于频繁，大量消耗CPU资源，如果以3个像素为单位移动则会好很多。

- 不要使用table布局，因为table中某个元素旦触发了reflow，那么整个table的元素都会触发reflow。那么在不得已使用table的场合，可以设置table-layout:auto;或者是table-layout:fixed这样可以让table一行一行的渲染，这种做法也是为了限制reflow的影响范围

  ###### 优化：　　

  1、浏览器自己的优化：浏览器会维护1个队列，把所有会引起回流、重绘的操作放入这个队列，等队列中的操作到了一定的数量或者到了一定的时间间隔，浏览器就会flush队列，进行一个批处理。这样就会让多次的回流、重绘变成一次回流重绘。

  2、我们要注意的优化：我们要减少重绘和重排就是要减少对渲染树的操作，则我们可以合并多次的DOM和样式的修改。并减少对style样式的请求。

  （1）直接改变元素的className

  （2）display：none；先设置元素为display：none；然后进行页面布局等操作；设置完成后将元素设置为display：block；这样的话就只引发两次重绘和重排；

  （3）不要经常访问浏览器的flush队列属性；如果一定要访问，可以利用缓存。将访问的值存储起来，接下来使用就不会再引发回流；

  （4）使用cloneNode(true or false) 和 replaceChild 技术，引发一次回流和重绘；

  （5）将需要多次重排的元素，position属性设为absolute或fixed，元素脱离了文档流，它的变化不会影响到其他元素；

  （6）如果需要创建多个DOM节点，可以使用DocumentFragment创建完后一次性的加入document；

# 12.常见的HTTP的头部？？

（1）通用首部表示一些通用信息，

1.请求指令cache-control : no-cache no-store max-age
   响应指令cache-control : no-cache no-store max-age s-maxage public private
2.connection: keep-alive close
3.Date：表示报文创建时间，

（2）请求首部就是请求报文中独有的，如cookie，和缓存相关的如if-Modified-Since

1.Accept 首部字段可通知服务器，用户代理能够处理的媒体类型及媒体 类型的相对优先级 

2.Accept-Charset 首部字段可用来通知服务器用户代理支持的字符集及
字符集的相对优先顺序。
3.Accept-Encoding 首部字段用来告知服务器用户代理支持的内容编码及
内容编码的优先级顺序。

4.Accept-Language 首部字段 Accept-Language 用来告知服务器用户代理能够处理的自然语言集（指中文或英文等）
5.Authorization 首部字段 Authorization 是用来告知服务器，用户代理的认证信息（证书值）。
6.Host 首部字段 Host 会告知服务器，请求的资源所处的互联网主机名和端
口号。

7.If-Modified-Since 8.If-None-Match

 9.User-Agent首部字段 User-Agent 会将创建请求的浏览器和用户代理名称等信息传 达给服务器 

（3）响应首部就是响应报文中独有的，如set-cookie，和重定向相关的location，

1.Accept-Ranges 首部字段 Accept-Ranges 是用来告知客户端服务器是否能处理范围请求，以指定获取服务器端某个部分的资源。可指定的字段值有两种，可处理范围请求时指定其为 bytes，反之则
指定其为 none。
2.Age 首部字段 Age 能告知客户端，源服务器在多久前创建了响应。字段值
的单位为秒。
3.ETag 首部字段 ETag 能告知客户端实体标识。服务器会为每份资源分配对应的 ETag
值。资源改变，Etag值也会变。
4.Location 几乎所有的浏览器在接收到包含首部字段 Location 的响应后，都会强
制性地尝试对已提示的重定向资源的访问

（4）实体首部用来描述实体部分，如allow用来描述可执行的请求方法，content-type描述主题类型，content-Encoding描述主体的编码方式

？（5）Cookie首部字段
set-cookie和cookie
set-cookie中设置httpOnly
Cookie 的 HttpOnly 属性是 Cookie 的扩展功能，它使 JavaScript 脚本
无法获得 Cookie。其主要目的为防止跨站脚本攻击（Cross-site
scripting，XSS）对 Cookie 的信息窃取。


# 13.HTTP 缓存、强缓存、协商缓存

 HTTP Cache
我们知道通过网络获取资源缓慢且耗时，需要三次握手等协议与远程服务器建立通信，对于大点的数据需要多次往返通信大大增加了时间开销，并且当今流量依旧没有理想的快速与便宜。对于开发者来说，长久缓存复用重复不变的资源是性能优化的重要组成部分。

因为服务器上的资源不是一直固定不变的，大多数情况下它会更新，这个时候如果我们还访问本地缓存，那么对用户来说，那就相当于资源没有更新，用户看到的还是旧的资源；所以我们希望服务器上的资源更新了浏览器就请求新的资源，没有更新就使用本地的缓存，以最大程度的减少因网络请求而产生的资源浪费。

HTTP 缓存机制就是：配置服务器响应头来告诉浏览器是否应该缓存资源、是否强制校验缓存、缓存多长时间；浏览器非首次请求根据响应头是否应该取缓存、缓存过期发送请求头验证缓存是否可用还是重新获取资源的过程。

缓存分为：强缓存和协商缓存，根据响应的header内容来决定。

|          | 获取资源形式 | 状态码              | 发送请求到服务器                 |
| -------- | ------------ | ------------------- | -------------------------------- |
| 强缓存   | 从缓存取     | 200（from cache）   | 否，直接从缓存取                 |
| 协商缓存 | 从缓存取     | 304（not modified） | 是，通过服务器来告知缓存是否可用 |

强缓存相关字段有expires，cache-control。

协商缓存相关字段有:Last-Modified/If-Modified-Since，Etag/If-None-Match

-  **Expires/ Cache-Control(优先使用）**        用来控制缓存的失效日期，控制浏览器是直接从浏览器缓存取数据还是重新发请求到服务器取数据。 

  Expires：是Web服务器响应消想头字段,在响应http请求时告诉浏览器在过期时间前浏览器可以直接从浏览器缓存取数据,而无需再次请求。 Expires的一个缺点就是,返回的到期时间是服务器的时间,这样存在一个问题如果客户端的时间与服务器的时间相差很大(比如时钟不同步,或者跨时区),那么误差就很大,所以在HTTP1.1版开始,使用Cache-Control: max-age=(秒)替代

- Last-Modified和 If-Modified-Since

  ​	   简单的说, Last-Modified与 If-Modified- Since都是用于记录页面最后修改时间的HTTP头信息,只是Last-Modified是由服务器往客户端发送的HTTP头,而If-Modified-Since则是由客户端往服务器发送的头,可以看到,再次请求本地在在的缓存页面时,客户端会通过If-Modified-Since头把浏览器端级体页面的最后一次被服务器修改的时间一起发到服务器去,服务器会把这个时与服务器上实际文件的最后修改时间进行比较,通过这个时间最断客户端的面是否是最新如果不是最新的,就返回HTP状态码200和新的文件内容,客户端接到之后,会丢弃旧文件,把新文件缓存起来,并显示到浏览器中:如果是最新的则返回304告诉客户端其本地缓存中的页面是最新的,就直接把本地缓存文件显示到浏览器中,这样在网络上传输的数据量就会大大减少,同时也减轻了服务器的负担

- ETag和 If-None-Match
         ETag和 If-None-Match是一种常用的判断资源是否改变的方法。类似于Last-Modified和 If-Modified-Since.但是有所不同的是 Last-Modified和 If-Modified-Since只判断资源的最后修改时间,而ETag和 If-None-Match可以是源任的任何属性:比如资源的MD5等。
            ETag和|f-None- Match的工作原理是在 HTTP Response中添加ETags信息,当客户端再次请求该资源时,将在HTTP Request中加入 If-None-Match信息(也就是 ETags的值)·如果服务器验证资源的ETags没有改变(该资源的内容没有改变),将返回一个304状态:否则,服务器将返回200状态,并返回该资源和新的 ETags.(服务器端返回的格式:ETag: 50b1c1d4f775c61: df3"         客户端的查询更新格式是这样的:          If-None-Match: W/ 50b1c1d4f775c61: df3" )

![img](C:\Users\superssssss\Desktop\Interview\mj-images\311436_1552361773903_9DC69E327B4B3691E94CD9D52D10E2C1)

# 14.SEO搜索引擎优化 

1.它是一种通过分析搜索引擎的排名规律，了解各种搜索引擎怎样进行搜索、怎样抓取互联网页面、怎样确定特定关键词的搜索结果[排名](https://baike.baidu.com/item/%E6%8E%92%E5%90%8D/1446803)的技术。搜索引擎采用易于被搜索引用的手段，对网站进行有针对性的优化，提高网站在搜索引擎中的自然排名 进而使该[网站访问量](https://baike.baidu.com/item/%E7%BD%91%E7%AB%99%E8%AE%BF%E9%97%AE%E9%87%8F/5248340)得以提升，最终提高本网站宣传能力或者销售能力的一种现代技术 。

1. 网站定位

   网站定位又叫关键词定位，是非常重要的一步，是优化的方向，也是企业发展的方向，所以要明确，自己推广的是什么，后续工作才好展开。

2. 标题优化

   网站标题优化，即title标签内容的优化，它是客户第一眼就看到的内容，很大程度上决定了客户的去留，所以标题优化好了，后面的事情就好办多了。

3. 关键词优化

   关键词虽然不能从网站上直接看出来，但它们却是整个网站优化的纽带，标题需要围绕它来写，内容需要围绕它来展开，切忌不要在同一个页面同时优化过多关键词，一般一个页面优化1-2个就可以了。

4. 描述优化

   一个网页的描述内容，是对这个网页的简单介绍，让客户知道这个页面里面是什么，如果不够精彩，用户很可能就不会点击进去看。描述内容要紧紧围绕标题和关键词来展开，不要脱离主题。

5. 内容优化

   这里的内容是指网站页面上直接可见的内容，比如产品，新闻，案例等，都属于内容。内容要尽量原创，尽量保证高质量的内容，更新要有规律。

6. 外链优化

   选择优质且有相关性的平台，交换友链或者直接发布外链，都是不错的选择。但不要什么样的平台都去换链，有时候会得不偿失，要注意相关性，且要有流量。

7. 数据分析

   作为一个合格的优化人员，一定要会查看数据，并作出整理分析，从数据中能看出哪里的工作需要加强，哪个地方还存在问题等。

   

#  15.语义化的作用

语义化标签就是尽量使用有相对应的结构的含义的Html的标签 。主要目的就是让大家直观的认识标签和属性的用途和作用。 

 那语义化的网页的好处，最主要的就是对搜索引擎友好，有了良好的结构和语义你的网页内容自然容易被搜索引擎抓取，你网站的推广便可以省下不少的功夫。我们的网站都希望能再前几名中展示出来,所以我们要遵守语义化,在合理的地方使用合理的标签.这是非常重要的. 

 作用： 1.结构更好,更利于搜索引擎的抓取（SEO的优化）和开发人员的维护(可维护性更高,因为结构清晰,so易于阅读). 2.更有利于特殊终端的阅读(手机,个人助理等）. 

# 16.内存泄漏 ????

浏览器调试工具里有工具可以排查，可以定位内存泄露的地方 ？

（1）定义：程序的运行需要内存。只要程序提出要求，操作系统或者运行时（runtime）就必须供给内存。对于持续运行的服务进程（daemon），必须及时释放不再用到的内存。否则，内存占用越来越高，轻则影响系统性能，重则导致进程崩溃。不再用到的内存，没有及时释放，就叫做内存泄漏（memory leak） 

 （2)目前JS的垃圾回收机制无非就是两种：

1.基本的垃圾回收算法称为**“标记-清除”**，定期执行以下“垃圾回收”步骤:

- 垃圾回收器获取根并**“标记”**(记住)它们。
- 然后它访问并“标记”所有来自它们的引用。
- 然后它访问标记的对象并标记它们的引用。所有被访问的对象都被记住，以便以后不再访问同一个对象两次。
- 以此类推，直到有未访问的引用(可以从根访问)为止。
- 除标记的对象外，所有对象都被删除。 

2.最常使用的方法叫做["引用计数"](https://en.wikipedia.org/wiki/Reference_counting)（reference counting）：语言引擎有一张"引用表"，保存了内存里面所有的资源（通常是各种值）的引用次数。如果一个值的引用次数是`0`，就表示这个值不再用到了，因此可以将这块内存释放。 

如果一个值不再需要了，引用数却不为`0`，垃圾回收机制无法释放这块内存，从而导致内存泄漏。 

最好能有一种方法，在新建引用的时候就声明，哪些引用必须手动清除，哪些引用可以忽略不计，当其他引用消失以后，垃圾回收机制就可以释放内存。这样就能大大减轻程序员的负担，你只要清除主要引用就可以了。

ES6 考虑到了这一点，推出了两种新的数据结构：[WeakSet](http://es6.ruanyifeng.com/#docs/set-map#WeakSet) 和 [WeakMap](http://es6.ruanyifeng.com/#docs/set-map#WeakMap)。它们对于值的引用都是不计入垃圾回收机制的，所以名字里面才会有一个"Weak"，表示这是弱引用。

下面以 WeakMap 为例，看看它是怎么解决内存泄漏的。

> ```
> const wm = new WeakMap();
> 
> const element = document.getElementById('example');
> 
> wm.set(element, 'some information');
> wm.get(element) // "some information"
> ```

上面代码中，先新建一个 Weakmap 实例。然后，将一个 DOM 节点作为键名存入该实例，并将一些附加信息作为键值，一起存放在 WeakMap 里面。这时，WeakMap 里面对`element`的引用就是弱引用，不会被计入垃圾回收机制。

也就是说，DOM 节点对象的引用计数是`1`，而不是`2`。这时，一旦消除对该节点的引用，它占用的内存就会被垃圾回收机制释放。Weakmap 保存的这个键值对，也会自动消失。

基本上，如果你要往对象上添加数据，又不想干扰垃圾回收机制，就可以使用 WeakMap。

# 17.跨域问题@ 

**一、jsonp 原理**

基本思想是，网页通过添加一个`<script>`元素，向服务器请求JSON数据，这种做法不受同源政策限制；服务器收到请求后，将数据放在一个指定名字的回调函数里传回来。 

详细解释：在页面上有三种资源是可以与页面本身不同源的。它们是：js脚本，css样式文件，图片。jsonp就是利用了<script>标签可以链接到不同源的js脚本，来到达跨域目的。当链接的资源到达浏览器时，浏览器会根据他们的类型来采取不同的处理方式，比如，如果是css文件，则会进行对页面 repaint，如果是img 则会将图片渲染出来，如果是script 脚本，则会进行执行,我们可以进行跨域取得数据 

首先，网页动态插入`<script>`元素，由它向跨源网址发出请求。

> ```
> function addScriptTag(src) {
>   var script = document.createElement('script');
>   script.setAttribute("type","text/javascript");
>   script.src = src;
>   document.body.appendChild(script);
> }
> window.onload = function () {
>   addScriptTag('http://example.com/ip?callback=foo');
> }
> function foo(data) {
>   console.log('Your public IP address is: ' + data.ip);
> };
> ```

上面代码通过动态添加`<script>`元素，向服务器`example.com`发出请求。注意，该请求的查询字符串有一个`callback`参数，用来指定回调函数的名字，这对于JSONP是必需的。

服务器收到这个请求以后，把传递进来的函数名和它需要给你的数据拼接成一个字符串 ，会将数据放在回调函数的参数位置返回。

> ```
> foo({ "ip": "8.8.8.8"});
> ```

由于`<script>`元素请求的脚本，直接作为代码运行。这时，只要浏览器定义了`foo`函数，该函数就会立即调用。作为参数的JSON数据被视为JavaScript对象，而不是字符串，因此避免了使用`JSON.parse`的步骤。

**jsonp 跨域与 ajax**  

知道了 jsonp 跨域的原理，但是这和ajax有什么关系呢？显然script标签加载js脚本和ajax一点关系都没有，在没有ajax技术之前，script标签就存在了的。只不过是jquery的封装，使用了ajax来向服务器传递 jsonp 和 jsonpCallback 这两个参数而已。我们服务器端和客户端实现对参数 jsonp 和 jsonpCallback 的值，协调好，那么就没有必要使用ajax来传递着两个参数了，就像上面第二个例子那样，直接构造一个script标签就行了。不过实际上，我们还是会使用ajax的封装，因为它在调用完成之后，又将动态添加的script标签去掉了

**除了jsonp跨域方法之外的其他跨域方法**

其实除了jsonp跨域之外，还有其他方法绕过同源策略，

1）??因为同源策略是针对客户端的，在服务器端没有什么同源策略，是可以随便访问的，所以我们可以通过下面的方法绕过客户端的同源策略的限制：客户端先访问 同源的服务端代码，该同源的服务端代码，使用httpclient等方法，再去访问不同源的 服务端代码，然后将结果返回给客户端，这样就间接实现了跨域。

===== Node中间件代理(两次跨域)？？？？

实现原理：**同源策略是浏览器需要遵循的标准，而如果是服务器向服务器请求就无需遵循同源策略。**
代理服务器，需要做以下几个步骤：

- 接受客户端请求 。
- 将请求 转发给服务器。
- 拿到服务器 响应 数据。
- 将 响应 转发给客户端。

 

**二、CORS**（Cross-Origin Resource Sharing 跨域资源共享）：

在服务端开启cors也可以支持浏览器的跨域访问。jsonp和cors的区别是jsonp几乎所有浏览器都支持，但是只能是get，而cors有些老浏览器不支持，但是get/post都支持

1.cors简单请求和非简单请求的区别 ？？？？

- 简单请求：

> （1) 请求方法是以下三种方法之一：HEAD GET POST
>
> （2）HTTP的头信息不超出以下几种字段：
>
> - Accept
> - Accept-Language
> - Content-Language
> - Last-Event-ID
> - Content-Type：只限于三个值`application/x-www-form-urlencoded`、`multipart/form-data`、`text/plain`

？？？这是为了兼容表单（form），因为历史上表单一直可以发出跨域请求。AJAX 的跨域设计就是，只要表单可以发，AJAX 就可以直接发。

对于简单请求，浏览器直接发出CORS请求。具体来说，就是在头信息之中，增加一个`Origin`字段。 用来说明，本次请求来自哪个源（协议 + 域名 + 端口）。服务器根据这个值，决定是否同意这次请求。

如果`Origin`指定的源，不在许可范围内，服务器会返回一个正常的HTTP回应。浏览器发现，这个回应的头信息没有包含`Access-Control-Allow-Origin`字段（详见下文），就知道出错了

如果`Origin`指定的域名在许可范围内，服务器返回的响应，会多出几个头信息字段。

> ```
> Access-Control-Allow-Origin: http://api.bob.com//请求时Origin字段的值或者*，表示接受任意域名的请求。
> Access-Control-Allow-Credentials: true、、可选。它的值是一个布尔值，表示是否允许发送Cookie。默认情况下，Cookie不包括在CORS请求之中
> Access-Control-Expose-Headers: FooBar//想6个基本字段外的拿到其他字段，必须指定
> Content-Type: text/html; charset=utf-8
> ```

- 凡是不同时满足上面两个条件，就属于非简单请求。

非简单请求是那种对服务器有特殊要求的请求，比如请求方法是`PUT`或`DELETE`，或者`Content-Type`字段的类型是`application/json`。

非简单请求的CORS请求，会在正式通信之前，增加一次HTTP查询请求，称为"预检"请求（preflight）。

浏览器先询问服务器，当前网页所在的域名是否在服务器的许可名单之中，以及可以使用哪些HTTP动词和头信息字段。只有得到肯定答复，浏览器才会发出正式的`XMLHttpRequest`请求，否则就报错。

**三、配置代理服务器** ？

jsonp不是很灵活，只能发送get请求，不能发送psot请求，而cors虽然可以支持多种请求格式，但是如果请求携带cookie的话，还需要服务端和客户端分别配置一下，个人感觉也很麻烦。相对于前两种，使用代理服务器解决跨域问题就简单了好多。

浏览器由于同源策略的原因，不同域名之间发送ajax请求，响应的数据不会被浏览器加载。而服务器向服务器发送请求则没有同源策略的限制。 

代理服务器只是起一个中转作用，配置代理服务器的方法有很多种，比如利用apache、nginx、tomcat等等，今天给大家介绍的是用nodejs配置代理服务器，用nodejs配置代理服务器，我们需要借助两个npm包，一个是web开发框架**express**，一个是express中间件**http-proxy-middleware** 。 

这个**http-proxy-middleware**中间件用的很广泛，在vue-cli或者create-react-app生成的项目中都内置了这个中间件 





nginx跨域原理   nginx反向代理？？？？？？？？？？？？？？？？？？？？？？

实现原理类似于Node中间件代理，需要你搭建一个中转nginx服务器，用于转发请求。

使用nginx反向代理实现跨域，是最简单的跨域方式。只需要修改nginx的配置即可解决跨域问题，支持所有浏览器，支持session，不需要修改任何代码，并且不会影响服务器性能。

实现思路：通过nginx配置一个代理服务器（域名与domain1相同，端口不同）做跳板机，反向代理访问domain2接口，并且可以顺便修改cookie中domain信息，方便当前域cookie写入，实现跨域登录。

先下载[nginx](http://nginx.org/en/download.html)，然后将nginx目录下的nginx.conf修改如下:



四、WebSocket是一种通信协议，

使用`ws://`（非加密）和`wss://`（加密）作为协议前缀。该协议不实行同源政策，只要服务器支持，就可以通过它进行跨源通信。 因为浏览器发出的WebSocket请求的头信息 有了`Origin`这个字段，所以WebSocket才没有实行同源政策。因为服务器可以根据这个字段，判断是否许可本次通信。 

#  18.iframe 缺点 通信？

 标签规定一个内联框架，在当前html中嵌入另一个文档; 简而言之，iframe标签是框架的一种形式，一般用来包含别的页面。 

![img](C:\Users\superssssss\Desktop\Interview\mj-images\20190618084734291.png) 

① iframe会阻塞主页面的Onload事件；
② 搜索引擎的检索程序无法解读这种页面，不利于SEO;
③ 会影响页面的并行加载。
知识补充：什么是并行加载？
并行加载：同一时间针对同一域名下的请求。一般情况，iframe和所在页面在同一个域下面，而浏览器的并加载的数量是有限制的。

提出解决方案：使用js动态给iframe的src加载页面内容，示例代码如下：

```
 <iframe id="fram"></iframe>
 document.getelementbyid("fram").src="a2.html"
```

**iframe通信，同源和不同源两种情况，多少种方法**

根据iframe中src属性是同域链接还是跨域链接，通信方式也不同 

一、同域下父子页面的通信   

父页面调用子页面方法：FrameName.window.childMethod();   

子页面调用父页面方法：parent.window.parentMethod(); 

DOM元素访问：   获取到页面的window.document对象后，即可访问DOM元素  注意事项 ：  要确保在iframe加载完成后再进行操作，如果iframe还未加载完成就开始调用里面的方法或变量，会产生错误。判断iframe是否加载完成有两种方法：  1. iframe上用onload事件   2. 用document.readyState=="complete"来判断

二、跨域父子页面通信方法

父页面向子页面传递数据  ** 2.实现的技巧是利用location对象的hash值**，通过它传递通信数据。在父页面设置iframe的src后面多加个data字符串，然后在子页面中通过某种方式能即时的获取到这儿的data就可以了，例如：   1. 在子页面中通过setInterval方法设置定时器，监听location.href的变化即可获得上面的data信息   2. 然后子页面根据这个data信息进行相应的逻辑处理   

子页面向父页面传递数据   实现技巧就是利用一个代理iframe，它嵌入到子页面中，并且和父页面必须保持是同域，然后通过它充分利用上面第一种通信方式的实现原理就把子页面的数据传递给代理iframe，然后由于代理的iframe和主页面是同域的，所以主页面就可以利用同域的方式获取到这些数据。使用 window.top或者window.parent.parent获取浏览器最顶层window对象的引用。   

**1.document.domain**          2.在上面

如果两个窗口一级域名相同，只是二级域名不同，那么设置document.domain属性，就可以规避同源政策，拿到DOM。

对于完全不同源的网站，目前有三种方法，可以解决跨域窗口的通信问题。

> - 片段识别符（fragment identifier）
> - window.name
> - 跨文档通信API（Cross-document messaging）

**3.window.name**

浏览器窗口有`window.name`属性。这个属性的最大特点是，无论是否同源，只要在同一个窗口里，前一个网页设置了这个属性，后一个网页可以读取它。

父窗口先打开一个子窗口，载入一个不同源的网页，该网页将信息写入`window.name`属性。

> ```
> window.name = data;
> ```

接着，子窗口跳回一个与主窗口同域的网址。

> ```
> location = 'http://parent.url.com/xxx.html';
> ```

然后，主窗口就可以读取子窗口的`window.name`了。

> ```
> var data = document.getElementById('myFrame').contentWindow.name;
> ```

这种方法的优点是，`window.name`容量很大，可以放置非常长的字符串；缺点是必须监听子窗口`window.name`属性的变化，影响网页性能

**4.window.postMessage**

上面两种方法都属于破解，HTML5为了解决这个问题，引入了一个全新的API：跨文档通信 API（Cross-document messaging）。这个API为`window`对象新增了一个`window.postMessage`方法，允许跨窗口通信，不论这两个窗口是否同源。

举例来说，父窗口`http://aaa.com`向子窗口`http://bbb.com`发消息，调用`postMessage`方法就可。

> ```
> var popup = window.open('http://bbb.com', 'title');
> popup.postMessage('Hello World!', 'http://bbb.com');
> ```

子窗口向父窗口发送消息的写法类似。

> ```
> window.opener.postMessage('Nice to see you', 'http://aaa.com');
> ```

父窗口和子窗口都可以通过`message`事件，监听对方的消息。

> ```
> window.addEventListener('message', function(e) {
>   console.log(e.data);
> },false);
> ```



#  19.GET和POST的区别

get参数通过url传递，post放在request body中。

get请求在url中传递的参数是有长度限制的，而post没有。

get比post更不安全，因为参数直接暴露在url中，所以不能用来传递敏感信息。

get请求只能进行url编码，而post支持多种编码方式

get请求会浏览器主动cache，而post支持多种编码方式。

get请求参数会被完整保留在浏览历史记录里，而post中的参数不会被保留。

GET和POST本质上就是TCP链接，并无差别。但是由于HTTP的规定和浏览器/服务器的限制，导致他们在应用过程中体现出一些不同。

GET产生一个TCP数据包；POST产生两个TCP数据包。

# 20.前端优化？？？

降低请求量：合并资源，减少HTTP 请求数，minify / gzip 压缩，webP，lazyLoad。

加快请求速度：预解析DNS，减少域名数，并行加载，CDN 分发。

缓存：HTTP 协议缓存请求，离线缓存 manifest，离线数据缓存localStorage。

渲染：JS/CSS优化，加载顺序，服务端渲染，pipeline。

性 能 优 化：
减少 HTTP 请求
使用内容发布网络（CDN）
添加本地缓存
压缩资源文件
将 CSS 样式表放在顶部，把 javascript 放在底部（浏览器的运行机制决定）
避免使用 CSS 表达式
减少 DNS 查询
使用外部 javascript 和 CSS
避免重定向
图片 lazyLoad

# 21.计算机网络分层结构，每层协议 

![img](C:\Users\superssssss\Desktop\Interview\mj-images\format,png) 



链接层
Ethernet  美 [ˈiːθərnet] （以太网协议）：规定了一组电信号构成一个数据包，叫做"帧"（Frame）。每一帧分成两个部分：标头（Head）和数据（Data）
ARP协议：Address Resolution Protocol，地址解析协议，将已知IP地址转换为MAC地址
BARP协议：Rebellion（美 [rɪˈbeljən] 反逆）Address Resolution Protocol，逆转地址解析协议，将已知MAC地址转换为IP地址
VLAN：Virtual Local Area Network，虚拟局域网
STP：Spanning Tree Protocol，生成树协议
ppp点对点协议 ：Point-to-Point Protocol点到点协议

**"链接层"的功能，它在"实体层"的上方，确定了0和1的分组方式** 

网络层
ICMP：Internet Control Message Protocol，互联网控制报文协议
IP ：Internet Protocol，互联网协议
OSPF：Open Shortest Path First，开放式最短路径优先
BGP：Border Gateway Protocol，边界网关协议
IPSec：Internet Protocol Security，互联网安全协议
GRE：Generic Routing Encapsulation，通用路由封转协议

网络层"出现以后，每台计算机有了两种地址，一种是MAC地址，另一种是网络地址。两种地址之间没有任何联系，MAC地址是绑定在网卡上的，网络地址则是管理员分配的，它们只是随机组合在一起。网络地址帮助我们确定计算机所在的子网络，MAC地址则将数据包送到该子网络中的目标网卡

IP协议的作用主要有两个，一个是为每一台计算机分配IP地址，另一个是确定哪些地址在同一个子网络。 

传输层

- UDP：User Datagram Protocol，用户数据报协议
- TCP：Transmission Control Protocol，传输控制协议

应用层

DHCP： Dynamic Host Configuration Protocol，动态主机配置协议
HTTP： Hypertext Transfer Protocol，超文本传输协议
HTTPS： Hyper Text Transfer Protocol over Secure Socket Layer 或 Hypertext Transfer Protocol Secure，超文本传输安全协议
RTMP： Real Time Messaging Protocol，实时消息传输协议
P2P： Peer-to-peer networking，对等网络
DNS： Domain Name System，域名系统
GTP： GPRSTunnellingProtocol，GPRS隧道协议
RPC：Remote Procedure Call，远程过程调用

# 22.宏任务微任务 ??

网页白屏会从什么方面解决 ????