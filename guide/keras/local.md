

# 部署本地开发环境
**1. 安装Kares 1.2.0 **
<code>
sudo apt-get update

sudo apt-get install python
sudo apt-get install python-pip
sudo apt-get install python-dev
wget https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-0.11.0-cp27-none-linux_x86_64.whl
sudo pip install tensorflow-0.11.0-cp27-none-linux_x86_64.whl
sudo pip install keras==1.2.0 -i http://pypi.douban.com/simple/ --trusted-host pypi.douban.com
</code>
目前UCloud的Keras 1.2.0的镜像使用的backend为TensorFlow 0.11

**2. 安装UCloud UFile SDK**
<code>
wget http://sdk.ufile.ucloud.com.cn/python_sdk.tar.gz
tar zxvf python_sdk.tar.gz
cd ufile-python
sudo python setup.py install
</code>
注: UFile SDK仅兼容request 2.1.0以下版本

**3. 安装UAI-Inference SDK**
<code>
git clone https://github.com/ucloud/uai-sdk.git
cd uai-sdk
sudo python setup.py install
</code>

**4. 安装Flask Http框架**
<code>
sudo pip install Flask
</code>

**5. 安装其它依赖（客户业务相关依赖）**
<code>
e.g. sudo apt-get install zlib1g-dev ...

​    sudo pip install numpy pillow ...
</code>

**6. Ready to Go**

