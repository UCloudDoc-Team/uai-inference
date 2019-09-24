{{indexmenu_n>55}}

# 本地测试Mnist在线服务
我们已经生成了在线服务镜像uhub.service.ucloud.cn/uai_demo/tf-mnist-infer:latest，我们可以在本地启动该服务并测试

## 1. 运行本地服务
### 执行CPU在线服务
镜像准备完成后，可在本地运行docker中的服务。
<code>
$ sudo docker run -it -p 8080:8080 uhub.service.ucloud.cn/uai_demo/tf-mnist-infer-cpu:latest
</code>

### 执行GPU在线服务
镜像准备完成后，如果你本地有NVidia GPU 并且安装了nvidia-docker，你可以本地启动GPU 在线服务。
<code>
$ sudo docker run -it -p 8080:8080 uhub.service.ucloud.cn/uai_demo/tf-mnist-infer-gpu:latest
</code>



## 2. 发送Http 测试请求
在我们github的目录由一张测试的图片
<code>
$ cd ~/uai-sdk/examples/tensorflow/inference/mnist_1.1

$ curl -X POST http://localhost:8080/service -T 2.jpg
</code>

