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


# 在裝置上安裝 {{site.data.keyword.iot_short_notm}} Edge（預覽）
{: #edge_install_device}

**重要事項：**{{site.data.keyword.iot_full}} Edge 特性僅是有限預覽程式的一部分。未來更新可能包含與此特性的目前版本不相容的變更。請試用，並且[讓我們知道您的想法 ![外部鏈結圖示](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}。

若要讓 IBM Horizon with {{site.data.keyword.iot_short_notm}} 管理 {{site.data.keyword.iot_short_notm}} Edge 預覽機器，您必須先在其上安裝 Horizon+WIoTP 軟體，然後使用 Horizon Exchange 來登錄機器。這些步驟可讓您在暫時測試環境中使用預覽開發儲存庫中的應用程式。

## 安裝之前

### Ubuntu 型 Edge 節點的必要條件

- 如果尚未安裝 Docker，請將它新增至登錄：

`wget -qO- https://download.docker.com/linux/ubuntu/gpg| apt-key add - && add-apt-repository "deb [arch=$(dpkg --print-architecture)] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"`

- 新增基礎 Horizon 套件（ANAX 代理程式）

```
wget -qO - http://pkg.bluehorizon.network/bluehorizon.network-public.key | apt-key add -
   cat <<EOF > /etc/apt/sources.list.d/bluehorizon.list
   deb [arch=$(dpkg --print-architecture)] http://pkg.bluehorizon.network/linux/ubuntu $(lsb_release -cs)-updates main
EOF
```

### Raspberry PI 的必要條件

- 將下列幾行新增至 `/etc/apt/sources.list.d/bluehorizon.list`：
```
deb [arch=armhf] http://pkg.bluehorizon.network/linux/raspbian jessie-testing main
deb-src [arch=armhf] http://pkg.bluehorizon.network/linux/raspbian jessie main
deb [arch=armhf] http://pkg.bluehorizon.network/linux/raspbian jessie-updates main
deb-src [arch=armhf] http://pkg.bluehorizon.network/linux/raspbian jessie-updates main
```

- 將下列這一行新增至 `/etc/apt/sources.list.d/docker.list`：

`deb [arch=armhf] https://download.docker.com/linux/raspbian stretch edge`

- 安裝 `docker-ce`：

`apt-get update && apt-get install docker-ce`


## 安裝 Horizon 及 WIoTP Horizon 套件

`apt-get update && apt-get install -y horizon-wiotp`

## 登錄 Edge 節點

執行代理程式設定：

在正式作業環境中，執行：`wiotp_agent_setup --org <orgId> --deviceType <deviceType> --deviceId <deviceId> --deviceToken <deviceToken>`

它將會執行下列步驟：
- 配置 WIoTP Edge Core IoT Workload。
- 產生 edge-mqttbroker 憑證。
- 使用 hzn 工具，針對 WIoT Platform 登錄節點（如需相關資訊，請使用 `hzn --help`）。

如果在裝置中找到多個 IP 位址，系統會提示您選擇一個 IP 位址，以產生內部憑證。稍後將使用此 IP，來測試 Edge 節點與傳送資料之裝置間的連線。

請確定執行期間沒有任何錯誤訊息。

請等待幾秒，然後驗證已建立 Edge 元件。

請確認是否已建立 Edge 元件映像檔 `docker images`（TAG 版本可能會不同）。

**附註**：下載映像檔並啟動容器的程序可能需要一些時間。請檢查 syslog 檔案，以查看記載詳細資料。

```
# docker images
REPOSITORY                                        TAG                 IMAGE ID            CREATED             SIZE
wiotp-connect/edge/amd64/edge-connector           1.0.1               3d98b90aaa16        3 hours ago         31.7MB
wiotp-infomgmt/edge/amd64/edge-simulated-im       1.0.3               6af448fb2204        7 hours ago         27.6MB
wiotp-connect/edge/amd64/edge-mqttbroker          1.0.2               1973bc0f3d8a        7 hours ago         31.2MB
```

請檢查容器是否已起始 `docker ps`

範例：
```
# docker ps
CONTAINER ID        IMAGE                                                   COMMAND             PORTS
7cbfc719cb71        wiotp-infomgmt/edge/amd64/edge-simulated-im:1.0.1       "/start.sh"
a40f926f78f3        wiotp-connect/edge/amd64/edge-connector:1.0.0           "/start.sh"         0.0.0.0:1883->1883/tcp, 0.0.0.0:8883->8883/tcp
22d8e82f09f3        wiotp-connect/edge/amd64/edge-mqttbroker:1.0.1          "/start.sh"
```

## 模擬 Edge 節點上的裝置

您可以使用 mosquitto-clients 模擬連接至 Edge 節點的裝置：

`apt-get install mosquitto-clients`

在 Edge 節點上執行下列指令（將 OrgId、DeviceType、DeviceID、DeviceToken 及 EdgeNodeIP 取代為適當值)。
若要找出需要傳遞的 IP，請在`產生 Edge 內部憑證`組件下方，檢查於執行 `wiotp_agent_setup` 指令之後顯示的訊息。

```
mosquitto_pub -p 8883 -i 'd:<orgId>:<deviceType>:<deviceId>' -u 'use-token-auth' -P '<deviceToken>' -t 'iot-2/evt/evTest/fmt/json' -m '{"message": "Hello World"}' --cafile /var/wiotp-edge/persist/dc/ca/ca.pem -h <EdgeNodeIP>
```

範例：

```
mosquitto_pub -p 8883 -i 'd:nwr48y:myDevice:device1' -u 'use-token-auth' -P '12345678' -t 'iot-2/evt/evTest/fmt/json' -m '{"message": "Hello World"}' --cafile /var/wiotp-edge/persist/dc/ca/ca.pem -h 10.0.2.15
```

附註：如果您在 Edge 節點外部執行 mosquitto_pub，請使用 -h `<host>` 選項，並調整 --cafile 路徑。

### 取消登錄 Edge 節點

若要取消登錄 Edge 節點，請執行：

`hzn unregister`

### 解除安裝 Horizon-wiotp

若要從節點中完全移除 Horizon 代理程式，請執行下列指令：

`apt-get purge -y horizon horizon-wiotp`

### 疑難排解

檢查 Horizon 代理程式版本：

`dpkg -l | grep horizon`

檢查 Horizon 代理程式即時日誌：

`journalctl -af -u horizon.service`

收集 syslog：

`tail -n <num-max-of-lines> /var/log/syslog `

依容器過濾日誌訊息：

`tail -n <num-max-of-lines> /var/log/syslog | grep 'edge-connector'`

`tail -n <num-max-of-lines> /var/log/syslog | grep 'edge-mqttbroker'`

`tail -n <num-max-of-lines> /var/log/syslog | grep 'edge-im'`


## 尋找相關資訊
{: #more_info}

如需 {{site.data.keyword.iot_short_notm}} Edge 的相關資訊，請參閱下列主題：
- [{{site.data.keyword.iot_short_notm}} Edge 概觀](WIoTP_edge.html#edge_overview)
- [配置 {{site.data.keyword.iot_short_notm}} Edge](WIoTP_edge_config.html#edge_configure)
- [開發 {{site.data.keyword.iot_short_notm}} Edge](WIoTP_edge_dev.html#edge_dev)
