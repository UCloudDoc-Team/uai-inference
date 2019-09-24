{{indexmenu_n>30}}

# 在线服务代码简介
完整的推理服务代码位于[[https://github.com/ucloud/uai-sdk/tree/master/examples/mxnet/inference/mnist]]，推理服务的代码为mnist\_inference.py，我们同时提供了conf.json和模型checkpoint\_dir。

## mnist_inference.py
 minst\_inference.py 实现了load\_model和execute两个函数。

### 创建 MnistModel 类
minst\_inference.py首先需要实现一个在线服务的类，该类继承了MXNetAiUcloudModel（MXNet 在线服务基类）
<code>
""" A very simple MNIST inferencer.
"""
from __future__ import absolute_import
from __future__ import division
from __future__ import print_function
import numpy as np
import mxnet as mx
from PIL import Image
from collections import namedtuple
from uai.arch.mxnet_model import MXNetAiUcloudModel

class MnistModel(MXNetAiUcloudModel):
    """ Mnist example model
    """
    def __init__(self, conf):
        super(MnistModel, self).__init__(conf)
</code>

### 实现load_model
<code>
def load_model(self):
    sym, self.arg_params, self.aux_params = mx.model.load_checkpoint(self.model_prefix, self.num_epoch)
    self.model = mx.mod.Module(symbol=sym, context=mx.cpu())
</code>

### 实现execute
实现execute分为四个部分：

  * 通过self.model.bind进行数据输入
  * 通过self.model.set\_params加载模型参数
  * 请求推理操作：self.model.forward
  * 将请求结果转化成string，并合并成results（results也是一个list，和data list是一一对应的关系）
<code>
def execute(self, data, batch_size):
    """1"""
    BATCH = namedtuple('BATCH', ['data', 'label'])
    self.model.bind(data_shapes=[('data', (batch_size, 1, 28, 28))],
                    label_shapes=[('softmax_label', (batch_size, 10))],
                    for_training=False)

    """2"""
    self.model.set_params(self.arg_params, self.aux_params)

    """3"""
    ret = []
    for i in range(batch_size):
        im = Image.open(data[i]).resize((28, 28))
        im = np.array(im) / 255.0
        im = im.reshape(-1, 1, 28, 28)
        self.model.forward(BATCH([mx.nd.array(ims)], None))

        """4"""
        predict_values = self.model.get_outputs()[0].asnumpy()
        val = predict_values[0]
        ret_val = np.array_str(np.argmax(val)) + '\n'
        ret.append(ret_val)
    return ret
</code>

