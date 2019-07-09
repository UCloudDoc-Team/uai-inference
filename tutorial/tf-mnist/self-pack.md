{{indexmenu_n>45}}

## 使用自定义镜像打包

接下来我们使用自定义的镜像打包

### 前期准备

我们还需要准备

  - 代码
  - 模型文件
  - mnist.conf文件

#### 准备代码

我们在/data/mnist/下创建code目录，并将mnist\\\_inference.py放入该目录下：

    /data/mnist
    |_ code
    |_ |_ mnist_inference.py

#### 模型文件

我们可以使用[tf-mnist](/ai/uai-train/tutorial/tf-mnist)中训练出来的mnist模型文件，我们也可以自己训练一个新的，当然我们的github也提供了训练好的模型<https://github.com/ucloud/uai-sdk/tree/master/examples/tensorflow/inference/mnist_1.1/checkpoint_dir>  
我们把模型文件放入/data/mnist/code/目录下：

    /data/mnist
    |_ code
    |  |_ mnist_inference.py
    |  |_ checkpoint_dir
    |  |  |_ checkpoint
    |  |  |_ mnist.mod.data-00000-of-00001
    |  |  |_ mnist.mod.index
    |  |  |_ mnist.mod.meta

#### mnist.conf文件

mnist.conf的结构如下：

    {
        "http_server" : {
            "exec" : {
                "main_class": "MnistModel",
                "main_file": "mnist_inference"
            },
            "tensorflow" : {
                "model_dir" : "./checkpoint_dir"
            }
        }
    }

其中**exec.main\\\_file**定义了入口python模块:mnist\\\_inference.py（注：这边需要将.py
删除，因为django会以模块的形式import mnist\\\_inference）  
其中**exec.main\\\_class**定义了推理服务的对象类：MnistModel  
其中**tensorflow.model\\\_dir**定义了模型的相对路径  
我们同样将mnist.conf放入/data/mnist/目录下：

    /data/mnist
    |_ code
    |  |_ minst_inference.py
    |  |_ checkpoint_dir
    |  |  |_ checkpoint
    |  |  |_ mnist.mod.data-00000-of-00001
    |  |  |_ mnist.mod.index
    |  |  |_ mnist.mod.meta
    |  |_ mnist.conf

### 打包CPU在线服务镜像

我们直接使用docker build命令来打包mnist镜像，我们需要首先创建一个mnist.Dockerfile

    FROM uhub.service.ucloud.cn/uaishare/cpu_uaiservice_ubuntu-14.04_python-2.7.6_tensorflow-1.1.0:v1.2
    
    EXPOSE 8080
    ADD ./code/ /ai-ucloud-client-django/
    ADD ./mnist.conf  /ai-ucloud-client-django/conf.json
    ENV UAI_SERVICE_CONFIG /ai-ucloud-client-django/conf.json
    CMD cd /ai-ucloud-client-django && gunicorn -c gunicorn.conf.py httpserver.wsgi

Dockerfile里面做了以下几个事情：

1.  基于uai inference 的tensorflow 1.1.0 的基础镜像来构建
2.  export 8080端口
3.  将当前目录下code/ 下所有文件放入/ai-ucloud-client-django/
4.  将mnist.conf 放入/ai-ucloud-client-django/conf.json
5.  指定UAI Inference server在启动时使用/ai-ucloud-client-django/conf.json 配置文件
6.  启动http server

### 打包GPU在线服务镜像

我们直接使用docker build命令来打包GPU版本的mnist镜像，我们需要首先创建一个mnist-gpu.Dockerfile

    FROM uhub.service.ucloud.cn/uaishare/gpu_uaiservice_ubuntu-16.04_python-3.6_tensorflow-1.6.0:v1.0
    
    EXPOSE 8080
    ADD ./code/ /ai-ucloud-client-django/
    ADD ./mnist.conf  /ai-ucloud-client-django/conf.json
    ENV UAI_SERVICE_CONFIG /ai-ucloud-client-django/conf.json
    CMD cd /ai-ucloud-client-django && gunicorn -c gunicorn.conf.py httpserver.wsgi

Dockerfile里面做了以下几个事情：

1.  基于uai inference 的tensorflow 1.6.0 的GPU基础镜像来构建
2.  export 8080端口
3.  将当前目录下code/ 下所有文件放入/ai-ucloud-client-django/
4.  将mnist.conf 放入/ai-ucloud-client-django/conf.json
5.  指定UAI Inference server在启动时使用/ai-ucloud-client-django/conf.json 配置文件
6.  启动http server

#### Build CPU在线服务镜像

    $ cd /data/mnist/
    $ vim mnist.Dockerfile
    $ sudo docker build -t uhub.service.ucloud.cn/uai_demo/tf-mnist-infer-cpu:latest -f mnist.Dockerfile .

我们就可以生成镜像：uhub.service.ucloud.cn/uai\_demo/tf-mnist-infer-cpu:latest

#### Build GPU在线服务镜像

    $ cd /data/mnist/
    $ vim mnist-gpu.Dockerfile
    $ sudo docker build -t uhub.service.ucloud.cn/uai_demo/tf-mnist-infer-gpu:latest -f mnist-gpu.Dockerfile .

我们就可以生成GPU镜像：uhub.service.ucloud.cn/uai\_demo/tf-mnist-infer-gpu:latest
