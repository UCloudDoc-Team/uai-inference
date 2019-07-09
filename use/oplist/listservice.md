{{indexmenu_n>10}}

# listservice

### 命令作用

查看当前账号、当前项目下所有UAI Inference在线服务清单

### 准备工作

1）安装UAI SDK

    git clone https://github.com/ucloud/uai-sdk
    cd uai-sdk
    sudo python setup.py install

2）获取用户公钥和私钥

[key](/ai/uai-inference/base/key)

  - 登陆UCLOUD官方网站，进入Console页面：<https://console.ucloud.cn/dashboard>
  - 点击左上角的“产品与服务”选项，选择“监控管理”列表下的“API密钥
    UAPI”选项后，点击API密钥中的“显示”选项，按照提示获取用户的公钥和私钥。

### 执行listservice命令

说明1：uai\\\_tool.py默认路径为$uai-sdk安装路径/uai\\\_tools/uai\\\_tool.py  
说明2：若不指定service\\\_id，返回所有服务清单；指定service\\\_id时，可精确返回单独在线服务  
说明3：若不指定project\\\_id时，自动使用账号默认项目。若账号下有多个项目，**强烈建议明确指定project\\\_id**  
`python uai_tool.py listservice --public_key PUBLIC_KEY
--private_key PRIVATE_KEY
[--project_id PROJECT_ID]
[--region REGION]
[--zone ZONE]
[--service_id SERVICE_ID]
`

  - 参数说明  

| **参数**         | **说明** | **是否必需**                                   |
| -------------- | ------ | ------------------------------------------ |
| public\\\_key  | 用户的公钥  | 是                                          |
| private\\\_key | 用户的私钥  | 是                                          |
| project\_id    | 项目ID   | 否                                          |
| region         | 地域     | 否                                          |
| zone           | 可用区    | 否                                          |
| service\\\_id  | 服务ID   | 否，如果提供该参数则给出该服务ID的详细信息；否则给出项目组下的所有服务的详细信息。 |

### listservice命令样例

    python uai_tool.py listservice --public_key=PUBLIC_KEY --private_key=PRIVATE_KEY --service_id=SERVICE_ID

  - 返回说明

成功执行后，将返回服务的详细信息。输入service\\\_id时正常返回样例如下：

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

RetCode字段为0时表示正常返回，否则为错误码。返回错误码时可以依据Message字段的信息进行错误分析。

### 图形化界面

在UAI-Inference产品首页，即可显示“默认项目”下所有服务的列表以及其基本信息。切换项目组查看其他项目组下的服务列表。  
![](/images/use/oplist/listservice/listservice.png)
