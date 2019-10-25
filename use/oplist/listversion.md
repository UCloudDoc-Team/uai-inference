

# listversion
## 命令作用
查看某个具体UAI Inference在线服务下所有版本清单

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


## 执行listversion命令
说明1：uai\_tool.py默认路径为$uai-sdk安装路径/uai\_tools/uai\_tool.py 
说明2：执行该命令时，请确认[[ai:uai-inference:use:oplist:create|]]执行成功，以便填写service\_id 

<code>
python uai_tool.py listversion    --public_key PUBLIC_KEY

​                                  --private_key PRIVATE_KEY
​			          [--project_id PROJECT_ID]
​		        	  [--region REGION]
  			          [--zone ZONE]
​                                  --service_id SERVICE_ID
​			          [--srv_version SERVICE_VERSION]
</code>

  * 参数说明 

| 参数 | 说明 | 是否必需 |
| ---- | ---- | -------- |
|public\_key |用户的公钥 |是 |
|private\_key |用户的私钥 |是 |
|project_id|项目ID|否|
| region   	 | 地域                	        | 否         |
| zone           | 可用区				| 否         |
|service\_id|服务ID|是|
|srv\_version |服务版本 |否，如果提供则给出当前服务版本的详细信息，否则给出当前服务ID下的所有版本服务的详细信息。 |

## listversion命令样例

<code>
python uai_tool.py listversion --public_key=PUBLIC_KEY --private_key=PRIVATE_KEY --service_id=SERVICE_ID --service_version=SERVICE_VERSION
</code>

  * 返回说明
成功执行后，将返回服务的详细信息。输入service\_version时正常返回样例如下：

<code>
GetUAISrvVersionList Success:
{
TotalCount : 1,
Message : ,
SrvVersionSet :
	[

		{
		SrvPaasID : soscpul-866mpd2,
		Status : Error,
		UfileBucket : bbl,
		UfileName : ufile_tf_1.1.0.tar,
		SrvVerMemo : ,
		ModifyTime : 1498632781,
		PythonVersion :
			{
			PkgId : 2,
			PkgVersion : 2.7.6,
			PkgName : python,
			PkgType : 2
			},
		PipPKGIDs :
			[
	
			],
		ServiceID : uaiservice-5njuws,
		AIFrameVersion :
			{
			PkgId : 7,
			PkgVersion : 1.1.0,
			PkgName : tensorflow,
			PkgType : 3
			},
		SrvVersion : 5-6,
		AptGetPKGIDs :
			[
	
			],
		DeployWeight : 10,
		OSVersion :
			{
			PkgId : 1,
			PkgVersion : 14.04.05,
			PkgName : ubuntu,
			PkgType : 2
			},
		CreateTime : 1498632726,
		UfileURL :
		}
	],
RetCode : 0
}
</code>

RetCode字段为0时表示正常返回，否则为错误码。返回错误码时可以依据Message字段的信息进行错误分析。

## 图形化界面

1. 在UAI-Inference产品首页，可获得默认项目下所有服务的列表以及其基本信息。 
{{ :ai:uai-inference:use:oplist:listversion:listversion0.png |}} 

2. 点击想查看版本的服务ID或者“详情”选项，进入服务概览页面。
{{ :ai:uai-inference:use:oplist:listversion:listversion1.png |}} 

3. 进入服务概览页面后，点击“版本管理”选项，进入版本管理页面，获取当前服务ID下的所有服务版本的详细信息。
{{ :ai:uai-inference:use:oplist:listversion:listversion2.png |}} 

