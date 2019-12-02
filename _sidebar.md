<div class="sidebar_title icon__uai"> AI在线服务 UAI-Inference</div>

* [概览](ai/uai-inference/overview)
* 产品简介
    * [UAI-Inference基础知识](ai/uai-inference/intro/infer)
    * [产品优势](ai/uai-inference/intro/feature)
    * [Hot-Standby机制](ai/uai-inference/intro/hot-standby)
    * 产品架构
        * [设计原理](ai/uai-inference/intro/structure/principle)
        * [弹性扩缩容机制](ai/uai-inference/intro/structure/auto)
        * [开发综述](ai/uai-inference/intro/structure/dev-principle)
        * [服务请求方式](ai/uai-inference/intro/structure/requests)
        * [开源Docker镜像](ai/uai-inference/intro/structure/docker)
        * [开源案例](ai/uai-inference/intro/structure/example)
    * [学习视频](ai/uai-inference/intro/video)
* [产品定价](ai/uai-inference/price)
* 快速上手
    * 快速上手（TF-Mnist案例)
        * [MNIST 介绍](ai/uai-inference/set-up/tf-mnist/intro)
        * [环境准备](ai/uai-inference/set-up/tf-mnist/prepare)
        * [创建私有的UHub镜像仓库](ai/uai-inference/set-up/tf-mnist/uhub)
        * [制作Mnist在线服务镜像](ai/uai-inference/set-up/tf-mnist/pack)
        * [部署CPU在线服务APP](ai/uai-inference/set-up/tf-mnist/inference)
        * [部署GPU在线服务APP](ai/uai-inference/set-up/tf-mnist/gpu-inference)
        * [在线服务代码简介](ai/uai-inference/set-up/tf-mnist/coding)
        * [使用自定义镜像打包](ai/uai-inference/set-up/tf-mnist/self-pack)
        * [本地测试Mnist在线服务](ai/uai-inference/set-up/tf-mnist/local-test)
* 基础环境指南
    * [Docker使用指南](ai/uai-inference/basic/docker)
    * [UHub使用指南](ai/uai-inference/basic/uhub)
* 用户指南
    * [开发指南简介](ai/uai-inference/guide/intro)
    * 开发综述
        * [设计原理](ai/uai-inference/guide/general/principle)
        * [开发综述](ai/uai-inference/guide/general/dev-principle)
    * [UAI Inference 开发概述](ai/uai-inference/guide/principle)
    * TensorFlow 开发指南
        * [镜像基础包](ai/uai-inference/guide/tensorflow/packages)
        * [部署本地开发环境](ai/uai-inference/guide/tensorflow/local)
        * [API调用方法](ai/uai-inference/guide/tensorflow/coding)
        * [打包镜像](ai/uai-inference/guide/tensorflow/pack)
        * [自定义打包镜像](ai/uai-inference/guide/tensorflow/self-pack)
        * [本地测试方法](ai/uai-inference/guide/tensorflow/test)
        * [TensorFlow MNIST 案例](ai/uai-inference/guide/tensorflow/mnist)
        * [TensorFlow MNIST JSON 案例](ai/uai-inference/guide/tensorflow/mnist-json)
    * Keras开发指南
        * [镜像基础包](ai/uai-inference/guide/keras/packages)
        * [部署本地开发环境](ai/uai-inference/guide/keras/local)
        * [API调用方法](ai/uai-inference/guide/keras/coding)
        * [打包镜像](ai/uai-inference/guide/keras/pack)
        * [自定义打包镜像](ai/uai-inference/guide/keras/self-pack)
        * [本地测试方法](ai/uai-inference/guide/keras/test)
        * [Keras MNIST 案例](ai/uai-inference/guide/keras/example)
    * Caffe 开发指南
        * [Caffe 镜像基础包](ai/uai-inference/guide/caffe/packages)
        * [Caffe 本地安装部署开发环境](ai/uai-inference/guide/caffe/local)
        * [Caffe API代码说明](ai/uai-inference/guide/caffe/coding)
        * [Caffe 打包镜像说明](ai/uai-inference/guide/caffe/pack)
        * [Caffe 自定义打包镜像说明](ai/uai-inference/guide/caffe/self-pack)
        * [本地测试方法](ai/uai-inference/guide/caffe/test)
        * [Caffe MNIST 案例](ai/uai-inference/guide/caffe/example)
    * MXNet 开发指南
        * [镜像基础包](ai/uai-inference/guide/mxnet/packages)
        * [部署本地开发环境](ai/uai-inference/guide/mxnet/local)
        * [API调用方法](ai/uai-inference/guide/mxnet/coding)
        * [打包镜像](ai/uai-inference/guide/mxnet/pack)
        * [自定义打包镜像](ai/uai-inference/guide/mxnet/self-pack)
        * [本地测试方法](ai/uai-inference/guide/mxnet/test)
        * [MXNet MNIST 案例](ai/uai-inference/guide/mxnet/example)
