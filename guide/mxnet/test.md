

# 本地测试方法

如果使用docker镜像打包，可选择镜像模式。否则，使用代码模式。

## 镜像模式
用户执行[[ai:uai-inference:use:oplist:packdata_docker|]]将代码放入镜像中后，可根据以下步骤进行本地测试。
### 1. 运行本地服务
镜像准备完成后，可在本地运行docker中的服务。
<code>
sudo docker run -d --net=bridge --name=uai_inference_test -p 8080:8080 <image_name>
</code>

### 2. 发送HTTP测试请求
<code>
curl -X POST http://localhost:8080/service -T <file_name>
</code>

## 代码模式
AI在线服务包含了一个uai-sdk-httpserver，其是基于Flask的http server逻辑，可以用来测试您所写的inference代码。

### 1. 代码准备
首先下载测试所需服务代码。
[[https://github.com/ucloud/uai-sdk-httpserver]]
<code>
git clone https://github.com/ucloud/uai-sdk-httpserver.git
</code>
请将您所实现的所有代码直接放入http-server-httpserver/ 目录下,并确保主类（实现了load\_mode()和execute()函数的类）在这些代码文件中。

### 2. 模型准备
请将您的模型文件目录checkpoint_dir直接放入http-server-httpserver/ 目录下，基于MXNet框架村脸后的模型文件通常包含参数文件（.params后缀）和框架文件（.json后缀），所以通常该模型文件目录包含了上述这些格式的相关模型文件。

### 3. 运行HTTP Server
本地代码测试: 即您可以完全在您自己本地机器上进行AI inference项目的测试。所有文件和模型都位于http-server-httpserver/目录下。
<code>
python server.py --port=8080 --json_conf="mnist.conf"
</code>
其中port参数是您本地测试使用的端口号，json_conf参数是您测试配置信息文件名，测试配置信息文件也放在http-server-httpserver/目录下。

**测试配置信息文件定义如下：**
<code>
{
    "http_server" : {

​        "exec" : {
​            "main_class": "MnistModel",
​            "main_file": "mnist_inference"
​        },
​        "mxnet" : {
​            "model_dir" : "./checkpoint_dir",
​            "model_name" : "mnist-model",
​            "num_epoch" : 10
​        }
​    }
}
</code>

上述配置文件的参数说明:
  * main\_class：模型代码主类名
  * main\_file：模型代码文件名
  * model\_dir：模型文件目录
  * model\_name: 模型文件的名字（不带后缀）
  * num\_epoch: 保存的模型所在的epoch

### 4. 发送Http 测试请求
<code>
curl -X POST http://localhost:8080/service -T <file_name>
</code>