 

# 用CLI创建新任务
## Step0: 准备工作
### 1. 安装UCloud UFile SDK  

<code>
wget http://sdk.ufile.ucloud.com.cn/python_sdk.tar.gz

tar zxvf python_sdk.tar.gz
cd ufile-python
sudo python setup.py install
</code>
**注：UFile SDK仅兼容request 2.10.0以下版本**

### 2. 安装UAI SDK

<code>
git clone https://github.com/ucloud/uai-sdk

cd uai-sdk
sudo python setup.py install
</code>

### 3. 获取用户公钥和私钥 

  * 登陆UCloud官网，进入Console页面
  * 点击左上角的“产品与服务”选项，选择“监控管理”列表下的“API密钥 UAPI”选项后，点击API密钥中的“显示”选项，按照提示获取用户的公钥和私钥。

### 4. UAI SDK工具使用

  * 以下所有命令行工具均为uai_tool.py（默认存放路径为$uai-sdk安装路径/uai\_tools/uai\_tool.py）

## Step1: 创建AI在线服务

### 创建弹性服务

  * 执行[](ai/uai-inference/use/oplist/create)命令 

<code>
python uai_tool.py create \

​	--public_key=PUBLIC_KEY \
​	--private_key=PRIVATE_KEY \
​	--service_name=SERVICE_NAME \
​	--cpu=CPU --memory=MEMORY
</code>

### 创建独占服务

  * 执行[[ai:uai-inference:use:oplist:create]]命令 

<code>
python uai_tool.py create \

​	--public_key=PUBLIC_KEY \
​	--private_key=PRIVATE_KEY \
​	--service_name=SERVICE_NAME \
​	--gpu=GPU
</code>

  * 返回说明 
创建服务指令成功执行后，将返回服务的资源ID以及服务ID。RetCode字段为0时表示正常返回，否则为错误码。返回错误码时可依据Message字段信息进行错误分析。正确返回样例如下：

<code>
CreateUAIServiceSuccess:
{

​    ResourceID: uaiservice-xxxx,
​    Message: ,
​    ServiceID:  uaiservice-xxxx,
​    RetCode: 0
}
</code>

## Step2: 准备好部署所需文件（包括模型以及代码文件），打包服务所需的镜像

**注：镜像打包时涉及到Docker的使用，建议sudo执行相关指令 ** 

### 基于自定义的镜像打包模式

* 执行[](ai/uai-inference/use/oplist/packdata_docker)命令，通过uai-sdk的工具，将本地文件打包成Docker镜像并上传至Uhub 

<code>
python uai_tool.py packdocker self \

​	--bimg_name=BIMG_NAME \
​	--pack_file_path=PACK_FILE_PATH \
​	--conf_file=CONF_FILE \
​	--uhub_username UHUB_USERNAME \
​	--uhub_password UHUB_PASSWORD \
​	--uhub_registry UHUB_REGISTRY \
​	--uhub_imagename UHUB_IMAGENAME
</code>

### 基于深度学习框架的镜像打包模式

* 执行[](ai/uai-inference/use/oplist/packdata_docker)]命令，通过uai-sdk的工具，将本地文件打包成Docker镜像并上传至Uhub 

* 支持caffe, keras, mxnet, tf(tensorflow)四种深度学习框架 

#### 1. caffe
<code>
python uai_tool.py packdocker caffe \

​        --public_key xxxxx  \
​        --private_key xxxxx  \
​        --main_class MnistModel  \
​        --main_module mnist_inference  \
​        --model_dir checkpoint_dir  \
​        --model_name mnist_model  \
​        --pack_file_path mnist_caffe \
​        --uhub_username xxxxx  \
​        --uhub_password xxxxx  \
​        --uhub_registry xxxxx  \
​        --uhub_imagename caffe-inference:test-2
</code>
#### 2. keras
<code>
python uai_tool.py packdocker keras \

