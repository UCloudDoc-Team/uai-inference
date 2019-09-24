{{indexmenu_n>30}}

# 在线服务代码简介
完整的推理服务代码位于[[https://github.com/ucloud/uai-sdk/tree/master/examples/keras/inference/mnist]]，推理服务的代码为mnist\_inference.py，我们同时提供了conf.json和模型checkpoint\_dir

## mnist_inference.py
 minst\_inference.py 实现了load\_model和execute两个函数。

### 创建 MnistModel 类
minst\_inference.py首先需要实现一个在线服务的类，该类继承了KerasAiUcloudModel（Keras 在线服务基类）
<code>
"""A very simple MNIST inferencer.
"""
from __future__ import absolute_import
from __future__ import division
from __future__ import print_function
import numpy as np
from PIL import Image
from keras.models import load_model
from keras.models import model_from_json
from uai.arch.keras_model import KerasAiUcloudModel

class MnistModel(KerasAiUcloudModel):
    """ Mnist example model
    """
    def __init__(self, conf):
        super(MnistModel, self).__init__(conf)
</code>

### 实现load_model
<code>
def load_model(self):
    model_json_file = open(self.model_arch_file, 'r')
    model_json = model_json_file.read()
    model_json_file.close()
    model_json = model_from_json(model_json)
    model_json.load_weights(self.model_weight_file)
    self.model = model_json
</code>

### 实现execute
实现execute分为三个部分：

  * 加载图像数据；
  * 请求推理操作：self.model.predict()
  * 将请求结果转化成string，并合并成results（results也是一个list，和data list是一一对应的关系）
<code>
def execute(self, data, batch_size):
    """1"""
    ims = []
    for i in range(batch_size):
        im = Image.open(data[i]).resize((28, 28))
        im = np.array(im) / 255.0
        ims.append(im)
    ims = np.array(ims)
    ims = ims.reshape(-1, 784)

    """2"""
    predict_values = self.model.predict(ims)

    """3"""
    ret = []
    for val in predict_values:
        ret_val = str(np.argmax(val).item()) + '\n'
        ret.append(ret_val)
    return ret
</code>