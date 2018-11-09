---

copyright:
  years: 2017, 2018
lastupdated: "2018-01-11"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}


# 指南 1：连接传送带设备  
创建带有 IoT 设备的基本传送带，该设备将监视数据发送到 {{site.data.keyword.Bluemix_notm}} 上的 {{site.data.keyword.iot_full}}。
{:shortdesc}

## 概述和目标
{: #overview}  
本指南是系列指南中的第一个，逐步指导您完成将设备连接到 {{site.data.keyword.iot_short_notm}}、监视和处理设备数据等过程。每个指南都在先前指南上构建，每个指南中的样本代码都用作下一指南的输入。您也可以独立使用这些指南，方法是修改样本代码以满足您的需要。

在这第一个指南中，我们设置了一条连接的传送带，并使用它来将 IoT 数据发送到 {{site.data.keyword.iot_short_notm}}。根据技能级别，您可以遵循以下一个或两个路径来设置传送带：
- 路径 A  
此路径通过在 {{site.data.keyword.Bluemix_notm}} 上安装传送带模拟器应用程序，让您快速入门。应用程序向 {{site.data.keyword.iot_short_notm}} 自注册设备，并自动将格式正确的数据发送到平台。此路径的指示信息位于[步骤 2A - 使用 GitHub 中的模拟器样本应用程序](#deploy_app)。  
- 路径 B  
此路径在技术上更具挑战性，需要额外的硬件、Python 编程技能以及手动向 {{site.data.keyword.iot_short_notm}} 注册设备。此路径的指示信息位于[步骤 2B - 使用 Raspberry Pi 和电动机构建物理传送带](#raspberry)。

作为本指南的一部分，您将：
- 使用 Cloud Foundry CLI 创建和部署 {{site.data.keyword.iot_short_notm}} 组织。
- 构建和部署样本传送带设备。
- 将模拟传送带连接设备连接到 {{site.data.keyword.iot_short_notm}}。
- 使用 {{site.data.keyword.iot_short_notm}} 仪表板监视和可视化设备数据。

要搭配其他 IoT 设备开始使用 {{site.data.keyword.iot_short_notm}}，请参阅[入门教程](/docs/services/IoT/getting-started.html)。
{: tip}

## 先决条件
{: #prereqs}

您需要以下帐户和工具：
* [{{site.data.keyword.Bluemix_notm}} 帐户 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://console.ng.bluemix.net/registration/){: new_window}
* [Cloud Foundry 命令行界面 (cf CLI) ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://github.com/cloudfoundry/cli#downloads){: new_window}  
使用 cf CLI 将应用程序和服务部署到 {{site.data.keyword.Bluemix_notm}}。有关更多信息，请参阅 [Cloud Foundry CLI 文档 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://docs.cloudfoundry.org/cf-cli/){: new_window}
* 可选：[Git ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://git-scm.com/downloads){: new_window}  
如果选择使用 Git 来下载代码样本，那么还必须具有 [GitHub.com 帐户 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://github.com){: new_window}。如果没有 GitHub.com 帐户，您还可以将代码下载为压缩文件。
* 可选：运行*传送带*样本 Web 应用程序以发送加速计量表数据的移动电话。

## 步骤 1 - 部署 {{site.data.keyword.iot_short_notm}}  
{: #deploy_watson_iot_platform_service}
{{site.data.keyword.iot_short_notm}} 提供对 IoT 设备和数据的强大应用程序访问，可帮助您快速编写分析应用程序、可视化仪表板和移动 IoT 应用程序。
后续步骤将在 {{site.data.keyword.Bluemix_notm}} 环境中部署名称为 `iotp-for-conveyor` 的 {{site.data.keyword.iot_short_notm}} 服务实例。如果您已具有正在运行的服务实例，那么可以将该实例与该指南配合使用，并跳过此第一步。请确保在继续执行指南时使用正确的服务名称和 {{site.data.keyword.Bluemix_notm}} 空间。
{: tip}
1. 在命令行中，通过运行 cf api 命令来设置 API 端点。   
将 `API-ENDPOINT` 值替换为您区域的 API 端点。
   ```
cf api API-ENDPOINT
   ```
示例：`cf api https://api.ng.bluemix.net`
<table>
<tr>
<th>区域</th>
<th>API 端点</th>
</tr>
<tr>
<td>美国南部</td>
<td>https://api.ng.bluemix.net</td>
</tr>
<tr>
<td>英国</td>
<td>https://api.eu-gb.bluemix.net</td>
</tr>
 <!--<tr>
 <td>Germany</td>
 <td>https://api.eu-de.bluemix.net</td>
 </tr>-->
</table>
2. 登录到 {{site.data.keyword.Bluemix_notm}} 帐户。
  ```
cf login
  ```
如果出现提示，请选择要在其中部署 {{site.data.keyword.iot_short_notm}} 和样本应用程序的组织和空间。
3. 将 {{site.data.keyword.iot_short_notm}} 服务部署到 {{site.data.keyword.Bluemix_notm}}。  
   ```
cf create-service iotf-service iotf-service-free YOUR_IOT_PLATFORM_NAME
  ```
对于 YOUR_IOT_PLATFORM_NAME，请使用 *iotp-for-conveyor*。
示例：“cf create-service iotf-service iotf-service-free iotp-for-conveyor”
3. 创建样本传送带设备。
 - 路径 A：[步骤 2A - 使用来自 GitHub 的模拟器样本应用程序](#deploy_app)。
 - 路径 B：[步骤 2B - 使用 Raspberry Pi 和电动机构建物理传送带](#raspberry)。

## 步骤 2A - 部署样本传送带 Web 应用程序
{: #deploy_app}

样本应用程序使您能够模拟 {{site.data.keyword.Bluemix_notm}} 连接的工业传送带。您可以启动和停止传送带，并调整传送带的速度。对传送带的每个更改都将以应用程序中显示的 MQTT 消息的形式发送到 {{site.data.keyword.Bluemix_notm}}。您可以使用缺省仪表板卡来监视传送带行为。样本应用程序使用以下地址的 Node.js 客户机库进行构建：[https://github.com/ibm-watson-iot/iot-nodejs ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://github.com/ibm-watson-iot/iot-nodejs){: new_window}

![传送带应用程序](images/app_conveyor_belt.png "传送带应用程序")

1. 使用您最喜欢的 Git 工具来克隆以下存储库
https://github.com/ibm-watson-iot/guide-conveyor-simulator
在 Git shell 中，使用以下命令：
  ```bash
git clone https://github.com/ibm-watson-iot/guide-conveyor-simulator
  ```
2. 在命令行上，将目录切换至样本应用程序所在的目录。
  ```bash
cd guide-conveyor-simulator
  ```
3. 在 *guide-multry-simulator* 目录中，将应用程序推送到 {{site.data.keyword.Bluemix_notm}}，并通过在 cf push 命令中替换 *YOUR_APP_NAME*，为其提供新名称。请使用“--no-start”选项，因为在应用程序绑定到 {{site.data.keyword.iot_short_notm}} 后，您将在下一个阶段启动应用程序。
**注：**部署应用程序可能需要几分钟时间。
```bash
cf push YOUR_APP_NAME --no-start
  ```  
1. 在 *guide-multry-simulator* 目录中，使用为每个应用程序提供的名称，将应用程序绑定到 {{site.data.keyword.iot_short_notm}} 的实例。
```bash
cf bind-service YOUR_APP_NAME iotp-for-conveyor
  ```
有关绑定应用程序的更多信息，请参阅[连接应用程序](/docs/services/IoT/platform_authorization.html#bluemix-binding)。
2. 启动应用程序以使该绑定生效。
  ```bash
cf start YOUR_APP_NAME
  ```
在此阶段，样本 Web 应用程序部署在 {{site.data.keyword.Bluemix_notm}} 上。部署完成后，将显示一条消息以指示应用程序正在运行。
示例：
  ```bash
name:              YOUR_APP_NAME
requested state:   started
instances:         1/1
usage:             64M x 1 instances
routes:            YOUR_APP_NAM.mybluemix.net
last uploaded:     Tue 09 May 09:29:49 EDT 2017
stack:             cflinuxfs2
buildpack:         SDK for Node.js(TM) (ibm-node.js-4.8.0,
                   buildpack-v3.11-20170303-1144)
start command:     ./vendor/initial_startup.rb

     

state     since                  cpu    memory         disk            details
#0   running   2017-05-09T13:35:08Z   0.0%   19.6M of 64M   66.2M of 256M
  ```
要查看应用程序部署状态和应用程序 URL，可以运行以下命令：
  ```bash
cf apps
  ```
使用“cf logs YOUR_APP_NAME --recent”命令，对部署过程中的错误进行故障诊断。
{: tip}
1. 在浏览器中，访问应用程序。
打开以下 URL：“https://YOUR_APP_NAME.mybluemix.net”
示例：“https://conveyorbelt.mybluemix.net/”。
2. 输入设备的设备标识和标记。
样本应用程序会自动注册具有您所提供的设备标识和标记且类型为“iot-conveyor-belt”的设备。
有关注册设备的更多信息，请参阅[连接设备](/docs/services/IoT/iotplatform_task.html#iotplatform_subtask1)。
4. 继续执行[步骤 3 - 查看 {{site.data.keyword.iot_short_notm}} 中的原始数据](#see_live_data)。

## 步骤 2B - 构建 Raspberry Pi 驱动的传送带
{: #raspberry}

Raspberry Pi 解决方案使用 Python 客户机库进行构建，网址为：[https://github.com/ibm-watson-iot/iot-python ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://github.com/ibm-watson-iot/iot-python){: new_window}

### 电路示意图

![电路图](images/raspi_circuit.png)

### 所需连接

需要 Raspberry Pi 和 L298N 双 H 网桥电机电路板之间的以下连接。连接：

- Raspberry Pi 的引脚 2 接入 L298N 板上 +5v
- Raspberry Pi 的引脚 6 接入 L298N 板上 GND
- Raspberry Pi 的引脚 13 接入 L298N 板的 IN1
- Raspberry Pi 的引脚 15 接入 L298N 板的 IN2
- 电源的 +9v 接入 L298N 板上的 +12v
- 电源的 -9v 接入 L298N 板上的 GND
- 电机的 +ve 节点接入 L298N 板上的 OUT1
- 电机的 -ve 节点接入 L298N 板上的 OUT2

### 硬件需求

1. [Raspberry Pi(2/3) ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://www.raspberrypi.org/){: new_window}（具有 Raspbian Jessie (8.x)）。
2. [L298N 双 H 桥电机驱动器主板 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://tronixlabs.com.au/robotics/motor-controllers/l298n-dual-motor-controller-module-2a-australia/){: new_window}
3. 9V DC 电机
4. 9V 电池

### Raspberry Pi 软件需求
- Python 2.7.9 和以上版本，与 Raspbian Jessie (8.x) 一起安装。
- [Watson IoT Python 库 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://github.com/ibm-watson-iot/iot-python){: new_window}
- 可选：Git。
如果未在 Raspberry Pi 上安装 Git，那么可以使用以下命令进行安装：
```bash
$ sudo apt-get install git
```

### 详细步骤

1. 在 Raspberry Pi 中打开终端或 SSH。
2. 使用您最喜欢的 Git 工具，将以下存储库克隆到 Raspberry Pi：
https://github.com/ibm-watson-iot/guide-conveyor-rasp-pi
在 Git Shell 中，使用以下命令：
```bash
$ git clone https://github.com/ibm-watson-iot/guide-conveyor-rasp-pi
```
3. 向 {{site.data.keyword.iot_short_notm}} 注册设备。
有关注册设备的更多信息，请参阅[连接设备](/docs/services/IoT/iotplatform_task.html#iotplatform_subtask1)。
	 1. 在 {{site.data.keyword.Bluemix_notm}} 控制台中，单击 {{site.data.keyword.iot_short_notm}} 服务详细信息页面上的**启动**。
	     此时，{{site.data.keyword.iot_short_notm}} Web 控制台将在新浏览器选项卡中打开，URL 如下：
	     ```
	     https://ORG_ID.internetofthings.ibmcloud.com/dashboard/#/overview
	     ```
	     其中，ORG_ID 是 [{{site.data.keyword.iot_short_notm}} 组织](/docs/services/IoT/iotplatform_overview.html#organizations){: new_window}的唯一六字符标识。
	 2. 在“概述”仪表板中，从菜单窗格选择**设备**，然后单击**添加设备**。
	 3. 为要添加的设备创建设备类型。
	     1. 单击**创建设备类型**。
	     2. 输入设备类型名称 `iotp-for-conveyor` 以及设备类型描述。  
	**提示：**您可以输入任何设备类型名称，但系列中的其他指南需要使用设备类型 `iotp-for-conveyor` 的设备。如果使用其他设备类型，那么必须相应地修改其他指南中的设置。
	     
	     3. 可选：输入设备类型属性和元数据。
	 4. 单击**下一步**以开始添加具有所选设备类型的设备的过程。
	 5. 输入设备标识，例如 `conveyor_belt`。
	 5. 单击**下一步**以完成该过程。
	 6. 提供认证令牌或接受自动生成的令牌。
	 7. 验证摘要信息是否正确，然后单击**添加**以添加连接。
	 8. 在设备信息页面中，复制并保存以下详细信息：
	     * 组织标识
	     * 设备类型
	     * 设备标识
	     * 认证方法
	     * 认证令牌
	     您将需要“组织标识”、“设备类型”、“设备标识”和“认证令牌”的值来配置设备以连接到 {{site.data.keyword.iot_short_notm}}。
4. 导航到所克隆存储库的 *guide-conveyor-rasp-pi* 根下。
5. 使用 `sudo chmod +x setup.sh` 命令使 shell 脚本可执行。
6. 运行 *setup.sh* 文件，并在提示时输入从设备信息页面复制的详细信息。
```bash  
./setup.sh
```  

7. 运行 deviceClient 程序。  
当您运行该程序时，您的 Raspberry Pi 将启动电机，它将根据参数设置最多运行 2 分钟。

```bash
python deviceClient.py -t 2
```

在电动机运行期间，该程序会发布具有以下样本有效内容结构且事件类型为 `sensorData` 的事件：  
```
{
"d": {
        "elapsed": 1,
  "running": true,
  "temperature": 35.00,
  "ay": "0.00",
  "rpm": "0.6"
}
}
```

8. 继续执行[步骤 3 - 查看 {{site.data.keyword.iot_short_notm}} 中的原始数据](#see_live_data)。

## 步骤 3 - 查看 {{site.data.keyword.iot_short_notm}} 中的原始数据
{: #see_live_data}
1. 验证是否已向 {{site.data.keyword.iot_short_notm}} 注册设备。
 
 1. 登录到 {{site.data.keyword.Bluemix_notm}}“仪表板”，网址为：https://bluemix.net。
 2. 在[服务列表 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://bluemix.net/dashboard/services){: new_window} 中，单击 *iotp-for-conveyor* {{site.data.keyword.iot_short_notm}} 服务。
 3. 单击*启动*在新的浏览器选项卡中打开 {{site.data.keyword.iot_short_notm}} 仪表板。  
您可以为 URL 添加书签，以供稍后轻松访问。
   
示例：`https://*iot-org-id*.internetofthings.ibmcloud.com`。
 4. 从菜单中，选择**设备**并验证新设备是否显示。
2. 查看原始数据
 1. 从菜单中，选择**板**。
 3. 选择**设备中心分析**板。
 4. 找到**我关心的设备**卡，并选择设备。  
设备名会显示在“设备属性”卡中。

4. 将传感器数据发送到平台。   
当传感器读数更改时，设备会将数据发送至 {{site.data.keyword.iot_short_notm}}。您可以通过停止、启动或更改传送带的速度来模拟此数据的发送。   
**路径 A：**如果您正在移动浏览器上访问应用程序，请尝试通过摇一摇智能手机来触发传送带的加速度计数据。
{: tip}
3. 验证与已发布消息对应的已更新设备数据点是否显示在“设备属性”卡中。
  
消息示例 A：
  
  ```
{
	"d": {
			"id": "belt1",
		"ts": 1494341288662,
		"ay": "0.00",
		"running": true,
		"rpm": "0.6"
	}
}
  ```
消息示例 B：
  ```
{
	"d": {
        "elapsed": 1,
    "running": true,
    "temperature": 35.00,
    "ay": "0.00",
    "rpm": "0.6"
  }
}
  ```


## 步骤 4 - 可视化 {{site.data.keyword.iot_short_notm}} 中的实时数据
{: #add_card}
要创建仪表板卡来查看实时传送带数据，请执行以下操作：
1. 在同一“设备集中分析”板上，单击**添加新卡**，然后选择**折线图**。
2. 对于卡源数据，单击**卡**。   
此时将显示卡名称列表。

3. 选择**我关心的设备**，然后单击**下一步**。
4. 单击**连接新数据集**，然后为数据集参数输入以下值：
  - 事件：sensorData
  - 属性：d.rpm
  - 名称：Belt RPM
  - 类型：浮点数
  - 单位：rpm
5. 单击**下一步**。
6. 在卡预览页面上，选择 **L**，然后单击**下一步**。
7. 在卡信息页面上，将标题的名称更改为 `Belt data`，然后单击**提交**。
8. 更改传送带的速度，以查看新卡中的实时数据。
9. 可选：添加第二个数据集，以添加传送带的加速度数据。  
如果使用电话连接到样本应用程序，那么可以摇一摇电话，以发送传送带的加速度数据。
 
 1. 单击卡上的菜单图标并选择编辑卡。
 2. 对于卡源数据，选择**卡**。   
 3. 选择**我关心的设备**，然后单击**下一步**。
 4. 单击**连接新数据集**，然后输入以下值：
    - 事件：sensorData
    - 属性：d.ay
    - 名称：Accel. Y
    - 类型：浮点数
    - 单位：gs
 5. 单击**下一步**。
 6. 仅限路径 A：摇一摇电话以查看新卡中的实时加速度计数据。
有关创建板和卡的更多信息，请参阅[使用板和卡实现实时数据可视化](/docs/services/IoT/data_visualization.html#boards_and_cards)。

## 后续步骤
{: @whats_next}  
继续执行下一个指南，或跳至您感兴趣的其他主题：
- 路径 A：修改传送带应用程序以满足您的需要。  
有关技术详细信息，请参阅：
 - [https://github.com/ibm-watson-iot/guide-conveyor-simulator/blob/master/README.md ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://github.com/ibm-watson-iot/guide-conveyor-simulator/blob/master/README.md){: new_window}
 - [Node.js Client Library ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://github.com/ibm-watson-iot/iot-nodejs){: new_window}
 
 **注：**Bluemix 现在为 IBM Cloud。有关更多详细信息，请参阅 [IBM Cloud 博客](https://www.ibm.com/blogs/bluemix/2017/10/bluemix-is-now-ibm-cloud/){: new_window} ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")。
 
- 路径 B：修改 Raspberry Pi 设置以满足您的需要。
  
有关技术详细信息，请参阅：
 - [https://github.com/ibm-watson-iot/guide-conveyor-rasp-pi/blob/master/README.md ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://github.com/ibm-watson-iot/guide-conveyor-rasp-pi/blob/master/README.md){: new_window}
- [指南 2：使用基本实时规则和操作](getting-started-iot-rules.html)  
既然您已成功设置传送带，将其连接到 {{site.data.keyword.iot_short_notm}} 并发送一些数据，那么是时候通过使用规则和操作来使数据运作。

**注：**Bluemix 现在为 IBM Cloud。有关更多详细信息，请参阅 [IBM Cloud 博客](https://www.ibm.com/blogs/bluemix/2017/10/bluemix-is-now-ibm-cloud/){: new_window} ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")。

- [指南 3：监视设备数据](getting-started-iot-monitoring.html)  
现在，您已连接一个或多个设备并开始充分利用设备数据，现在是时候开始监视设备集合了。
- [指南 4：模拟大量设备](getting-started-iot-large-scale-simulation.html)  
通过路径 A 中的传送带样本应用程序，您可以手动模拟一个或几个传送带设备。本指南使您能够设置具有大量设备的模拟环境。

- [了解有关 {{site.data.keyword.iot_short_notm}}](/docs/services/IoT/iotplatform_overview.html) 的更多信息
- [了解有关 {{site.data.keyword.iot_short_notm}} API](/docs/services/IoT/reference/api.html) 的更多信息
