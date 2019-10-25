

# 开发综述
面向UAI Inference 开发在线服务Docker镜像的基本原则如下： 

  - 选择CPU节点在线服务任务时，使用基于docker的惊喜，内置AI框架和UAI-SDK，UCloud已经提供丰富的CPU基础镜像选择，客户可以自由选择
  - 选择GPU节点在线服务任务时，使用基于nvidia-docker的镜像，内部需要内置cuda、cudnn等库，UCloud已经提供丰富的GPU基础镜像选择，客户可以自由选择【即将推出】
  - **在线服务代码请封装在Docker镜像中**
  - **在线服务代码必须包含load_model 函数，用于初始化时加载模型**
  - **在线服务代码必须包含execute函数，用于处理在线服务请求**
  - 在线服务代码必须位于**/ai-ucloud-client-django/**，django server在此目录路径启动
  - 必须提供conf.json，来指导django server加载在线服务模块
  - 在线服务使用的节点**无外网访问能力**

## UAI Inference Docker镜像准备
用户可以根据如下指南开发对应AI框架的在线服务代码+容器：
[[ai:uai-inference:guide:tensorflow]] 
[[ai:uai-inference:guide:caffe]] 
[[ai:uai-inference:guide:keras]] 
[[ai:uai-inference:guide:mxnet]] 

