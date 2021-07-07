

# FAQ

## 1. 我的请求延时分布不显示

   请求延时分布的最小粒度为小时，因此请在**监控信息**的监控间隙选择6小时以上。

## 2. 我的服务部署后无法请求
可以查询实时日志查看节点的运行日志，常见的无法请求可能有如下原因：

  * 模型加载失败，请在本地测试在AI Inference平台部署的Docker 容器的正确性。
  * 模型加载缓慢，NVidia 的nvcc编译在编译代码时需要选择目标GPU平台（Tesla、Pascal、Volta等）以直接编译成静态优化的代码，否则将仅编译ptx中间代码然后在具体执行时由系统实时编译（例如编译代码时仅选择了Tesla平台，但是代码在Pascal平台执行，系统在执行GPU代码钱会进行编译操作），该编译操作相当耗时，因此会导致容器长时间无法服务。

**以下是使用UAI Inference的常见操作组合**

## 3. 如何部署并启用第一个AI在线服务？ 
**部署并启用一个新的AI在线服务任务:** [创建](uai-inference/use/new)

涉及的基本操作：
1. pack或者packdocker 
2. create 
3. deploy或者deploydocker 
4. listversion 
5. start 

是否支持命令行操作：支持

## 4. 如何创建多个版本服务并实现灰度部署？ 
**部署多个AI在线服务版本并实现灰度部署:** [灰度部署](uai-inference/use/graydeploy)

涉及的基本操作：
1. pack或者packdocker 
2. create 
3. deploy或者deploydocker 
4. listversion 
5. start 
6. modifyweight 

是否支持命令行操作：支持

## 5. 如何删除一个AI在线服务？
**说明：部署多个AI在线服务版本并实现灰度部署:** [删除](uai-inference/use/delete)

涉及的基本操作：
1. listservice 
2. stop 
3. delete 

是否支持命令行操作：支持
