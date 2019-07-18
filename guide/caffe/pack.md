{{indexmenu_n>6}}

===== Caffe 打包镜像说明 =====
==== 准备工作 ====
1）安装UAI SDK

<code>
git clone https://github.com/ucloud/uai-sdk
cd uai-sdk
sudo python setup.py install
</code>

3）获取用户公钥和私钥 

[[ai:uai-inference:base:key]]
  * 登陆UCLOUD官方网站，进入Console页面：[[https://console.ucloud.cn/dashboard|https://console.ucloud.cn/dashboard]]
  * 点击左上角的“产品与服务”选项，选择“监控管理”列表下的“API密钥 UAPI”选项后，点击API密钥中的“显示”选项，按照提示获取用户的公钥和私钥。


==== 准备打包所需文件 ====
用户需将AI在线服务所需的**代码以及模型文件**放在某一路径下，参数pack\_file\_path上传。文件目录结构示例如下：

<code>
／xxx/xxx/xxx/code (对应参数pack_file_path，绝对路径)
    code_files1
    code_files2
    code_files3
    checkpoint_dir (对应参数model_dir，相对路径)
        model_files1
        model_files2
        model_files3
</code>

==== 准备命令行工具 ====
为后续操作方便，可将UAI SDK安装包中的命令行工具（$uai-sdk安装路径/uai\_tools/uai\_tool.py）拷贝到代码pack\_file\_path的同级目录
<code>
uai_tool.py (从$uai-sdk安装路径/uai_tools/uai_tool.py拷贝）
 /xxx/xxx/xxx/code (对应参数pack_file_path，绝对路径)
    code_files1
    code_files2
    code_files3
    checkpoint_dir (对应参数model_dir，相对路径)
        model_files1
        model_files2
        model_files3
</code>

==== 执行packdocker命令 ====
**注：参数具体值根据实际修改** \\
<code>
python uai_tool.py packdocker caffe \
        --public_key xxxxx  \
        --private_key xxxxx  \
        --main_class MnistModel  \
        --main_module mnist_inference  \
        --model_dir checkpoint_dir  \
        --model_name mnist_model  \
        --pack_file_path ./code \
        --uhub_username xxxxx  \
        --uhub_password xxxxx  \
        --uhub_registry xxxxx  \
        --uhub_imagename caffe-inference:test \
        --ai_arch_v 'caffe-1.0.0' \
        --in_host no \
        --model_name mnist_model
</code>

  * 参数说明\\
1) 公共参数 \\

^ 参数                ^ 说明                                                                               ^ 是否必需  ^
| public\_key       | 用户的公钥                                                                            | 是     |
| private\_key      | 用户的私钥                                                                            | 是     |
| project\_id       | 项目ID                                                                             | 否     |
| pack\_file\_path  | 待打包文件所在路径                                                           | 是     |
| main\_module      | 包含主类的代码文件（不包含后缀名）                                                                | 是     |
| main\_class       | 主类名称                                                                             | 是     |
| model\_dir        | 模型文件的路径（相对路径）                                                                    | 是     |
| uhub\_username    | uhub镜像库登录用户名，即UCloud的账号邮箱                         | 是     |
| uhub\_password   |  uhub镜像库登录的密码，即UCloud的账号密码                          | 是     |
| uhub\_registry    | uhub镜像仓库名称，不支持大写字母                                                               | 是     |
| uhub\_imagename   | 生成的镜像名称，镜像名与tag间以冒号分割。如test:v1.1                                                 | 是     |
| ai\_arch\_v       | 可选参数。格式为"ai框架名称-版本号"，如caffe-1.0.0。如不填写，则默认选择系统支持的一个版本                       | 否     |
| in\_uhost         | 优化参数。当前打包程序是否运行在UCloud云主机中，如果是则为yes，否则为no（默认）。（注：如果运行在云主机中，则可利用内网万兆带宽，加速镜像上传下载）  | 否     |
| model\_name        | 模型名称                                                                             | 是     |

**注：打包工具会在pack\_file\_path下面生成一个 .conf 文件，该文件是mnist inference模块加载的配置文件，该文件会连同其他文件一起被打包进在线服务镜像。**\\
**注2: 打包工具会自动生成一个名为uaiservice.Dockerfile的文件，描述打包操作是如何执行的**

=== UAI Inference支持各版本明细 ===

^ 深度学习框架名称  ^ 版本名称         ^ ai\_arch\_v参数填写    ^ 说明         ^
| caffe     | 1.0.0        | caffe-1.0.0        |            |
| caffe     | 1.0.0        | caffe-1.0.0gpu      |     GPU   |
| caffe     | intel 1.0.0  | caffe-intel 1.0.0  | 填写时注意使用引号  |

==== 输出结果 ====
<code>
upload docker images successful. images:uhub.ucloud.cn/uai_demo/caffe-inference:test
</code>