====== modifymemo ======

==== 命令作用 ====
在部署版本[[ai:uai-inference:use:oplist:deploydocker|]]）成功后，修改版本的备注信息。 \\

==== 准备工作 ====
1）安装UAI SDK

<code>
git clone https://github.com/ucloud/uai-sdk
cd uai-sdk
sudo python setup.py install
</code>

2）获取用户公钥和私钥 

[[ai:uai-inference:base:key]]
  * 登陆UCLOUD官方网站，进入Console页面：[[https://console.ucloud.cn/dashboard|https://console.ucloud.cn/dashboard]]
  * 点击左上角的“产品与服务”选项，选择“监控管理”列表下的“API密钥 UAPI”选项后，点击API密钥中的“显示”选项，按照提示获取用户的公钥和私钥。

==== 执行modifymemo命令 ====

说明1：uai\_tool.py默认路径为$uai-sdk安装路径/uai\_tools/uai\_tool.py \\
说明2：执行该命令时，请确认[[ai:uai-inference:use:oplist:deploydocker|]]执行成功，以便填写service\_id, service\_version \\

<code>
python uai_tool.py modifymemo  --public_key PUBLIC_KEY
          	               --private_key PRIVATE_KEY
			       [--project_id PROJECT_ID]
			       [--region REGION]
                               [--zone ZONE]
                 	       --service_id SERVICE_ID
                               --srv_version SRV_VERSION
			       --srv_version_memo SRV_VERSION_MEMO
</code>

  * 参数说明\\
^参数 ^说明 ^是否必需 ^
|public\_key |用户的公钥|是|
|private\_key |用户的私钥|是|
|project\_id|项目ID|否|
| region   	 | 地域                	        | 否         |
| zone           | 可用区				| 否         |
|service\_id |服务ID|是|
|srv\_version |服务版本|是|
|srv\_version\_memo|修改后的版本备注|是|

==== modifyweight命令样例 ====

<code>
python uai_tool.py modifymemo --public_key=PUBLIC_KEY --private_key=PRIVATE_KEY --service_id=SERVICE_ID --srv_version=SRV_VERSION --srv_version_memo=SRV_VERSION_MEMO
</code>

  * 返回说明

成功执行后，正常返回样例如下：

<code>
ModifyUAISrvVersionMemo Success:
{
Message : ""
RetCode : 0
}
</code>

RetCode字段为0时表示正常返回，否则为错误码。返回错误码时可以依据Message字段的信息进行错误分析。

==== 图形化界面 ====

1. 在UAI-Inference产品服务首页，点击需要修改的目标服务ID，进入服务概览页面。  \\
{{ :ai:uai-inference:use:oplist:modifymemo:modifymemo0.png |}} \\

2. 进入服务概览页面后，点击“版本管理”选项，进入版本管理页面，找到拟修改备注的版本，点击版本号下方的“修改”选项 \\
{{ :ai:uai-inference:use:oplist:modifymemo:modifymemo1.png |}} \\

2. 在弹出的名称修改页面填写相关信息后，点击“确认”选项，即可修改版本备注信息。\\
{{ :ai:uai-inference:use:oplist:modifymemo:modifymemo2.png |}} \\
