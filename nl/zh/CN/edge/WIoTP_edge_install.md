---

copyright:
  years: 2018
lastupdated: "2018-07-11"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# 在设备上安装 {{site.data.keyword.iot_short_notm}} Edge（预览）
{: #edge_install_device}

**重要信息：**{{site.data.keyword.iot_full}} Edge 功能只作为受限预览程序的一部分提供。未来更新可能会包含与此功能当前版本不兼容的更改。请尝试此功能，[让我们了解您的想法 ![外部链接图标](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}。

为了使 {{site.data.keyword.iot_short_notm}} Edge（预览）机器能够通过 {{site.data.keyword.iot_short_notm}} 的 IBM Horizon 进行管理，您必须首先在其上安装 Horizon+WIoTP 软件，然后向 Horizon Exchange 注册该机器。通过以下步骤，可以在临时测试环境中使用预览开发存储库中的应用程序。

## 安装之前

### 基于 Ubuntu 的边缘节点的先决条件

- 如果尚未安装 Docker，请将 Docker 添加到注册表：

`wget -qO- https://download.docker.com/linux/ubuntu/gpg| apt-key add - && add-apt-repository "deb [arch=$(dpkg --print-architecture)] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"`

- 添加基本 Horizon 软件包（ANAX 代理程序）

```
wget -qO - http://pkg.bluehorizon.network/bluehorizon.network-public.key | apt-key add -
   cat <<EOF > /etc/apt/sources.list.d/bluehorizon.list
   deb [arch=$(dpkg --print-architecture)] http://pkg.bluehorizon.network/linux/ubuntu $(lsb_release -cs)-updates main
EOF
```

### Raspberry PI 的先决条件

- 将以下行添加到 `/etc/apt/sources.list.d/bluehorizon.list`：
```
deb [arch=armhf] http://pkg.bluehorizon.network/linux/raspbian jessie-testing main
deb-src [arch=armhf] http://pkg.bluehorizon.network/linux/raspbian jessie main
deb [arch=armhf] http://pkg.bluehorizon.network/linux/raspbian jessie-updates main
deb-src [arch=armhf] http://pkg.bluehorizon.network/linux/raspbian jessie-updates main
```

- 将以下行添加到 `/etc/apt/sources.list.d/docker.list`：

`deb [arch=armhf] https://download.docker.com/linux/raspbian stretch edge`

- 安装 `docker-ce`：

`apt-get update && apt-get install docker-ce`


## 安装 Horizon 和 WIoTP Horizon 软件包

`apt-get update && apt-get install -y horizon-wiotp`

## 注册边缘节点

运行代理程序设置：

对于生产环境，请运行：
`wiotp_agent_setup --org <orgId> --deviceType <deviceType> --deviceId <deviceId> --deviceToken <deviceToken>`

这将执行以下步骤：
- 配置 WIoTP Edge Core IoT 工作负载。
- 生成 edge-mqttbroker 证书。
- 使用 hzn 工具（使用 `hzn --help` 以获取更多信息）向 WIoT Platform 注册节点。

如果在设备中找到了多个 IP 地址，系统将提示您选择一个 IP 地址来生成内部证书。稍后将使用此 IP 来测试边缘节点与发送数据的设备之间的连接。

确保执行期间没有任何错误消息。

稍等几秒钟，然后验证边缘组件是否已创建。

检查是否已创建了边缘组件映像：`docker images`（TAG 版本可能有所不同）。

**注**：下载映像和启动容器的过程可能需要几分钟时间。请检查 syslog 文件以查看日志记录详细信息。

```
# docker images
REPOSITORY                                        TAG                 IMAGE ID            CREATED             SIZE
wiotp-connect/edge/amd64/edge-connector           1.0.1               3d98b90aaa16        3 hours ago         31.7MB
wiotp-infomgmt/edge/amd64/edge-simulated-im       1.0.3               6af448fb2204        7 hours ago         27.6MB
wiotp-connect/edge/amd64/edge-mqttbroker          1.0.2               1973bc0f3d8a        7 hours ago         31.2MB
```

检查容器是否已启动：`docker ps`

示例：
```
# docker ps
CONTAINER ID        IMAGE                                                   COMMAND             PORTS
7cbfc719cb71        wiotp-infomgmt/edge/amd64/edge-simulated-im:1.0.1       "/start.sh"
a40f926f78f3        wiotp-connect/edge/amd64/edge-connector:1.0.0           "/start.sh"         0.0.0.0:1883->1883/tcp, 0.0.0.0:8883->8883/tcp
22d8e82f09f3        wiotp-connect/edge/amd64/edge-mqttbroker:1.0.1          "/start.sh"
```

## 模拟边缘节点上的设备

可以使用 mosquitto-clients 来模拟连接到边缘节点的设备：

`apt-get install mosquitto-clients`

在边缘节点上运行以下命令（将 OrgId、DeviceType、DeviceID、DeviceToken 和 EdgeNodeIP 替换为相应的值）。
要了解需要传递的 IP，请检查运行 `wotp_agent_setup` 命令后，在 `Generating Edge internal certificates` 部分下显示的消息。

```
mosquitto_pub -p 8883 -i 'd:<orgId>:<deviceType>:<deviceId>' -u 'use-token-auth' -P '<deviceToken>' -t 'iot-2/evt/evTest/fmt/json' -m '{"message": "Hello World"}' --cafile /var/wiotp-edge/persist/dc/ca/ca.pem -h <EdgeNodeIP>
```

示例：

```
mosquitto_pub -p 8883 -i 'd:nwr48y:myDevice:device1' -u 'use-token-auth' -P '12345678' -t 'iot-2/evt/evTest/fmt/json' -m '{"message": "Hello World"}' --cafile /var/wiotp-edge/persist/dc/ca/ca.pem -h 10.0.2.15
```

注：如果在边缘节点外部运行 mosquitto_pub，请使用 -h `<host>` 选项，并调整 --cafile 路径。

### 注销边缘节点

要注销边缘节点，请运行以下命令：

`hzn unregister`

### 卸载 Horizon-wotp

要从节点中完全除去 Horizon 代理程序，请执行以下操作：

`apt-get purge -y horizon horizon-wiotp`

### 故障诊断

检查 Horizon 代理程序版本：

`dpkg -l | grep horizon`

检查 Horizon 代理程序实时日志：

`journalctl -af -u horizon.service`

收集 syslog：

`tail -n <num-max-of-lines> /var/log/syslog `

按容器过滤日志消息：

`tail -n <num-max-of-lines> /var/log/syslog | grep 'edge-connector'`

`tail -n <num-max-of-lines> /var/log/syslog | grep 'edge-mqttbroker'`

`tail -n <num-max-of-lines> /var/log/syslog | grep 'edge-im'`


## 查找更多信息
{: #more_info}

有关 {{site.data.keyword.iot_short_notm}} Edge 的更多信息，请参阅以下主题：
- [{{site.data.keyword.iot_short_notm}} Edge 概述](WIoTP_edge.html#edge_overview)
- [配置 {{site.data.keyword.iot_short_notm}} Edge](WIoTP_edge_config.html#edge_configure)
- [针对 {{site.data.keyword.iot_short_notm}} Edge 进行开发](WIoTP_edge_dev.html#edge_dev)
