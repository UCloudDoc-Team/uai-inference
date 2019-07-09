{{indexmenu_n>3}}

## UAI Inference 开发概述

面向UAI Inference 开发训练代码、Docker镜像的基本原则如下：  
\- 目前仅提供CPU节点的是在线服务任务，使用基于docker的镜像，UCloud已经提供丰富的CPU基础镜像选择，客户可以自由选择

1.  在线服务的基础镜像提供了基于django的http server模块来实现在线服务任务的HTTP化，**django
    server在docker内部的/ai-ucloud-client-django/**目录下
2.  **在线服务的代码和所使用的模型请封装在Docker镜像中**
3.  **在线服务的代码入口由ufile.json控制，该文件必须位于/ai-ucloud-client-django/下（历史原因，名字为ufile.json)，里面定义了如何加载inference代码**
4.  在线服务使用的django http server在容器启动时会自动拉起
5.  在线服务使用的节点**无外网访问能力**

用户可以根据如下指南开发对于AI框架的在线服务代码+容器：  
[tensorflow](/ai/uai-inference/guide/tensorflow)  
[caffe](/ai/uai-inference/guide/caffe)  
[keras](/ai/uai-inference/guide/keras)  
[mxnet](/ai/uai-inference/guide/mxnet)  
也可以根据以上准则开发自定义的 **在线服务容器**

#### UAI-Inference执行的概念图

![](/images/guide/service-general.png)

#### inference代码说明

![](/images/guide/code-overview.png)

#### ufile.json说明

ufile.json
包括两个部分，以https://github.com/ucloud/uai\\-sdk/blob/master/examples/tensorflow/mnist\\\_0.11/tf\\\_mnist.conf
为例：

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

##### 启动相关

**http\\\_server:exec** 下面的main\\\_class和main\\\_file 指定了django
server如何加载inference代码，其中main\\\_class指定inference代码的主类名，main\\\_file指定了该主类的python模块路径（即如何import）  
**注：AI Inference的Http
server的模块加载路径为/ai-ucloud-client-django/，因此所有inference相关的python模块都需要放入该目录下。**

##### AI框架相关

http\\\_server:tensorflow则指定了tensorflow相关的信息，该信息对于不同AI框架不同，详细可以查看对应AI框架的开发指南[guide](/ai/uai-inference/guide)
