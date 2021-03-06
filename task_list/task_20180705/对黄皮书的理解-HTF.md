# True黄皮书让我来解读
## 对初链的认识：
初链（TRUEChina）是一种通用型公链，同时也是一种给商业应用提供基础设施的底层公链，其愿景是打造承载未来商用去中心化的应用，是全球最早实现PBFT-POW混合共识机制设计的公链之一。解决了公有链领域去中心化和性能之间的矛盾。TrueChain初链是以PermissionlessPBFT为基础的公链，它拥有去中心化的社区共建组织架构，是一个面向各高性能行业应用，提供高效发布智能合约、监控全生命周期的管理工具，主要服务广告、版权、人力资源、征信、AI、游戏等行业。
![image](https://user-images.githubusercontent.com/32947900/42924046-11d47e34-8b5b-11e8-8662-fc25ec6dcf6c.jpg)
白皮书-ADTrue初链运作模式图
ADTrue是TrueChain初链的第一个应用，是为全球数字广告行业打造的底层区块链公链。通过汇聚全球数字广告资源和经验，实现数据互通、诚信交易、透明交易和效率提升。在此之后，TrueChain初链会陆续覆盖征信、AI、游戏等各行业，最终形成一个多行业跨链的区块链体系。

## ADTrue解决什么问题？
![image](https://user-images.githubusercontent.com/32947900/42924081-3d37ab28-8b5b-11e8-9a5c-00dbfb4ae500.jpg)
ADTrue应用实例，总结一下，核心上，ADTrue解决三个产业级的问题，连接、共享、诚信。
问题1：连接
现状：数据的连接，取决于BAT等垄断巨头是否开放及数据之间的技术打通标准
未来：基于公链与共识机制，连接并打通全球所有数字广告应用
问题2：共享
现状：完全取决于垄断巨头的意愿
未来：公链自身天然的共享特质
问题3：诚信
现状：第三方监测公司提供诚信报告
未来：区块链自身天然的数据不可篡改特性与节点验证

## 混合共识的优势
混合共识一定意义上来说满足了节点数目的可扩展性，可以在BFT参与者中敌手数目低于阈值且网络环境稳定的时候保持高性能的交易处理能力。混合共识的安全假设清晰明确。相比较一些基于无环有向图的设计，混合共识对于分布式应用的支持比较友好

## 技术架构：
初链的技术架构由下而上分为三层：混合共识机制、智能合约、合约抽象
- PBFT-POW混合共识机制
通俗易懂来讲，就是用PFBT来快速更新账本；用PoW来验证交易，辅助PBFT的成员变动，保留 PBFT 记录账本的机制不动，将超级节点的选取开放给公链，利用 POW 协议作为准系统支持超级节点的动态选取和协议达成，将主干节点社区的组建由私有链与联盟链性质转换为公有链性质。PBFT协议保证价值流通和商业应用所需要的性能，而POW协议保证去中心化。
- 智能合约
智能合约的运行必须依靠虚拟机完成，保证统一智能合约在不同环境下可以运算得到同一结果。初链继承了以太坊的虚拟机（EVM）的设计思路，在 PBFT 上推出 TVM。TVM 将植入每一个进行决策的主干结点，使得它们能根据单个需求进行调用请求。但TVM的构想还并不是很成熟,需要确定TVM的过渡状态、智能合约部署策略及从私链虚拟机向公链虚拟机的转变。同时还要确定从POW节点向PBFT节点转变的参数。
- 合约抽象
合约抽象层将抽象智能合约中的基本商业逻辑，简化开发者设置复杂智能合约的流程。

## 节点选取
从私有链与联盟链到公有链是一个质的转变。现在BFT社区节点选取多采用固定时间段更换一次,更换为新挖矿的P个节点(P为社区节点数目)。但这是反直觉的。如果之前的节点社区表现优异,为什么要强制撤换呢?而如果只因这些节点表现优异就不撤换的话,新节点就难以加入社区。所以需要在其间进行取舍,即每隔一段时间(K天)更换一次节点,而更换频率较低。在这里初链借鉴了Thunderella的想法,即慢链(POW链)可以用于证明BFT超级节点潜在的造假或出错情况。一旦这种情况发生,次日即可进行强制更换节点,而不必等到固定时点。更为关键的是选举机制的设定。一般混合机制是从最近挖矿的矿工节点中选取,但初链采用的是权益证明和随机选取相结合的方式。具体地说,全节点可以临时性冻结它们的代币资产,一定比例的超级节点将分配给全节点中权益证明较高的一些节点;另外部分的节点按照VRF随机过程选取,随机过程的种子由上一次选举的种子和节点社区容量决定。不同于Algorand,初链在此阶段将不会计算权益比例。随机过程选取的节点可以假定是完全不认识的,因此不可信。对于此种潜在情况,初链的策略是估算不可靠节点的比例,让该比例的不可靠节点不会影响正常决策。诚然,掌握巨大运算资源的一方可能操纵VRF随机过程,但这已不在此讨论范围内。

## 时间戳与自然时间限制
传统共识机制默认地给予了矿工、节点社区成员和领导者一定的时间窗口,导致在要求严格时间顺序的商业互换等智能合约中,可能存在恶意节点调换合约顺序、插入自身合约等不正当牟利手段。当吞吐量较高时,这种不当激励将被继续放大。更糟糕的是,这种恶意调换由于存在天然网络延迟导致顺序调换的可能而难以被察觉,除非这种调换被接受者自己所发现并掌握最终延迟的证据。为支持这种去中心化的商业互换,初链引入了粘性时间戳作为额外限制。这个时间戳是一个启发式的参数,会随时间进程而改变。当一笔交易等待验证时,用户把自己的物理时间戳作为交易元数据与交易打包上传。BFT链的验证器会随后根据以下算法检验该笔交易是否有效地被记录过或有意忽视过。如果出现被忽视、误记或延迟记录,则报告问题,即说明之前节点的记录是有问题的。

(1)检验验证器当前时间与物理时间戳的差值是否小于粘性时间戳。如果大于,则说明该笔交易上传到验证器的时间与其真实发出的时间相差过大,可以判定之前节点出现了忽视、延误或有意偏向等问题。
(2)检验交易是否被记录过。如果被记录过,则继续检验当前链的最后一笔交易的时间戳是否早于新上传的交易。如果出现链上最后一笔交易的时间戳晚于新上传交易,可以判定之前节点的记录出现了错误、延误或造假的可能。
(3)如果未被记录过或时间戳未出现顺序颠倒,则把这笔新的记录添加到账本中。通过以上方法,可以有效保证双重支付、不处理交易等BFT恶意节点的行为。
在这一阶段BFT记录账本时,领导者可以根据物理时间戳选择一批交易拆开。不过这一步并不是必须的,因为我们可以在之后的评估和证实过程中强制保证顺序。
这一改动有如下好处:
(1)交易顺序完全按照物理时间戳排列,因此可以避免同一节点恶意调换交易顺序的情况;
(2)一批交易的顺序由BFT社区按时间戳先后排列;
(3)由于时间窗口的限制,节点不能伪造虚假物理时间戳。
不过这种改动也具有明显的缺点。一旦根据网络延迟情况选取的粘性时间戳选取不合适,所有交易处理会直接被中断。而且BFT社区节点仍可以通过虚报时间和拒绝特定交易造假。尽管BFT社区节点无论怎样都能拒绝交易,但是假如一个节点是诚实的,有时也可能由于时间的不同步性拒绝一部分交易,所以无法区分。然而这一问题可以通过在选取BFT社区节点时增加对该节点必须拥有同步性时钟的限制而加以解决。

## 初链虚拟机(TVM)
在这一领域的范例是以太坊虚拟机(EVM)。它尝试跟随决定主义,在简化激励计算步骤上做出了优化。它还支持内存的非堆栈存储、合约授权以及调用间的价值存储。而初链将继续将EVM应用于慢链之上,并在PBFT主干节点中应用TVM,以使得每个全节点能够响应不同调用请求。但TVM的构想还并不是很成熟,需要确定TVM的过渡状态、智能合约部署策略及从私链虚拟机向公链虚拟机的转变。同时还要确定从POW节点向PBFT节点转变的参数。

## 关于PBFT共识机制
![image](https://user-images.githubusercontent.com/32947900/42924247-dd0e19ca-8b5b-11e8-9ba5-789bafc2b5eb.jpeg)
我的理解和EOS超级节点差不多，只是节点比EOS多，广播方式不同，从而PBFT在安全性上高一些。

具体步骤如下：

其中C为发送请求端，0123为服务端，3为宕机的服务端

Request：请求端C发送请求到任意一节点，这里是0

Pre-Prepare：服务端0收到C的请求后进行广播，扩散至123

Prepare：123,收到后记录并再次广播，1->023，2->013，3因为宕机无法广播

Commit：0123节点在Prepare阶段，若收到超过一定数量的相同请求，则进入Commit阶段，广播Commit请求

5.Reply：0123节点在Commit阶段，若收到超过一定数量的相同请求，则对C进行反馈

由此可以看出，拜占庭容错能够容纳将近1/3的错误节点误差，也就是EOS的21个节点，7个可以用就可以。

不知道理解的对不对！

## 奖励机制:
初链的混合共识机制运用公平的、随机抽取的、轮换的社区制度天然地避免了持币者拥有持续性地获得大量奖励,从而保证了财富的公平分配。为补偿计算算力的不均匀性,初链设置了从BFT与POW池获得奖励的奖励机制。在工程设计上,利用标识改变工作分配,让BFT交易、POW、激励池依次获得资源分配的优先级。通过这种方式,如果BFT节点由于K天数限制(最多每K天必须更换一次节点)被清出网络,它仍可以通过为网络其余部分提供补偿机制享受到投资硬件产生的收益。所有的节点都能完成POW和PBFT的任务,但是PBFT获得更高的优先级。
初链未来的优化方向：
改进所有节点的时间戳同步，而不需要中心化的NTP服务器。
喜欢奖励基础设施里的激励技术，这样重度投资基础设施的投资人不会遭遇“被忽视”，“亏本”的问题。
支持副本创建的分片技术，尽量减少被BFT会员会拒绝的交易集。
添加零知识证明以增强隐私。
EVM、TVM和Linux容器技术的混合基础设施。
改进虚拟机规范中的二进制数据编码方法，交易签名，收费表等章节。

## 总结：
初链的共识设计具有同样的一致性、活跃度、交易终结性和安全保障，这是与混合共识事实上的一致。讨论了轮值委员会成员的频率和物理时间戳限制等优化问题。 提出了在以太坊之上建立一个新的虚拟机的想法，它在无权限的设置中增加了基于许可链的交易处理功能。还使用数据共享和投机交易的思想，评估混合云基础设施中智能合约的运行情况，使用现有的志愿计算协议来实现引入的补偿基础结构。

以上为本人对于初链黄皮书的理解
