

# 用Console删除服务

## Step1: 查看待删除服务的运行状态
[](ai/uai-inference/use/oplist/listservice) 

  * 登录UCloud官方网站，点击右上角“控制台”选项，进入控制台页面。控制台页面显示账户的基本信息，点击左上角“产品与服务”选项，获取UCloud产品列表。选择“数据分析”列表下的“AI在线服务 UAI”选项，进入UAI产品页面。
![](ai/uai-inference/images/use/optlist/create/create_1.png)
  * 在服务列表找到拟删除的服务。（注意：产品首页默认列出的是“默认项目组”的服务列表，如有需要需更改项目组。）
![](ai/uai-inference/images/use/delete/service_list_under_default_org.png)
  * 查看服务的运行状态
1）当前服务运行状态为“待部署”或者“已停止”：跳转至Step3
2）当前服务运行状态为“部署中”: 部署中的服务无法删除，请等待部署结束后再进行相关操作。
3）当前服务运行状态为“正常”：跳转至Step2
## Step2: 停止待删除的服务
[](ai/uai-inference/use/oplist/stop) 

  * 选择目标服务操作菜单中的“停止”选项，确认弹出的信息无误后，停止当前服务。服务停止成功后，服务状态变更为“已停止”。
![](ai/uai-inference/images/use/delete/stop_srv.png)
## Step3: 删除服务
[](ai/uai-inference/use/oplist/delete) 

  * 选择目标服务操作菜单中的“删除”选项，确认弹出的信息无误后，删除当前服务。目标服务从服务列表消失，表明删除成功。
![](ai/uai-inference/images/use/delete:delete_srv.png)

