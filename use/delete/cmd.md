{{indexmenu_n>15}}

# 使用命令行删除服务

### Step0: 准备工作

1）安装UAI SDK

    git clone https://github.com/ucloud/uai-sdk
    cd uai-sdk
    sudo python setup.py install

2）获取用户公钥和私钥

[key](/ai/uai-inference/base/key)

  - 登陆UCLOUD官方网站，进入Console页面：<https://console.ucloud.cn/dashboard>
  - 点击左上角的“产品与服务”选项，选择“监控管理”列表下的“API密钥
    UAPI”选项后，点击API密钥中的“显示”选项，按照提示获取用户的公钥和私钥。

3）UAI SDK工具使用

  - 以下所有命令行工具均为uai\_tool.py（默认存放路径为$uai-sdk安装路径/uai\\\_tools/uai\\\_tool.py）

### Step1: 查看待删除服务的运行状态

  - 执行[listservice](/ai/uai-inference/use/oplist/listservice)命令



    python uai_tool.py listservice --public_key=PUBLIC_KEY --private_key=PRIVATE_KEY --service_id=SERVICE_ID

  - 返回说明  
    在执行指令时，传入拟删除的服务ID。RetCode字段为0时表示正常返回，否则为错误码。返回错误码时可以依据Message字段的信息进行错误分析。

成功执行后，正常返回样例如下：

    GetUAIServiceList Success:
    {
    TotalCount : 1,
    Message : ,
    RetCode : 0,
    ServiceSet :
        [
     
           {
           SrvPaasID : soscpul-866mpd2,
           Status : Normal,
           SrvName : sdk_cmd_test,
           SrvVerToStartCount : 7,
           BillUnitPrice : 0,
           ResourceID : uaiservice-5njuws,
           Region : pre,
           ModifyTime : 1498618738,
           SrvURL : soscpul-866mpd2.uae.service.ucloud.cn,
           BillUnit : ,
           CreateTime : 1498618737,
           BusinessGroup : Default,
           ServiceID : uaiservice-5njuws,
           SrvVerDeployingCount : 0,
           Memory : 1,
           SrvVerErrorCount : 7,
           SrvVerStartedCount : 1,
           SrvVerDeletedCount : 0,
           CPU : 1,
           BillType :
           }
        ]
    }

  - 找到“Status”字段的信息，获得当前服务的运行状态。  
    1）当前服务运行状态为“待部署(ToDeploy) ”或者“已停止(ToStart) ”： 跳转至Step3  
    2）当前服务运行状态为“部署中(Deploying) ”: 部署中的服务无法删除，请等待部署结束后再进行相关操作。  
    3）当前服务运行状态为“正常(Normal) ”：跳转至Step2  

### Step2: 停止待删除的服务

  - 执行[stop](/ai/uai-inference/use/oplist/stop)命令  
    `python uai_tool.py stop --public_key=PUBLIC_KEY
    --private_key=PRIVATE_KEY --service_id=SERVICE_ID
    `



  - 返回说明  
    在执行指令时，传入拟删除的服务ID。RetCode字段为0时表示正常返回，否则为错误码。返回错误码时可以依据Message字段的信息进行错误分析。

成功执行后，正常返回样例如下：

    StopUAIServiceSuccess:
    {
    Message: ,
    RetCode: 0
    }

### Step3: 删除服务

  - 执行[delete](/ai/uai-inference/use/oplist/delete)命令  
    `python uai_tool.py delete --public_key=PUBLIC_KEY
    --private_key=PRIVATE_KEY --service_id=SERVICE_ID
    `



  - 返回说明  
    在执行指令时，传入拟删除的服务ID。RetCode字段为0时表示正常返回，否则为错误码。返回错误码时可以依据Message字段的信息进行错误分析。

成功执行后，正常返回样例如下：

    DeleteUAIServiceSuccess:
    {
    Message: ,
    RetCode: 0
    }

### Step4: 确认是否删除成功

  - 重新执行Step1中的步骤，查看返回结果中的"ServiceSet"字段是否为空。
