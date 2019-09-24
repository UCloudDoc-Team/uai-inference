{{indexmenu_n>5}}

# Caffe API代码说明
## 获取方法
[[https://github.com/ucloud/uai-sdk]]
<code>
git clone https://github.com/ucloud/uai-sdk.git
</code>

### Caffe相关文件路径
<code>
uai-sdk

  examples/
    caffe/
  uai/
    arch/
      __init__.py
      caffe_model.py
</code>

## CaffeAiUcloudModel

CaffeAiUcloudModel 位于SDK文件uai/arch/caffe_model.py文件中, 所有Caffe Model类都需要继承CaffeAiUcloudModel类.
并实现CaffeAiUcloudModel 类中的三个函数：

<code>
__init__(self, conf=None, model_type='caffe')
load_model(self) 
execute(self, data, batch_size)
</code>

### __init__(self, conf=None, model_type='caffe') 

 用于初始化客户自定义的Caffe Model， 可以直接定义为：  

<code>
def __init__(self, conf=None, model_type='caffe'):
    super(CaffeAiUcloudModel, self).__init__(conf, model_type)
</code>

CaffeAiUcloudModel 类的\_\_init\_\_函数会执行如下操作：

 1. 创建self.output对象，可以通过self.output['tensor\_name'] = tensor 在load\_model和execute之间传递参数 

 2. 使用内置函数\_parse\_conf()来解析配置文件（例如解析模型文件路径等）
 模型的路径包括两部分： 
self.model\_arc\_file   
self.model\_weight\_file 
 3. 调用 load_model() 来加载模型

### load_model(self)
用于实现模型加载函数，在CaffeAiUcloudModel的\_\_init\_\_函数会自动调用load_model()。该函数需要包含以下几个元素：
1. 定义解析模型文件 
2. 定义加载模型  
3. 返回加载好的模型对象  

<code>
def load_model(self):

​    #TODO: Open Model File
​    #TODO: Load Model
​    #TODO: Return model_obj
​    pass
</code>

### execute(self, data, batch_size)

用于实现在线inference的执行逻辑。调用execute函数前，系统会自动对累积的请求进行batching，具体batching的方法可以参见https://github.com/ucloud/uai-sdk-httpserver/blob/master/inference.py 中inference_run函数的实现。  
#### 参数说明： 
**1. 入参**：

**data**，为一个数组类型，数组中对象的个数为batch\_size，data 数组中的每一个对象对应一个外部请求的输入
**batch\_size**，表示data数值中对象的个数
**2. 返回值**：
返回值必须也是一个数组类型，大小同样为batch\_size。该数组中的每一个对象都代表一个请求的返回值。注：请保证返回的对象和入参data中的对象是一一对应关系。

