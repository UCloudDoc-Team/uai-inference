

# modifynodecount

## 命令作用
在[部署](uai-inference/use/oplist/deploydocker)成功之后，修改服务的节点数目。 

## 准备工作
### 1. 安装UAI SDK

<code>
git clone https://github.com/ucloud/uai-sdk

cd uai-sdk
sudo python setup.py install
</code>

### 2. 获取用户公钥和私钥 

  * 登陆UCloud官网，进入Console页面
  * 点击左上角的“产品与服务”选项，选择“监控管理”列表下的“API密钥 UAPI”选项后，点击API密钥中的“显示”选项，按照提示获取用户的公钥和私钥。

## 执行modifynode命令
说明1：uai\_tool.py默认路径为$uai-sdk安装路径/uai\_tools/uai\_tool.py 

说明2：执行该命令时，请确认[部署](uai-inference/use/oplist/deploydocker)执行成功，以便填写service\_id, service\_version 

说明3：执行该命令时，请确认版本的状态为“未激活”或者“已激活”。

<code>
python uai_tool.py modifynodecount  --public_key PUBLIC_KEY

​          	                    --private_key PRIVATE_KEY
​			  	    [--project_id PROJECT_ID]
​			            [--region REGION]
​          			    [--zone ZONE]
​                 	  	    --service_id SERVICE_ID
​                         	    --srv_version SRV_VERSION
​			  	    --node_count NODE_COUNT
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
|node\_count|修改后的服务节点数|是|

## modifynode命令样例

<code>
python uai_tool.py modifynodecount --public_key=PUBLIC_KEY --private_key=PRIVATE_KEY --service_id=SERVICE_ID --srv_version=SRV_VERSION --node_count=NODE_COUNT
</code>

  * 返回说明

成功执行后，正常返回样例如下：

<code>
ModifyUAISrvVersionNodeRange Success:
{

Message : ,
RetCode : 0
}
</code>

RetCode字段为0时表示正常返回，否则为错误码。返回错误码时可以依据Message字段的信息进行错误分析。

## 图形化界面


1. 在UAI-Inference产品服务首页，点击需要修改的目标服务ID，进入服务概览页面。
![](ai/uai-inference/images/use/oplist/modifynodecount/modifynode0.png)
2. 在概览页面确认该服务的类型是否为**“独占服务”**，**只有独占服务才可以修改节点配置**。 
![](ai/uai-inference/images/use/oplist/modifynodecount/modifynode1.png)
3. 入服务概览页面后，点击“版本管理”选项，进入版本管理页面，找到拟修改权重的版本，点击操作选项“配置调整”（注意：只有“未激活”和“激活”状态的版本才有“配置调整”的选项哦）。 
![](ai/uai-inference/images/use/oplist/modifynodecount/modifynode2.png)
4. 在弹出的配置调整页面上填写修改后的节点数目即可。（版本默认的节点数为2，修改后的节点数不能小于2）
![](ai/uai-inference/images/use/oplist/modifynodecount/modifynode3.png)



