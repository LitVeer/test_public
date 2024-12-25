<h1>复习文档</h1>
<p>计算机体系本质：不断的抽象</p>
<p>‍</p>
<h2>第三章 数据链路层</h2>
<h3>三个基本问题</h3>
<ol>
<li>
<p>封装成帧</p>
<blockquote>
<p>​<img src="assets/image-20241223183816-az0xz5j.png" alt="image" />​</p>
<p>‍</p>
</blockquote>
<p>在数据部分的前后端添加帧首部与尾部</p>
<p>且规定 最大传输单元 MTU</p>
</li>
<li>
<p>透明传输</p>
<p>为解决数据内出现 SOH EOT 相同数据而提出</p>
<p>解决方法：转义字符 ESC（1110 0000）</p>
</li>
<li>
<p>差错检测（<strong>循环冗余检验 CRC</strong>）</p>
<p><strong>流程</strong>：</p>
<ol>
<li><strong>假定发送端的原始数据为</strong> <em><strong>k</strong></em> <strong>个比特。</strong></li>
<li><strong>对原始数据进行CRC</strong> <strong>运算，产生</strong> <em><strong>n</strong></em> <strong>位冗余码供差错检测使用。</strong></li>
<li><strong>将原始数据和冗余码构成帧发送出去。</strong></li>
<li><strong>接收端检验。</strong></li>
</ol>
<p>‍</p>
<p><strong>CRC运算：</strong></p>
<ol>
<li>在原始数据后添加 n 个 0</li>
<li>用得到的（k + n）位数据 除以 收发双方约定好的长度（n + 1）位的除数 P。得到的 n 位余数即为冗余码</li>
<li>冗余码被称为帧检验序列 FCS</li>
</ol>
<p>‍</p>
<p><strong>计算过程</strong>：</p>
<blockquote>
<p>​<img src="assets/image-20241223184822-de7qp24.png" alt="image" />​</p>
</blockquote>
<p><strong>接收端检验：</strong></p>
<blockquote>
<p>​<img src="assets/image-20241223184943-1ddmo3m.png" alt="image" />​</p>
</blockquote>
</li>
</ol>
<p>‍</p>
<p>‍</p>
<h3>CSMA / CD 协议（<strong>载波监听多点接入/碰撞检测</strong>）</h3>
<h6>流程：</h6>
<blockquote>
<p>​<img src="assets/image-20241223185529-7f9erym.png" alt="image" />​</p>
</blockquote>
<blockquote>
<p>​<img src="assets/image-20241223185539-nsffo9f.png" alt="image" />​</p>
</blockquote>
<p>‍</p>
<p>从上述我们不难看出;</p>
<ul>
<li>使用 CSMA/CD 协议的以太网不能进行全双工通信而只能进行双向交替通信（半双工通信）。(一个站不能同时进行发送和接收，但可以边发送边监听信道)</li>
<li>每台计算机在发送数据之后的一小段时间内，存在着遭遇碰撞的可能性。</li>
<li>16字总结：先听后发，边听边发，冲突停止，延迟重发。</li>
</ul>
<p>‍</p>
<p>‍</p>
<p>‍</p>
<h6>规避算法</h6>
<blockquote>
<ol>
<li>基本规避时间为2t (2t 称为争用期或碰撞窗口), 以太网规定具体的争用期时间为51.2µs （这也是为什么25.6<em>10^-6 * 2.0</em>10^8 = 5120m【实际上并没有这么远】局域网覆盖范围有上限的原因吧），对于10Mbit/s以太网也可以说争用期是512比特时间，1比特时间就是发送1比特需要的时间。例如：争用期是512比特，即争用期是发送512比特所需时间。</li>
<li>发生碰撞的计算机在停止发送数据后，等待信道空闲后不是立即发送而是要推迟（即退避）一个随机的时间才能再发送数据。具体的截断二进制指数退避算法如下：<br />
§  规 定 基本退避 时间为争用 期 2 t ；<br />
§  定义参数 k ， k = Min[ 重传次数 , 10 ] ；<br />
§  从整数集合 [0 , 1, …, (2 k - 1)] 中随机地取出一个数，记为 r 。 重传应推后的时间就是 r 倍的基本退避时间。<br />
§  当 重传达 16 次 仍不能成功 时则丢弃 该帧，并向高层报告 。</li>
</ol>
</blockquote>
<p>‍</p>
<p>‍</p>
<p>‍</p>
<h6>最短有效帧长</h6>
<p>如果一个帧太短，发送方很快就能发送完毕而检测不到碰撞，但这个帧却有可能在到达目的主机的途中与其他主机发送的帧发生碰撞。由于不再监听信道，发送方无法知道这个帧发生了碰撞。</p>
<p>因此：</p>
<ol>
<li>对于 10 Mbit/s 的传统以太网，在争用期内可发送 512 bit，即 64 字节。若前 64 字节未发生碰撞，则后续发送的数据就一定不会发生碰撞。</li>
<li>以太网规定最短有效帧长为 64 字节，凡长度小于 64 字节的帧都是无效帧。如果要发送的数据非常少，则必须加入一些填充字节，使帧长不小于 64 字节。</li>
</ol>
<p>‍</p>
<p>‍</p>
<blockquote>
<p>关于涉及最短帧长的考点：</p>
<p>需要理解最短帧长的概念及计算公式， 最短帧长就是要求发送方在数据的传播过程中仍然不停地发送数据帧，即争用时间内发出的数据总数。 在CSMA/CD协议中，只有收到大于最短帧长的数据帧且经过争用期这段时间还没有检测到碰撞，才认为未检测到数据冲突。</p>
<p>最小数据帧长度 = 2 * 传播时延 * 数据传输率</p>
<p>因此当最短数据帧长度减小时，根据公式，如果保持数据传输率不变，传播时延就必须要减少，而传播时延等于站点间距离除于数据的传播速率，从而站点间距离就必须要减少。</p>
</blockquote>
<p>‍</p>
<p>‍</p>
<h6><strong>帧间最小间隔</strong></h6>
<p>以太网还规定帧间最小间隔为 9.6 <strong>µs</strong> 。一台计算机在检测到总线开始空闲后，还要等待 9.6 <strong>µs</strong> 才能再次发送数据。</p>
<pre><code>这样做是为了使刚刚收到数据帧的计算机的接收缓存来得及清理 ，为接收下一帧做好 准备。
</code></pre>
<p>‍</p>
<p>‍</p>
<p>‍</p>
<h2>第四章 网络层</h2>
<p>‍</p>
<h3>无分类编址  CIDR</h3>
<p><strong>IP地址</strong> ::= { &lt;网络号&gt;  ：&lt;主机号&gt; }</p>
<p><strong>子网掩码</strong>：用于划分网络号与主机号</p>
<p>简单例子：192.168.128.5/18</p>
<table>
<thead>
<tr>
<th align="center"></th>
<th align="center">十进制</th>
<th align="center">二进制</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center">IP地址</td>
<td align="center">192.168.128.5</td>
<td align="center">11000000.10101000.10000000.00000101</td>
</tr>
<tr>
<td align="center">子网掩码</td>
<td align="center">255.255.192.0</td>
<td align="center">11111111.11111111.11000000.00000000</td>
</tr>
<tr>
<td align="center">网络号（ IP &amp; 掩码）</td>
<td align="center">192.168.128</td>
<td align="center">11000000.10101000.10000000.00000000</td>
</tr>
<tr>
<td align="center">主机号</td>
<td align="center">.5</td>
<td align="center">00000000.00000000.00000000.00000101</td>
</tr>
</tbody>
</table>
<p>‍</p>
<p><strong>子网划分</strong></p>
<table>
<thead>
<tr>
<th align="center">地址块</th>
<th align="center">十进制</th>
<th align="center">二进制</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center">原地址块</td>
<td align="center">192.168.0.0/16</td>
<td align="center">11000000.10101000.00000000.00000000</td>
</tr>
<tr>
<td align="center">子网1</td>
<td align="center">192.168.0.0/18</td>
<td align="center">11000000.10101000.00000000.00000000</td>
</tr>
<tr>
<td align="center">子网2</td>
<td align="center">192.168.64.0/18</td>
<td align="center">11000000.10101000.01000000.00000000</td>
</tr>
<tr>
<td align="center">子网3</td>
<td align="center">192.168.128.0/18</td>
<td align="center">11000000.10101000.10000000.00000000</td>
</tr>
<tr>
<td align="center">子网4</td>
<td align="center">192.168.192.0/18</td>
<td align="center">11000000.10101000.11000000.00000000</td>
</tr>
</tbody>
</table>
<p>原地址块需划分4（2^n^）个子网，则向主机号借用 2（n）位数作为子网号，则新子网的网络号为原网络号+子网号，</p>
<p>‍</p>
<p>‍</p>
<p>‍</p>
<p>‍</p>
<h3>地址解析协议 ARP</h3>
<p>ARP 高速缓存：用于保存IP 地址与MAC 地址映射表</p>
<p>应用场景：主机间 IP 地址交换</p>
<ol>
<li>若本地主机不存在目的主机 MAC 地址，则广播 ARP包</li>
<li>目的主机收到 ARP 包后，单播本地主机，发送带有本机 MAC 信息的ARP 包</li>
</ol>
<p>​<img src="assets/image-20241217214539-6iynk2f.png" alt="image" />​</p>
<p>Notice：每个地址映射都设置生存时间（10-20min），超时则删除映射</p>
<p>‍</p>
<p>‍</p>
<p>‍</p>
<h3>网际控制报文协议 ICMP</h3>
<p>ICMP：主机或路由器用于报告差错与异常情况（使用校验和）</p>
<p>Notice：</p>
<ol>
<li>ICMP 报文封装在 IP 数据报中，作为 IP 数据报的数据部分。但 ICMP 不是高层协议，而是 IP 层的协议</li>
<li>ICMP只能搭配IPv4工作，如果是IPv6（下文会介绍），需要使用ICMPv6。</li>
<li>ICMP 允许主机或路由器报告差错情况和提供有关异常情况的报告。</li>
</ol>
<p>‍</p>
<h6>ICMP 报文格式：</h6>
<p>​<img src="assets/image-20241217212343-j8419g0.png" alt="image" />​</p>
<h6>ICMP 报文类型</h6>
<p>​<img src="assets/image-20241217212725-5qf6ft2.png" alt="image" />​</p>
<p>‍</p>
<p>‍</p>
<h3>内部网关协议	RIP</h3>
<h6><strong>RIP距离向量算法：</strong></h6>
<p>相邻路由：X</p>
<ol>
<li>
<p>接受相邻路由发送的路由表</p>
<ol>
<li>将路由表中下一跳主机改为  X</li>
<li>将路由表中距离+1</li>
</ol>
</li>
<li>
<p>对比原路由表与新路由表</p>
<ol>
<li>若原路由表没有该目的网络，则添加到原路由表</li>
<li>若有则比较下一跳路由，相等则更新距离</li>
<li>若不相等则对比距离，更新距离小的目的网络</li>
</ol>
</li>
</ol>
<p>‍</p>
<p>‍</p>
<h6>RIP报文格式</h6>
<p>​<img src="assets/image-20241217205839-0a39nc3.png" alt="image" />​</p>
<p>‍</p>
<p>‍</p>
<h6>RIP 优缺点</h6>
<ol>
<li>协议、算法实现简单，网络开销小</li>
<li>RIP 最大距离为16，限制了网络规模</li>
<li>消息传播较慢</li>
</ol>
<p>‍</p>
<p>‍</p>
<p>‍</p>
<p>‍</p>
<h2>第五章 传输层</h2>
<p>网络层为主机之间通信提供服务</p>
<p>传输层为应用进程之间的通信提供服务</p>
<p>‍</p>
<h3>TCP与UDP</h3>
<h5>UDP：用户数据报协议</h5>
<h6>特点：</h6>
<ol>
<li>
<p>UDP 是无连接的，即发送数据之前不需要建立连接。</p>
</li>
<li>
<p>UDP 使用尽最大努力交付，即不保证可靠交付。</p>
</li>
<li>
<p>UDP 是面向报文的，即一次发送和交付一个完整的报文。</p>
</li>
</ol>
<blockquote>
<p>解释：UDP 对应用层交下来的报文，既不合并，也不拆分，而是保留这些报文的边界。接收方 UDP 对 IP 层交上来的 UDP 用户数据报，在去除首部后就原封不动地交付上层的应用进程，一次交付一个完整的报文，因此应用程序必须选择合适大小的报文：</p>
<p>若报文太长，UDP 把它交给 IP 层后，IP 层在传送时可能要进行分片，这会降低 IP 层的效率。<br />
若报文太短，UDP 把它交给 IP 层后，会使 IP 数据报的首部的相对长度太大，这也降低了 IP 层的效率。</p>
</blockquote>
<ol start="4">
<li>UDP 没有拥塞控制，很适合实时通信，因为实时通信要求源主机以恒定的速率发送数据，并允许丢失部分数据。</li>
<li>UDP 支持一对一、一对多、多对一和多对多的交互通信。</li>
<li>UDP 的首部开销小，只有 8 个字节。</li>
</ol>
<p>‍</p>
<h6>UDP首部格式</h6>
<ul>
<li>源端口：源端口号。在需要对方回信时选用，不需要时可全写０。</li>
<li>目的端口：目的端口号。在终点交付报文时必须用到。若不存在对应于该端口的应用进程，就丢弃报文，并由ICMP发送“端口不可达”的差错报文给发送方，这就是上一章网络层讲的路由分析诊断程序tracert。</li>
<li>长度：UDP 用户数据报的长度。其最小值是８字节（仅有首部）。</li>
<li>检验和：差错校验机制，检测 UDP 数据报在传输中是否有错，有就丢弃。</li>
</ul>
<p>​<img src="assets/image-20241218144200-b6lvr83.png" alt="image" />​</p>
<p>‍</p>
<p>‍</p>
<h5>TCP：传输控制协议</h5>
<h6>特点:</h6>
<ol>
<li>
<p>TCP 是面向连接的运输层协议。TCP 在传送数据之前，必须先建立连接；在传送数据完毕后，必须释放已经建立的连接。</p>
<blockquote>
<p>传输前使用&quot;三报文握手&quot;,传输后&quot;四报文挥手&quot;</p>
</blockquote>
</li>
<li>
<p>每一条 TCP 连接只能有两个端点(endpoint)，每一条 TCP 连接只能是点对点的（一对一）。</p>
<blockquote>
<p>使用套接字连接: socket = { IP地址 :  端口 }</p>
</blockquote>
<blockquote>
<p>TCP连接 ::= {socket1, socket2} = { { IP<del>1</del>: port<del>1</del> }  ,  { IP<del>2</del>, port<del>2</del> } }</p>
</blockquote>
</li>
<li>
<p>TCP 提供可靠交付的服务。通过 TCP 连接传送的数据，无差错、不丢失、不重复，并且按序到达。</p>
</li>
<li>
<p>TCP 提供全双工通信</p>
</li>
<li>
<p>面向字节流</p>
<blockquote>
<p>指TCP不关心应用层给的报文长度, 而是根据目的主机的窗口值与当前网路拥塞程度调整报文长度,进行发送</p>
</blockquote>
</li>
</ol>
<p>‍</p>
<p>‍</p>
<h6>可靠传输工作原理</h6>
<ol>
<li>
<p>停止等待协议</p>
<ol>
<li>
<p>若无差错</p>
<p>发送每一个分组后，就停止发送，等待返回确认，收到确认后再发送下一个分组</p>
</li>
<li>
<p>若有差错</p>
<p>超时重传：在一定时间内没收到确认，则重传分组</p>
<p>对于重复的分组以及晚到的确认，则丢弃</p>
</li>
</ol>
</li>
</ol>
<p>‍</p>
<ol start="2">
<li>
<p><strong>滑动窗口协议（连续ARQ协议）----停止等待协议的改进</strong></p>
<blockquote>
<p><img src="assets/0d52bacde34258bdeefeca96adabfad-20241221000815-vk8961t.jpg" alt="0d52bacde34258bdeefeca96adabfad" />​</p>
</blockquote>
<p>连续发送序号为5 - 9的分组，接收方在一定时间内后返回最大的确认号，发送方发送指定序号的分组</p>
<p>若成功接受，则根据发送方、接受方根据最大确认号向前滑动窗口</p>
</li>
<li>
<p>选择确认 SACK</p>
<p>接收到的字节流序号不连续，能否设法让发送方只传送缺少的数据而不重传已经正确到达的数据？</p>
<p>SACK简介：</p>
<p>TCP的报文段的确认字段是一种累积确认，它只通告收到的最后一个按序列到达的字节，而没有通告所有收到的失序到达的那些字节，虽然这些字节已经被接收方接收并暂存在接收缓存中。这些没有被确认的字节可能会因为超时而被发送方重传。所以一个可选的功能选择确认（Selective ACK，SACK）可以解决。<br />
每一个字节块需要用两个边界序号来标识。TCP在首部提供了一个 可 变 长的 “ SACK选项字段 ”来存放这些信息。除此之外，要使用选择确认功能，在建立TCP连接时，双方还要分别在SYN报文段和SYN+ACK报文段的首部选项中添加“允许SACK选项字段”，表示都支持选择确认功能。</p>
<p>注意：SACK(Selective ACK)是TCP选项，它使得接收方能告诉发送方哪些报文段丢失，哪些报文段重传了，哪些报文段已经提前收到等信息。根据这些信息TCP就可以只重传哪些真正丢失的报文段。需要注意的是只有收到失序的分组时才会可能会发送SACK，TCP的ACK还是建立在累积确认的基础上的。也就是说如果收到的报文段与期望收到的报文段的序号相同就会发送累积的ACK，SACK只是针对失序到达的报文段的。SACK包括了两个TCP选项，一个选项用于标识是否支持SACK，是在TCP连接建立时时发送；另一种选项则包含了具体的SACK信息。</p>
</li>
</ol>
<p>‍</p>
<p>‍</p>
<h6>TCP的流量控制</h6>
<p>流量控制(flow control)就是让发送方的发送速率不要太快，要让接收方来得及接收</p>
<blockquote>
<p>流量控制的基本方法：就是接收方根据自己的接收能力控制发送方的发送速率。TCP采用控制发送方发送窗口大小（发送窗口的上限就是TCP报文段首部窗口写入的数值大小）的方法来实现TCP连接上的流量控制。<br />
在建立连接时，接收方在报头中会携带接收窗口大小的信息。发送方的发送窗口不能超过接收方的接收窗口的数值。需要注意的是，TCP的窗口单位是字节而不是报文段。</p>
</blockquote>
<p>当发送者收到了一个窗口为0的应答，发送者便停止发送，等待接收者的下一个应答。<strong>若含有新窗口值的报文段在传送过程中丢失，将导致</strong>​<strong>死锁****局面的产生：A</strong> <strong>一直等待</strong> <strong>B</strong> <strong>发送的非零窗口的通知，而</strong> <strong>B</strong> <strong>也一直等待</strong> <strong>A</strong> <strong>发送的数据。</strong></p>
<blockquote>
<p>为了避免流量控制引发的死锁，TCP使用了持续计时器。每当发送者收到一个零窗口的应答后就启动该计时器。时间一到便主动发送零窗口探测报文询问接收者的窗口大小。若接收者仍然返回零窗口，则重置该计时器继续等待；若窗口不为0，则表示应答报文丢失了，此时重置发送窗口后开始发送，这样就避免了死锁的产生。</p>
</blockquote>
<p>‍</p>
<p>‍</p>
<p>‍</p>
<h6>TCP的拥塞控制</h6>
<blockquote>
<p>拥塞控制就是防止过多的数据注入到网络中，以防网络中的路由器或链路过载。它是一个全局性的过程，涉及到所有的主机和路由器。</p>
<p>流量控制是指在给定的发送端和接收端之间的点对点通信量的控制，即接收端抑制发送端发送数据的速率，以便使接收端来得及接收。</p>
</blockquote>
<p>‍</p>
<p><strong>TCP的拥塞控制方法</strong></p>
<p>示意图</p>
<blockquote>
<p>​<img src="assets/image-20241221005025-o8yg5aj.png" alt="image" />​</p>
</blockquote>
<blockquote>
<p>发送方维持一个叫做拥塞窗口 cwnd (congestion win-dow)的状态变量。由于不考虑接收窗口，于是发送方让自己的发送窗口等于拥塞窗口。</p>
<p>发送方控制拥塞窗口的原则是：</p>
<p>●  只要网络没有出现拥塞，拥塞窗口就可以再增大一些，以便把更多的分组发送出去，提高网络的利用率。<br />
●  但只要网络出现拥塞（依据就是出现了 超时 ），拥塞窗口就减小一些，以减少注入到网络中的分组数，缓解网络出现的拥塞。</p>
</blockquote>
<p>‍</p>
<p><strong>TCP 进行拥塞控制的算法有四种：</strong></p>
<p><strong>慢开始</strong> <strong>(slow-start)、<strong>​</strong>拥塞避免</strong> <strong>(congestion avoidance)、<strong>​</strong>快重传</strong> <strong>(fast retransmit)和</strong>​<strong>快恢复</strong> <strong>(fast recovery)</strong>   <strong>。</strong></p>
<ol>
<li>
<p>慢开始</p>
<p>刚建立连接时，传输少量数据量，随后逐渐增大拥塞窗口 cwnd（每一个轮次增大1）</p>
<p><strong>轮次</strong>：发送方把拥塞窗口所允许发送的报文段都连续发送出去，并收到对这些报文段的确认所经历的时间</p>
<blockquote>
<p>注意：发送方只要收到一个对新报文段的确认，其拥塞窗口 cwnd 就立即加 1，并可以立即发送新的报文段，而不需要等这个轮次中所有的确认都收到后再发送新的报文段。</p>
<p>另外，为了防止拥塞窗口 cwnd 增长过大引起网络拥塞，需要设置一个慢开始门限 ssthresh，其用法如下：</p>
<p>●  当 cwnd &lt; ssthresh 时，使用 慢开始 算法。<br />
●  当 cwnd &gt; ssthresh 时，停止使用 慢开始 算法而改用 拥塞避免 算法。<br />
●  当 cwnd = ssthresh 时，既可使用 慢开始 算法，也可使用 拥塞避免 算法。</p>
</blockquote>
</li>
<li>
<p>拥塞避免</p>
<blockquote>
<p>​<img src="assets/image-20241221011811-shhy5xx.png" alt="image" />​</p>
</blockquote>
<p>使cwnd线性增加，若发生拥塞，则把 慢开始门限 设置为当前拥塞窗口一半，cwnd设为1</p>
</li>
<li>
<p>快重传</p>
<p>要求接收方每收到一个失序报文段，就立即发出对已收到报文段的重复确认。当发送方收到连续三个重复确认，就执行“快重传”</p>
</li>
<li>
<p>快恢复</p>
<p>快重传的同时，执行快恢复</p>
<p>快恢复：拥塞窗口cwnd减半，设置 慢开始门限 为同样数值，然后执行拥塞避免算法，使拥塞窗口线性增大</p>
</li>
</ol>
<p>‍</p>
<p>‍</p>
<h6>TCP的三次握手与四次挥手</h6>
<p>图示</p>
<blockquote>
<p>​<img src="assets/0d52bacde34258bdeefeca96adabfad-20241221000815-vk8961t.jpg" alt="0d52bacde34258bdeefeca96adabfad" />​</p>
</blockquote>
<p>发送方先发送连接请求，接收方再发送连接接受，发送再发送确认请求</p>
<blockquote>
<p>x、y：双方选择的初始序号</p>
</blockquote>
<p>开始传输</p>
<p>发送方发送连接释放，接收方发送释放确认，并发送连接释放，发送方再发送释放确认</p>
<blockquote>
<p>u、w：传输最后字节的序号 + 1</p>
<p>v：额外的数据</p>
</blockquote>
<p>Tips：等待约2MSL时间后，连接释放</p>
<p>‍</p>
<p>‍</p>
<h5>TCP与UDP的区别</h5>
<table>
<thead>
<tr>
<th align="center">区别</th>
<th align="center">TCP</th>
<th align="center">UDP</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center">协议数据单元</td>
<td align="center">TCP报文段</td>
<td align="center">UDP报文、UDP用户数据报</td>
</tr>
<tr>
<td align="center">是否连接</td>
<td align="center">传输前建立连接，传输结束要释放连接</td>
<td align="center">无需建立连接，直接交付</td>
</tr>
<tr>
<td align="center">是否可靠</td>
<td align="center">可靠,使用流量控制与拥塞控制</td>
<td align="center">不可靠,不使用流量控制与拥塞控制</td>
</tr>
<tr>
<td align="center">是否有序</td>
<td align="center">有序</td>
<td align="center">无序</td>
</tr>
<tr>
<td align="center">传输速度</td>
<td align="center">慢</td>
<td align="center">快</td>
</tr>
<tr>
<td align="center">连接对象个数</td>
<td align="center">一对一</td>
<td align="center">一对一、一对多、多对一、多对多</td>
</tr>
<tr>
<td align="center">传输方式</td>
<td align="center">面向报文</td>
<td align="center">面向字节流</td>
</tr>
<tr>
<td align="center">首部开销</td>
<td align="center">开销大，20Byte-60Byte</td>
<td align="center">开销小，8Byte</td>
</tr>
<tr>
<td align="center"></td>
<td align="center"></td>
<td align="center"></td>
</tr>
</tbody>
</table>
<p>‍</p>
<p>‍</p>
<p>‍</p>
<p>‍</p>
<p>‍</p>
<p>‍</p>
<p>‍</p>
<h2>第六章 应用层</h2>
<p>‍</p>
<p>‍</p>
<h3>Base64编码</h3>
<p>关于这个编码的规则：</p>
<p>①.把3个字节变成4个字节。</p>
<p>②每76个字符加一个换行符。</p>
<p>③.最后的结束符也要处理。</p>
<p>‍</p>
<p>‍</p>
<p>‍</p>
