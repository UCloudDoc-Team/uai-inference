{{indexmenu_n>10}}

# 本地测试方法

如果使用docker镜像打包，可选择镜像模式。否则，使用代码模式。

## 镜像模式
用户执行[[ai:uai-inference:use:oplist:packdata_docker|]]将代码放入镜像中后，可根据以下步骤进行本地测试。
### 1. 运行本地服务
镜像准备完成后，可在本地运行docker中的服务。
<code>
sudo docker run -d --net=bridge --name=uai_inference_test -p 8080:8080 <image_name>
</code>

### 2.发送Http 测试请求
<code>
curl -X POST http://localhost:8080/service -T <file_name>
</code>

## 代码模式
AI在线服务包含了一个uai-sdk-httpserver，其是基于Flask的http server逻辑，可以用来测试您所写的inference代码。

### 1.代码准备
首先下载测试所需服务代码。
[[https://github.com/ucloud/uai-sdk-httpserver]]
<code>
git clone https://github.com/ucloud/uai-sdk-httpserver.git
</code>

### 2.模型准备
请将您所实现的所有代码直接放入http-server-httpserver/ 目录下,并确保包含主类（实现了load\_mode()和execute()函数的类）的文件在该目录下。
<code>
http-server-httpserver

​        mnist_inference（主文件）
​        其他文件
​        checkpoint_dir (模型文件目录)
​                checkpoint
​                xxx.mod
​                xxx.mod.meta
​                其他文件
</code>
**注：**
a. 请将您的模型文件目录checkpoint_dir直接放入http-server-httpserver/ 目录下。
b. 将Tensorflow框架训练后的模型文件置于checkpoint_dir目录下（如checkpoint及文件格式为
   mod和mod.meta的文件等）
c. 请确保checkpoint文件中的model_checkpoint_path和all_model_checkpoint_paths后面所填写
   的是相对路径，方便代码调试。


### 3. 编写测试配置文件
   根据上述文件，可编写测试配置信息文件mnist.conf：
   <code>

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
</code>
上述代码的参数说明
  * main\_class：模型代码主类名
  * main\_file：模型代码文件名
  * model\_dir：模型文件目录  
测试配置文件置于http-server-httpserver/目录下。

### 4.运行本地服务

本地代码测试: 即您可以完全在您自己本地机器上进行AI项目部署及测试。所有文件和模型都位于http-server-httpserver/目录下。
<code>
python server.py --port=8080 --json_conf="mnist.conf"
</code>
其中port参数是您本地测试使用的端口号，json_conf参数是您测试配置信息文件名。

### 5.发送Http 测试请求

<code>
curl -X POST http://localhost:8080/service -T <file_name>
</code>


