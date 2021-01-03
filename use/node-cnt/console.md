

# 用Console修改在线服务节点数量

  * 登录UCloud官方网站（ucloud.cn），点击右上角“控制台”选项，进入控制台页面。控制台页面显示账户的基本信息，点击左上角“产品与服务”选项，获取UCloud产品列表。选择“数据分析”列表下的“AI在线服务 UAI”选项，进入UAI产品页面。
![](ai/uai-inference/images/use/oplist/create/create_1.png)

  * UAI产品页面如下图所示，产品页首页显示当前账户下所有服务的列表以及其基本信息。
![](ai/uai-inference/images/use/oplist/create/create_2.png)

## 修改独占服务节点数量
独占服务除了自定义伸缩规则[动态调整节点数量](uai-inference/use/auto-scale)，也可以选择通过手动操作调整节点数量。独占服务节点的最低数量为2（我们会确保两个节点位于不同的set中，以保证可用性），你可以在任意时间，修改服务节点的数量，UAI Inference系统会根据你的要求调整节点。

任务启动后，我们可以看到默认的节点数为2: 
![](ai/uai-inference/images/use/node-cnt/gpu-start.png)

我们可以点击**配置调整**按钮来挑战节点数：
![](ai/uai-inference/images/use/node-cnt/change-node.png)

并选择需要的节点数：
![](ai/uai-inference/images/use/node-cnt/change-cnt.png)

之后系统会自动根据请求调整节点数。

