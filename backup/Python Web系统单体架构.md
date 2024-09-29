# 前言

系统架构图是系统开发、维护和管理过程中不可或缺的工具，它可以帮助开发者更高效地进行系统设计和开发。

# 系统架构图

以下架构图是从物理部署的角度，以组件级别的颗粒度来分解系统的结构。这个系统的整体视图展示了从客户端到后端服务的完整流程，涉及使用的主要技术和框架，描述组件间的交互逻辑。**不涉及功能模块、开发规范等更细节的层面。**

*   服务级别：Web服务、数据库服务、任务队列服务
*   组件级别：图中显示了主要的系统组件，如Nginx、Django、数据库等。

![test.jpg](https://raw.githubusercontent.com/Spute/news12.github.io/refs/heads/main/assets/css/images/bg.jpg)

![image.png](https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/ef01c9fc2980428c8a58087cc1d84aaf~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg56iL5bqP5ZGY6Z2e6bG8:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMzM2ODU1OTM1OTgzNTc1NyJ9&rk3s=f64ab15b&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1728035783&x-orig-sign=qoxyf%2FDU4TkiA1WfnkT78yZy5O8%3D)

# 一些观点

*   **谨慎地引入组件**。如无必要，勿增实体。为非必要的功能增加组件，不能带来实际收益，反而带来新的问题。

    *   如果没有削峰填谷、解耦任务、支持异步需求，不要引入celery任务队列，因为会降低系统可用性（组件增多了，故障的可能性也变大了）。

*   **不要超前设计**

    *   数据库读写变成瓶颈时，再考虑做主从同步，分库分表

*   **不盲目选择新东西**。微服务架构更新，但不一定比单体架构好。

    *   业务体量不大、团队较小时，单体架构已经可以满足绝大需求，使用微服务反而会带来运维复杂度增高、技术门槛变高等问题。
