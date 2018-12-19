---

copyright:
  years: 2017, 2018
lastupdated: "2018-05-17"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# 指南 2：监视设备数据
既然您已连接了一个或多个设备，现在是时候开始实时监视设备数据了。
{:shortdesc}

## 概述和目标
{: #overview}  

在本指南中，您将在 {{site.data.keyword.Bluemix}} 上部署监视应用程序以从设备查看数据。

与指南 1 中所示，可以遵循以下一个或两个路径：
- 路径 A：[步骤 1A - 部署并连接监视 Web 应用程序](#deploy_app)  
通过以下路径，您将部署现成可用的应用程序，用于监视您在组织中运行的传送带设备。
- 路径 B：[步骤 1B - 使用窗口小部件库创建监视用户界面](#widget-library)
略微更复杂的路径 B 引入了窗口小部件库，并引导您创建一些基本用户界面。

## 先决条件
{: #prereqs}  
开始之前，请确保满足以下需求。

### 本地环境
- [Node.js ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://nodejs.org){: new_window} V6.x 或更高版本。
- 路径 A：[Angular CLI ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://github.com/angular/angular-cli){: new_window} V1.x 或更高版本。  
- [Cloud Foundry CLI ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://github.com/cloudfoundry/cli#downloads){: new_window}  
使用 cf CLI 将应用程序和服务部署到 {{site.data.keyword.Bluemix_notm}}。有关更多信息，请参阅 [Cloud Foundry CLI 文档 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://docs.cloudfoundry.org/cf-cli/){: new_window}  
- 可选：[Git ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://git-scm.com/downloads){: new_window}  
如果选择使用 Git 来下载代码样本，那么还必须具有 [GitHub.com 帐户 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://github.com){: new_window}。如果没有 GitHub.com 帐户，您还可以将代码下载为压缩文件。

### 其他要求
您还必须具有设备类型为 `iot-conveyor-belt` 的已连接设备，该设备发送事件名称为 `sensorData` 的事件，并带有包括以下属性的消息有效内容：
```
{
	"d": {
			"id": "belt1",
		"ts": 1494946276931,
		"ay": "0.00",
		"running": true,
		"rpm": "1.0"
		}
}
```
有关设备事件和消息传递格式的更多信息，请参阅[发布事件](/docs/services/IoT/devices/mqtt.html#publishing_events)。  
如果您已完成[指南 1：{{site.data.keyword.iot_short_notm}} 和模拟传送带入门](getting-started-iot-conveyor.html)，那么表明您已设置完毕。  
{: tip}

## 步骤 1A - 部署和连接监视 Web 应用程序
{: #deploy_app}

Plant Floor Monitoring 样本应用程序列出所有 iot-conveyor-belt 类型的设备，这些设备使用事件数据的子集（如 RPM、上次更新和设备标识）连接到 {{site.data.keyword.iot_full}} 组织。

样本应用程序是使用 Node.js 客户机库构建的，网址为：[https://github.com/ibm-watson-iot/iot-nodejs ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://github.com/ibm-watson-iot/iot-nodejs){: new_window}

![基于 Node.js 的监视应用程序](images/app_monitor.png "基于 Node.js 的监视应用程序")

作为此步骤的一部分，您将：
- 使用 Cloud Foundry 部署来自 GitHub 的样本监视 Web 应用程序。
- 使用 API 密钥和认证令牌配置样本应用程序以连接到 {{site.data.keyword.iot_short_notm}}。
- 使用 Web 应用程序来监视所连接的传送带设备。  

### 部署和连接监视 Web 应用程序的详细步骤
以下步骤将指导您在 {{site.data.keyword.Bluemix_notm}} 上创建和部署应用程序。有关在本地运行应用程序的信息，请参阅 GitHub 中的自述文件。
1. 克隆 Node.js *Plant Floor Monitoring* 样本应用程序 GitHub 存储库。  
使用您最喜欢的工具来克隆以下存储库：  
https://github.com/ibm-watson-iot/guide-conveyor/tree/master/ui-angular
在 Git shell 中，使用以下命令：
```bash
git clone https://github.com/ibm-watson-iot/guide-conveyor/tree/master/ui-angular
  ```
2. 为应用程序创建 API 密钥和认证令牌组合。  
配置应用程序以连接到组织时，您将需要这些信息。有关注册设备的更多信息，请参阅[连接应用程序](/docs/services/IoT/platform_authorization.html)。  
 1. 打开 {{site.data.keyword.iot_short_notm}} 仪表板。
 2. 选择**应用程序**。
 3. 单击**生成 API 密钥**
 4. 复制列出的 API 密钥和认证令牌值。
 5. 选择**可视化应用程序**作为 API 角色。  
**提示：**如果向应用程序添加更多功能，那么需要将其提升到更高的角色。
 6. 添加注释，以便您可以轻松识别此 API 密钥和认证令牌组合。
 7. 单击**生成**。
3. 配置应用程序以连接到 {{site.data.keyword.Bluemix_notm}}。浏览到 *guide-conveyor-ui-angular* 存储库的根目录，并创建包含以下内容的 `basicConfig.json` 文件：
  ```
{
  "org": "your orgID",
  "apiKey": "your API key",
  "apiToken": "your Authentication Token"
}
  ```
将参数值替换为您刚刚创建的 {{site.data.keyword.Bluemix_notm}} 组织的对应值：orgID、API 密钥和认证令牌。  
示例：
```
 {   
  "org": "3v5whr",    
  "apiKey": "a-3v5whr-jhkmsghlqv",  
  "apiToken": "-q0MkPN2cNYB6+?ISk"  
}
```
4. 使用 cloudfoundry CLI 登录到 {{site.data.keyword.Bluemix_notm}} 帐户。  
有关更多信息，请参阅 [Cloud Foundry CLI 文档 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://docs.cloudfoundry.org/cf-cli/){: new_window}  
从命令行输入以下命令：  
  ```
cf login
  ```
如果出现提示，请选择要在其中部署监视样本应用程序的组织和空间。
5. 如有必要，请通过运行 cf api 命令来设置 API 端点。   
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
6. 将目录更改为样本应用程序所在的目录。  
  ```
cd guide-conveyor-ui-angular
  ```
7. 运行 `npm install -g @angular/cli` 以安装 Angular CLI。
8. 运行 `npm install`。
9. 运行 `npm run push` 以构建项目并推送到您的组织。  
已在 {{site.data.keyword.Bluemix_notm}} 上部署样本 Web 应用程序。  
部署完成后，将显示一条消息以指示应用程序正在运行。   
示例：  
  ```
requested state: started
instances: 1/1
usage: 64M x 1 instances
urls: iotmonitoringcontrol-undertided-eng.mybluemix.net
last uploaded: Tue May 16 19:01:13 UTC 2017
stack: cflinuxfs2
buildpack: https://github.com/cloudfoundry/nodejs-buildpack
     state     since                    cpu    memory     disk        details
#0   running   2017-05-16 03:03:05 PM   0.0%   0 of 64M   0 of 256M
  ```
10. 打开 Web 应用程序。  
在 {{site.data.keyword.Bluemix_notm}} 应用程序仪表板中，单击**访问应用程序 URL**，以打开 Web 应用程序。  
通过单击**路径**来访问和管理应用程序 URL。   
缺省 URL 类似于：  
`https://iotmonitoringcontrol-RANDOM-STRING-ENG.mybluemix.net`

## 步骤 1B - 使用窗口小部件库创建监视用户界面
{: #widget-library}

基于样本应用程序的窗口小部件库包括电机转速计、加速计量表数据量表和电机速度图，其中显示连接到 {{site.data.keyword.iot_short_notm}} 组织的单个 iot-conveyor-belt 类型设备的数据。您可以使用样本代码为您的 {{site.data.keyword.iot_short_notm}} 已连接设备构建完整的前端应用程序。

![基于监视应用程序的窗口小部件库](images/app_monitor_b.png "基于监视应用程序的窗口小部件库")

作为此步骤的一部分，您将：
- 使用 Cloud Foundry 部署来自 GitHub 的样本 Web 应用程序。
- 使用 API 密钥和认证令牌配置样本应用程序以连接到 {{site.data.keyword.iot_short_notm}}。
- 配置三个用户界面窗口小部件，以将设备数据显示为量表和图形。
- 使用 Web 应用程序来监视所连接的传送带设备。  

### 使用窗口小部件库创建监视用户界面的详细步骤
以下步骤将指导您在 {{site.data.keyword.Bluemix_notm}} 上创建和部署应用程序。有关在本地运行应用程序的信息，请参阅 GitHub 中的自述文件。
1. 克隆 *Widget Library Monitoring* 样本应用程序 GitHub 存储库。  
使用您最喜欢的工具来克隆以下存储库：  
https://github.com/ibm-watson-iot/guide-conveyor/tree/master/ui
在 Git Shell 中，使用以下命令：
```
git clone https://github.com/ibm-watson-iot/guide-conveyor/tree/master/ui
```
2. 安装应用程序依赖关系。  
浏览至 *guide-conveyor-ui-html* 存储库的根目录，然后运行以下命令：
```
npm install
```
3. 构造用户界面。  
要构建应用程序用户界面，必须在每个用户界面组件的应用程序 index.html 文件中，添加窗口小部件作为 JavaScript 代码。  
每个窗口小部件使用以下 JavaScript 参数：  
`WIoTPWidget.CreateWIDGET_TYPE("ELEMENT_ID","EVENT_NAME", "DEVICE_TYPE", "DEVICE_ID", "PROPERTY" , {WIDGET_DEFAULT_OVERRIDE}, [WIDGET_SPECIFIC_SETTINGS])`

下表提供了参数的描述：

|参数|描述|    
| ----- | ---- |   
|WIDGET_TYPE |要创建的窗口小部件的类型。示例：`Gauge` 或 `Chart` |
|ELEMENT_ID |窗口小部件显示在应用程序中的元素标识。示例：`RPM` |
|EVENT_NAME |包含要显示的属性的设备事件名称。示例：`sensorData` |
|DEVICE_TYPE |设备类型。示例：`iot-conveyor-belt` |
|DEVICE_ID |提供要显示的数据的设备标识。示例：`belt1` |
|PROPERTY |要显示的设备消息有效内容属性。示例：`rpm` |
|WIDGET_DEFAULT_OVERRIDE |窗口小部件配置设置，用于覆盖缺省设置。|
|WIDGET_SPECIFIC_SETTINGS |窗口小部件的一个或多个其他参数，请参阅示例。|

有关每个窗口小部件类型的详细信息，请参阅后面的示例以及 [IoT窗口小部件 GitHub 存储库 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://github.com/ibm-watson-iot/iot-widgets){: new_page} 中的文档。
 1. 添加 RPM 量表。  
此量表以最小值为 0，最大值为 5 rpm 的量表显示传送带 rpm。
    1. 打开以下模板以进行编辑：`public/index.html`  
    2. 找到 rpm 量表占位符：`<!--- place holder for rpm gauge  -->`
    3. 添加具有唯一标识的以下 div 元素，如下所示：
 ```html
 <div id="rpmgauge" ></div>
 ```  
    3. 找到 JavaScript 占位符：`/// Add your scripts here`
    4. 添加 rpm JavaScript 代码。  
示例：  
 ```javascript
 WIoTPWidget.CreateGauge("rpmgauge","sensorData", "iot-conveyor-belt", "belt1", "rpm" ,{
            label: {
                format: function(value, ratio) {
                    return value;
                },
                show: true // to turn off the min/max labels.
            },
         min: 0.0, // 0 is default, can handle negative min e.g. vacuum / voltage / current flow / rate of change
         max: 5.0, // 100 is default
         units: 'rpm'
       },['#FF0000', '#F97600', '#F6C600', '#60B044']);
 ```
 2. 添加加速量表。  
此量表将加速计量表读数显示为具有读数 -1 到 1 之间的量表。
    1. 找到加速计量表占位符：`<!--- place holder for accelerometer gauge  -->`
    2. 添加具有唯一标识的以下 div 元素，如下所示：
 ```html
 <div id="aygauge" ></div>
 ```  
    3. 找到 javascript 占位符：`/// Add your scripts here`
    4. 添加加速计量表 JavaScript 代码。  
示例：   
 ```javascript
 WIoTPWidget.CreateGauge("aygauge","sensorData", "iot-conveyor-belt", "belt1", "ay" ,{
      label: {
          format: function(value, ratio) {
              return value;
          },
          show: true // to turn off the min/max labels.
      },
   min: -1.0, // 0 is default,can handle negative min e.g. vacuum / voltage / current flow / rate of change
   max: 1.0, // 100 is default
   units: 'g'//,
 },['#FF0000', '#F97600', '#F6C600', '#60B044']);
 ```
 3. 添加电机速度表。  
此图表以折线图显示电机速度。
    1. 找到电机速度量表占位符：`<!--- place holder for motor speed gauge  -->`
    2. 添加具有唯一标识的以下 div 元素，如下所示：
 ```html
 <div id="speedchart" ></div>
 ```  
    3. 找到 JavaScript 占位符：`/// Add your scripts here`
    4. 添加快速图表 JavaScript 代码。  
示例：  
 ```javascript
 WIoTPWidget.CreateChart("speedchart ","sensorData", "iot-conveyor-belt", "belt1",
 ["rpm", "ay"], [["line","rpm"],["line","ay"]],['#2ca02c','#d62728']);
 ```
4. 将应用程序部署到 {{site.data.keyword.Bluemix_notm}}  
 1. 使用 {{site.data.keyword.iot_short_notm}} 服务名称更新 manifest.yml 文件。  
例如，如果部署了 {{site.data.keyword.iot_short_notm}} 服务作为[指南 1：连接传送带设备](getting-started-iot-monitoring.html)，那么 YOUR_PLATFORM_NAME 是 `iotp-for-conveyor`。
<pre><code>
declared-services:
  YOUR_IOT_PLATFORM_NAME:  </br>
    label: iotf-service  </br>
    plan: iotf-service-free  </br>
applications:  </br>
\- path: .  </br>
  memory: 128M  </br>
  instances: 1  </br>
  domain: mybluemix.net  </br>
  disk_quota: 1024M  </br>
  services:  </br>
  \- YOUR_IOT_PLATFORM_NAME  </br>
</pre></code>
 2. 使用 cloudfoundry CLI 登录到 {{site.data.keyword.Bluemix_notm}} 帐户。  
有关更多信息，请参阅 [Cloud Foundry CLI 文档 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://docs.cloudfoundry.org/cf-cli/){: new_window}  
从命令行输入以下命令：  
   ```
 cf login
   ```
如果出现提示，请选择要在其中部署监视样本应用程序的组织和空间。
 5. 如有必要，请通过运行 cf api 命令来设置 API 端点。   
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
 6. 将目录更改为样本应用程序所在的目录。  
   ```
 cd guide-conveyor-ui-html
   ```
 2. 运行 cf push 命令以将应用程序推送到 {{site.data.keyword.Bluemix_notm}}：  
为应用程序提供一个唯一的名称。
```
cf push YOUR_APP_NAME
```
5. 打开基于窗口小部件库的 Web 应用程序。  
在 {{site.data.keyword.Bluemix_notm}} 应用程序仪表板中，单击**访问应用程序 URL**，以打开 Web 应用程序。  
通过单击**路径**来访问和管理应用程序 URL。   
缺省 URL 类似于：  
`https://YOUR_APP_NAME.mybluemix.net`

## 步骤 2 - 查看已连接的设备
{: #view_devices}

现在，Web 控制台已启动并正在运行，您可以查看已连接的传送带设备。
1. 在 Web 控制台的**设备**部分中，验证是否列出了传送带且显示了正确的 RPM 和“正在运行”状态。
2. 更改传送带设备的 RPM 值，并验证该值在监视应用程序上是否按预期更新。


## 后续步骤
{: @whats_next}  
继续执行下一个指南，或跳至您感兴趣的其他主题：
- 路径 A：修改监视应用程序以满足您的需要。  
有关技术详细信息，请参阅：
 - [https://github.com/ibm-watson-iot/guide-conveyor/tree/master/ui-angular/blob/master/README.md![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://github.com/ibm-watson-iot/guide-conveyor/tree/master/ui-angular/blob/master/README.md){: new_window}
 - [Node.js 客户机库 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://github.com/ibm-watson-iot/iot-nodejs){: new_window}
- 路径 B：修改窗口小部件库应用程序以满足您的需求。  
有关技术详细信息，请参阅：
 - [https://github.com/ibm-watson-iot/guide-conveyor/tree/master/ui/blob/master/README.md![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://github.com/ibm-watson-iot/guide-conveyor/tree/master/ui/blob/master/README.md){: new_window}
- [指南 3：模拟大量设备](getting-started-iot-large-scale-simulation.html)  
通过向环境中添加大量自运行模拟器来扩展基本模拟。
- [了解有关 {{site.data.keyword.iot_short_notm}}](/docs/services/IoT/iotplatform_overview.html){:new_window} 的更多信息
- [了解有关 {{site.data.keyword.iot_short_notm}} API](/docs/services/IoT/reference/api.html){:new_window} 的更多信息
