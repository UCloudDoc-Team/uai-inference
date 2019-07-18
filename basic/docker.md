{{indexmenu_n>2}}


====== Docker使用指南 ======

==== Docker 入门介绍 ====
如果你对Docker 不是很了解，可以通过该文件了解docker的基本概念和操作方法：
http://uaidocs.cn-bj.ufileos.com/docs%2F%2Fdocker%E7%AE%80%E4%BB%8B-v1.0.pdf

==== 如何安装Docker ====
安装版本 docker-ce，另外安装的Ubuntu机器必须是64bit的。
安装说明在这里: https://store.docker.com/editions/community/docker-ce-server-ubuntu?tab=description

  * 设置repository

<code>
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
</code>

  * 获取 Docker CE

<code>
sudo apt-get -y install docker-ce
</code>

  * 测试Docker安装是否成功

<code>
sudo docker run hello-world
</code>

==== 如何安装Nvidia-Docker  ====
详细教程请参考：https://github.com/NVIDIA/nvidia-docker

**安装方法：**
<code>
# Install nvidia-docker and nvidia-docker-plugin
wget -P /tmp https://github.com/NVIDIA/nvidia-docker/releases/download/v1.0.1/nvidia-docker_1.0.1-1_amd64.deb
sudo dpkg -i /tmp/nvidia-docker*.deb && rm /tmp/nvidia-docker*.deb

# Test nvidia-smi
nvidia-docker run --rm nvidia/cuda nvidia-smi
</code>
