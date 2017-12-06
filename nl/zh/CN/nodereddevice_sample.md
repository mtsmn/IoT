---

copyright:
  years: 2016, 2017
lastupdated: "2017-11-02"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 创建并连接 Node-RED 设备模拟器
使用 Node-RED 创建设备模拟器，将模拟设备数据发送到 {{site.data.keyword.iot_full}} 组织。  
{:shortdesc}

## 概述

Node-RED 这款工具以全新的有趣方式将硬件设备、API 和在线服务连接在一起。有关更多信息，请参阅 [Node-RED ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://nodered.org/){: new_window} Web 站点。  

可以在您自己的环境中运行 Node-RED 实例或将其用作 {{site.data.keyword.Bluemix_notm}} 应用程序。以下步骤包含 {{site.data.keyword.Bluemix_notm}} 的指示信息。

创建并连接 Node-RED 设备模拟器，以将 MQTT 设备消息发送到 {{site.data.keyword.iot_short_notm}}。在以下示例中，设备模拟器会模拟将货物容器的数据发送到 MQTT 代理程序，例如 {{site.data.keyword.iot_short_notm}}。

## 步骤

1. 通过完成以下步骤来创建 Node-RED 设备模拟器：   
    1. 登录到 {{site.data.keyword.Bluemix_notm}}，网址为：https://console.ng.bluemix.net
    2. 选择**目录**选项卡。
    3. 找到服务目录的“样板”部分，然后单击 **Internet of Things Platform Starter**。
**提示：**单击[此处 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://console.ng.bluemix.net/catalog/starters/internet-of-things-platform-starter){: new_window} 可直接转至 Internet of Things Platform Starter 页面。
    4. 在 Internet of Things Platform Starter 页面上，选择您要部署 Node-RED 的空间，验证“创建应用程序”的选项，并单击**创建**以将 Node-RED 添加到 Bluemix 组织。例如：<ul>
     <li> 空间：dev<li> 应用程序名称：myDevice

<li> 主机名：myDevice  
    </ul>  
让其他选项保留为缺省值。部署应用程序后，将显示“使用 Node-RED 开始编码”页面。
**注：**编译打包过程可能会花费几分钟。  

2. 单击“路由”链接以打开 Node-RED 或访问应用程序 URL，例如 `http://myDevice.mybluemix.net`  
3. 完成以下步骤以**保护 Node-RED 编辑器**：
    1. 单击**下一步**
    2. 添加用户名和密码。  
    **注：**对于密码，您必须满足必需的复杂性规则，否则**下一步**按钮将保持灰色。
  
    3. 可选。选中**允许任何人查看编辑器，但不允许进行任何更改**或**不建议：允许任何人访问编辑器并进行更改**
    4. 单击**完成**以完成配置。
4. 单击**转至 Node-RED 流程编辑器**
5. 输入您先前创建的用户名和密码。  
设备模拟器流程可供流程编辑器使用。流程会模拟**热敏器**，将其位置、温度和湿度数据发送到 {{site.data.keyword.iot_short_notm}}。  
6. 通过检查是否针对**发送到 IBM IoT Platform** 节点显示具有已连接消息的点，确认设备已连接。此检查对于**温度监视器**子流程的 **IBM IoT App In** 节点也有效。  
7. 通过完成以下步骤来发送和接收 MQTT 设备消息：  
    1. 单击**发送数据**注入节点，以触发将数据发送到 {{site.data.keyword.iot_short_notm}}。
**注：**您可以激活**调试输出有效内容**调试节点，以查看发送的数据并检查**设备有效内容**功能节点，以查看用于构建有效内容的代码。 
    2. 单击**发送数据**后，MQTT 消息将发送到 {{site.data.keyword.iot_short_notm}}，并由 **IBM IoT App In** 节点接收。**温度监视器**子流程确定温度是否在定义的限制内，并在调试选项卡上显示消息。**注：**单击**温度阈值**开关节点，以检查并更改阈值
    3. 选中调试选项卡。此时将显示一条消息，例如，**safe permits (17) within safe limits**。
    
现在，您已将样本 IoT 设备连接到 {{site.data.keyword.iot_short_notm}}，并且可看到设备数据。
