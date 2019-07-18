{{indexmenu_n>10}}


====== 使用Console创建新任务 ======
  * 登录Ucloud官方网站（ucloud.cn），点击右上角“控制台”选项，进入控制台页面。控制台页面显示账户的基本信息，点击左上角“产品与服务”选项，获取Ucloud产品列表。选择“数据分析”列表下的“AI在线服务 UAI”选项，进入UAI产品页面。\\
{{ai:uai-service:use:oplist:create:create_1.png?800|}} \\

  * UAI产品页面如下图所示，产品页首页显示当前账户下所有服务的列表以及其基本信息。\\
{{ai:uai-service:use:oplist:create:create_2.png?800|}} \\

==== Step1: 创建AI在线服务 ====
[[ai:uai-inference:use:oplist:create]] \\

=== 创建弹性服务 ===
  * 点击“创建新服务”选项，并选择**弹性服务**， 可以根据需要选择特定的配置。\\
{{:ai:uai-inference:use:new:create-cpu.png?600|}} \\

=== 创建独占服务 ===
  * 点击“创建新服务”选项，并选择**独占服务**， 可以根据需要选择特定的配置。\\
{{:ai:uai-inference:use:new:create-gpu.png?600|}} \\

=== 完成服务APP创建 ===
  * 设置好服务的信息后，点击“确定”选项，页面自动跳转至UAI产品首页，在服务列表中可以观察到新建的服务信息。\\
{{ai:uai-service:use:oplist:create:create_4.png?600|}} \\


  * 点击服务ID后（此处对应“uaiservice-lv0cle”)，跳转至服务概览页面，显示服务的基本信息，配置信息以及操作日志。\\
{{ai:uai-service:use:oplist:create:create_5.png?600|}} \\

----

==== Step2: 准备好部署所需Docker镜像 ====

通过uai-sdk的工具，将本地代码打包成Docker镜像并上传至Uhub，具体步骤参见[[ai:uai-inference:use:oplist:packdata_docker|]]

----

==== Step3: 部署AI在线服务的第一个版本 ====
在服务列表页面，选择需要部署新版本的服务ID，并点击部署按钮，打开部署界面 \\
{{:ai:uai-service:use:oplist:deploydocker待部署.png?800|}} \\

=== 部署界面填写 ===
在下拉框中选择[[ai:uai-inference:use:oplist:packdata_docker|]]上传到Uhub的镜像完成部署。\\
{{:ai:uai-inference:use:new:deoloy-image.png?600|}} \\

信息填写完成后，点击“确定”选项后，界面自动跳转至服务概览页面，可以看到部署版本的操作日志。\\
注意：**部署耗时可能需要10分钟，请耐心等待。** \\
{{:ai:uai-service:use:oplist:deploydocker部署中.png?600|}} \\

=== 部署完成 ===
点击“版本管理”选项后，查看部署版本的详细信息，样例如下：\\
注意1：当版本部署后运行状态是“错误”时，可以点击“部署日志”选项，查看错误信息。\\
注意2：成功部署的版本初始状态为“未激活”，点击“启动”即可激活该版本的服务。\\
{{:ai:uai-service:use:oplist:deploydocker代码包部署完成.png?600|}} \\


==== Step4: 启动AI在线服务的第一个版本 ====
[[ai:uai-inference:use:oplist:start]] \\
  * 版本部署成功后，该版本的状态默认为“未激活”， 点击“启动”选项激活该版本的服务，此时版本状态变更为“已激活”。
{{:ai:uai-service:use:graydeploy:start未激活.png?800|}}

==== Step5: 获取服务的URL ====
  * 第一个版本部署成功之后，系统会为服务分配一个URL地址：SrvURL，在“服务概览”页面的“基本信息”模块可以直接复制该地址。
{{ai:uai-service:use:new:cp_srv_url.png?800|}}
  * 部署成功的版本对应服务URL为：版本号-dot-SrvURL。至此，第一个版本的服务已经部署完成，借助“版本号-dot-SrvURL”即可访问该版本服务啦。

