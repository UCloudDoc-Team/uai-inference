{{indexmenu_n>20}}

## 操作环境准备

我们可以在CPU的云主机上为MNIST的训练做准备，该云主机需要满足以下条件:

1.  需要是Linux或类Linux环境
2.  安装docker，建议使用docker-ce，且版本 \> 10.0
3.  该主机可以访问uhub.service.ucloud.cn 或 uhub.ucloud.cn

本Tutorial将以UCloud 普通云主机为范例操作（你也可以使用自己的主机或其他系统）

### 创建云主机

我们根据[ubuntu](/ai/uai-inference/base/ubuntu)操作申请一台2核4G的CPU云主机作为操作平台。

#### 安装docker

1.设置官方docker软件包源

    sudo apt-get -y install \
      apt-transport-https \
      ca-certificates \
      curl
    
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    
    sudo add-apt-repository \
           "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
           $(lsb_release -cs) \
           stable"
    
    sudo apt-get update

2.安装docker-ce

    $ sudo apt-get -y install docker-ce

3.测试docker安装

    $ sudo docker run hello-world

### 安装UAI SDK

uai-sdk使用时需要依赖UCloud UFile SDK,因此我们先安装UCloud UFile SDK。

    $ wget http://sdk.ufile.ucloud.com.cn/python_sdk.tar.gz
    $ tar zxvf python_sdk.tar.gz
    $ cd ufile-python
    $ sudo python setup.py install

我们需要从github上面下载uai-sdk：

    $ cd ~
    $ git clone https://github.com/ucloud/uai-sdk.git
    $ cd uai-sdk
    $ sudo python setup.py install

### 准备操作环境

我们在/data/目录下创建一个mnist目录来执行操作：

    $ cd /data/
    $ mkdir mnist/
