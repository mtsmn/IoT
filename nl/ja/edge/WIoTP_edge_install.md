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


# デバイスへの {{site.data.keyword.iot_short_notm}} エッジのインストール (プレビュー)
{: #edge_install_device}

**重要:** {{site.data.keyword.iot_full}} エッジ機能は、限定されたプレビュー・プログラムの一部としてのみ使用できます。今後の更新によって、この機能の現行バージョンと互換性のない変更が行われる可能性があります。 この機能を試して、[ご意見をお寄せください ![外部リンク・アイコン](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}。

{{site.data.keyword.iot_short_notm}} エッジ・プレビュー・マシンを {{site.data.keyword.iot_short_notm}} を使用する IBM Horizon によって管理するには、最初に Horizon+WIoTP ソフトウェアをマシンにインストールしてから、マシンを Horizon Exchange に登録する必要があります。これらの手順を実行することで、一時テスト環境内のプレビュー開発リポジトリーからアプリケーションを使用できるようになります。

## インストールする前に

### Ubuntu ベースのエッジ・ノードの場合の前提条件

- Docker をレジストリーに追加します (まだインストールされていない場合)。

`wget -qO- https://download.docker.com/linux/ubuntu/gpg| apt-key add - && add-apt-repository "deb [arch=$(dpkg --print-architecture)] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"`

- Horizon 基本パッケージ (ANAX エージェント) を追加します。

```
wget -qO - http://pkg.bluehorizon.network/bluehorizon.network-public.key | apt-key add -
   cat <<EOF > /etc/apt/sources.list.d/bluehorizon.list
   deb [arch=$(dpkg --print-architecture)] http://pkg.bluehorizon.network/linux/ubuntu $(lsb_release -cs)-updates main
EOF
```

### Raspberry PI の場合の前提条件

- `/etc/apt/sources.list.d/bluehorizon.list` に以下の行を追加します。
```
deb [arch=armhf] http://pkg.bluehorizon.network/linux/raspbian jessie-testing main
deb-src [arch=armhf] http://pkg.bluehorizon.network/linux/raspbian jessie main
deb [arch=armhf] http://pkg.bluehorizon.network/linux/raspbian jessie-updates main
deb-src [arch=armhf] http://pkg.bluehorizon.network/linux/raspbian jessie-updates main
```

- `/etc/apt/sources.list.d/docker.list` に以下の行を追加します。

`deb [arch=armhf] https://download.docker.com/linux/raspbian stretch edge`

- `docker-ce` をインストールします。

`apt-get update && apt-get install docker-ce`


## Horizon パッケージおよび WIoTP Horizon パッケージのインストール

`apt-get update && apt-get install -y horizon-wiotp`

## エッジ・ノードの登録

エージェントのセットアップを実行します。

実稼働環境の場合は、以下を実行します。
`wiotp_agent_setup --org <orgId> --deviceType <deviceType> --deviceId <deviceId> --deviceToken <deviceToken>`

これにより、以下の手順が実行されます。
- WIoTP エッジのコア IoT ワークロードの構成
- edge-mqttbroker 証明書の生成
- hzn ツール (詳しくは、`hzn --help` を参照) を使用した WIoT Platform へのノードの登録

デバイス内で複数の IP アドレスが検出された場合は、内部証明書を生成するために、いずれかのアドレスを選択するようプロンプトが出されます。この IP は、後からエッジ・ノードとデータを送信するデバイス間の接続をテストするために使用されます。

実行中にエラー・メッセージが表示されないことを確認してください、

数秒待ってから、エッジ・コンポーネントが作成されたことを確認します。

`docker images` を使用して、エッジ・コンポーネントのイメージが作成されたかどうかを確認します (TAG のバージョンが異なる場合があります)。

**注**: イメージのダウンロードとコンテナーの開始には、数分かかる場合があります。syslog ファイルを調べて、ロギングの詳細を確認します。

```
# docker images
REPOSITORY                                        TAG                 IMAGE ID            CREATED             SIZE
wiotp-connect/edge/amd64/edge-connector           1.0.1               3d98b90aaa16        3 hours ago         31.7MB
wiotp-infomgmt/edge/amd64/edge-simulated-im       1.0.3               6af448fb2204        7 hours ago         27.6MB
wiotp-connect/edge/amd64/edge-mqttbroker          1.0.2               1973bc0f3d8a        7 hours ago         31.2MB
```

`docker ps` を使用して、コンテナーが開始されたかどうかを確認します。

例:
```
# docker ps
CONTAINER ID        IMAGE                                                   COMMAND             PORTS
7cbfc719cb71        wiotp-infomgmt/edge/amd64/edge-simulated-im:1.0.1       "/start.sh"
a40f926f78f3        wiotp-connect/edge/amd64/edge-connector:1.0.0           "/start.sh"         0.0.0.0:1883->1883/tcp, 0.0.0.0:8883->8883/tcp
22d8e82f09f3        wiotp-connect/edge/amd64/edge-mqttbroker:1.0.1          "/start.sh"
```

## エッジ・ノード上のデバイスのシミュレート

mosquitto-clients を使用して、エッジ・ノードに接続しているデバイスをシミュレートできます。

`apt-get install mosquitto-clients`

エッジ・ノードで以下のコマンドを実行します (OrgId、DeviceType、DeviceID、DeviceToken、および EdgeNodeIP を適切な値に置き換えます)。
どの IP アドレスを渡す必要があるかを調べるには、`wiotp_agent_setup` コマンドの実行後に表示されるメッセージ (`Generating Edge internal certificates` 部分の下) を確認します。

```
mosquitto_pub -p 8883 -i 'd:<orgId>:<deviceType>:<deviceId>' -u 'use-token-auth' -P '<deviceToken>' -t 'iot-2/evt/evTest/fmt/json' -m '{"message": "Hello World"}' --cafile /var/wiotp-edge/persist/dc/ca/ca.pem -h <EdgeNodeIP>
```

例:

```
mosquitto_pub -p 8883 -i 'd:nwr48y:myDevice:device1' -u 'use-token-auth' -P '12345678' -t 'iot-2/evt/evTest/fmt/json' -m '{"message": "Hello World"}' --cafile /var/wiotp-edge/persist/dc/ca/ca.pem -h 10.0.2.15
```

注: エッジ・ノード外部で mosquitto_pub を実行する場合は、-h `<host>` オプションを使用し、--cafile パスを調整します。

### エッジ・ノードの登録抹消

エッジ・ノードを登録抹消するには、以下を実行します。

`hzn unregister`

### Horizon-wiotp のアンインストール

ノードから Horizon エージェントを完全に削除するには、以下を実行します。

`apt-get purge -y horizon horizon-wiotp`

### トラブルシューティング

Horizon エージェントのバージョンを確認するには、以下を実行します。

`dpkg -l | grep horizon`

Horizon エージェントのリアルタイム・ログを確認するには、以下を実行します。

`journalctl -af -u horizon.service`

syslog を収集するには、以下を実行します。

`tail -n <num-max-of-lines> /var/log/syslog `

コンテナー別にログ・メッセージをフィルタリングするには、以下を実行します。

`tail -n <num-max-of-lines> /var/log/syslog | grep 'edge-connector'`

`tail -n <num-max-of-lines> /var/log/syslog | grep 'edge-mqttbroker'`

`tail -n <num-max-of-lines> /var/log/syslog | grep 'edge-im'`


## 詳細情報の入手
{: #more_info}

{{site.data.keyword.iot_short_notm}} エッジについて詳しくは、以下のトピックを参照してください。
- [{{site.data.keyword.iot_short_notm}} エッジの概要](WIoTP_edge.html#edge_overview)
- [{{site.data.keyword.iot_short_notm}} エッジの構成](WIoTP_edge_config.html#edge_configure)
- [{{site.data.keyword.iot_short_notm}} エッジのための開発](WIoTP_edge_dev.html#edge_dev)
