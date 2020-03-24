

# 设计原理
UAI-Inference平台使用CPU/GPU 计算节点来提供AI Inference在线服务任务的基础算力。平台利用Docker容器技术来封装训练任务，并内置Django Server来接受外部HTTP请求
![](/ai/uai-inference/images/general/ai_inference综述.png)

其所设计到的主要技术和产品包括包括：
  - **Docker** [容器技术](uai-inference/basic/docker)
  - **UHub** [UCloud Docker Hub](uai-inference/basic/uhub)

## UAI Inference执行的概念图
UAI Inference平台在执行在线服务任务分为两块：

  * 初始化(init)，即在django server启动时会调用 load\_model 来加载AI 模型
  * 执行(service)，在外部请求被django server接收后，会调用execute来处理推理请求
![](/ai/uai-inference/images/general/ai_inference执行.png)
![](/ai/uai-service/images/guide/service-general.png)
![](/ai/uai-service/images/guide/code-overview.png)

## UAI Inference 初始化流程
UAI Inference初始化时，Django server会先根据conf.json 加载AI Inference模块，然后调用该模块的load_model来加载模型：
![](/ai/uai-inference/images/general/init.png)

## UAI Inference Auto-batching机制
UAI Inference通过data buffer queue的方式将累积的请求batch成一个请求：
![](/ai/uai-inference/images/general/ai_inference_batch.png)

