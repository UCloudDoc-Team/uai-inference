

# 部署GPU在线服务APP
现在我们可以使用UAI-Inference平台来部署GPU版本的Mnist在线服务了：

  * 创建Mnist Inference GPU在线服务APP
  * 创建Mnist GPU在线服务

## 创建MNist GPU在线服务APP

进入AI Inference界面，点击“创建新服务”选项，创建一个新服务：

![](ai/uai-inference/images/tutorial/tf-mnist/tutorial1.png)

在弹出的“创建新服务”页面上，选择服务类型为**“独占服务“**，填写相关信息，确认后点击“确认”选项，创建新的GPU在线服务。

![](ai/uai-inference/images/tutorial/tf-mnist/tutorial2.png)





## 部署MNist GPU在线服务APP

服务创建完成后，即可在服务列表页查看到新创建的服务。点击“部署”选项，在弹出的页面上填写相关信息，完成部署操作。 
![](ai/uai-inference/images/tutorial/tf-mnist/tutorial3.png)
![](ai/uai-inference/images/tutorial/tf-mnist/mnist_tf.png)

部署操作执行后，页面自动跳转至服务的概览页面，整个部署过程耗时几分钟。部署完成之后，服务状态为“未激活”，点击“启动“选项，启动当前服务。同时可以在该页面获取Mnist在线服务的URL地址，用于访问服务。
![](ai/uai-inference/images/tutorial/tf-mnist/tutorial4.png)

## 测试Mnist在线服务

借助以下命令可以对该服务进行测试：
<code>
curl -X POST http://<URL>/service -T 2.jpg
</code>


