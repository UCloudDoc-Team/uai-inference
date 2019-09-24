{{indexmenu_n>70}}

# 部署CPU在线服务APP
现在我们可以使用UAI Inference平台来实现Mnist在线服务了：

  * 创建Mnist Inference 在线服务APP
  * 创建Mnist 在线服务

## 创建Mnist Inference 在线服务APP
我们进入[[https://console.ucloud.cn/uai]] UAI Inference界面，创建一个新服务：
{{:ai:uai-inference:tutorial:tf-mnist:创建ai服务.png|}}

{{:ai:uai-inference:tutorial:tf-mnist:创建ai服务2.png|}}

## 部署Mnist 在线服务
点击部署按钮，根据提示操作即可部署完毕：
{{:ai:uai-inference:tutorial:tf-mnist:部署.png|}}

部署完毕后，点击详情按钮，进入任务详细页面点击启动按钮后，任务就启动了（启动过程可能需要几分钟，请耐心等待）：
{{:ai:uai-inference:tutorial:tf-mnist:启动.png?400|}}

我们可以在详细页面获取Mnist在线服务的URL地址，URL地址我们将在测试mnist在线服务时使用：
{{:ai:uai-inference:tutorial:tf-mnist:地址.png?400|}}

## 测试Mnist 在线服务
在我们github的目录有一张测试图片2.jpg,我们使用该图片进行测试。
首先我们需要进入云主机。我们可以通过如下命令来测试：
<code>
$ cd ~/uai-sdk/examples/caffe/inference/mnist

$ curl -X POST http://<URL>/service -T 2.jpg
</code>
这里的URL即为我们在UAI-inference平台上获得的URL地址。
输出结果：2，则测试成功。

