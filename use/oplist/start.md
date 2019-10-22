{{indexmenu_n>7}}

# start
## 命令作用
在部署版本（[[ai:uai-inference:use:oplist:deploy|]]或[[ai:uai-inference:use:oplist:deploydocker|]]）成功后，使该版本生效 

## 准备工作
### 1. 安装UAI SDK

<code>
git clone https://github.com/ucloud/uai-sdk

cd uai-sdk
sudo python setup.py install
</code>

### 2. 获取用户公钥和私钥 

  * 登陆UCloud官网，进入Console页面：[[https://console.ucloud.cn/dashboard|https://console.ucloud.cn/dashboard]]
  * 点击左上角的“产品与服务”选项，选择“监控管理”列表下的“API密钥 UAPI”选项后，点击API密钥中的“显示”选项，按照提示获取用户的公钥和私钥。

## 执行start命令
说明1：uai\_tool.py默认路径为$uai-sdk安装路径/uai\_tools/uai\_tool.py 
说明2：执行该命令时，请确认[[ai:uai-inference:use:oplist:deploy|]]或[[ai:uai-inference:use:oplist:deploydocker|]]执行成功，以便填写service\_id、paas\_id、service\_version 
说明3：执行该命令时，请确认版本的状态为“未激活”。
<code>
python uai_tool.py start    --public_key PUBLIC_KEY

​          	            --private_key PRIVATE_KEY
​			    [--project_id PROJECT_ID]
​			    [--region REGION]
​			    [--zone ZONE]
​                            --service_id SERVICE_ID
​                            --srv_version SRV_VERSION
</code>

  * 参数说明

| 参数 | 说明 | 是否必需 |
| ---- | ---- | -------- |
|public\_key |用户的公钥|是|
|private\_key |用户的私钥|是|
|project\_id|项目ID|否|
| region   	 | 地域                	        | 否         |
| zone           | 可用区				| 否         |
|service\_id |服务ID|是|
|srv\_version |服务版本|是|

## start命令样例

<code>
python uai_tool.py start --public_key=PUBLIC_KEY --private_key=PRIVATE_KEY --service_id=SERVICE_ID --sre_version=SRV_VERSION
</code>

  * 返回说明
成功执行后，正常返回样例如下：

<code>
StartUAIService Success:
{

Message : ""
RetCode : 0
}
</code>

RetCode字段为0时表示正常返回，否则为错误码。返回错误码时可以依据Message字段的信息进行错误分析。

## 图形化界面

1. 启动服务：在UAI-Inference产品首页，找到目标版本所在的服务，选择该服务操作中的“启动”选项，或者点击目标服务ID，进入服务概览页面，点击“启动”选项。

  * 方法一：选择目标版本所在服务操作中的“启动”选项 
{{ :ai:uai-inference:use:oplist:start:start0.png |}}
  * 方法二：点击目标版本所在的服务ID，进入服务概览页面，点击“启动”选项 
{{ :ai:uai-inference:use:oplist:start:start1.png |}}

  弹出启动服务页面后，选择需要启动的版本，点击“确定”选项，启动该版本的服务。
{{ :ai:uai-inference:use:oplist:start:start2.png |}} 

  注意：选择在线服务中任何一个版本状态为“已激活”时，表明该服务已经成功启动。


2. 启动版本：点击“版本管理”选项，进入版本管理页面，选择需要启动的版本，点击“启动”选项。
{{ :ai:uai-inference:use:oplist:start:start3.png |}}
    确认版本信息无误后，点击“确定”选项。
{{ :ai:uai-inference:use:oplist:start:start4.png |}}


3. 版本启动后，可以在服务概览页面获取相应的监控信息。
{{ :ai:uai-inference:use:oplist:start:start5.png |}}


