{{indexmenu_n>4}}

# 服务请求方式 
UAI Inference 服务根据算力模式分为两种请求模式：

  * 令牌鉴权请求模式（算力独占模式使用）
  * 直连请求模式（算力共享模式使用）

## 令牌鉴权请求模式
该模式用于算力独占模式，由于算力独占模式支持用户直接部署公网可直接访问的AI在线服务，因此为了防止公网服务被恶意滥用，我们提供token令牌鉴权的方式来控制服务的访问权限。请求的HTTP的HEAD 中需要包含两个关键字：

| Key  | Value说明 |
| ---- | --------- |
| ServiceID  | UAI Inference APP的服务ID，通常为 uaiservice-xxx |
| Token      | 用户自行创建的Token令牌，如何创建请查看[[management_monitor:utoken:operation:mgr_token:create_token]]   |

使用Token令牌鉴权模式的请求目标url均使用[外网]http://uinference.ucloud.cn/xxx 或 [内网]http://uinference.service.ucloud.cn/xxx，系统会自动根据ServiceID进行路由，并通过Token进行访问权限控制。

### 请求案例
**curl 案例**
  curl -X POST -H "ServiceID:uaiservice-xxx" -H "Token:xxx" http://uinference.service.ucloud.cn/service -T xxx.png

**ab 测试案例**
   ab -n 1000 -c 4 -p xxx.png  -H "ServiceID:uaiservice-xxx" -H "Token:xxx" http://uinference.service.ucloud.cn/service

## 直连请求模式
该模式用于算力共享模式，由于该模式下只允许客户通过内网访问AI在线服务，因此不通过token令牌鉴权。用户仅需要获取服务地址（详细请查看：[[ai:uai-inference:use:new:console]]的Step5: 获取服务的URL）直接访问即可。

### 请求案例

**curl 案例**
  curl -X POST http://xxx.uae.service.ucloud.cn/service -T xxx.png

