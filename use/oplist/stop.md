{{indexmenu_n>8}}

# stop
## 命令作用
在部署版本（[[ai:uai-inference:use:oplist:deploy|]]或[[ai:uai-inference:use:oplist:deploydocker|]]）成功并[[ai:uai-inference:use:oplist:start|]]后，强制停止服务或某个特定版本 

## 准备工作
## 1. 安装UAI SDK

<code>
git clone https://github.com/ucloud/uai-sdk

cd uai-sdk
sudo python setup.py install
</code>

## 2. 获取用户公钥和私钥 

[[ai:uai-inference:base:key]]
  * 登陆UCloud官网，进入Console页面：[[https://console.ucloud.cn/dashboard|https://console.ucloud.cn/dashboard]]
  * 点击左上角的“产品与服务”选项，选择“监控管理”列表下的“API密钥 UAPI”选项后，点击API密钥中的“显示”选项，按照提示获取用户的公钥和私钥。

## 执行stop命令
说明1：uai\_tool.py默认路径为$uai-sdk安装路径/uai\_tools/uai\_tool.py 
说明2：执行该命令时，请确认[[ai:uai-inference:use:oplist:deploy|]]或[[ai:uai-inference:use:oplist:deploydocker|]]执行成功，以便填写service\_id、paas\_id、service\_version 
说明3：执行该命令时，若停止整个服务，service\_version参数可以不设置，请确认服务状态为“正常运行”。
说明4：执行该命令时，若停止单个版本，service\_version参数必须设置，请确认版本状态为“已激活”。

<code>
python uai_tool.py stop    --public_key PUBLIC_KEY

​                           --private_key PRIVATE_KEY
​		           [--project_id PROJECT_ID]
​			   [--region REGION]
​			   [--zone ZONE]
​      	                   --service_id SERVICE_ID
​                           [--srv_version SRV_VERSION]
</code>

### 参数说明：

| 参数 | 说明 | 是否必需 |
| ---- | ---- | -------- |
|public\_key|用户的公钥|是|
|private\_key|用户的私钥|是|
|project\_id|项目ID|否|
| region   	 | 地域                	        | 否         |
| zone           | 可用区				| 否         |
|service\_id|服务ID|是|
|service\_version|服务版本|否（如果不指定，默认停止该服务，否则停止指定版本。）|

## stop命令样例

<code>
python uai_tool.py stop --public_key=PUBLIC_KEY --private_key=PRIVATE_KEY --service_id=SERVICE_ID --srv_version=SRV_VERSION
</code>

  * 返回说明
成功执行后，正常返回样例如下：

<code>
StopUAIService Success:
{

Message : ""
RetCode : 0
}
</code>

RetCode字段为0时表示正常返回，否则为错误码。返回错误码时可以依据Message字段的信息进行错误分析。

## 图形化界面

1. 停止服务：在UAI-Inference产品首页，选择目标服务操作中的“停止”选项，或者点击目标服务ID，进入服务概览页面，点击“停止”选项，停止整个服务。
**注意：只有“正常”状态的服务才可以停止**
  * 方法一 选择目标服务操作中的“停止”选项
{{ :ai:uai-inference:use:oplist:stop:stop0.png |}} 
  * 方法二 点击服务ID，进入服务概览页面，点击“启动”选项 
{{ :ai:uai-inference:use:oplist:stop:stop1.png |}} 
    弹出停止服务页面后，确认信息无误后，点击“确定”选项，停止该服务。 
{{ :ai:uai-inference:use:oplist:stop:stop2.png |}} 
    操作执行正确后，服务状态更新为“已停止”。

2. 停止版本：进入概览页面后，点击“版本管理”选项，进入版本管理页面，选择需要停止的版本，点击“停止”选项。
**注意：只有“已激活”状态的版本才可以停止**
{{ :ai:uai-inference:use:oplist:stop:stop3.png |}} 
    确认版本信息无误后，点击“确定”选项。
{{ :ai:uai-inference:use:oplist:stop:stop4.png |}} 
    操作执行正确后，版本状态更新为“未激活”。 

