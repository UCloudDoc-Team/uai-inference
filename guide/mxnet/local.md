{{indexmenu_n>2}}

## MXNet 本地安装部署开发环境

**1 安装MXNet 0.9.5**

    sudo apt-get update
    sudo apt-get install -y build-essential git
    sudo apt-get install -y libopenblas-dev
    sudo apt-get install -y libopencv-dev
    git clone https://github.com/dmlc/mxnet.git mxnet --recursive --branch v0.9.5 --depth 1
    cd mxnet/setup_utils && bash bash install-mxnet-ubuntu-python.sh

**2 安装UCloud UFile SDK（可选，UFile SDK在使用命令行部署任务时会使用）**

    wget http://sdk.ufile.ucloud.com.cn/python_sdk.tar.gz
    tar zxvf python_sdk.tar.gz
    cd ufile-python
    sudo python setup.py install

注 UFIle SDK仅兼容request 2.1.0以下版本

**3 安装Ucloud AI Inference SDK**

    git clone https://github.com/ucloud/uai-sdk
    cd uai-sdk
    sudo python setup.py install

**4 安装Flask Http框架**

    sudo pip install Flask

**5 安装其它依赖包**

    e.g. sudo apt-get install zlib1g-dev ...
         sudo pip install numpy pillow ...

**6 Ready to Go**
