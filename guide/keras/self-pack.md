

# 自定义打包镜像

## 准备工作
安装UAI SDK

<code>
git clone https://github.com/ucloud/uai-sdk

cd uai-sdk
sudo python setup.py install
</code>

## 准备打包所需文件

### 准备代码以及模型文件
用户需将AI在线服务所需的**代码以及模型文件**放在某一路径下，参数pack\_file\_path上传。文件目录结构示例如下：

<code>
/xxx/xxx/xxx/

​    code/ (对应参数pack_file_path，绝对路径)
​        code_files1
​        code_files2
​        code_files3
​        checkpoint_dir (对应参数model_dir，相对路径)
​            model_files1
​            model_files2
​            model_files3
</code>

### 准备模块配置文件
用户同时需要准备conf.json 来告诉django server如何加载自定义的Inference模块，Keras的conf.json 结构如下：
<code>
{
    "http_server" : {

​        "exec" : {
​            "main_class": "MnistModel",
​            "main_file": "mnist_inference"
​        },
​        "keras" : {
​            "model_dir" : "./checkpoint_dir",
​            "model_name" : "mnist_model",
​            "all_one_file" : false,
​            "model_arch_type" : "json"
​        }
​    }
}
</code>

  * **exec.main\_class** 用于提示django server应该创建那个Inference 类对象
  * **exec.main\_file** 用于提示django server应该加载哪个inference python主模块
  * **keras.model\_dir**(可选) 对应了MnistModel中self.model\_dir的值，用于给Inference 类对象传递模型路径参数。
  * **keras.xxx**(可选) KerasAiUcloudModel会根据keras下model\_dir, model\_name, all\_one\_file, model\_arch\_type的参数拼接keras模型路径，详细请[参见](https://github.com/ucloud/uai-sdk/blob/master/uai/arch/keras_model.py)。

## 准备命令行工具
为后续操作方便，可将UAI SDK安装包中的命令行工具（$uai-sdk安装路径/uai\_tools/uai\_tool.py）拷贝到代码pack\_file\_path的同级目录
<code>
/xxx/xxx/xxx/

​    uai_tool.py (从$uai-sdk安装路径/uai_tools/uai_tool.py拷贝)
​    conf.json
​    code/ (对应参数pack_file_path，绝对路径)
​        code_files1
​        code_files2
​        code_files3
​        checkpoint_dir (对应参数model_dir，相对路径)
​            model_files1
​            model_files2
​            model_files3
</code>

## 执行packdocker命令
**注：参数具体值根据实际修改** 
<code>
python uai_tool.py packdocker self \

​        --bimg_name <base_image_name> \
​	--pack_file_path ./code \
​        --conf_file conf.json \
​        --uhub_username xxxxx \
​        --uhub_password xxxxx \
​        --uhub_registry xxxxx \
​        --uhub_imagename keras-inference:test \
​        --in_host no
</code>

### 公共参数说明

| 参数 | 说明 | 是否必需 |
| ---- | ---- | -------- |
| bimg\_name       | 用户指定的基础镜像名字                                                  | 是     |
| pack\_file\_path  | 待打包文件所在路径                                                           | 是     |
| conf\_file             | 模块配置文件conf.json的名字                                             | 是     |
| uhub\_username    | uhub镜像库登录用户名，即UCloud的账号邮箱                         | 是     |
| uhub\_password   |  uhub镜像库登录的密码，即UCloud的账号密码                          | 是     |
| uhub\_registry    | uhub镜像仓库名称，不支持大写字母                                                               | 是     |
| uhub\_imagename   | 生成的镜像名称，镜像名与tag间以冒号分割。如test:v1.1                                                 | 是     |
| in\_uhost         | 优化参数。当前打包程序是否运行在UCloud云主机中，如果是则为yes，否则为no（默认）。（注：如果运行在云主机中，则可利用内网万兆带宽，加速镜像上传下载）  | 否     |

### UAI-Inference支持的基础镜像列表
请参见[](ai/uai-inference/general/dockers)中Keras基础镜像列表。

## 输出结果
<code>
upload docker images successful. images:uhub.ucloud.cn/uai_demo/keras-inference:test
</code>