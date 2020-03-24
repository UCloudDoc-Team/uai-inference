

# API调用方法
## 获取方法
[代码](https://github.com/ucloud/uai-sdk)
<code>
git clone https://github.com/ucloud/uai-sdk.git
</code>

## TensorFlow相关文件路径 
<code>
uai-sdk

  examples
    tensorflow
  uai
    arch
      __init__.py
      tf_model.py
</code>

## 简介 
### TFAiUcloudModel

基础类 TFAiUcloudModel位于SDK文件uai/arch/tf\_model.py文件中，所有Tensorflow Model类都需要继承TFAiUcloudModel类，并实现TFAiUcloudModel类中的三个函数：
<code>
__init__(self, conf=None, model_type='tensorflow')
load_model(self) 
execute(self, data, batch_size)
</code>

#### __init__(self, conf=None, model_type='tensorflow') 
用于初始化客户自定义的Tensorflow Model，可以直接定义为：
<code>
def __init__(self, conf=None, model_type='tensorflow'):
    super(TFAiUcloudModel, self).__init__(conf, model_type)
</code>

TFAiUcloudModel类的\_\_init\_\_函数会执行如下操作：
1.1 创建self.output对象，可以通过self.output['tensor\_name'] = tensor 在load\_model和execute之间传递参数 
1.2 使用内置函数\_parse\_conf()来解析配置文件（例如解析模型文件路径等）
默认的模型路径路径会被保存在: self.model\_dir 对象中 
1.3 调用 load\_model() 来加载模型 

#### load_model(self)
用于实现模型加载函数，在TFAiUcloudModel的\_\_init\_\_函数会自动调用load\_model()，该函数需要包含以下几个元素：

1. 定义 tf.session 
2. 定义tf.graph 
3. 使用tf.Saver 或其他类加载模型 
4. 定义Input Tensor和output Tensor 
5. 将 tf.session, input Tensor, output Tensor等加入self.output 

<code>
def load_model(self):
    #TODO: Define Session of Tensorflow
    #TODO: Initialize Variables in Your Neural Network
    #TODO: Load Model
    #TODO: Register Ops into Self.output Dict
</code>

#### execute(self, data, batch_size)
用于实现在线inference的执行逻辑。调用execute函数前，系统会自动对累积的请求进行batching，具体batching的方法可以参见https://github.com/ucloud/uai-sdk-httpserver/blob/master/inference.py 中inference\_run函数的实现.

参数说明：
  * 入参：
data，为一个数组类型，数组中对象的个数为batch\_size，data 数组中的每一个对象对应一个外部请求的输入 
batch\_size，表示data数值中对象的个数 
  * 返回值：
返回值必须也是一个数组类型，大小同样为batch\_size。该数组中的每一个对象都代表一个请求的返回值。
注：请保证返回的对象和入参data中的对象是一一对应关系。

<code>
def execute(self, data, batch_size):

​    #TODO: Assignment of Variables in Your Neural Network
​    #TODO: Pretreatment of Data
​    #TODO: Predict Testing Data
​    #TODO: Return Results
</code>

详细实例用法可参考[案例](uai-inference/guide/tensorflow/mnist)

