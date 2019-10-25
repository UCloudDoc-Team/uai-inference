

# 在线服务代码简介
完整的推理服务代码位于[[https://github.com/ucloud/uai-sdk/tree/master/examples/caffe/inference/mnist]]，推理服务的代码为mnist\_inference.py，我们同时提供了conf.json和模型checkpoint\_dir

## mnist_inference.py
 minst\_inference.py 实现了load\_model和execute两个函数。

### 创建 MnistModel 类
minst\_inference.py首先需要实现一个在线服务的类，该类继承了CaffeAiUcloudModel（Caffe 在线服务基类）
<code>
""" A very simple MNIST inferencer.
"""
from __future__ import absolute_import
from __future__ import division
from __future__ import print_function

import caffe
from uai.arch.caffe_model import CaffeAiUcloudModel

class MnistModel(CaffeAiUcloudModel):
    """ Mnist example model
    """
    def __init__(self, conf):
        super(MnistModel, self).__init__(conf)
</code>

### 实现load_model
load\_model实现借助caffe.Net函数。
<code>
def load_model(self):
    self.model = caffe.Net(self.model_arch_file, self.model_weight_file, caffe.TEST)
</code>

### 实现execute
实现execute分为四个部分：

  * 通过caffe.io.Transformer进行图片预处理设置；
  * 加载图像，执行预处操作，并载入到blob中；
  * 请求推理操作：self.model.forward()
  * 将请求结果转化成string，并合并成results（results也是一个list，和data list是一一对应的关系）
<code>
def execute(self, data, batch_size):
    ret = []
    for i in range(batch_size):
	""" 1
	"""
        transformer = caffe.io.Transformer({'data': self.model.blobs['data'].data.shape})
        transformer.set_transpose('data', (2, 0, 1))
        transformer.set_raw_scale('data', 255)

	""" 2
	"""
        im = caffe.io.load_image(data[i], color=False)
        self.model.blobs['data'].data[...] = transformer.preprocess('data', im)

	""" 3
	"""
        self.model.forward()

	""" 4
	"""
        prob = self.model.blobs['prob'].data[0].flatten()
        ret_val = str(prob.argsort()[-1]) + '\n'
        ret.append(ret_val)
    return ret
</code>

