

# pack
## 命令作用
将用户本地代码打包成一个tar包并上传到UCloud Ufile产品中，该tar包可以在控制台界面使用**“代码模式”**部署版本（操作参见[](ai/uai-inference/use/oplist/deploy)） 
注意：该命令与tar命令区别是它不但会在本地生成tar包，还会上传到UCloud的Ufile中

## 准备工作
### 1. 安装UCloud UFile SDK  

<code>
wget http:sdk.ufile.ucloud.com.cn/python_sdk.tar.gz
tar

 zxvf python_sdk.tar.gz
cd ufile-python
sudo python setup.py install
</code>
**注：UFile SDK仅兼容request 2.10.0以下版本**

## 2. 安装UAI SDK

<code>
git clone https:github.com/ucloud/uai-sdk

cd uai-sdk
sudo python setup.py install
</code>

## 3. 获取用户公钥和私钥 

  * 登陆UCloud官网，进入Console控制台
  * 点击左上角的“产品与服务”选项，选择“监控管理”列表下的“API密钥 UAPI”选项后，点击API密钥中的“显示”选项，按照提示获取用户的公钥和私钥。

## 准备打包所需文件
用户需将AI在线服务所需的**代码以及模型文件**放在某一路径下，在部署时将该路径作为参数pack\_file\_path上传。文件目录结构示例如下：

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

file_dir(对应参数pack_file_path，相对路径)
    code_files1
    code_files2
    code_files3
    checkpoint_dir (对应参数model_dir，相对路径)
        model_files1
        model_files2
        model_files3
</code>

## 执行pack命令
<code>
python uai_tool.py pack    {caffe,keras,mxnet,tf}


​			   --public_key PUBLIC_KEY
​                           --private_key PRIVATE_KEY
​			   [--project_id PROJECT_ID]
​                           --bucket BUCKET
​                           --pack_file_path PACK_FILE_PATH
​                           --upload_name UPLOAD_NAME
​                           --main_module MAIN_MODULE 
​                           --main_class MAIN_CLASS
​                           --model_dir MODEL_DIR 
​                           --code_files CODE_FILES
​                           [--model_name MODEL_NAME]
​                           [--all_one_file ALL_ONE_FILE]
​                           [--model_arch_type MODEL_ARCH_TYPE]
​                           [--num_epoch NUM_EPOCH]
</code>

  ###  参数说明
1. 公共参数

| 参数 | 说明 | 是否必需 |
| ---- | ---- | -------- |
| public\_key       | 用户的公钥                                  | 是     |
| private\_key      | 用户的私钥                                  | 是     |
| project\_id       | 项目ID                                   | 否     |
| bucket            | 用户对象存储空间域名（Ufile Bucket）               | 是     |
| pack\_file\_path  | 待打包文件所在路径（**相对路径**）                  | 是     |
| upload\_name      | 上传的tar文件名                              | 是     |
| main\_module        | 包含主类的代码文件（不包含后缀名）                      | 是     |
| main\_class       | 主类名称                                   | 是     |
| model\_dir        | 模型文件的路径（**相对路径**）                          | 是     |
| code\_files       | 所需代码文件，需包含文件后缀名（若有多个，请用','隔开，中间不包含空格）  | 是     |

2. 其他参数（各框架另外所需参数）

| 参数名称 | 说明 | 是否必须 | 默认值 | 适用框架 |
| -------- | ---- | -------- | ------ | -------- |
| model\_name        | 模型名称                          | 是     | 无       | Caffe, Keras, MXNet  |
| all_one\_file      | 模型的框架及参数是否在同一文件中（true或false）  | 是     | false   | Keras                |
| model\_arch\_type  | 模型框架文件后缀名                     | 否     | 'json'  | Keras                |
| num\_epoch         | 保存模型文件时的epoch数                | 是     | 无       | MXNet                |

## pack命令样例
1）caffe
<code>
python uai_tool.py pack caffe --public_key=PUBLIC_KEY --private_key=PRIVATE_KEY --bucket=UFILE_BUCKET --pack_file_path=/PACK/FILE/PATH --upload_name=UPLOAD_NAME.tar --main_module=MAIN_MODULE --main_class=MAIN_CLASS --model_dir=MODEL_DIR --code_files=CODE_FILES1,CODE_FILES2,CODE_FILES3 --model_name=MODEL_NAME
</code>
2）keras
<code>
python uai_tool.py pack keras --public_key=PUBLIC_KEY --private_key=PRIVATE_KEY --bucket=UFILE_BUCKET --pack_file_path=/PACK/FILE/PATH --upload_name=UPLOAD_NAME.tar --main_module=MAIN_MODULE --main_class=MAIN_CLASS --model_dir=MODEL_DIR --code_files=CODE_FILES1,CODE_FILES2,CODE_FILES3 --model_name=MODEL_NAME --all_one_file=ALL_ONE_FILE --model_arch_type=MODEL_ARCH_TYPE
</code>
3）mxnet
<code>
python uai_tool.py pack mxnet --public_key=PUBLIC_KEY --private_key=PRIVATE_KEY --bucket=UFILE_BUCKET --pack_file_path=/PACK/FILE/PATH --upload_name=UPLOAD_NAME.tar --main_module=MAIN_MODULE --main_class=MAIN_CLASS --model_dir=MODEL_DIR --code_files=CODE_FILES1,CODE_FILES2,CODE_FILES3 --model_name=MODEL_NAME --num_epoch=NUM_EPOCH
</code>
4）tensorflow
<code>
python uai_tool.py pack tf --public_key=PUBLIC_KEY --private_key=PRIVATE_KEY --bucket=UFILE_BUCKET --pack_file_path=/PACK/FILE/PATH --upload_name=UPLOAD_NAME.tar --main_module=MAIN_MODULE --main_class=MAIN_CLASS --model_dir=MODEL_DIR --code_files=CODE_FILES1,CODE_FILES2,CODE_FILES3
</code>
  * 输出说明
成功执行后，将返回如下信息：
<code>
upload local file :PACK_FILE_PATH/UPLOAD_NAME.tar to ufile key=UPLOAD_NAME.tar successful
</code>

