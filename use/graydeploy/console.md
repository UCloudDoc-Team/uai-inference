

# 用Console灰度部署

## Step1: 创建AI在线服务
我们可以根据[](ai/uai-inference/use​/new/​console)的说明创建AI在线服务APP

## Step2: 部署多个版本的AI在线服务

  * 第一个版本的服务启动成功后（版本状态变更为“已激活”），点击页面上的“部署新版本”选项，按照提示按需部署多个服务版本。
![](ai/uai-inference/images/use/graydeploy:service_list_long.png)

## Step3: 调整各版本的灰度流量
[](ai/uai-inference/use/oplist/modifyweight) 

  * 进入“版本管理”页面，选择需要进行灰度流量调整的版本，点击隐藏在操作中的“灰度流量调整”选项，按照实际所需调整灰度流量。（版本状态为“未激活”或者“已激活”时才有该选项）。
![](ai/uai-inference/images/use/graydeploy:modify_weight.png)
  * 点击“启动”选项启动该版本后，所有“已激活”版本的流量占比依据权重进行分配，计算公式如下：
![](ai/uai-inference/images/use/graydeploy:屏幕快照_2017-07-01_14.27.55.png)
  * 进行灰度流量调整后服务流量占比示例如下所示：
![](ai/uai-inference/images/use/graydeploy:service_after_alter_weight.png)

