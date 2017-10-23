# 基础概念
- 分布式系统 <br>
    分布式系统是若干独立计算机的集合，这些计算机对于用户来说就像是单个相关系统。
    - 硬件，机器本身独立；
    - 软件，对用户像与单个系统交互。
- 中间件 <br>
    分布式系统通过一个“软件层”组织，使种类各异的计算机和网络呈现为单个系统，位于由用户和应用程序组成的高层和操作系统组成的低层之间。

# 目标
分布式系统提供的好处。
- 可访问性 <br> 
    访问共享
- 透明性 <br>
    |透明性|说明|
    |----|-----|
    |访问|隐藏数据表示形式不同以及资源访问传输方式不同|
    |位置|隐藏资源所在位置|
    |迁移|隐藏资源是否移动到另外位置|
    |重定向|隐藏资源是否在使用过程中移动到另一个位置|
    |复制|隐藏是否对资源进行复制|
    |并发|隐藏资源是否由若干相互竞争用户共享|
    |故障|隐藏资源故障和恢复|
- 开放性 <br>
    良好的接口定义，实现互操作性（不同实现的组件进行共存并协同工作）和可移植性。同时，可以灵活组合、添加与替换系统组件。
> 策略与机制分离，系统提供基础功能（存储），用户定义策略（存储时间、储存位置等配置信息）。
- 可扩展性 <br>
    - 规模可扩展性：集中式服务，存在性能瓶颈；集中式存储，造成网络过载，单点失效问题；集中式算法，网络过载。
    - 地域可扩展性：系统基于同步通信，广域网不可靠的，高延时，点对点的。<局域网，广播>
    - 管理可扩展性；资源（付费）、管理和安全问题。

> 解决方案：
> 1. 隐藏通信等待时间，异步通信。
> 2. 将部分计算量放在浏览器端。
> 3. 分布技术，因特网DNS，由域组成分层树状结构。
> 4. 缓存，复制的特殊形式，存在一致性问题。
> 5. 复制，多副本，负载均衡，存在一致性问题。
> 可扩展性通常表现为性能下降。

# 分类


# Reference
- 《分布式系统原理与范型》
   

