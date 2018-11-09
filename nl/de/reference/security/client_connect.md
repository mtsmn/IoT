---

copyright:
  years: 2019
lastupdated: "2018-07-11"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Clientkonnektivität überwachen (Beta)
{: #connect_status}

Mit {{site.data.keyword.iot_full}} können Sie den Verbindungsstatus von Clients abfragen und überwachen. Sie können in Echtzeit wissen, ob ein Client verbunden ist, und Verbindungsproblemen beheben.{:shortdesc}

Sie können beispielsweise sehen, wann ein Client zuletzt aktiv war und warum er nicht verbunden wurde, oder Sie können alle Clients anzeigen, die am vergangenen Tag keine Verbindung hergestellt haben, sodass Sie alle Probleme beheben können.

**Wichtig:** Die Funktion zur Überwachung der Clientkonnektivität steht nur im Rahmen eines eingeschränkten Betaprogramms zur Verfügung. Zukünftige Aktualisierungen enthalten möglicherweise Änderungen, die mit der aktuellen Version dieser Funktion nicht kompatibel sind. Starten Sie einen Versuch und [senden Sie uns Ihren Erfahrungsbericht ![Symbol für externen Link](../../../../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

## Clientverbindungsstatus abfragen
{: #query_status}

Mit der API für den Clientverbindungsstatus können Sie den Verbindungsstatus für alle Clients abrufen und abfragen, die mit {{site.data.keyword.iot_short_notm}} verbunden sind.

Wenn Sie den Verbindungsstatus abfragen, können Sie die folgenden Informationen anzeigen:

 - Konnektivitätsstatus (entweder verbunden oder getrennt)
 - Zeitpunkt der letzten Verbindung im ISO 8601-Format
 - Zeitpunkt des letzten Verbindungsabbaus im ISO 8601-Format
 - Letzte Aktivitätsereignis (z. B. MQTT-Publizierung oder MQTT-Subskription)
 - Zeitpunkt der letzten Aktivität im ISO 8601-Format
 - Entität, die den Verbindungsabbau verursacht hat, wenn die Verbindung zum Client getrennt wird (Client, Server oder Netz)
 - MQTT-Ursachencode für die Verbindungsabbau, wenn die Verbindung zum Client getrennt wird

**Beispiele**

Gehen Sie wie folgt vor, um den Clientverbindungsstatus für einen einzelnen Client abzurufen:

`GET {orgId}.internetofthings.ibmcloud.com/api/v0002/clientconnectionstates/{clientId}`

Gehen Sie wie folgt vor, um den Verbindungsstatus für alle Clients abzurufen:

`GET {orgId}.internetofthings.ibmcloud.com/api/v0002/clientconnectionstates`

Gehen Sie wie folgt vor, um alle verbundenen Clients abzurufen:

`GET {orgId}.internetofthings.ibmcloud.com/api/v0002/clientconnectionstates?connectionStatus=connected`

Gehen Sie wie folgt vor, um alle Clients abzurufen, die innerhalb der letzten zwei Tage aktiv waren:

`GET {orgId}.internetofthings.ibmcloud.com/api/v0002/clientconnectionstates?lastConnectedAfter={currentTime-2D}`

**Hinweis:** Zwischen dem Zeitpunkt, zu dem ein Client eine Verbindung herstellt und dem Zeitpunkt, zu dem der Verbindungsstatus von der API den korrekten Status widerspiegelt, kann es eine kurze Verzögerung geben.

For more information about the API, including all of the available query filters, see [im Swagger-Dokument zu APIs für den Clientverbindungsstatus ![Symbol für externen Link](../../../../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/clientstate-beta.html#!/Client_Connection_State/get_clientconnectionstates_clientId){: new_window}.

## Clientverbindungsstatus überwachen
{: #monitor_status}

 Alle Clients verfügen über ein Überwachungstopic, mit dem Sie den Empfang von Aktualisierungen für den Live-Konnektivitätsstatus für den Client subskribieren können. Weitere Informationen finden Sie in [Gerätestatusnachrichten subskribieren](../../applications/mqtt.html#subscribe_device_commands).

## Fehlerbehebung bei Clientkonnektivität

Mit der API für Verbindungsprotokolle können Sie eine Liste der Verbindungsprotokollereignisse für ein Gerät abrufen, um Konnektivitätsprobleme zu diagnostizieren. In den Einträgen werden erfolgreiche Verbindungen, nicht erfolgreiche Verbindungsversuche, absichtliche Verbindungstrennungsereignisse und vom Server initiierte Verbindungstrennungsereignisse aufgezeichnet.

Weitere Informationen finden Sie [im Swagger-Dokument zu APIs für Verbindungsprotokolle ![Symbol für externen Link](../../../../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html?cm_mc_uid=08862496634215124007223&cm_mc_sid_50200000=36272221529958773076#!/Device_Problem_Determination/get_logs_connection){: new_window}.
