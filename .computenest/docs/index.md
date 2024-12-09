# 达梦数据库DM8社区版计算巢快速部署

>**免责声明**：本服务由第三方提供，我们尽力确保其安全性、准确性和可靠性，但无法保证其完全免于故障、中断、错误或攻击。因此，本公司在此声明：对于本服务的内容、准确性、完整性、可靠性、适用性以及及时性不作任何陈述、保证或承诺，不对您使用本服务所产生的任何直接或间接的损失或损害承担任何责任；对于您通过本服务访问的第三方网站、应用程序、产品和服务，不对其内容、准确性、完整性、可靠性、适用性以及及时性承担任何责任，您应自行承担使用后果产生的风险和责任；对于因您使用本服务而产生的任何损失、损害，包括但不限于直接损失、间接损失、利润损失、商誉损失、数据损失或其他经济损失，不承担任何责任，即使本公司事先已被告知可能存在此类损失或损害的可能性；我们保留不时修改本声明的权利，因此请您在使用本服务前定期检查本声明。如果您对本声明或本服务存在任何问题或疑问，请联系我们。

## 概述
100% 自主研发的大型通用关系型数据库，具备极致兼容性、高可用性、高可靠性和高安全性，是解决我国基础软件领域“卡脖子”痛点的数据库产品，目前已助力 50+ 重点行业实现核心系统升级。

产品官网地址：[达梦数据库管理系统DM8](https://eco.dameng.com/info/products/dm8)


## 前提条件
<font style="color:rgb(51, 51, 51);">部署达梦数据库DM8社区版服务实例，需要对部分阿里云资源进行访问和创建操作。因此您的账号需要包含如下资源的权限。</font><font style="color:rgb(51, 51, 51);"> </font>**<font style="color:rgb(51, 51, 51);">说明</font>**<font style="color:rgb(51, 51, 51);">：当您的账号是RAM账号时，才需要添加此权限。</font>

| <font style="color:rgb(51, 51, 51);">权限策略名称</font> | <font style="color:rgb(51, 51, 51);">备注</font> |
| --- | --- |
| <font style="color:rgb(51, 51, 51);">AliyunECSFullAccess</font> | <font style="color:rgb(51, 51, 51);">管理云服务器服务（ECS）的权限</font> |
| <font style="color:rgb(51, 51, 51);">AliyunVPCFullAccess</font> | <font style="color:rgb(51, 51, 51);">管理专有网络（VPC）的权限</font> |
| <font style="color:rgb(51, 51, 51);">AliyunROSFullAccess</font> | <font style="color:rgb(51, 51, 51);">管理资源编排服务（ROS）的权限</font> |
| <font style="color:rgb(51, 51, 51);">AliyunComputeNestUserFullAccess</font> | <font style="color:rgb(51, 51, 51);">管理计算巢服务（ComputeNest）的用户侧权限</font> |


## 计费说明
<font style="color:rgb(51, 51, 51);">达梦数据库DM8社区版在计算巢部署的费用主要涉及：</font>

+ <font style="color:rgb(51, 51, 51);">所选vCPU与内存规格</font>
+ <font style="color:rgb(51, 51, 51);">系统盘类型及容量</font>
+ <font style="color:rgb(51, 51, 51);">公网带宽</font>


## 部署架构
本服务部署在单台ECS实例上，架构如下：

![](./images/architecture_ecs_single.png)

默认不支持公网访问数据库以保证安全性。

## 参数说明
| <font style="color:rgb(51, 51, 51);">参数组</font> | <font style="color:rgb(51, 51, 51);">参数项</font> | <font style="color:rgb(51, 51, 51);">说明</font> |
| --- | --- | --- |
| <font style="color:rgb(51, 51, 51);">服务实例</font> | <font style="color:rgb(51, 51, 51);">服务实例名称</font> | <font style="color:rgb(51, 51, 51);">长度不超过64个字符，必须以英文字母开头，可包含数字、英文字母、短划线（-）和下划线（_）</font> |
| | <font style="color:rgb(51, 51, 51);">地域</font> | <font style="color:rgb(51, 51, 51);">服务实例部署的地域</font> |
| | <font style="color:rgb(51, 51, 51);">付费类型</font> | <font style="color:rgb(51, 51, 51);">资源的计费类型：按量付费和包年包月</font> |
| <font style="color:rgb(51, 51, 51);">资源和数据库配置</font> | <font style="color:rgb(51, 51, 51);">实例类型</font> | <font style="color:rgb(51, 51, 51);">可用区下可以使用的实例规格</font> |
| | <font style="color:rgb(51, 51, 51);">实例密码</font> | <font style="color:rgb(51, 51, 51);">长度8-30，必须包含三项（大写字母、小写字母、数字、 ()`~!@#$%^&*-+=|{}[]:;'<>,.?/ 中的特殊符号）</font> |
| | <font style="color:rgb(51, 51, 51);">SYSDBA密码</font> | <font style="color:rgb(51, 51, 51);">SYSDBA登录密码,长度9-48，不支持特殊字符为密码</font> |
| <font style="color:rgb(51, 51, 51);">可用区配置</font> | <font style="color:rgb(51, 51, 51);">可用区</font> | <font style="color:rgb(51, 51, 51);">ECS实例所在可用区</font> |
| | <font style="color:rgb(51, 51, 51);">VPC ID</font> | <font style="color:rgb(51, 51, 51);">资源所在VPC</font> |
| | <font style="color:rgb(51, 51, 51);">交换机ID</font> | <font style="color:rgb(51, 51, 51);">资源所在交换机</font> |

## 部署流程
1. 访问计算巢达梦数据库DM8社区版[部署链接](https://computenest.console.aliyun.com/service/instance/create/cn-hangzhou?type=user&ServiceName=达梦数据库DM8社区版)，按提示填写部署参数
2. 选择付费类型
   ![](./images/pay_type_config.png)
3. 填写实例参数和数据库SYSDBA密码
   ![](./images/resource_config.png)
4. 填写可用区和网络参数并点击“下一步：确认订单”![](./images/zone_config.png)
5. 确认部署参数并查看预估价格后，点击立即创建，等待服务实例部署完成
5. 服务实例部署完成后，点击实例ID进入到详情界面![]()
6. 按照服务实例概览页的说明登录数据库
   ![](./images/how_to_use.png)

## 如何管理数据库服务
达梦数据库是以容器形式部署在ECS实例上，已注册为systemctl系统服务并开机自启动。您可以通过命令来管理系统服务。
```bash
启动：sudo systemctl start dm8-database
停止：sudo systemctl stop dm8-database
重启：sudo systemctl restart dm8-database
```

## 登录数据库
1. 默认不支持通过公网访问数据库端口。请将您的应用服务器加入到和数据库相同的安全组以访问数据库服务。如需要从公网访问数据库，请先配置安全组规则。
2. 从服务实例概览页可以查看数据库端口。
3. 登录数据库
   ```bash
   #登录ECS实例，执行命令进入容器
   sudo docker exec -it application-dm8-1 bash

   # 在容器shell中执行命令连接数据库
   source /etc/profile
   /opt/dmdbms/bin/disql SYSDBA/PASSWORD # 将PASSWORD替换为您的SYSDBA密码
   ```
