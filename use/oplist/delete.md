

# delete
## 命令作用
在[部署版本](uai-inference/use/oplist/deploy)或[](uai-inference/use/oplist/deploydocker)成功并已经[不对外服务](uai-inference/use/oplist/stop)后，删除服务或某个特定版本 

## 准备工作
1. 安装UAI SDK

<code>
git clone https://github.com/ucloud/uai-sdk

cd uai-sdk
sudo python setup.py install
</code>

2. 获取用户公钥和私钥 

  * 登陆UCloud官网，进入Console页面
  * 点击左上角的“产品与服务”选项，选择“监控管理”列表下的“API密钥 UAPI”选项后，点击API密钥中的“显示”选项，按照提示获取用户的公钥和私钥。

## 执行delete命令

说明1：uai\_tool.py默认路径为$uai-sdk安装路径/uai\_tools/uai\_tool.py 
说明2：执行该命令时，请确认[](uai-inference/use/oplist/deploy)或[](uai-inference/use/oplist/deploydocker)执行成功，以便填写service\_id、paas\_id、service\_version 
说明3：执行该命令时，若停止整个服务，service\_version参数可以不设置，请确认服务状态为“待部署”或“已停止”。
说明4：执行该命令时，若停止单个版本，service\_version参数必须设置，请确认版本状态为“未激活”或“错误”。

<code>
python uai_tool.py delete  --public_key PUBLIC_KEY

​                   	     --private_key PRIVATE_KEY
​			     [--project_id PROJECT_ID]
​			     [--region REGION]
​			     [--zone ZONE]
​                   	     --service_id SERVICE_ID
​                             [--srv_version SRV_VERSION]
</code>

  * 参数说明

| 参数 | 说明 | 是否必需 |
| ---- | ---- | -------- |
| public\_key       | 用户的公钥          | 是                            |
| private\_key      | 用户的私钥          | 是                            |
| project\_id       | 项目ID           | 否                            |
| region   	 | 地域                	        | 否         |
| zone           | 可用区				| 否         |
| service\_id       | 服务ID           | 是                            |
| service\_version  | 服务版本           | 否（如果不指定，默认删除该服务，否则删除指定版本）   |

## delete命令样例

<code>
python uai_tool.py delete --public_key=PUBLIC_KEY --private_key=PRIVATE_KEY --service_id=SERVICE_ID --srv_version=SRV_VERSION
</code>

  * 返回说明

成功执行后，正常返回样例如下：

<code>
DeleteUAIService Success:
{

Message : ""
RetCode : 0
}
</code>
RetCode字段为0时表示正常返回，否则为错误码。返回错误码时可以依据Message字段的信息进行错误分析。

## 图形化界面

### 1. 删除服务
在UAI-Inference产品首页，选择目标服务操作中的“删除”选项，或者点击目标服务ID，进入服务概览页面，点击“删除”选项，删除整个服务（注意：只有“待部署”或者“已停止”状态的服务才可以删除哦）。
  * 方法一：选择目标服务操作中的“删除”选项 
![](ai/uai-inference/images/use/oplist/delete/delete0.png)
  * 方法二：点击目标服务ID，进入服务概览页面，点击“删除”选项 
![](ai/uai-inference/images/use/oplist/delete/delete1.png)
弹出删除服务页面后，查看提示信息，确认删除是勾选“同意”，点击“确定”选项，删除该服务。
![](ai/uai-inference/images/use/oplist/delete/delete2.png)
操作执行正确后，服务列表会删除相关服务信息。

### 2. 删除版本
进入概览页面后，点击“版本管理”选项，进入版本管理页面，选择需要删除的版本，点击隐藏操作中的“删除”选项（注意：只有“未激活”或者“错误”状态的版本才可以删除哦）。
![](ai/uai-inference/images/use/oplist/delete/delete3.png)
确认版本信息无误后，点击“确定”选项。
![](ai/uai-inference/images/use/oplist/delete/delete4.png)
操作执行正确后，版本列表会删除相关版本信息。