* 实践场景
    * TensorFlow Mnist
        * [MNIST 介绍](ai/uai-inference/tutorial/tf-mnist/intro)
        * [操作环境准备](ai/uai-inference/tutorial/tf-mnist/prepare)
        * [在线服务代码简介](ai/uai-inference/tutorial/tf-mnist/coding)
        * [创建私有的UHub镜像仓库](ai/uai-inference/tutorial/tf-mnist/uhub)
        * [制作Mnist在线服务镜像](ai/uai-inference/tutorial/tf-mnist/pack)
        * [使用自定义镜像打包](ai/uai-inference/tutorial/tf-mnist/self-pack)
        * [本地测试Mnist在线服务](ai/uai-inference/tutorial/tf-mnist/local-test)
        * [部署CPU在线服务APP](ai/uai-inference/tutorial/tf-mnist/inference)
        * [部署GPU在线服务APP](ai/uai-inference/tutorial/tf-mnist/gpu-inference)
    * Caffe Mnist
        * [MNIST 介绍](ai/uai-inference/tutorial/caffe-mnist/intro)
        * [操作环境准备](ai/uai-inference/tutorial/caffe-mnist/prepare)
        * [在线服务代码简介](ai/uai-inference/tutorial/caffe-mnist/coding)
        * [创建私有的UHub镜像仓库](ai/uai-inference/tutorial/caffe-mnist/uhub)
        * [使用自定义镜像打包](ai/uai-inference/tutorial/caffe-mnist/pack)
        * [本地测试Mnist在线服务](ai/uai-inference/tutorial/caffe-mnist/local-test)
        * [部署CPU在线服务APP](ai/uai-inference/tutorial/caffe-mnist/cpu-inference)
    * MXNet Mnist
        * [MNIST 介绍](ai/uai-inference/tutorial/mxnet-mnist/intro)
        * [操作环境准备](ai/uai-inference/tutorial/mxnet-mnist/prepare)
        * [在线服务代码简介](ai/uai-inference/tutorial/mxnet-mnist/coding)
        * [创建私有的UHub镜像仓库](ai/uai-inference/tutorial/mxnet-mnist/uhub)
        * [使用自定义镜像打包](ai/uai-inference/tutorial/mxnet-mnist/pack)
        * [本地测试Mnist在线服务](ai/uai-inference/tutorial/mxnet-mnist/test)
        * [部署CPU在线服务APP](ai/uai-inference/tutorial/mxnet-mnist/cpu-inference)
    * Keras Mnist
        * [MNIST 介绍](ai/uai-inference/tutorial/keras-mnist/intro)
        * [操作环境准备](ai/uai-inference/tutorial/keras-mnist/prepare)
        * [在线服务代码简介](ai/uai-inference/tutorial/keras-mnist/code)
        * [创建私有的UHub镜像仓库](ai/uai-inference/tutorial/keras-mnist/uhub)
        * [使用自定义镜像打包](ai/uai-inference/tutorial/keras-mnist/pack)
        * [本地测试Mnist在线服务](ai/uai-inference/tutorial/keras-mnist/test)
        * [部署CPU在线服务APP](ai/uai-inference/tutorial/keras-mnist/cpu-inference)
* 所有操作清单
    * [UAI Inference常见操作](ai/uai-inference/use/intro)
    * 创建新任务
        * [用Console创建新任务](ai/uai-inference/use/new/console)
        * [用CLI创建新任务](ai/uai-inference/use/new/cmd)
    * 调整在线服务节点数量
        * [用Console修改在线服务节点数量](ai/uai-inference/use/node-cnt/console)
        * [用CLI修改在线服务节点数量](ai/uai-inference/use/node-cnt/cmd)
    * [弹性伸缩规则设置](ai/uai-inference/use/auto-scale)
    * 服务授权与访问
        * [用Console为服务授权](ai/uai-inference/use/auth/console)
    * 获取实时监控信息
        * [获取实时监控信息API](ai/uai-inference/use/getmetric/api)
    * 灰度部署
        * [用Console灰度部署](ai/uai-inference/use/graydeploy/console)
        * [用CLI灰度部署](ai/uai-inference/use/graydeploy/cmd)
    * 删除服务
        * [用Console删除服务](ai/uai-inference/use/delete/console)
        * [用CLI删除服务](ai/uai-inference/use/delete/cmd)
    * 基本操作清单
        * [tar](ai/uai-inference/use/oplist/tar)
        * [pack](ai/uai-inference/use/oplist/packdata)
        * [packdocker](ai/uai-inference/use/oplist/packdata_docker)
        * [create](ai/uai-inference/use/oplist/create)
        * [deploy](ai/uai-inference/use/oplist/deploy)
        * [deploydocker](ai/uai-inference/use/oplist/deploydocker)
        * [start](ai/uai-inference/use/oplist/start)
        * [stop](ai/uai-inference/use/oplist/stop)
        * [delete](ai/uai-inference/use/oplist/delete)
        * [listservice](ai/uai-inference/use/oplist/listservice)
        * [listversion](ai/uai-inference/use/oplist/listversion)
        * [modifyweight](ai/uai-inference/use/oplist/modifyweight)
        * [modifyname](ai/uai-inference/use/oplist/modifyname)
        * [modifymemo](ai/uai-inference/use/oplist/modifymemo/index)
        * [modifynodecount](ai/uai-inference/use/oplist/modifynode)
* [FAQ](ai/uai-inference/qa)






    
   
   
    
        