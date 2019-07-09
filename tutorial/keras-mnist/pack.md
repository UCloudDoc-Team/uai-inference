{{indexmenu_n>50}}

## 使用自定义镜像打包

接下来我们使用自定义的镜像打包。

### 前期准备

我们还需要准备

  - 代码
  - 模型文件
  - keras\_mnist.conf文件

#### 准备代码

我们在/data/mnist下创建code目录，将mnist\\\_inference.py放入/data/mnist/code目录下：

    /data/mnist/
    |_ code
    |_ |_ mnist_inference.py

#### 模型文件

我们可以使用UAI-Train训练平台训练模型，当然我们的github也提供了训练好的模型  
<https://github.com/ucloud/uai-sdk/tree/master/examples/keras/inference/mnist/checkpoint_dir>  
我们把模型文件放入/data/mnist/code/checkpoint\\\_dir目录下：

    /data/mnist/
    |_ code
    |  |_ checkpoint_dir
    |  |  |_ mnist_model.h5
    |  |  |_ mnist_model.json
    |  |_ mnist_inference.py

#### keras\_mnist.conf

keras\\\_mnist.conf向UAI-Inference平台提供了加载mnist模型的基本信息，我们的github提供了该文件<https://github.com/ucloud/uai-sdk/blob/master/examples/keras/inference/mnist/keras_mnist.conf>  
keras\\\_mnist.conf的结构如下：

    {
        "http_server" : {
            "exec" : {
                "main_class": "MnistModel",
                "main_file": "mnist_inference"
            },
            "keras" : {
                "model_dir" : "./checkpoint_dir",
                "model_name" : "mnist_model",
                "all_one_file" : false,
                "model_arch_type" : "json"
            }
        }
    }

``` 
 * 其中**exec.main\_class**定义了推理服务的对象类：MnistModel \\
 * 其中**exec.main\_file**定义了入口python模块:mnist\_inference.py（注：需要将.py 删除，因为django会以模块的形式import mnist\_inference）\\
 * 其中**mxnet.model\_dir**定义了模型的相对路径，用于给Inference 类对象传递模型路径参数 \\
 * **keras.xxx**(可选)：KerasAiUcloudModel会根据keras下model\_dir, model\_name, all\_one\_file, model\_arch\_type的参数拼接keras模型路径，详细请参见[[https://github.com/ucloud/uai-sdk/blob/master/uai/arch/keras_model.py]]。
```

我们同样将keras\\\_mnist.conf放入/data/mnist/目录下：

    /data/mnist/
    |_ code
    |  |_ checkpoint_dir
    |  |  |_ mnist_model.h5
    |  |  |_ mnist_model.json
    |  |_ mnist_inference.py
    |_ keras_mnist.conf

### 打包CPU在线服务镜像

我们直接使用docker build命令来打包mnist镜像，我们需要首先创建一个mnist-cpu.Dockerfile

    FROM uhub.service.ucloud.cn/uaishare/cpu_uaiservice_ubuntu-14.04_python-2.7.6_keras-1.2.0:v1.2
    EXPOSE 8080
    ADD ./code/ /ai-ucloud-client-django/
    ADD ./keras_mnist.conf  /ai-ucloud-client-django/conf.json
    ENV UAI_SERVICE_CONFIG /ai-ucloud-client-django/conf.json
    CMD cd /ai-ucloud-client-django && gunicorn -c gunicorn.conf.py httpserver.wsgi

Dockerfile里面做了以下几个事情：

1.  基于uai inference 的keras的CPU基础镜像来构建
2.  export 8080端口
3.  将当前目录下code/ 下所有文件放入/ai-ucloud-client-django/
4.  将keras\\\_mnist.conf 放入/ai-ucloud-client-django/conf.json，django
    server将自动加载json config文件
5.  指定UAI Inference server在启动时使用/ai-ucloud-client-django/conf.json 配置文件
6.  使用gunicorn运行server

我们将这个Dockerfile文件放置于/data/mnist/目录下：

    /data/mnist/
    |_ code
    |  |_ checkpoint_dir
    |  |  |_ mnist_model.h5
    |  |  |_ mnist_model.json
    |  |_ mnist_inference.py
    |_ keras_mnist.conf
    |_ mnist-cpu.Dockerfile

#### Build CPU在线服务镜像

    $ cd /data/mnist/
    $ sudo docker build -t uhub.service.ucloud.cn/uai_demo/keras-mnist-infer:latest -f mnist-cpu.Dockerfile .

我们就可以生成镜像：uhub.service.ucloud.cn/uai\\\_demo/keras-mnist-infer:latest

### 上传在线服务镜像至uhub镜像仓库

我们将得到的uhub.service.ucloud.cn/uai\\\_demo/keras-mnist-infer:latest镜像上传到uhub镜像仓库中去。

    $ sudo docker push uhub.service.ucloud.cn/uai_demo/keras-mnist-infer:latest
