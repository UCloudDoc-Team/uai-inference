{{indexmenu_n>20}}

====== TensorFlow MNIST Json 案例 ======

json格式是一种常见的数据封装格式，我们可以通过代码的修改支持inference结果通过json格式来返回，这种转换的代码非常简单。

===== 准备工作 =====
请先阅读TensorFlow MNIST案例，编写自己的MNIST案例，或者开发别的案例

===== 使用json格式封装输入、输出数据 =====
您可以使用json格式封装输入、输出数据，这里给出了一个使用输入、输出数据为json格式的execute函数的范例（具体代码可以参见https://github.com/ucloud/uai-sdk/blob/master/examples/tensorflow/inference/mnist_0.11/mnist_inference_json.py）。\\
https://github.com/ucloud/uai-sdk/blob/master/examples/tensorflow/inference/mnist_0.11/2.json 是该案例的input json文件, 这里输入数据是json格式，有两个属性，一个是img，其值是图片的二进制值，一个是appid，其值是图片的ID。\\
<code>
{
   "img": "xxxx",
   "appid": "deadbeaf"
}
</code>

处理之前，代码会先依次读取appid和img两个属性对应的值。并对img数据进行处理，获取MNIST的识别结果"2", 最终结果会以json类型的方式返回，{“deadbeaf” : "2"}。（具体的json返回格式您可以根据需求自行定义）\\
具体代码实现如下：\\
<code>
def execute(self, data, batch_size):
    sess = self.output['sess']
    x = self.output['x']
    y_ = self.output['y_']

    ids = []
    imgs = []
    for i in range(batch_size):
      json_input = json.load(data[i])
      # 读取appid
      data_id = json_input['appid']
      # 读取img，并解码
      img_data = json_input['img'].decode('base64')

      im = Image.open(StringIO.StringIO(img_data)).resize((28, 28)).convert('L')
      im = np.array(im)
      im = im.reshape(784)
      im = im.astype(np.float32)
      im = np.multiply(im, 1.0 / 255.0)
      imgs.append(im)
      ids.append(data_id)

    imgs = np.array(imgs)
    print(imgs.shape)
    predict = sess.run(y_, feed_dict={x: imgs})
    ret = []

    for i in range(batch_size):
      读取inference结果，并编码成json格式
      ret_val = np.array_str(np.argmax(predict[i]))
      ret_item = json.dumps({ids[i]: ret_val})
      ret.append(ret_item)
    return ret
</code>

这部分代码的重点在于：
  * 对json对象的解析（这里我们使用json python包）\\

<code>
	json_input = json.load(data[i])
      # 读取appid
      data_id = json_input['appid']
      # 读取img，并解码
      img_data = json_input['img'].decode('base64')
</code>

  * 将返回结果使用json对象封装 \\

<code>
ret_val = np.array_str(np.argmax(predict[i]))
      ret_item = json.dumps({ids[i]: ret_val})
      ret.append(ret_item)
</code>

===== 测试MNIST json案例 =====
我们同样提供了MNIST json案例的测试工具，启动server 服务的方法与MNIST案例相同，但是我们需要使用2.json作为input进行测试：\\
<code>
> curl -X POST http://localhost:8080/service -T http-server-flask/2.json
> { "deadbeef" : "2"}
</code>