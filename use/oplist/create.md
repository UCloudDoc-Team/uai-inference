

# create
## 命令作用
创建一个新的UAI inference服务，每个服务有一个独立URL链接对外提供访问，并支持多版本灰度发布 

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

## 执行create命令
说明1：uai\_tool.py默认路径为$uai-sdk安装路径/uai\_tools/uai\_tool.py 
<code>
python uai_tool.py create    --public_key PUBLIC_KEY

​          	             --private_key PRIVATE_KEY
​			     [--project_id PROJECT_ID]
​			     [--region REGION]
​			     [--zone ZONE]
​                  	     --service_name SERVICE_NAME
​			     [--cpu CPU]
​			     [--memory MEMORY]
​			     [--gpu GPU]
​			     [--business_group BUSINESS_GROUP]
</code>

  * 参数说明

| **参数** | **说明** | **是否必须** |
| -------- | -------- | ------------ |
| public\_key    | 用户的公钥                      | 是         |
| private\_key   | 用户的私钥                      | 是         |
| project\_id    | 项目ID                         | 否         |
| region   	 | 地域                	        | 否         |
| zone           | 可用区				| 否         |
| service\_name  | 指定创建服务的名称（仅能包含数字、字母以及下划线）  | 是         |
| cpu            | 弹性计算节点的CPU数量，可取值为1，2，4，8    | 否，创建弹性服务时必填         |
| memory         | 弹性计算节点的内存（GB），与cpu取值相同      | 否，创建弹性服务时必填         |
| gpu            | 独占计算节点类型，包括：1\_P40			| 否，创建独占服务时必填    |
| business_group | 用户组					      | 否	              |

目前弹性服务和独占服务的可支持机型列表如下。“对应参数”一列表示选择该机型时，creat命令中应填写的参数

| **服务类型** | **支持机型** | **对应参数** |
| ------------ | ------------ | ------------ |
|独占服务    |P40单卡：CPU4核 内存16GB	| \-\-gpu=1_P40    |
|弹性服务    |1C1G：CPU1核 内存1GB	| \-\-cpu=1  \-\-memory=1    |
|           |2C2G：CPU2核 内存2GB	| \-\-cpu=2  \-\-memory=2    |
|           |4C4G：CPU4核 内存4GB	| \-\-cpu=4  \-\-memory=4    |
|           |8C8G：CPU8核 内存8GB	| \-\-cpu=8  \-\-memory=8    |





## create命令样例

  * 创建弹性服务

<code>
python uai_tool.py create --public_key=PUBLIC_KEY --private_key=PRIVATE_KEY --service_name=SERVICE_NAME --cpu=CPU --memory=MEMORY
</code>

  * 创建独占服务

<code>
python uai_tool.py create --public_key=PUBLIC_KEY --private_key=PRIVATE_KEY --service_name=SERVICE_NAME --gpu=GPU
</code>

  * 输出说明 

成功执行后，将返回生成的服务的资源ID以及服务ID。正常返回样例如下：

<code>
CreateUAIService Success:
{

​    ResourceID : uaiservice-xxxx,
​    Message : ,
​    ServiceID :  uaiservice-xxxx,
​    RetCode : 0
}
</code>

RetCode字段为0时表示正常返回，否则为错误码。返回错误码时可以依据Message字段的信息进行错误分析。

## 图形化界面

1.  登录UCloud官方网站（ucloud.cn），点击右上角“控制台”选项，进入控制台页面。控制台页面显示账户的基本信息，点击左上角“产品与服务”选项，获取Ucloud产品列表。选择“人工智能”列表下的“AI在线服务”选项，进入UAI-Inference产品页面。
{{ :ai:uai-service:use:oplist:create:create_1.png |}}

2.  在UAI-Inference产品首页，显示当前账户默认项目组下所有服务列表，点击“创建新服务”按钮。 
{{ :ai:uai-inference:use:oplist:create:create1.png |}}

3.  打开“创建新服务”界面，选择所需的服务类型，填写服务名称、业务组以及运行时的节点配置。图示给出了创建“弹性服务”时的样例。
{{ :ai:uai-inference:use:oplist:create:create2.png |}}

4.  点击“确定”选项，页面自动跳转至UAI产品首页，在服务列表中可以观察到新建的服务信息。 
{{ :ai:uai-inference:use:oplist:create:create3.png |}}

5.  点击服务ID后，跳转至服务概览页面，显示服务的基本信息，配置信息、监控信息等。 
{{ :ai:uai-inference:use:oplist:create:create4.png |}}

6. 创建新的UAI在线服务完成啦！接下来可以开始部署在线服务了。 

