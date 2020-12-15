

# tar
## 命令作用
将用户本地代码打包成一个tar包，该tar包可以在控制台界面使用**“代码模式”**[部署版本](uai-inference/use/oplist/deploy) 
注意：该命令与pack命令区别是它仅在本地打包，不会将tar包上传到UCloud的US3中

## 准备工作
### 1. 安装UCloud US3 SDK  

<code>
wget http://sdk.ufile.ucloud.com.cn/python_sdk.tar.gz

tar zxvf python_sdk.tar.gz
cd ufile-python
sudo python setup.py install
</code>
**注：US3 SDK仅兼容request 2.10.0以下版本**

### 2. 安装UAI SDK

<code>
git clone https://github.com/ucloud/uai-sdk

cd uai-sdk
sudo python setup.py install
</code>

## 准备打包所需文件
用户需将AI在线服务所需的**代码以及模型文件**放在某一路径下，用参数pack\_file\_path指定。打包成功后，会在该路径下生成tar文件。目录结构示例如下：

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

## 执行tar命令
<code>
python uai_tool.py tar     {caffe,keras,mxnet,tf}

​                           --pack_file_path PACK_FILE_PATH
​                           --tar_name UPLOAD_NAME
​                           --main_module MAIN_MODULE 
​                           --main_class MAIN_CLASS
​                           --model_dir MODEL_DIR 
​                           --code_files CODE_FILES
​                           [--model_name MODEL_NAME]
​                           [--all_one_file ALL_ONE_FILE]
​                           [--model_arch_type MODEL_ARCH_TYPE]
</code>

### 参数说明
1. 公共参数 

| 参数 | 说明 | 是否必需 |
| ---- | ---- | -------- |
| pack\_file\_path  | 待打包文件所在路径（**相对路径**）                  | 是     |
| tar\_name         | 生成的tar文件名（如mnist.tar）                  | 是     |
| main\_module      | 包含主类的代码文件（不包含后缀名）                      | 是     |
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

## tar命令样例
### 1. caffe
<code>
python uai_tool.py tar caffe \

--main_class MnistModel  \
--main_module mnist_inference  \
--model_dir checkpoint_dir  \
--code_files mnist_inference.py  \
--model_name mnist_model  \
--pack_file_path mnist_caffe \
--tar_name mnist_caffe.tar 
</code>

### 2. keras
<code>
python uai_tool.py tar keras \

--main_class MnistModel  \
--main_module mnist_inference  \
--model_dir checkpoint_dir  \
--code_files mnist_inference.py  \
--model_name mnist_model  \
--pack_file_path mnist_keras \
--tar_name mnist_keras.tar
</code>

### 3. mxnet
<code>
python uai_tool.py tar mxnet \

--main_class MnistModel  \
--main_module mnist_inference  \
--model_dir checkpoint_dir  \
--code_files mnist_inference.py  \
--model_name mnist-model  \
--num_epoch 10 \
--pack_file_path mnist_mxnet  \
--tar_name mnist_mxnet.tar
</code>

### 4. tensorflow
<code>
python uai_tool.py tar tf \

--main_class MnistModel \
--main_module mnist_inference \
--model_dir checkpoint_dir \
--code_files mnist_inference.py \
--pack_file_path tests/mnist_tf \
--tar_name mnist_tf.tar
</code>

  * 输出说明
成功执行后，将返回如下信息：
<code>
Finish packing the files.
</code>
成功执行后，会在参数pack\_file\_path所指定的路径下生成tar文件。