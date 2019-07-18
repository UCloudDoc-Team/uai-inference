{{indexmenu_n>5}}

====== deploy ======
==== 命令作用 ====
在[[ai:uai-inference:use:oplist:create|]]在线服务成功后，使用[[ai:uai-inference:use:oplist:packdata|]]上传到Ufile的tar代码包部署版本 \\
**建议优先采用docker镜像方式部署**

==== 准备工作 ====
1）安装UCloud UFile SDK  

<code>
wget http://sdk.ufile.ucloud.com.cn/python_sdk.tar.gz
tar zxvf python_sdk.tar.gz
cd ufile-python
sudo python setup.py install
</code>
**注：UFile SDK仅兼容request 2.10.0以下版本**

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

==== 执行deploy命令 ====
说明1：uai\_tool.py默认路径为$uai-sdk安装路径/uai\_tools/uai\_tool.py \\
说明2：执行该命令时，请确认[[ai:uai-inference:use:oplist:create|]]执行成功，以便填写service\_id \\
说明3：执行该命令时，请确认[[ai:uai-inference:use:oplist:packdata|]]执行成功，以便填写ufile\_url \\

<code>
python uai_tool.py deploy  --public_key PUBLIC_KEY 
			     --private_key PRIVATE_KEY 
		             --service_id SERVICE_ID
			     [--project_id PROJECT_ID] 
                             --ufile_url UFILE_URL
			     [--deploy_weight DEPLOY_WEIGHT]
			     --ai_arch_v AI_ARCH_V
                             [--os OS]
                             [--language LANGUAGE]
                             [--os_deps OS_DEPS]
                             [--pip PIP]
</code>

  * 参数说明\\

^参数 ^说明 ^是否必需 ^
| public\_key | 用户的公钥 | 是 |
| private\_key | 用户的私钥 | 是 |
| service\_id | 在线服务的任务ID | 是 |
| project\_id | 项目ID | 否 |
| deploy\_weight | 灰度发布的权重，可设置为1-100之间的整数 | 否，默认为10 |
| ufile\_url | 用户上传tar包的地址 | 是 |
| ai\_arch\_v| 用户选用的深度学习框架信息（可选项：tensorflow-0.11.0, tensorflow-1.1.0, keras-1.2.0, mxnet-0.9.5, caffe-1.0.0）| 是|
|os |操作系统环境 |否, 默认ubuntu |
|language |语言环境 |否，默认Python-2.7.6 |
|os\_deps |用户所需apt-get依赖包名称（不包含版本号，若有多个，请用','隔开，中间不包含空格）|否 |
|pip |用户所需pip包名称（不包含版本号，若有多个，请用','隔开，中间不包含空格）|否 |

**注：如何获取ufile\_name对应的ufile tar包全下载路径请参见：[[ai:uai-inference:base:ufile:download]]，部署前确认系统未对该url进行自动转义。**

==== deploy命令样例 ====

1）caffe

<code>
python uai_tool.py deploy --public_key=PUBLIC_KEY --private_key=PRIVATE_KEY --service_id=SERVICE_ID --ufile_url=UFILE_URL --ai_arch_v=AI_ARCH_V --os=OS --language=LANGUAGE --os_deps=OS_DEP1,OS_DEP2,OS_DEP3 --pip=PIP1,PIP2,PIP3
</code>

2）keras

<code>
python uai_tool.py deploy --public_key=PUBLIC_KEY --private_key=PRIVATE_KEY --service_id=SERVICE_ID --ufile_url=UFILE_URL --ai_arch_v=AI_ARCH_V --os=OS --language=LANGUAGE --os_deps=OS_DEP1,OS_DEP2,OS_DEP3 --pip=PIP1,PIP2,PIP3
</code>

3）mxnet

<code>
python uai_tool.py deploy --public_key=PUBLIC_KEY --private_key=PRIVATE_KEY --service_id=SERVICE_ID --ufile_url=UFILE_URL --ai_arch_v=AI_ARCH_V --os=OS --language=LANGUAGE --os_deps=OS_DEP1,OS_DEP2,OS_DEP3 --pip=PIP1,PIP2,PIP3
</code>

4）tensorflow

<code>
python uai_tool.py deploy --public_key=PUBLIC_KEY --private_key=PRIVATE_KEY --service_id=SERVICE_ID --ufile_url=UFILE_URL --ai_arch_v=AI_ARCH_V --os=OS --language=LANGUAGE --os_deps=OS_DEP1,OS_DEP2,OS_DEP3 --pip=PIP1,PIP2,PIP3
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

部署过程中会不断检测当前部署状态。RetCode字段为0时表示正常返回，否则为错误码。依据Message字段的信息可以检测部署进行状态以及错误分析。**部署耗时大概为10分钟，请耐心等待。**
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

**当前版本的UAI-Inference已经不支持“代码模式“的部署，请使用命令行工具执行该指令。**



