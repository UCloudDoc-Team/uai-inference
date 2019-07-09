{{indexmenu_n>20}}

# 使用命令行修改在线服务节点数量

### 修改弹性服务节点数量

**弹性服务无法修改节点数量，节点数量由UAI Inference平台自动控制**

### 修改独占服务节点数量

**独占服务需要通过手动操作调整节点数量**，独占服务节点的最低数量为2（我们会确保两个节点位于不同的set中，以保证可用性），你可以在任意时间，修改服务节点的数量，UAI
Inference系统会根据你的要求调整节点。

#### Step0: 准备工作

1）安装UCloud UFile SDK

    wget http://sdk.ufile.ucloud.com.cn/python_sdk.tar.gz
    tar zxvf python_sdk.tar.gz
    cd ufile-python
    sudo python setup.py install

**注：UFile SDK仅兼容request 2.1.0以下版本**

2）安装UAI SDK

    git clone https://github.com/ucloud/uai-sdk
    cd uai-sdk
    sudo python setup.py install

3）获取用户公钥和私钥

[key](/ai/uai-inference/base/key)

  - 登陆UCLOUD官方网站，进入Console页面：<https://console.ucloud.cn/dashboard>
  - 点击左上角的“产品与服务”选项，选择“监控管理”列表下的“API密钥
    UAPI”选项后，点击API密钥中的“显示”选项，按照提示获取用户的公钥和私钥。

4）UAI SDK工具使用

  - 以下所有命令行工具均为uai\_tool.py（默认存放路径为$uai-sdk安装路径/uai\\\_tools/uai\\\_tool.py）

#### Step1: 创建AI独占型在线服务

\* 根据[new](/ai/uai-inference/use/new)的说明来部署AI**独占型**在线服务APP。

#### Step2: 调整在线服务的节点数量

\* 执行modifynodecount命令

``` 

python uai_tool.py modifynodecount --public_key=PUBLIC_KEY --private_key=PRIVATE_KEY --service_id=SERVICE_ID --srv_version=SRV_VERSION --node_count=NODE_COUNT

```

  - 返回说明

成功执行后，正常返回样例如下：

    ModifyUAISrvVersionNodeRange Success:
    {
    Message : ,
    RetCode : 0
    }

RetCode字段为0时表示正常返回，否则为错误码。返回错误码时可以依据Message字段的信息进行错误分析。
