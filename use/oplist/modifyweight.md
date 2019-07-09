{{indexmenu_n>12}}

# modifyweight

### 命令作用

在部署版本（[](/ai/uai-inference/use/oplist/deploy)或[](/ai/uai-inference/use/oplist/deploydocker)）成功后，调整版本的权重。版本权重将在多个版本激活时，影响访问流量分配的比重。  

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

### 执行modifyweight命令

说明1：uai\\\_tool.py默认路径为$uai-sdk安装路径/uai\\\_tools/uai\\\_tool.py  
说明2：执行该命令时，请确认[](/ai/uai-inference/use/oplist/deploydocker)执行成功，以便填写service\\\_id,
service\\\_version  
说明3：执行该命令时，请确认版本的状态为“未激活”或者“已激活”。  

    python uai_tool.py modifyweight    --public_key PUBLIC_KEY
                                   --private_key PRIVATE_KEY
                       [--project_id PROJECT_ID]
                           [--region REGION]
                           [--zone ZONE]
                               --service_id SERVICE_ID
                                   --srv_version SRV_VERSION
                       --deploy_weight DEPLOY_WEIGHT

  - 参数说明  
    ^参数 ^说明 ^是否必需 ^

|                  |                            |   |
| ---------------- | -------------------------- | - |
| public\\\_key    | 用户的公钥                      | 是 |
| private\\\_key   | 用户的私钥                      | 是 |
| project\\\_id    | 项目ID                       | 否 |
| region           | 地域                         | 否 |
| zone             | 可用区                        | 否 |
| service\\\_id    | 服务ID                       | 是 |
| srv\\\_version   | 服务版本                       | 是 |
| deploy\\\_weight | 修改后的灰度发布的权重，可设置为1-100之间的整数 | 是 |

### modifyweight命令样例

    python uai_tool.py modifyweight --public_key=PUBLIC_KEY --private_key=PRIVATE_KEY --service_id=SERVICE_ID --srv_version=SRV_VERSION --deploy_weight=DEPLOY_WEIGHT

  - 返回说明

成功执行后，正常返回样例如下：

    ModifyUAISrvVersionWeight Success:
    {
    Message : ""
    RetCode : 0
    }

RetCode字段为0时表示正常返回，否则为错误码。返回错误码时可以依据Message字段的信息进行错误分析。

### 图形化界面

1\. 在UAI产品服务首页，点击需要修改的目标服务ID，进入服务概览页面。  
![](/images/use/oplist/modifyweight/modifyweight0.png) 2.
入服务概览页面后，点击“版本管理”选项，进入版本管理页面，找到拟修改节点配置的版本，点击操作选项“灰度流量调整”（注意：只有“未激活”和“激活”状态的版本才有“灰度流量调整”的选项哦）。  
![](/images/use/oplist/modifyweight/modifyweight1.png) 3.
在弹出的灰度流量调整页面上填写修改后的灰度权重即可。（“未激活”状态的版本流量占比默认为10，“激活”状态的版本流量占比系统自行计算调整）
![](/images/use/oplist/modifyweight/modifyweight2.png)
