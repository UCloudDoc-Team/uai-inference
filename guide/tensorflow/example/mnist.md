{{indexmenu_n>10}}

====== TensorFlow MNIST 案例 ======
本案例所使用的模型和代码基于Tensorflow 教程MNIST案例（https://www.tensorflow.org/tutorials/mnist/beginners/），您可以在https://github.com/ucloud/uai-sdk/tree/master/examples/tensorflow/inference/下面下载完整的代码、训练好的模型以及一张用于测试的图片。 包括两个Case Study： \\
  * TensorFlow-1.1： https://github.com/ucloud/uai-sdk/tree/master/examples/tensorflow/inference/mnist_1.1 \\
  * TensorFlow-0.11: https://github.com/ucloud/uai-sdk/tree/master/examples/tensorflow/inference/mnist_0.11 \\
注：本案例基于TensorFlow-1.1版本实现，如果使用其他版本的Tensorflow运行该案例，您可能需要对代码进行微调。

===== 准备工作 =====
请根据 开发指南>Tensorflow开发指南>Tensorflow 本地安装部署开发环境[[ai:uai-inference:guide:tensorflow:local]]完成第1至第5步的安装，完成基本环境的部署\\
然后安装图片识别相关的PIL库，安装方法入下：\\
<code>
sudo apt-get install libtiff5-dev libjpeg8-dev zlib1g-dev libfreetype6-dev liblcms2-dev libwebp-dev tcl8.6-dev tk8.6-dev python-tk
sudo pip install Pillow
</code>
此时您就可以着手开发您自己的MNIST在线inference服务了。

===== 编写MNIST案例 =====
本案例使用的MNIST模型的训练代码来自Tensorflow的MNIST案例，您可以在https://github.com/tensorflow/tensorflow/blob/r1.1/tensorflow/examples/tutorials/mnist/mnist_softmax.py 找到源码，并可以用该程序训练新的MNIST模型。\\
下面我们将逐步介绍如何编写MNIST在线inference服务代码，完整的代码你可以访问https://github.com/ucloud/uai-sdk/blob/master/examples/tensorflow/inference/mnist_1.1/mnist_inference.py

=== 创建MnistModel类 ===

<code>
#导入 uai.arch.tf_model.TFAiUcloudModel 基准类（Tensorflow框架基准类）
from uai.arch.tf_model import TFAiUcloudModel

class MnistModel(TFAiUcloudModel):
  def __init__(self, model_dir):
    super(MnistModel, self).__init__(model_dir)

  #模型加载函数，在初始化MnistModel类，执行__init__时会被自动调用。
  def load_model(self):

  #inference 执行函数
  def execute(self, data, batch_size):
</code>

=== 编写load_model函数 ===
定义MNIST模型的网络结构，并从self.model\_dir目录加载模型 \\

<code>
 def load_model(self):  
       sess = tf.Session()


    """Define MNIST net
       y = x * W + b
       y_ = softmax(y)
    """
    x = tf.placeholder(tf.float32, [None, 784])
    W = tf.Variable(tf.zeros([784, 10]))
    b = tf.Variable(tf.zeros([10]))
    y = tf.matmul(x, W) + b
    y_ = tf.nn.softmax(y)


    """
       Load model from self.model_dir
       The default DIR name is checkpoint_dir/, it should include following files:
         checkpoint: tf checkpoint config file
         model.mod: model file
         model.mod.meta: model meta data file
    """
    saver = tf.train.Saver()
    params_file = tf.train.latest_checkpoint(self.model_dir)
    saver.restore(sess, params_file)


    """
       Register ops into self.output dict.
       So func execute() can get these ops
    """
    self.output['sess'] = sess
    self.output['x'] = x
    self.output['y_'] = y_
</code>

=== 编写execute函数 ===
UAI http server在收到请求后会调用execute()函数来执行模型的inference逻辑，execute()函数接收两个参数并有一个返回值：\\
- **入参**：\\
**data**，为一个数组类型，数组中对象的个数为batch\_size，data 数组中的每一个对象对应一个外部请求的输入\\
**batch\_size**，表示data数值中对象的个数 \\
- **返回值**：\\
反回值必须也是一个数组类型，大小同样为batch\_size。该数组中的每一个对象都代表一个请求的返回值。\\

注：请保证返回的对象和入参data中的对象是一一对应关系。

**需要注意的是通过sess.run返回的参数为numpy对象格式，该格式并不能被http server的框架所识别，因此需要镜像特殊转换，我们可以使用numpy.array\_str(value)的方式将numpy对象转换成普通的string对象**

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
依据[[ai:uai-inference:guide:tensorflow:test|]]进行。
==== 镜像模式 ====
假设将uai-sdk至于/data目录下，
<code>
cd /data/uai-sdk
cp uai_tools/uai_tool.py ./
cp examples/tensorflow/inference/mnist_1.1/2.jpg ./  [该图片用于测试]
</code>
=== 1.打包镜像 ===
<code>
python uai_tool.py packdocker tf \
--public_key <您的公钥>  \
--private_key <您的私钥>  \
--main_class MnistModel \
--main_module mnist_inference \
--model_dir checkpoint_dir \
--pack_file_path ./examples/tensorflow/inference/mnist_1.1/ \
--uhub_username <uhub账号> \
--uhub_password <uhub密码> \
--uhub_registry <uhub中的registry> \
--uhub_imagename tf-inference:test
</code>
执行完该命令后，会在本地生成docker镜像，名称为：uhub.ucloud.cn/xxxxx/tf-inference:test（xxxxx为上述命令中的uhub_registry）。\\
同时会上传至您的Uhub镜像库中，本地测试完毕后可直接进行在线部署。\\

=== 2.运行本地服务 ===
镜像准备完成后，可在本地运行docker中的服务。\\
<code>
sudo docker run -d --net=bridge --name=uai_inference_test -p 8080:8080 uhub.ucloud.cn/xxxxx/tf-inference:test
</code>

=== 3.发送Http 测试请求 ===
<code>
curl -X POST http://localhost:8080/service -T 2.jpg
</code>

==== 代码模式 ====
=== 拷贝代码和模型 ===
在https://github.com/ucloud/uai-sdk-httpserver上下载uai-sdk-httpserver项目，进入该项目，我们开始正式部署MNIST案例：
<code>
cd uai-sdk-httpserver
cp YOUR_CODE_PATH/mnist_inference.py ./
cp -r YOUR_MODEL_PATH/checkpoint_dir ./
</code>

=== 编写tf_mnist.conf ===

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

=== 准备就绪 ===
所有准备工作已经就绪，uai-sdk-httpserver应该包含如下内容：
<code>
> ls uai-sdk-httpserver
> http_server.py inference.py server.py checkpoint_dir/ mnist_inference.py tf_mnist.conf
</code>

=== 执行Http Server ===

<code>
python server.py --port=8080 --json_conf="tf_mnist.conf" 
</code>

=== Ready to Service ===

=== 测试本地服务 ===
在https://github.com/ucloud/uai-sdk/tree/master/examples/tensorflow/inference/mnist_1.1下面有一张2.jpg的图片用户测试，将其放到uai-sdk-httpserver/目录下

<code>
> curl -X POST http://localhost:8080/service -T uai-sdk-httpserver/2.jpg
> 2
</code>