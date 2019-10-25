

# 制作Mnist在线服务镜像
接下来我们需要使用UAI-SDK提供的打包工具来打包Mnist的训练镜像。

## 前期准备
我们还需要准备

  * 代码
  * 模型文件

### 准备代码
我们再将mnist\_inference.py放入/data/mnist/目录下：

<code>
$ cd /data/mnist

$ cp cp ~/uai-sdk/examples/tensorflow/inference/mnist_1.1/mnist_inference.py ./
</code>

得到的目录情况如下：
<code>
/data/mnist

|_ mnist_inference.py
</code>

### 模型文件
我们可以使用[[ai:uai-train:tutorial:tf-mnist]]中训练出来的mnist模型文件，我们也可以自己训练一个新的，当然我们的github也提供了训练好的模型[[https://github.com/ucloud/uai-sdk/tree/master/examples/tensorflow/inference/mnist_1.1/checkpoint_dir]] 

我们把模型文件放入/data/mnist/目录下：
<code>
/data/mnist

|_ mnist_inference.py
|_ checkpoint_dir
|  |_ checkpoint
|  |_ mnist.mod.data-00000-of-00001
|  |_ mnist.mod.index
|  |_ mnist.mod.meta
</code>

### mnist.conf
该config会由系统自动生成

## 打包在线服务镜像
TensorFlow镜像打包工具为 uai-sdk/uai\_tools/uai\_tool.py，我们将该工具也放入/data/mnist。
<code>
$ cd /data/mnist

$ cp ~/uai-sdk/uai_tools/uai_tool.py ./
</code>

整个目录结构如下：
<code>
/data/mnist

|_ mnist_inference.py
|_ checkpoint_dir
|  |_ checkpoint
|  |_ mnist.mod.data-00000-of-00001
|  |_ mnist.mod.index
|  |_ mnist.mod.meta
|_ uai_tool.py
</code>

## 打包Mnist镜像
我们使用uai\_tool.py 打包mnist镜像，具体的参数说明在[[ai:uai-inference:guide:tensorflow:pack]]。

我们用如下命令来打包：
<code>
python uai_tool.py packdocker tf \

​        --public_key <YOUR_PUBLIC_KEY> \
​	--private_key <YOUR_PRIVATE_KEY> \
​        --main_class <MAIN_INFERENCE_CLASS> \
​        --main_module <MAIN_INFERENCE_MODULE> \
​        --model_dir <MODEL_DIR> \
​        --pack_file_path <TAP_PACK_PATH> \
​	--uhub_username <YOUR_UHUB_USER_NAME> \
​	--uhub_password <YOUR_UHUB_PASSWORD> \
​	--uhub_registry <YOUR_UHUB_REFDISTRY> \
​	--uhub_imagename <YOUR_UHUB_IMAGENAME> \
​        --ai_arch_v=<TF_ARCH> \
​        --internal_uhub <false/true> \
</code>

### public_key & private_key
这里的公私钥是UCloud用户账号的唯一标识，可以依据[[ai:uai-train:base:key]]的方法获取你账号的公私钥参数

### main_class
推理服务的对象类，本案例为MnistModel 

### main_module
入口python模块，本案例为mnist\_inference.py

### model_dir
模型文件的存放目录，本案例为 ./checkpoint_dir/，打包工具会将模型文件夹自动放入: /ai-ucloud-client-django/ 目录下。

### pack_file_path
待打包文件所在路径，本案例为./code/

### uhub_username, uhub_password, uhub_registry
uhub的用户名密码为UCloud console图形界面登录时所用的邮箱和密码。而uhub\_registry就是uhub镜像库的名字，在本案例中为 uai\_demo

### uhub_imagename
自定义的docker镜像名字，在本案例中我们使用tf-mnist-infer

### ai_arch_v
在线服务镜像版本，这里我们选择tensorflow-1.1.0，这样就可以以tensorflow-1.1.0为基础镜像打包mnist的训练镜像。其他TensorFlow 基础镜像选择方法请参见[[ai:uai-train:guide:tensorflow:packing]]

### internal_uhub
如果是使用ucloud云主机操作打包工具，则选择false，如果是使用公网执行打包操作，则选择true

### 执行打包Mnist镜像命令
我们执行如下命令就可以生成mnist在线服务镜像
<code>
$ sudo python uai_tool.py packdocker tf  \

​                        --public_key <YOUR_PUBLIC_KEY> \
​			--private_key <YOUR_PRIVATE_KEY> \
​			--main_class MnistModel \
​        		--main_module mnist_inference \
​        		--model_dir checkpoint_dir/ \
​        		--pack_file_path ./code/ \
​			--uhub_username <YOUR_UHUB_USER_NAME> \
​			--uhub_password <YOUR_UHUB_PASSWORD> \
​			--uhub_registry uai_demo \
​			--uhub_imagename tf-mnist-infer \
​			--ai_arch_v=tensorflow-1.1.0 \
​                        --in_host no \
</code>

最终我们将获得镜像：uhub.service.ucloud.cn/uai_demo/tf-mnist-infer:latest

