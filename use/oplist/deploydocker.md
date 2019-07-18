{{indexmenu_n>6}}

====== deploydocker ======
==== 命令作用 ====
在[[ai:uai-inference:use:oplist:create|]]在线服务成功后，使用[[ai:uai-inference:use:oplist:packdata_docker|]]打包的Docker镜像部署版本 \\

==== 准备工作 ====
1）安装UCloud UFile SDK  

<code>
wget http://sdk.ufile.ucloud.com.cn/python_sdk.tar.gz
tar zxvf python_sdk.tar.gz
cd ufile-python
sudo python setup.py install
</code>
**注：UFile SDK仅兼容request 2.1.0以下版本**

2）安装UAI SDK

<code>
git clone https://github.com/ucloud/uai-sdk
cd uai-sdk
sudo python setup.py install
</code>

3）获取用户公钥和私钥 

[[ai:uai-inference:base:key]]
  * 登陆UCLOUD官方网站，进入Console页面：[[https://console.ucloud.cn/dashboard|https://console.ucloud.cn/dashboard]]
  * 点击左上角的“产品与服务”选项，选择“监控管理”列表下的“API密钥 UAPI”选项后，点击API密钥中的“显示”选项，按照提示获取用户的公钥和私钥。


==== 执行deploydocker命令 ====

说明1：uai\_tool.py默认路径为$uai-sdk安装路径/uai\_tools/uai\_tool.py \\
说明2：执行该命令时，请确认[[ai:uai-inference:use:oplist:create|]]执行成功，以便填写service\_id \\
说明3：执行该命令时，请确认[[ai:uai-inference:use:oplist:packdata_docker|]]执行成功，以便填写image\_name \\

<code>

 python uai_tool.py deploydocker    --public_key  PUBLIC_KEY
			  	    --private_key  PRIVATE_KEY 
			            [--project_id  PROJECT_ID]
                                    [--region  REGION]
                                    [--zone  ZONE] 
         			    --service_id  SERVICE_ID 
          			    --uimg_name  UIMG_NAME
                                    [--deploy_weight  DEPLOY_WEIGHT]
                                    [--srv_version_memo  SRV_VERSION_MEMO]
</code>

  * 参数说明\\

^ 参数              ^ 说明                      ^ 是否必需     ^
| public\_key     | 用户的公钥                   | 是        |
| private\_key    | 用户的私钥                   | 是        |
| project\_id    | 项目ID                         | 否         |
| region   	 | 地域                	        | 否         |
| zone           | 可用区				| 否         |
| service\_id    | 在线服务的任务ID              | 是        |
| uimg\_name     | 待部署的Uhub镜像名（包含tag）      | 是        |
| deploy\_weight  | 灰度发布的权重，可设置为1-100之间的整数  | 否，默认为10  |
| srv\_version\_memo  | 部署服务的备注信息  | 否 |

==== deploydocker命令样例 ====

<code>
python uai_tool.py deploydocker --public_key PUBLIC_KEY --private_key PRIVATE_KEY --service_id SERVICE_ID --uimg_name USER_IMAGE_NAME
</code>

  * 输出说明
命令行部署指令成功执行后，会返回部署的版本号。RetCode字段为0时表示正常返回，否则为错误码。依据Message字段的信息可以检测部署进行状态以及错误分析。正常返回示例如下：

<code>
DeployUAIService Success:
{
Message : ,
RetCode : 0,
SrvVersion : x-x
}
</code>

部署过程中会不断检测当前部署状态。RetCode字段为0时表示正常返回，否则为错误码。依据Message字段的信息可以检测部署进行状态以及错误分析。**部署耗时几分钟，请耐心等待。**
正常返回示例如下：

<code>
CheckUAIDeployProgress Success:
{
Status : Deploying,
SrvPaasID : ,
RetCode : 0,
DeployLog : "",
SrvURL : "",
Message : Step2：Building user image
</code>

RetCode字段为0时表示正常返回，否则为错误码。依据Message字段的信息可以检测部署进行状态以及错误分析。 \\

部署成功后，将返回部署服务的域名（SrvURL）和状态，部署成功后版本状态为“未激活”（ToStart）。正常返回样例如下：
<code>
CheckUAIDeployProgress Success:
{
Status : ToStart,
SrvPaasID : xxxx,
RetCode : 0,
DeployLog : "",
SrvURL : xxxx.uae.service.ucloud.cn,
Message :
}
</code>

==== 图形化界面 ====

1. 在服务列表页面，选择需要部署新版本的服务ID，并点击部署按钮，打开部署界面 \\
{{ :ai:uai-inference:use:oplist:deploydocker:depoy0.png |}}

2. 在部署界面的“用户镜像”中选择[[ai:uai-inference:use:oplist:packdata_docker|]]上传到Uhub的镜像完成部署。\\
{{ :ai:uai-inference:use:oplist:deploydocker:deploy1.png |}}

3. 信息填写完成后，点击“确定”选项后，界面自动跳转至服务概览页面，可以观察到当前任务的基本信息。\\
注意：**部署耗时几分钟，请耐心等待。** \\
{{ :ai:uai-inference:use:oplist:deploydocker:deploy2.png |}}

4. 点击“版本管理”选项后，查看部署版本的详细信息。样例如下：\\
  * 注意1：当版本部署后运行状态是“错误”时，可以点击“部署日志”选项，查看错误信息。\\
{{ :ai:uai-inference:use:oplist:deploydocker:deploy3.png |}}

  * 注意2：成功部署的版本初始状态为“未激活”，点击“启动”即可激活该版本的服务。\\
{{ :ai:uai-inference:use:oplist:deploydocker:deploy4.png |}}



