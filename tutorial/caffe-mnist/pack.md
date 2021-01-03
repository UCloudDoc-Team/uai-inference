

# 使用自定义镜像打包
接下来我们使用自定义的镜像打包。

## 前期准备
我们还需要准备

  * 代码
  * 模型文件
  * caffe_mnist.conf文件

### 准备代码
我们在/data/mnist下创建code目录，将mnist\_inference.py放入/data/mnist/code目录下：
<code>
/data/mnist/

|_ code
|_ |_ mnist_inference.py
</code>

### 模型文件 ###
我们可以使用UAI-Train训练平台训练模型，当然我们的[github](https://github.com/ucloud/uai-sdk/tree/master/examples/caffe/inference/mnist/checkpoint_dir)也提供了训练好的模型 


我们把模型文件放入/data/mnist/code/checkpoint\_dir目录下：
<code>
/data/mnist/

|_ code
|  |_ checkpoint_dir
|  |  |_ mnist_model.caffemodel
|  |  |_ mnist_model.prototxt
|  |_ mnist_inference.py
</code>

### caffe_mnist.conf
caffe\_mnist.conf向UAI-Inference平台提供了加载mnist模型的基本信息，我们的github提供了该[文件](https://github.com/ucloud/uai-sdk/blob/master/examples/caffe/inference/mnist/caffe_mnist.conf)
caffe\_mnist.conf的结构如下：
<code>
{
    "http_server" : {

​        "exec" : {
​            "main_class": "MnistModel",
​            "main_file": "mnist_inference"
​        },
​        "caffe" : {
​            "model_dir" : "./checkpoint_dir",
​            "model_name" : "mnist_model"
​        }
​    }
}
</code>

  * 其中**exec.main\_class**定义了推理服务的对象类：MnistModel 
  * 其中**exec.main\_file**定义了入口python模块:mnist\_inference.py（注：这边需要将.py 删除，因为django会以模块的形式import mnist\_inference）
  * 其中**caffe.model\_dir**定义了模型的相对路径 
  * 其中**caffe.model\_name**定义了模型文件的名称 
我们同样将caffe\_mnist.conf放入/data/mnist/目录下：
<code>
/data/mnist/
  * |_ code
|  |_ checkpoint_dir
|  |  |_ mnist_model.caffemodel
|  |  |_ mnist_model.prototxt
|  |_ mnist_inference.py
|_ caffe_mnist.conf
</code>

## 打包GPU在线服务镜像
我们直接使用docker build命令来打包GPU版本的mnist镜像，我们需要首先创建一个mnist-gpu.Dockerfile

<code>
FROM uhub.service.ucloud.cn/uaishare/gpu_uaiservice_ubuntu-14.04_python-2.7.6_caffe-1.0.0:v1.0

EXPOSE 8080
ADD ./code/ /ai-ucloud-client-django/
ADD ./caffe_mnist.conf  /ai-ucloud-client-django/conf.json
ENV UAI_SERVICE_CONFIG "/ai-ucloud-client-django/conf.json"

CMD cd /ai-ucloud-client-django && gunicorn -c gunicorn.conf.py httpserver.wsgi
</code>

Dockerfile里面做了以下几个事情：
  - 基于uai inference 的caffe 1.0.0 的GPU基础镜像来构建
  - export 8080端口
  - 将当前目录下code/ 下所有文件放入/ai-ucloud-client-django/
  - 将caffe\_mnist.conf  放入/ai-ucloud-client-django/conf.json
  - 指定UAI Inference server在启动时使用/ai-ucloud-client-django/conf.json 配置文件
  - 使用gunicorn运行server
我们将这个Dockerfile文件放置于/data/mnist/目录下：
<code>
/data/mnist/
  - |_ code
|  |_ checkpoint_dir
|  |  |_ mnist_model.caffemodel
|  |  |_ mnist_model.prototxt
|  |_ mnist_inference.py
|_ caffe_mnist.conf
|_ mnist-cpu.Dockerfile
|_ mnist-gpu.Dockerfile
</code>

### Build GPU在线服务镜像
<code>
$ cd /data/mnist/

$ sudo docker build -t uhub.service.ucloud.cn/uai_demo/caffe-mnist-infer-gpu:latest -f mnist-gpu.Dockerfile .
</code>
我们就可以生成GPU镜像：uhub.service.ucloud.cn/uai_demo/caffe-mnist-infer-gpu:latest

## 上传在线服务镜像至uhub镜像仓库
以CPU镜像为例，我们将得到的uhub.service.ucloud.cn/uai\_demo/caffe-mnist-infer-gpu:latest镜像上传到uhub镜像仓库中去。
<code>
$ sudo docker push uhub.service.ucloud.cn/uai_demo/caffe-mnist-infer-gpu:latest
</code>

