{{indexmenu_n>0}}
==== 获取实时监控信息API ====
=== GetUAISrvRealTimeMetric ===

  * 参数说明\\
^参数 ^说明 ^是否必需 ^
|public\_key |用户的公钥|是|
|private\_key |用户的私钥|是|
|project\_id|项目ID|否|
| region   	 | 地域                	        | 否         |
| zone           | 可用区				| 否         |
|service\_id |服务ID|是|
|srv\_version |服务版本|否|

  * 请求响应\\
^参数 ^类型 ^说明 
|Action|String|响应动作: GetUAISrvRealTimeMetricResponse|
|RetCode|Integer| 执行成功与否，0表示成功，其他值则为错误代码  |
|CurrentMetric| Object  | Metric结构体，目前仅包含Qps（当前时间30s内统计值） |

  * 返回说明 \\
成功执行后，正常返回样例如下：

<code>
{
Message : ,
RetCode : 0,
RealTimeMetric : 
  {
      Qps : 0
   }
}
</code>