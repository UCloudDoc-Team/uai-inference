{{indexmenu_n>5}}

===== 部署CPU在线服务APP =====
现在我们可以使用UAI Inference平台来实现Mnist在线服务了：
  * 创建Mnist Inference 在线服务APP
  * 创建Mnist 在线服务

==== 创建Mnist Inference 在线服务APP ====
我们进入[[https://console.ucloud.cn/uai]] AI Inference界面，创建一个新服务：\\
{{:ai:uai-inference:tutorial:tf-mnist:创建ai服务.png|}}

{{:ai:uai-inference:tutorial:tf-mnist:创建ai服务2.png|}}

==== 部署Mnist 在线服务 ====
点击部署按钮，根据提示操作即可部署完毕：\\
{{:ai:uai-inference:tutorial:tf-mnist:部署.png|}}

部署完毕后，点击详情按钮，进入任务详细页面点击启动按钮后，任务就启动了（启动过程可能需要几分钟，请耐心等待）：\\
{{:ai:uai-inference:tutorial:tf-mnist:启动.png?400|}}

我们可以在详细页面获取Mnist在线服务的URL地址：\\
{{:ai:uai-inference:tutorial:tf-mnist:地址.png?400|}}

==== 测试Mnist 在线服务 ====
我们可以通过如下命令来测试：
<code>
curl -X POST http://<URL>/service -T 2.jpg
</code>