​        --public_key xxxxx \
​        --private_key xxxxx  \
​        --main_class MnistModel  \
​        --main_module mnist_inference  \
​        --model_dir checkpoint_dir  \
​        --model_name mnist_model  \
​        --pack_file_path mnist_keras  \
​        --uhub_username xxxxx  \
​        --uhub_password xxxxx  \
​        --uhub_registry xxxxx  \
​        --uhub_imagename keras-inference:test-2
</code>
#### 3. mxnet
<code>
python uai_tool.py packdocker mxnet \

​        --public_key xxxxx \
​        --private_key xxxxx  \
​        --main_class MnistModel  \
​        --main_module mnist_inference  \
​        --model_dir checkpoint_dir  \
​        --model_name mnist-model  \
​        --pack_file_path mnist_mxnet  \
​        --num_epoch 10  \
​        --uhub_username xxxxx  \
​        --uhub_password xxxxx  \
​        --uhub_registry xxxxx  \
​        --uhub_imagename mxnet-inference:test-2 
</code>
#### 4. tensorflow
<code>
python uai_tool.py packdocker tf \

​        --public_key xxxxx \
​        --private_key xxxxx  \
​        --main_class MnistModel \
​        --main_module mnist_inference \
​        --model_dir checkpoint_dir \
​        --pack_file_path mnist_tf \
​        --uhub_username xxxxx \
​        --uhub_password xxxxx \
​        --uhub_registry xxxxx \
​        --uhub_imagename tf-inference:test-2
</code>

  * 返回说明 
成功执行后，将返回如下信息：
<code>
upload docker images successful. images:uhub.ucloud.cn/fanrongtest/keras-inference:test-2
</code>

## Step3: 部署AI在线服务的第一个版本

  * 执行[](ai/uai-inference/use/oplist/deploydocker)命令 

<code>

python uai_tool.py deploydocker --public_key PUBLIC_KEY --private_key PRIVATE_KEY --service_id SERVICE_ID --uimg_name UIMG_NAME

</code>

  * 返回说明 
命令行部署指令成功执行后，会返回部署的版本号。RetCode字段为0时表示正常返回，否则为错误码。依据Message字段的信息可以检测部署进行状态以及错误分析。正常返回示例如下：

<code>
DeployUAIServiceByDocker Success:
{

Message : ,
RetCode : 0,
SrvVersion : x-x
}
</code>

部署过程中会不断检测当前部署状态。RetCode字段为0时表示正常返回，否则为错误码。依据Message字段的信息可以检测部署进行状态以及错误分析。**部署耗时大概为10分钟，请耐心等待。**
正常返回示例如下：

<code>
CheckUAIDeployProgress Success:
{

Status : Deploying,
SrvPaasID : ,
RetCode : 0,
DeployLog : "",
SrvURL : "",
Message : Step2：Building user image
</code>

RetCode字段为0时表示正常返回，否则为错误码。依据Message字段的信息可以检测部署进行状态以及错误分析。 

部署成功后，将返回部署服务的域名（SrvURL）和状态，部署成功后版本状态为“未激活”（ToStart）。正常返回样例如下：
<code>
CheckUAIDeployProgress Success:
{

Status : ToStart,
SrvPaasID : xxxx,
RetCode : 0,
DeployLog : "",
SrvURL : xxxx.paas.service.ucloud.cn,
Message :
}
</code>

## Step4: 启动AI在线服务的第一个版本

  * 执行[](ai/uai-inference/use/oplist/start)命令 
<code>
python uai_tool.py start --public_key=PUBLIC_KEY --private_key=PRIVATE_KEY --service_id=SERVICE_ID --paas_id=PAAS_ID --service_version=SERVICE_VERSION
</code>

  * 返回说明 

RetCode字段为0时表示正常返回，否则为错误码。返回错误码时可依据Message字段信息进行错误分析。成功执行后，返回样例如下：
<code>
StartUAIService Success:
{

Message : ,
RetCode : 0
}
</code>

   * 版本对应服务URL为：版本号-dot-SrvURL。至此，第一个版本的服务已经部署完成，借助“版本号-dot-SrvURL”即可访问该版本服务啦


