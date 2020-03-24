

# modifyname
## 命令作用
在[](uai-inference/use/oplist/create)在线服务成功后，修改在线服务名称 

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


## 执行modifyname命令
说明1：uai\_tool.py默认路径为$uai-sdk安装路径/uai\_tools/uai\_tool.py 
说明2：执行该命令时，请确认[](uai-inference/use/oplist/create)执行成功，以便填写service\_id 

<code>
python uai_tool.py modifyname    --public_key PUBLIC_KEY
          	                 --private_key PRIVATE_KEY
			         [--project_id PROJECT_ID]
			         [--region REGION]
			         [--zone ZONE]
                 	         --service_id SERVICE_ID
                 	         --service_name SERVICE_NAME

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
|service\_name |用户拟修改的服务名称|是|

## modifyname命令样例

<code>
python uai_tool.py modifyname --public_key=PUBLIC_KEY --private_key=PRIVATE_KEY --service_id=SERVICE_ID --service_name=SERVICE_NAME
</code>

  * 返回说明

成功执行后，正常返回样例如下：

<code>
ModifyUAISrvName Success:
{

Message : ""
RetCode : 0
}
</code>

RetCode字段为0时表示正常返回，否则为错误码。返回错误码时可以依据Message字段的信息进行错误分析。

## 图形化界面

1. 在UAI-Inference产品首页，找到想要修改的服务，点击该服务名称旁边的“修改”选项。 
![](ai/uai-inference/images/use/oplist/modifyname/modifyname1.png)

2. 在弹出的名称修改页面填写相关信息后，点击“确认”选项，即可修改服务名称。需要注意的是，服务名称只能包含数字、字母以及下划线，至少包含6个字符。
![](ai/uai-inference/images/use/oplist/modifyname/modifyname2.png)

