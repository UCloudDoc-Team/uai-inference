{{indexmenu_n>1}}

# Intel Caffe高性能CPU解决方案
UCloud基于Intel Caffe版本和Intel Xeon CPU提供了高性能CPU 在线服务解决方案。
**通过结合Intel Caffe和AI Inference系统，可以提供具备高性价比的在线服务系统。**

## Intel Caffe

Intel Caffe为Intel公司针对Intel 高性能芯片维护的Caffe版本，其兼容所有BVLC Caffe的功能，同时提供针对Intel芯片的高性能优化解决方案。Intel Caffe基于Intel MKL和MKLDNN库实现，包含了大量CPU相关的优化。

## AI Inference + Intel Caffe 解决方案
AI Inference结合Intel Caffe提供高性能、高性价比的CPU解决方案。
该方案基于AI Inference提供的基础Intel Caffe在线服务镜像生产客户自己的在线服务Docker镜像，并部署在AI Inference PaaS平台或者UCloud CaaS平台来提供AI 在线服务。
{{:ai:uai-service:solution:intel-caffe-sol.png?600|}}

## 使用方法
Intel Caffe基础镜像如下：
<code>
UCloud内网：uhub.service.ucloud.cn/uaishare/mkl_uaiservice_ubuntu-14.04_python-2.7.6_intel_caffe-1.0.0:v1.1

公网：uhub.ucloud.cn/uaishare/mkl_uaiservice_ubuntu-14.04_python-2.7.6_intel_caffe-1.0.0:v1.1
</code>

Intel Caffe的使用方法和Caffe类似，仅在生产docker的时候直接使用Intel Caffe的基础镜像。
在使用packdocker命令时，参数使用：\-\-ai\_arch\_v=caffe-intel-1.0.0

## 案例分析
### RFCN 物体识别
案例基于https://github.com/Orpine/py-R-FCN.git，我们已经针对RFCN生产了Intel-Caffe的镜像，内部包含了Intel-Caffe以及RFCN的组件，镜像地址为：
<code>
UCloud内网：uhub.service.ucloud.cn/uaishare/mkl_uaiservice_ubuntu-14.04_python- 2.7.6_intel_caffe-rfcn:v1.0

公网：uhub.ucloud.cn/uaishare/mkl_uaiservice_ubuntu-14.04_python- 2.7.6_intel_caffe-rfcn:v1.0 
</code>
实现RFCN在线服务说明：
http://uaidocs.ufile.ucloud.com.cn/service/Intel-Caffe-RFCN.pdf

#### 性能对比
测试图片py\-R\-FCN/data/demo/000456.jpg
使用模型 resnet101

| 运行环境 | Caffe 类型 | 平均延时(ms) |
| -------- | ---------- | ------------ |
| K80 单核                                 | BVLC Caffe-GPU  | 272       |
| AI Inference (8核8G节点)                    | Intel-Caffe     | 1588      |
| E5-V3 CPU (8C32G)                      | Intel-Caffe     | 634       |
| AI Inference on UCloud CaaS 容器服务 (8C8G)  | Intel-Caffe     | 649       |
| Skylake 6216 (8C8G)                    | Intel-Caffe     | 416       |

8C8G的CPU节点的性能可以接近K80单核（K40）性能的 45%，使用Intel\-Caffe方案配合UAI Inference服务极具性价比

### CTPN 图片中文字检测
案例基于https://github.com/qingswu/CTPN.git，我们已经针对CTPN生产了Intel-Caffe的镜像，内部包含了Intel-Caffe以及CTPN的组件，镜像地址为：
<code>
UCloud内网：uhub.service.ucloud.cn/uaishare/mkl_uaiservice_ubuntu-14.04_python- 2.7.6_intel_caffe-ctpn:v1.0

公网：uhub.ucloud.cn/uaishare/mkl_uaiservice_ubuntu-14.04_python- 2.7.6_intel_caffe-ctpn:v1.0 
</code>
实现CTPN在线服务说明：
 http://uaidocs.ufile.ucloud.com.cn/service/Intel-Caffe-CTPN.pdf

#### 性能对比
测试图片CTPN/demo\_images/img\_1.jpg
使用模型 vgg16

| 运行环境 | Caffe 类型 | 平均延时(ms) |
| -------- | ---------- | ------------ |
| K80 单核                                | BVLC Caffe-GPU      | 302        |
| AI Inference (8核8G节点)                   | Intel-Caffe         | 2380       |
| E5-V3 CPU                             | Intel-Caffe(8C32G)  | 875        |
| AI Inference on UCloud CaaS 容器服务(8C8G)  | Intel-Caffe         | 884        |
| Skylake 6216 (8C8G)                   | Intel-Caffe         | 612        |

8C8G的CPU节点的性能可以接近K80单核（K40）性能的 35%，使用Intel\-Caffe方案配合UAI Inference服务极具性价比

### 风格画迁移
案例基于https://github.com/fzliu/style-transfer.git实现，该案例可以直接基于UCloud基础的Intel-Caffe镜像使用。
由于该案例的风格画转换执行时间过长，因此目前暂时无法使用AI Inference执行。建议使用普通云主机、CaaS容器服务[[compute:caas]] 或 AI Train服务[[ai:uai-train]]来实现。
基于Intel-Caffe实现风格画转换的方法：http://uaidocs.ufile.ucloud.com.cn/service/Intel-Caffe-Style.pdf

#### 性能对比
测试图片style\-transfer/images/style/starry\_night.jpg style\-transfer/images/content/sanfrancisco.jpg
使用模型 vgg19

| 运行环境 | Caffe 类型 | 执行时间(min) |
| -------- | ---------- | ------------- |
| K80 单核             | BVLC Caffe-GPU  | 24         |
| E5-V3 CPU (8C32G)  | Intel-Caffe     | 43.5       |
| E5-V3 CPU (8C32G)  | BVLC Caffe      | 268        |


