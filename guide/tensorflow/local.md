

# 部署本地开发环境
（案例使用的TensorFlow版本为1.1）

## 1. 安装Tensorflow 1.1
```
sudo apt-get update
sudo apt-get install python
sudo apt-get install python-pip
sudo apt-get install python-dev
wget https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-1.1.0-cp27-none-linux_x86_64.whl
sudo pip install tensorflow-1.1.0-cp27-none-linux_x86_64.whl
```

如果需要使用TensorFlow 0.11， 替换最后两句为：

```
wget https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-0.11.0-cp27-none-linux_x86_64.whl
sudo pip install tensorflow-0.11.0-cp27-none-linux_x86_64.whl
```

## 2. 安装UCloud UFile SDK
可选，UFile SDK在使用命令行部署任务时会使用
```
wget http://sdk.ufile.ucloud.com.cn/python_sdk.tar.gz
tar zxvf python_sdk.tar.gz
cd ufile-python
sudo python setup.py install
```
注：UFile SDK仅兼容request 2.1.0以下版本

## 3. 安装Ucloud AI Inference SDK
```
git clone https://github.com/ucloud/uai-sdk
cd uai-sdk
sudo python setup.py install
```

## 4. 安装Flask Http框架
<code>
sudo pip install Flask
</code>

## 5. 安装其它依赖包
<code>
sudo apt-get install zlib1g-dev ...

sudo pip install numpy pillow ...
</code>

## 6. Ready to Go

