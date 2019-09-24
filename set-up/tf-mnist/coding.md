{{indexmenu_n>7}}

# 在线服务代码简介
本代码基于TensorFlow 1.1 的mnist with summaries例子实现在线推理服务[[https://github.com/tensorflow/tensorflow/blob/v1.1.0/tensorflow/examples/tutorials/mnist/mnist_with_summaries.py]]。

完整的推理服务代码位于[[https://github.com/ucloud/uai-sdk/tree/master/examples/tensorflow/inference/mnist_1.1]]，推理服务的代码为mnist\_inference.py，我们同时提供了conf.json和模型checkpoint\_dir

## mnist_inference.py
 minst\_inference.py 实现了load\_model和execute两个函数。

### 创建 MnistModel 类
minst\_inference.py首先需要实现一个在线服务的类，该类继承了TFAiUcloudModel（TensorFlow 在线服务基类）
<code>
"""A very simple MNIST inferencer.
"""
from __future__ import absolute_import
from __future__ import division
from __future__ import print_function


from PIL import Image
import numpy as np
import tensorflow as tf

from uai.arch.tf_model import TFAiUcloudModel

class MnistModel(TFAiUcloudModel):
  """Mnist example_tf model
  """

  def __init__(self, conf):
    super(MnistModel, self).__init__(conf)
</code>

### 实现load\_model
load\_model实现分为三个部分：

  * 创建graph
  * 使用tf.train.Saver() 加载模型，模型目录地址可以从**self.model\_dir**获取，该变量由TFAiUcloudModel实现，并在初始化时从conf.json中获取
  * 将执行推理所需的sess、x、y\_ 三个变量保存到MnistModel.output全局变量中
<code>
    def load_model(self):
    sess = tf.Session()

    """ 1
       Define MNIST net
       y = x * W + b
       y_ = softmax(y)
    """
    x = tf.placeholder(tf.float32, [None, 784])
    W = tf.Variable(tf.zeros([784, 10]))
    b = tf.Variable(tf.zeros([10]))
    y = tf.matmul(x, W) + b
    y_ = tf.nn.softmax(y)

    """ 2
       Load model from self.model_dir
       The default DIR name is checkpoint_dir/, it should include following files:
         checkpoint: tf checkpoint config file
         model.mod: model file
         model.mod.meta: model meta data file
    """
    saver = tf.train.Saver()
    params_file = tf.train.latest_checkpoint(self.model_dir)
    saver.restore(sess, params_file)

    """ 3
       Register ops into self.output dict.
       So func execute() can get these ops
    """
    self.output['sess'] = sess
    self.output['x'] = x
    self.output['y_'] = y_
</code>

### 实现execute
实现execute实现分为四个部分：

  * 从MnistModel.output全局变量中获取sess、x、y\_ 三个变量
  * 从data获取batching的请求数据，并转化为numpy list（我们将所有的请求batch成了一个矩阵imgs)
  * 请求推理操作：predict\_values = sess.run(y\_, feed_dict={x: imgs})
  * 将请求结果转化成string，并合并成results（results也是一个list，和data list是一一对应的关系）

<code>
  def execute(self, data, batch_size):
    """ 1
    """
    sess = self.output['sess']
    x = self.output['x']
    y_ = self.output['y_']

    """ 2
    """
    imgs = []
    for i in range(batch_size):
      im = Image.open(data[i]).resize((28, 28)).convert('L')
      im = np.array(im)
      im = im.reshape(784)
      im = im.astype(np.float32)
      im = np.multiply(im, 1.0 / 255.0)
      imgs.append(im)
    
    """ 3
    """
    imgs = np.array(imgs)
    predict_values = sess.run(y_, feed_dict={x: imgs})
    print(predict_values)
    
    """ 4
    """
    ret = []
    for val in predict_values:
      ret_val = np.array_str(np.argmax(val)) + '\n'
      ret.append(ret_val)
    return ret
</code>

