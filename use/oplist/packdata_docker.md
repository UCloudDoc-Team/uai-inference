{{indexmenu_n>3}}

# packdocker

## 命令作用
将用户本地代码打包到Docker镜像中，该Docker镜像可以在控制台界面使用**“镜像模式”**部署版本（操作参见[[ai:uai-inference:use:oplist:deploydocker|]]） 

除了支持指定caffe, keras, mxnet, tensorflow等多种深度学习框架进行镜像打包外，同时支持**自定义镜像打包模式**。

## 准备工作
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

[[ai:uai-inference:base:key]]

  * 登陆UCloud官网，进入Console页面：[[https://console.ucloud.cn/dashboard|https://console.ucloud.cn/dashboard]]
  * 点击左上角的“产品与服务”选项，选择“监控管理”列表下的“API密钥 UAPI”选项后，点击API密钥中的“显示”选项，按照提示获取用户的公钥和私钥。

## 准备打包所需文件
用户需将AI在线服务所需的**代码以及模型文件**放在某一路径下，参数pack\_file\_path上传。文件目录结构示例如下：

<code>
file_dir (对应参数pack_file_path，相对路径)

​    code_files1
​    code_files2
​    code_files3
​    checkpoint_dir (对应参数model_dir，相对路径)
​        model_files1
​        model_files2
​        model_files3
</code>

## 准备命令行工具
为后续操作方便，可将UAI SDK安装包中的命令行工具（$uai-sdk安装路径/uai\_tools/uai\_tool.py）拷贝到代码pack\_file\_path的同级目录
<code>
uai_tool.py (从$uai-sdk安装路径/uai_tools/uai_tool.py拷贝）

file_dir (对应参数pack_file_path，相对路径)
    code_files1
    code_files2
    code_files3
    checkpoint_dir (对应参数model_dir，相对路径)
        model_files1
        model_files2
        model_files3
</code>

## 执行packdocker命令
### 1. 基于自定义的镜像打包模式
<code>

sudo python uai_tool.py packdocker self    --bimg_name BIMG_NAME
					   --pack_file_path PACK_FILE_PATH
					   --conf_file CONF_FILE
                                           --uhub_username UHUB_USERNAME
                                           --uhub_password UHUB_PASSWORD 
                                           --uhub_registry UHUB_REGISTRY
                                           --uhub_imagename UHUB_IMAGENAME
                                           [--in_uhost IN_UHOST]
				
</code>

 * 参数说明

| 参数 | 说明 | 是否必需 |
| ---- | ---- | -------- |
| bimg\_name | 打包镜像所需的基础镜像名称，含tag                                                       | 是     |
| pack\_file\_path  | 待打包文件所在路径（**相对路径**）                                                       | 是     |
| conf_file  | 部署服务所需的配置文件名称，含后缀名                                                       | 是     |
| uhub\_username    | uhub账号名称                                                               | 是     |
| uhub\_password    | uhub账号密码                                                               | 是     |
| uhub\_registry    | uhub镜像仓库名称，不支持大写字母                                                               | 是     |
| uhub\_imagename   | 生成的镜像名称，**含tag**，镜像名与tag间以冒号分割。如test:v1.1                                   | 是    |
| in\_uhost         | 优化参数。当前打包程序是否运行在UCloud云主机中，如果是则为yes，否则为no（默认）。（注：如果运行在云主机中，则可利用内网万兆带宽，加速镜像上传下载）  | 否     |

### 2. 基于深度学习框架的镜像打包模式
<code>
sudo python uai_tool.py packdocker   {tf,caffe,mxnet,keras}

​				     --public_key PUBLIC_KEY
​                           	     --private_key PRIVATE_KEY
​			             [--project_id PROJECT_ID]
​                               	     --pack_file_path PACK_FILE_PATH
​                          	     --main_module MAIN_MODULE 
​                                     --main_class MAIN_CLASS
​                                     --model_dir MODEL_DIR 
​                                     --uhub_username UHUB_USERNAME
​                                     --uhub_password UHUB_PASSWORD 
​                                     --uhub_registry UHUB_REGISTRY
​                                     --uhub_imagename UHUB_IMAGENAME
​                                     [--in_uhost IN_UHOST]
​                                     [--model_name MODEL_NAME]
​                                     [--all_one_file ALL_ONE_FILE]
​                                     [--model_arch_type MODEL_ARCH_TYPE]
​                                     [--num_epoch NUM_EPOCH]
​                                     [--ai_arch_v AI_ARCH_V]
​                  
</code>

**注：该操作涉及到docker的使用，建议sudo执行该指令。**

### 参数说明
1. 公共参数 

| 参数 | 说明 | 是否必需 |
| ---- | ---- | -------- |
| public\_key       | 用户的公钥                                                                            | 是     |
| private\_key      | 用户的私钥                                                                            | 是     |
| project\_id       | 项目ID                                                                               | 否     |
| pack\_file\_path  | 待打包文件所在路径（**相对路径**）                                                       | 是     |
| main\_module      | 包含主类的代码文件（不包含后缀名）                                                        | 是     |
| main\_class       | 主类名称                                                                             | 是     |
| model\_dir        | 模型文件的路径（**相对路径**）                                                          | 是     |
| uhub\_username    | uhub账号名称                                                               | 是     |
| uhub\_password    | uhub账号密码                                                               | 是     |
| uhub\_registry    | uhub镜像仓库名称，不支持大写字母                                                               | 是     |
| uhub\_imagename   | 生成的镜像名称，**含tag**，镜像名与tag间以冒号分割。如test:v1.1                                   | 是    |
| in\_uhost         | 优化参数。当前打包程序是否运行在UCloud云主机中，如果是则为yes，否则为no（默认）。（注：如果运行在云主机中，则可利用内网万兆带宽，加速镜像上传下载）  | 否     |
| ai\_arch\_v       | 可选参数。格式为"ai框架名称-版本号"，如tensorflow-1.4.0。如不填写，则默认选择系统支持                | 否     |


2. 其他参数（各框架另外所需参数）

| 参数名称 | 说明 | 是否必须 | 默认值 | 适用框架 |
| -------- | ---- | -------- | ------ | -------- |
| model\_name        | 模型名称                          | 是     | 无    | Caffe, Keras, MXNet  |
| all_one\_file      | 模型的框架及参数是否在同一文件中（true或false）  | 是     | 无    | Keras                |
| model\_arch\_type  | 模型框架文件后缀名                     | 否     | no   | Keras                |
| num\_epoch         | 保存模型文件时的epoch数                | 是     | 无    | MXNet                |

3. UAI Inference支持各版本明细 

| 深度学习框架名称 | 版本名称 | ai\_arch\_v参数填写 | 说明 |
| ---------------- | -------- | ------------------- | ---- |
| tensorflow  | 1.1.0        | tensorflow-1.1.0   |                                     |
| tensorflow  | 1.2.0        | tensorflow-1.2.0   |                                     |
| tensorflow  | 1.4.0        | tensorflow-1.4.0   |                                     |
| caffe       | 1.0.0        | caffe-1.0.0        |                                     |
| caffe       | intel 1.0.0  | 'caffe-intel 1.0.0'  | 填写时注意使用引号  |
| keras       | 1.2.0        | keras-1.2.0        |                                     |
| keras       | 2.0.8        | keras-2.0.8        |                                     |
| mxnet       | 0.9.5        | mxnet-0.9.5        |                                     |
## packdocker命令样例

### 1. 基于自定义的镜像打包模式 
<code>
sudo python uai_tool.py packdocker self --bimg_name=BIMG_NAME --pack_file_path=PACK_FILE_PATH --conf_file=CONF_FILE --uhub_username UHUB_USERNAME --uhub_password UHUB_PASSWORD --uhub_registry UHUB_REGISTRY --uhub_imagename UHUB_IMAGENAME
</code>

### 2. 基于深度学习框架的镜像打包模式 
**注：参数具体值根据实际修改** 
1. caffe
  <code>
  sudo python uai_tool.py packdocker caffe \

  ​     --public_key xxxxx  \
  ​     --private_key xxxxx  \
  ​     --main_class MnistModel  \
  ​     --main_module mnist_inference  \
  ​     --model_dir checkpoint_dir  \
  ​     --model_name mnist_model  \
  ​     --pack_file_path mnist_caffe \
  ​     --uhub_username xxxxx  \
  ​     --uhub_password xxxxx  \
  ​     --uhub_registry xxxxx  \
  ​     --uhub_imagename caffe-inference:test
  </code>

2. keras
   <code>
   sudo python uai_tool.py packdocker keras \

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
   ​        --uhub_imagename keras-inference:test
   </code>

3. mxnet
      <code>
      sudo python uai_tool.py packdocker mxnet \

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
      ​        --uhub_imagename mxnet-inference:test
      </code>

4. tensorflow
         <code>
         sudo python uai_tool.py packdocker tf \

     ​            --public_key xxxxx \
     ​            --private_key xxxxx  \
     ​            --main_class MnistModel \
     ​            --main_module mnist_inference \
     ​            --model_dir checkpoint_dir \
     ​            --pack_file_path /data/sdk-test/uai-sdk/tests/mnist_tf \
     ​            --uhub_username xxxxx \
     ​            --uhub_password xxxxx \
     ​            --uhub_registry xxxxx \
     ​            --uhub_imagename tf-inference:test
     ​    </code>

  * 输出说明
成功执行后，将返回如下信息：
<code>
upload docker images successful. images:uhub.ucloud.cn/uai_demo/keras-inference:test
</code>

