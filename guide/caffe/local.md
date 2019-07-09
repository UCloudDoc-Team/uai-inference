{{indexmenu_n>4}}

## Caffe 本地安装部署开发环境

\*\*1 安装Caffe BVLC Master \*\*

    sudo apt-get update
    sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler
    sudo apt-get install --no-install-recommends libboost-all-dev
    sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev libatlas-base-dev python-numpy
    git clone https://github.com/BVLC/caffe
    cd caffe && for req in $(cat python/requirements.txt);do sudo pip install $req;done
    
    cp Makefile.config.example Makefile.config
    
    # 修改Makefile.config文件，设置CPU_ONLY:= 1
    echo "CPU_ONLY:= 1" >> Makefile.config
    
    make all
    make test
    make runtest
    make pycaffe
    
    echo "export PYTHONPATH=`pwd`/python:$PYTHONPATH" >> ~/.bashrc
    source ~/.bashrc

**2 安装Ucloud Ufile SDK**

    wget http://sdk.ufile.ucloud.com.cn/python_sdk.tar.gz
    tar zxvf python_sdk.tar.gz
    cd ufile-python
    sudo python setup.py install

注: UFIle SDK仅兼容request 2.1.0以下版本

**3 安装Ucloud AI Inference SDK**

    git clone https://github.com/ucloud/uai-sdk.git
    cd uai-sdk
    sudo python setup.py install

**4 安装Flask Http框架**

    sudo pip install Flask

**5 安装其它依赖（客户业务相关依赖）**

    e.g. sudo apt-get install zlib1g-dev ...
        sudo pip install numpy pillow ...

**6 Ready to Go**
