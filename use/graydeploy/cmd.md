{{indexmenu_n>20}}

====== 使用命令行灰度部署 ======
==== Step0: 准备工作 ====
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

4）UAI SDK工具使用
  * 以下所有命令行工具均为uai_tool.py（默认存放路径为$uai-sdk安装路径/uai\_tools/uai\_tool.py）


==== Step1: 创建AI在线服务 ====

* 根据[[ai:uai-inference:use:new]]的说明来部署AI在线服务APP。

==== Step2: 部署多个版本的AI在线服务 ====
  * 参照[[ai:uai-inference:use:new:cmd]]中Step3操作，按需部署多个版本的AI在线服务。 \\
各个版本服务部署成功后，默认灰度权值为10。\\

==== Step3: 调整各版本的灰度流量 ====
  * 执行[[ai:uai-inference:use:oplist:modifyweight]]命令 \\
按需调整版本的灰度流量，系统会依据“已激活”状态版本的灰度权重自动调整流量占比。\\
<code>
python uai_tool.py modifyweight --public_key=PUBLIC_KEY --private_key=PRIVATE_KEY --service_id=SERVICE_ID --srv_version=SERVICE_VERSION --deploy_weight=DEPLOY_WEIGHT
</code>

  * 返回说明 \\

RetCode字段为0时表示正常返回，否则为错误码。返回错误码时可依据Message字段信息进行错误分析。成功执行后，正常返回样例如下：

<code>
ModifyUAISrvVersionWeight Success:
{
  Message : ,
  RetCode : 0
}
</code>



