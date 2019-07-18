{{indexmenu_n>30}}

====== Keras MNIST 案例 ======
本案例所使用的模型和代码基于Keras教程MNIST案例（https://keras.io/getting-started/sequential-model-guide/#examples），您可以在https://github.com/ucloud/uai-sdk/blob/master/examples/keras/mnist/下面下载完整的代码、训练好的模型以及一张用户测试的图片.

注：本案例基于Keras-1.2.2版本实现，如果使用其他版本的Keras运行该案例，您可能需要对代码进行微调。

===== 准备工作 =====
请根据 开发指南>Keras开发指南> Keras 本地安装部署开发环境[[ai:uai-inference:guide:keras:local]]完成第1至第5步的安装，完成基本环境的部署。\\

然后安装图片识别相关的PIL库，安装方法入下：\\
<code>
sudo apt-get install libtiff5-dev libjpeg8-dev zlib1g-dev libfreetype6-dev liblcms2-dev libwebp-dev tcl8.6-dev tk8.6-dev python-tk
sudo pip install Pillow
</code>
此时您就可以着手开发您自己的MNIST在线inference服务了。

===== 编写MNIST案例 =====
本案例使用的MNIST模型的训练代码来自Keras的MNIST案例，您可以在https://github.com/kurapan/CNN-MNIST/blob/master/src/mnist_keras.py 找到源码，并可以用该程序训练新的MNIST模型。\\

下面我们将逐步介绍如何编写MNIST在线inference服务代码，完整的代码你可以访问https://github.com/ucloud/uai-sdk/blob/master/examples/keras/inference/mnist/mnist_inference.py

=== 创建MnistModel类 ===

<code>
from uai.arch.keras_model import KerasAiUcloudModel

class MnistModel(KerasAiUcloudModel):
  def __init__(self, conf):
   super(MnistModel, self).__init__(conf)

  #模型加载函数，在初始化MnistModel类，执行__init__时会被自动调用。
  def load_model(self):

  #inference 执行函数
  def execute(self, data, batch_size):
</code>

=== 编写load_model函数 ===
加载好模型(从self.model\_arc\_file获取json描述文件路径，从self.model\_weight\_file获取权重文件路径

<code>
  def load_model(self):
    model_json_file = open(self.model_arc_file, 'r')
    model_json = model_json_file.read()
    model_json_file.close()
    model_json = model_from_json(model_json)
    model_json.load_weights(self.model_weight_file)
    return model_json
</code>

=== 编写execute函数 ===
UAI http server在收到请求后会调用execute()函数来执行模型的inference逻辑，execute()函数接收两个参数并有一个返回值：\\
- **入参**：\\
**data**，为一个数组类型，数组中对象的个数为batch\_size，data 数组中的每一个对象对应一个外部请求的输入 \\
**batch\_size**，表示data数值中对象的个数 \\
- **返回值**：\\
反回值必须也是一个数组类型，大小同样为batch\_size。该数组中的每一个对象都代表一个请求的返回值。\\

注：请保证返回的对象和入参data中的对象是一一对应关系。

**需要注意的是通过model.predict 返回的参数为numpy对象格式，该格式并不能被http server的框架所识别，因此需要镜像特殊转换，我们可以使用numpy.array\_str(value)的方式将numpy对象转换成普通的string对象**

<code>
def execute(self, data, batch_size):
    sess = self.output['sess']
    x = self.output['x']
    y_ = self.output['y_']

    imgs = []
    for i in range(batch_size):
      im = Image.open(data[i]).resize((28, 28)).convert('L')
      im = np.array(im)
      im = im.reshape(784)
      im = im.astype(np.float32)
      im = np.multiply(im, 1.0 / 255.0)
      imgs.append(im)

    imgs = np.array(imgs)
    predict_values = sess.run(y_, feed_dict={x: imgs})
    print(predict_values)

    ret = []
    for val in predict_values:
      ret_val = np.array_str(np.argmax(val)) + '\n'
      ret.append(ret_val)
    return ret
</code>

===== 测试MNIST案例 =====
以下步骤依据[[ai:uai-inference:guide:keras:test|]]进行。

==== 镜像模式 ====
假设将uai-sdk至于/data目录下，
<code>
cd /data/uai-sdk
cp uai_tools/uai_tool.py ./
cp examples/keras/inference/mnist/2.jpg ./  [该图片用于测试]
</code>
=== 1.打包镜像 ===
<code>
python uai_tool.py packdocker keras \
--public_key <您的公钥>  \
--private_key <您的私钥>  \
--main_class MnistModel  \
--main_module mnist_inference  \
--model_dir checkpoint_dir  \
--model_name mnist_model  \
--pack_file_path ./examples/keras/inference/mnist/  \
--uhub_username <uhub账号> \
--uhub_password <uhub密码> \
--uhub_registry <uhub中的registry> \
--uhub_imagename keras-inference:test
</code>
执行完该命令后，会在本地生成docker镜像，名称为：uhub.ucloud.cn/xxxxx/keras-inference:test（xxxxx为上述命令中的uhub_registry）。\\
同时会上传至您的Uhub镜像库中，本地测试完毕后可直接进行在线部署。\\

=== 2.运行本地服务 ===
镜像准备完成后，可在本地运行docker中的服务。\\
<code>
sudo docker run -d --net=bridge --name=uai_inference_test -p 8080:8080 uhub.ucloud.cn/xxxxx/keras-inference:test
</code>

=== 3.发送Http 测试请求 ===
<code>
curl -X POST http://localhost:8080/service -T 2.jpg
</code>

==== 代码模式 ====
=== 1.拷贝代码和模型 ===
在https://github.com/ucloud/uai-sdk-httpserver上下载uai-sdk-httpserver项目，进入该项目，我们开始正式部署MNIST案例：
<code>
cd uai-sdk-httpserver
cp YOUR_CODE_PATH/mnist_inference.py ./
cp -r YOUR_MODEL_PATH/checkpoint_dir ./
</code>

=== 2.编写keras_mnist.conf ===

<code>
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
            "model_arc_type" : "json"
        }
    }
}
</code>

=== 3.准备就绪 ===
所有准备工作已经就绪，uai-sdk-httpserver应该包含如下内容：
<code>
> ls uai-sdk-httpserver
> http_server.py inference.py server.py checkpoint_dir/ mnist_inference.py keras_mnist.conf
</code>

=== 4.执行Http Server ===

<code>
python server.py --port=8080 --json_conf="keras_mnist.conf"
</code>

=== Ready to Service ===

=== 测试本地服务 ===
在https://github.com/ucloud/uai-sdk/tree/master/examples/keras/mnist 下面有一张2.jpg的图片用户测试，将其放到uai-sdk-httpserver/目录下

<code>
> curl -X POST http://localhost:8080/service -T uai-sdk-httpserver/2.jpg
> 2
</code>