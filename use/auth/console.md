{{indexmenu_n>10}}

# 使用Console为在线服务授权

**注：授权验证及外网访问当前仅支持独占性服务**

  - 登录Ucloud官方网站（ucloud.cn），点击右上角“控制台”选项，进入控制台页面。控制台页面显示账户的基本信息，点击左上角“产品与服务”选项，获取Ucloud产品列表。选择“数据分析”列表下的“AI在线服务
    UAI”选项，进入UAI产品页面。  
    ![](/ai/uai-service/use/oplist/create/create_1.png)  
  - UAI产品页面如下图所示，产品页首页显示当前账户下所有服务的列表以及其基本信息。  
    ![](/ai/uai-service/use/oplist/create/create_2.png)  

### Step1: 进入APP详细页面

选择需要授权的在线服务APP（注：仅独占服务可以进行Token令牌鉴权），进入详细页面：

  - 在服务列表中可以观察到所有APP的服务信息。  
    ![](/ai/uai-service/use/oplist/create/create_4.png)  
  - 点击服务ID或详细按钮，跳转至服务概览页面，显示服务的基本信息，授权管理信息，配置信息以及操作日志。

### Step2: 为当前APP授权

我们可以在**授权管理**控制项下面为当前APP授权，详细请查看[auth\_client](/management_monitor/utoken/operation/mgr_client/auth_client)

### Step3: 访问APP

对APP授权成功后，需要在http请求头部添加ServiceID与Token信息进行访问。 对于独占型服务，当前支持通过内网与外网进行访问。  

    通过curl工具访问示例
    curl -H "ServiceID: uaiservice-xxxxx" -H "Token:xxxxx" http://uinference.service.ucloud.cn （内网）
    curl -H "ServiceID: uaiservice-xxxxx" -H "Token:xxxxx" http://uinference.ucloud.cn （外网）
