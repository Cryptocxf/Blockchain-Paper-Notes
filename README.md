# Blockchain-Paper-Notes
记录个人区块链学习论文笔记
## 一、Towards Secure Industrial IoT: Blockchain System with Credit-Based Consensus Mechanism（面向安全工业物联网：基于信用共识机制的区块链系统）
本篇论文主要涉及了区块链（Blockchain）和工业物联网（Industrial Internet of Thing， IIOT）领域，通过将引入一种 “新的”共识机制以解决安全和隐私等问题。这篇也是基于区块链的物联网安全方向具有代表性的论文，以下是我个人对这篇论文的理解和个人思考，仅供参考。

- 原文链接：[Towards Secure Industrial IoT: Blockchain System with Credit-Based Consensus Mechanism](https://sci-hub.st/10.1109/tii.2019.2903342)

### 1.1、写作背景
论文名称     | 作者 / 单位   | 来源   | 年份|简要内容
-------- | -----   | -----   | -----   | -----
Towards Secure Industrial IoT: Blockchain System with Credit-Based Consensus Mechanism  | Junqin Huang   (上海交通大学)    | IEEE Transactions on Industrial Informatics   | 2019   | 面向工业IoT，提出了一种基于信用的共识机制的区块链系统

通过这篇论文了解到，现有IIoT系统容易出现**单点攻击**和**恶意攻击**，它**不能提供稳定的服务**，传统区块链是**强耗电-低输出**的，并**不适用能力受限的IoT设备**。IoT目前需要面对**三个主要的挑战**：==效率和安全之间的权衡==、==透明度和隐私共存==和==高并发性和低输出之间的矛盾==。以此为写作背景，作者提出了一种基于信任的共识机制的区块链系统。

### 1.2、主要内容
本篇论文的作者提出了一种基于信任的共识机制的区块链系统，同时能保证**系统安全和交易效率**。为了保护敏感数据的机密性，作者设计了一个数据权限管理方法去控制对传感器数据的访问。另外，本论文建立基于**有向无环图(Directed Acyclic Graph，DAG)架构的区块链**—***tangle*(纠缠)****，在性能上比传统区块链更高效。

#### 1.2.1 什么叫*tangle*网络？（论文未具体介绍）
在读论文前，我们需要了解什么叫*tangle*网络？具体怎样的一个构造？以及怎样理解这个网络？

- ***Tangle*** 网络是在2014年，由IOTA (埃欧塔) 基金会专门为物联网而设计的新型数据结构。([IOTA的详细介绍](https://www.iotachina.com/what-is-iota)) 。IOTA与Tangle的关系类似于Bitcoin和区块链的关系。我们用一个含有20笔交易的*tangle*网络来作为例子讲解，如下图所示，其中白色方块表示已经确认的交易，灰色方块表示未经确认的交易。[IOTA可视化网址](https://public-krwdbaytsx.now.sh/)。
![tangle网络](https://img-blog.csdnimg.cn/20210325173340982.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0EzMzI4MDAwMGY=,size_16,color_FFFFFF,t_70#pic_center)
- **（1）区块单元转变成了交易单元**：它消除了区块的概念，每笔交易都是链接在分布式分类中的单个节点。在提交一个新交易之前，它必须验证两个已经附加在一起的前交易(在*tangle*中未验证的交易)，这**称为tips**。然后，通过工作量证明(POW)算法，新交易与前两个交易捆绑在一起，新的交易可以广播到tangle网络。每笔新交易都将在以后由其他更新的交易进行验证。
- **（2）单链结构转变成了网络拓扑结构**：传统链结构的区块链采用同步共识，而tangle采用了**异步共识**。DAG结构的区块链并不是一直被单一的主链和分叉所约束，交易之间的关系看起来就像一个错综复杂的网络。这种新颖的体系结构和共识机制可以从理论上提高网络吞吐量和系统响应时间。
- 另外，交易不仅要被确认，还需要被大部分节点确认，以保证该交易的真实可靠性，tangle网络引入了一个累加权重(Cumulative Weight）概念，只有当累加权重越大，代表该交易更确信。例如，如下图所示，其中红色框框和线条表示交易7**直接确认**或**间接确认**的交易，蓝色框框和线表示直接确认或间接确认交易7的所有交易。交易7的累加权重等于13，它是由直接确认的交易8、9、10共3次，加上间接确认的交易11、12、13、14、15、16、17、18、19共9次，再加上自身1次，**共13次确认**。而累加权重下面的是**置信度（Confidence）**，确认交易的次数越多，其置信度越高，代表该笔交易可信度越高。

![tangle网络](https://img-blog.csdnimg.cn/20210325174252688.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0EzMzI4MDAwMGY=,size_16,color_FFFFFF,t_70#pic_center)
####  1.2.2 论文架构及内容
本篇论文主要提出了==系统体系结构、基于信用POW机制和数据权限管理方法==等三个方面。

- **(1) 系统体系结构**：作者假设了一个小型的智能工厂场景，并根据该场景中的功能划分为**轻节点**和**全节点**。轻节点：为功率受限的IoT设备，不存储区块链信息，可验证、运行POW算法和发送新的交易到全节点。全节点：为更强大的设备，如网关和服务器，主要维护整个区块链网络。如下图所示，为基于区块链的工业IoT系统架构。
![系统体系结构](https://img-blog.csdnimg.cn/20210325180044734.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0EzMzI4MDAwMGY=,size_16,color_FFFFFF,t_70#pic_center)
	- 无线传感器节点：初始化生成区块链账号，即一对公开秘密密钥$(PK, SK)$，作为系统中唯一的标识符。
	- 网关：作为全节点接收来自各种传感器的请求，并在*tangle*中广播交易，并只处理来自合法的，经过授权的传感器交易。
	- 管理者：为一个特定的全节点，负责管理IoT设备。管理器的公钥编码到网关中的软件中，只有管理器有权发布设备的授权列表，并管理IoT设备的添加/删除。
	- tangle网络：为公共的区块链网络，任何一方都可以访问该网络。网关保证了网络的安全，并通过广播交易和保留区块链的副本来保持稳定。

- **(2) 基于信用PoW机制**：该部分应该是本篇论文的核心，主要是引入了节点的**信用函数**表达式，并将PoW机制的难度值与每个节点的信用值成**反比关系**。
	- 作者定义了具有信用值$Cr_i$属性的节点$i$，其中$Cr_i$为Positive影响部分$Cr_i^P$与Negative影响部分$Cr_i^N$的累加和。
	$$Cr_i=\lambda _1Cr_i^P+\lambda _2Cr_i^N$$
其中$\lambda _1$和$\lambda _2$分别为对应的系数，可根据惩罚的难度动态调整。其信用值会根据节点的行为实时变化。正常行为会增加信用价值，异常行为会降低信用值。POW机制的难度根据每个节点的信用值自适应调整，信用值越低，运行时间越长。因此，该机制将使诚实节点消耗更少的资源，同时迫使恶意节点增加攻击代价。*tangle*网络是透明的，可以从DAG网络中获取所有传感器的交易信息和它们之间的关系。该系统中POW的难度与信用评分成反比，信用评分在整个系统周期内动态调整。
	- $Cr_i^P$与节点在单位时间内的正常交易数量**正相关**，即通过节点活跃度来度量，定义为：
	$$Cr_i^P=\frac {\sum _{k=1}^{n_i}\omega_k}{\Delta T}$$
	其中，$\omega_k$为第$k$次交易验证的次数，$\Delta T$为单位时间。
	- $Cr_i^N$与节点恶意行为次数**负相关**，定义为：
	$$Cr_i^N=-\sum_{k=1}^{m_i}\alpha (\beta )\frac {\Delta T}{t-t_k}$$
	$$\alpha (\beta )\begin{cases}\alpha_l,  & \text{如果$\beta $是lazy tips行为} \\\alpha_d, & \text{如果$\beta $是double-spending行为}\end{cases}$$
	其中$t_k$表示恶意攻击时间，$t$为当前时间，$\beta$为恶意节点的行为，$\alpha(\beta )$为惩罚系数。

- **(3) 数据权限管理方法**：作者提出了一种数据权限管理方法来支持系统中传感器数据的访问控制，和一种无需中央信任方的**密钥分发方案**。因此可以利用公开密钥加密来分发对称密钥，一次密钥分发分为三个步骤，如下图所示。
![数据权限管理方法](https://img-blog.csdnimg.cn/20210325183752587.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0EzMzI4MDAwMGY=,size_16,color_FFFFFF,t_70#pic_center)
	- **步骤1**：Manager生成对称密钥$SK_s$，并发送消息$M_1$给IoT设备。
	- **步骤2**：IoT设备解密$M_1$，并获得对称密钥，并发送$M_2$给Manager。
	- **步骤3**：Manager在$M_3$中返回$nonce_b$来完成这一轮的密钥分发。
	- 注：Manager只向收集敏感数据的设备分发密钥。生成对称密钥只执行一次，每条消息($M_i$)都使用发送方的密钥签名，以确保接收到的消息不被篡改或损坏。每个消息中的时间戳($TS$)标识消息的时效性，用于抵抗重放攻击。


### 1.3、个人思考与总结
作者基于工业IoT的安全问题，提出了一种基于信用的共识机制的区块链系统。
- 总体来说，本篇论文提出了三对相斥特性的权衡问题（包括安全vs效率、透明度vs隐私和高并发vs低输出）这是目前所有解决方案必须面对的问题，我觉得这种特性权衡的问题也是一个**小亮点**。
- 整体来看，该方案并不复杂，主要介绍了新型的系统体系结构、基于信用PoW共识和数据权限管理方法三个方面，个人觉得基于信用的POW机制算是**比较新颖**的点。
- 利用基于DAG结构的区块链实现该系统的体系结构以提高系统的吞吐量，基于信用POW机制有效防御恶意攻击和降低诚实节点的功耗，以及数据权限管理方法有效地保证一个透明系统中数据的保密性。
